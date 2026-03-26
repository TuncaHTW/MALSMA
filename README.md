# MALSMA
A conceptual approach for a machine-readable and verifiable dynamic taxonomy for security and data protection controls in accordance with ISO/IEC 27001.

Below is a refined README‑style description of the end‑to‑end workflow that transforms an ISO/IEC 27001 control into a machine‑readable JSON artifact. The example used in the pipeline is ISO/IEC 27001:2022 control 5.34 (“Privacy and protection of personal data”), focusing **only** on **workflow and process** from prompt to final JSON artifact, without going into JSON field‑by‑field details.

***

## Workflow from Prompt to Machine‑Readable JSON

The workflow has three main stages:

1. Prompt → CSV result set  
2. CSV → relational database  
3. Database → JSON output

Throughout all stages, the same conceptual entities are used:

- **MeasureType (MT)** – high‑level category  
- **ConcreteMeasure (CM)** – specific measure within one MeasureType  
- **Tool** – concrete technology/platform or key document  
- **Source** – official URL for the Tool  

***

## 1. Stage 1 – From prompt to CSV

### 1.1 Inputs to the prompt

The first stage uses a carefully designed natural‑language [prompt](./Prompt.md) that encodes:

- The scope of the standard: ISO/IEC 27001:2022 controls 5.1–8.34, with a current focus on control 5.34.  
- The four MeasureTypes:  
  - Governance & Strategy  
  - Organisational Processes  
  - Technical & Architectural implementations  
  - Roles  
- A predefined taxonomy of possible ConcreteMeasures for each MeasureType (e.g. “Privacy principles”, “DPIA”, “Privacy‑enhancing technology”, “Data Protection Officer”).  
- Instructions to identify relevant privacy/compliance tools and their official sources (URLs).  
- A required output format: CSV with the five columns  
  `ISOControl, MeasureType, ConcreteMeasure, Tool, Source`.

The *Prompt Architecture* diagram visualises this structure: starting from the ISO control, the prompt must map to a MeasureType, then to one or more ConcreteMeasures, and finally to 1–3 Tools and their Sources. 
</br>
</br>
<img width="1376" height="768" alt="prompt_architecture" src="https://github.com/user-attachments/assets/e02acc1b-9023-46f6-8615-1a0ef50aab1b" />

 *Figure 1: MALSMA Prompt Architecture.*


### 1.2 Prompt execution and mapping logic

When the prompt is executed, the model:

1. **Interprets ISO control 5.34**  
   - Understands it as a privacy and personal data protection requirement.  

2. **Assigns a MeasureType**  
   - Chooses the most appropriate MeasureType based on the control’s intent (for 5.34, all four MeasureTypes are relevant: governance aspects, processes, technical measures, and roles).

3. **Selects ConcreteMeasures**  
   - For each applicable MeasureType, selects one or more ConcreteMeasures from the predefined taxonomy (e.g. “Privacy principles” under Governance & Strategy, “DPIA” under Organisational Processes).

4. **Identifies Tools and Sources**  
   - For each ConcreteMeasure, finds 1–3 concrete tools (software products, SaaS platforms, or authoritative guidance) that help implement that measure, and records their official URLs as Sources.

### 1.3 CSV result set

The output of Stage 1 is a flat CSV table, where each row represents a link between:

- the ISO control (5.34),  
- one MeasureType,  
- one ConcreteMeasure,  
- one Tool,  
- and its Source URL.

This CSV is the **first machine‑consumable artifact**. It is easy to inspect manually and straightforward to import into a database.

***

## 2. Stage 2 – From CSV to database entries

### 2.1 Rationale

The CSV is denormalised by design: the same MeasureType, ConcreteMeasure, or Tool appears in many rows. To prepare for downstream automation and querying, the data is imported into a relational database.

### 2.2 Logical database structure

The database schema mirrors the conceptual model and are part of the workflow diagram. Logically, it consists of:

- A table for **MeasureTypes**.  
- A table for **ConcreteMeasures**, each linked to exactly one MeasureType.  
- A table for **Tools**, including their names and Sources.  
- A mapping table that ties **ISO controls** to combinations of ConcreteMeasures and Tools.

### 2.3 Import procedure

The import typically follows these steps:

1. **Load CSV into a staging structure.**  
2. **Populate MeasureTypes.**  
   - Insert each distinct MeasureType once.  
3. **Populate ConcreteMeasures.**  
   - Insert each distinct ConcreteMeasure, linked to its MeasureType.  
4. **Populate Tools.**  
   - Insert each distinct Tool along with its Source URL.  
5. **Create ISO mappings.**  
   - For every CSV row, insert a mapping record referencing:
     - the ISO control identifier (5.34),  
     - the ConcreteMeasure,  
     - the Tool.

After Stage 2, the database provides a normalised, queryable representation of the relationships implied by the CSV.

***

## 3. Stage 3 – From database to JSON output

### 3.1 Goal

The final stage uses the relational data to produce a **hierarchical, machine‑readable JSON artifact**. This JSON is intended for downstream systems (e.g. compliance tooling, analytics pipelines) and follows an OSCAL‑adjacent catalog style. 
</br>This README provides a conceptual example of such a [JSON artifact](./Catalog2) focusing the ISO 5.34 Control.

### 3.2 Export strategy

At a high level, the export process:

1. **Queries MeasureTypes**  
   - Retrieves all MeasureTypes from the database.  

2. **Builds nested structures for ConcreteMeasures**  
   - For each MeasureType, queries all associated ConcreteMeasures and nests them under that category in the JSON structure.  

3. **Attaches tools to measures**  
   - For each ConcreteMeasure, queries all linked Tools (through the ISO mapping table) and includes them under that measure, along with their Sources.

4. **Packages everything into a single JSON object**  
   - Wraps the grouped MeasureTypes, their ConcreteMeasures, and the associated Tools into a single catalog‑like JSON document.

The result is a **machine‑readable representation of how ISO 27001 controls or other standardized information security frameworks can be implemented in practice**, enriched with MeasureTypes, ConcreteMeasures, and concrete Tools.

***

## 4. End‑to‑end view

Summarising the pipeline:

1. **Prompt → CSV**  
   - A structured prompt encodes the taxonomy and mapping rules.  
   - The model reads ISO 27001 controls and outputs a CSV of mappings:
     `ISOControl, MeasureType, ConcreteMeasure, Tool, Source`.

2. **CSV → Database**  
   - The CSV is imported into a relational schema.  
   - MeasureTypes, ConcreteMeasures, and Tools are normalised; ISO mappings are stored explicitly.

3. **Database → JSON**  
   - Queries traverse the schema to reconstruct a hierarchical view.  
   - The hierarchy is serialized as JSON for downstream machine processing.
  
  
<img width="908" height="664" alt="MALSMA Workflow Architecture" src="https://github.com/user-attachments/assets/dd2e9980-91cd-47b3-8606-616af23efe4e" />

  *Figure 2: MALSMA Workflow Architecture.*

 

