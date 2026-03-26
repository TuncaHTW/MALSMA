## Prompt description

This prompt defines the mapping logic used to translate ISO/IEC 27001:2022 controls (with an initial focus on control 5.34) into a structured set of privacy and security measures. It instructs the language model to:

- Classify each ISO control into one high‑level **MeasureType** (Governance & Strategy, Organisational Processes, Technical & Architectural implementations, or Roles).  
- Derive one or more **ConcreteMeasures** that operationalise the selected MeasureType.  
- Identify 1–3 concrete **Tools** (software products, platforms, or authoritative documents) that can be used to implement each ConcreteMeasure, and record their official **Source** URLs.  
- Output the result as a CSV table with the columns `ISOControl, MeasureType, ConcreteMeasure, Tool, Source`.

The output of this prompt is later imported into the project’s database and serves as the basis for generating a machine‑readable JSON catalog of measures and tools.

***
### MALSMA Prompt
_"Use the ISO/IEC 27001:2022 controls (5.1–8.34) but focus only on Control 5.34 as a basis._

_**Task:**_

_Create a result set with the following schema:_

_**ISOControl:**_

_Original control number and short title (e.g. “5.15 Access control”, “8.7 Protection against malware”)._

_**MeasureType (MT):**_

_Governance & Strategy_

_Organisational Processes_

_Technical & Architectural implementations_

_Roles_

_**ConcreteMeasure (CM):**_
_Automatically assign one or more representative measures based on the MeasureType category, using this mapping:_

_**Governance & Strategy** → Privacy principles, protection goals, eight privacy design strategies, topic-specific policies, accountability structures, methodologies, council of ethics, user rights framework_

_**Organisational Processes** → Training and awareness-raising, transparency and communication, third-party management, creation of guarantees, compliance assessment, data protection impact assessment (DPIA), auditing, incident response, data lifecycle management_

_**Technical & Architectural implementations** → Privacy-enhancing technology, default settings for protection, access controls, data minimisation, features for individual privacy, usability, compliance tools, Privacy Engineering_

_**Roles** → Data Protection Officer, Top Management_

_**Tool:**_

_Name of a concrete technology / commercial product (software, SaaS platform, or technical component) or, if clearly relevant, a key legal/standards document. Prioritise commercial privacy tools that can be purchased or licensed and that help implement the ConcreteMeasure, such as:_

_GDPR/privacy compliance platforms (e.g. RoPA, DPIA, consent management, DSAR handling)._

_Privacy-enhancing technologies (e.g. anonymisation tools, PET-based platforms, consent management tools)._

_DPIA / PIA tools and other risk-assessment tools._

_For each ConcreteMeasure, ensure that at least one Tool is a commercial product (not just a law or high-level guideline)._

_**Source:**_

_Official website or product page of the tool, or, for documents, the official legal / regulatory / standardisation body site._

_Procedure:_

_Assign each ISO control (5.1–8.34) to exactly one MeasureType._

_Based on the MeasureType, select an appropriate ConcreteMeasure from the mapping above._

_For each ConcreteMeasure, find 1–3 real tools/documents, with at least one commercial product (software or SaaS) that can be used to implement the measure (e.g. DPIA tools, GDPR compliance platforms, PETs, consent-management tools)._

_**Output** the results as a CSV table with the columns:_

_**ISOControl, MeasureType, ConcreteMeasure, Tool, Source**"_


