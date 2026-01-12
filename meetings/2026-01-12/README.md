# Agenda

Date: [January 12th 2025, h 16:00 (CET)](https://everytimezone.com/s/434e7f38)

- Case study presentation by Dr. Ivo Velichov and Dr. Salvador González Gerpe
- FacadeX Metamodel draft specification
- Progress tasks from last meeting
- AOB

[Link to join the meeting](https://teams.microsoft.com/l/meetup-join/19%3ameeting_NmQxYjg2NzEtNzMxYy00ZTQwLWE1MjUtNzM1YTI3ZGJmYThk%40thread.v2/0?context=%7b%22Tid%22%3a%22e99647dc-1b08-454a-bf8c-699181b389ab%22%2c%22Oid%22%3a%22195812f3-ef82-4dff-99fb-63d33e4dfe64%22%7d)
[Timezone](https://everytimezone.com/s/434e7f38)


# Minutes of the Meeting  
**Community Group on FacadeX / SPARQL Anything**

**Date:** As per transcript  
**Chair:** Luigi Asprino  
**Participants:**  
Luigi Asprino, Ivo Velitchkov, Salvador González Gerpe, Enrico Daga, Mathias Vanden Auweele, Els de Vleeschauwer, Emidio Stani, Lennert Van de Velde and other community members. 

---

## Agenda
1. Case study presentation on facade-based data access  
2. Discussion on FacadeX specification  
3. Experiment with RML mappings  
4. Planning next steps and next meeting  

---

## 1. Opening and Agenda Overview
The meeting was opened by Luigi Asprino, who welcomed participants and apologized for issues with the meeting reminder. The agenda was confirmed, with the first item being a case study presentation by Ivo Velitchkov and Salvador González Gerpe. 

---

## 2. Case Study: Facade-Based Data Access in Public Procurement

[Ivo's Presentation](https://kvistgaard.github.io/slides/ppds/sparql-anything-poc/)

### 2.1 Context and Motivation
- The case study concerns public procurement data in the European Economic Area, covering approximately 30 countries and over 250,000 public institutions.
- Procurement data is fragmented, document-centric, and heterogeneous (multiple schemas, formats, and national portals).
- Limited semantics and inconsistent publication practices lead to reduced transparency, limited analytics, and difficulties for SMEs.
- The Public Procurement Data Space (PPDS) initiative aims to address these issues using a knowledge-graph-based approach.

### 2.2 Current Architecture
- Data sources include Tenders Electronic Daily (TED) and national procurement portals.
- Data is transformed into a large-scale knowledge graph (currently around 3.5 billion triples).
- The processing pipeline includes:
  - Graph construction
  - SHACL-based validation
  - Data enrichment and reconciliation
  - Exposure via dashboards and APIs

### 2.3 Motivation for Using SPARQL Anything
- Existing RML-based transformations tightly couple structural extraction and semantic modeling, resulting in rejected low-quality source files.
- SPARQL Anything enables:
  - Separation of structural extraction from semantic interpretation
  - Hash-based URI minting
  - Increased flexibility and governance using SHACL rules and SPARQL CONSTRUCT queries

---

## 3. Facade Strategy: Blind Graph, One-Eyed Graph, and Domain Graph

### 3.1 Definitions
- **Blind Graph:** Raw RDF graph produced directly by SPARQL Anything, reflecting the original structure of XML/JSON sources.
- **One-Eyed Graph:** Intermediate graph with shorter paths and meaningful predicates, improving readability and query performance.
- **Domain Graph:** Final graph where domain semantics (e.g., e-procurement ontology) are applied using SHACL rules.

### 3.2 Discussion Points
- SHACL rules embed SPARQL CONSTRUCT queries and are used for both validation and inference.
- Rules and constraints are treated as first-class graph resources, improving governance.
- JSON sources were not fully covered in the proof of concept but may be addressed in future work.

---

## 4. Technical Demonstration
Salvador González Gerpe demonstrated:
- Transformation flow: XML → Blind Graph → One-Eyed Graph → Domain Graph
- Use of SPARQL Anything service queries
- Application of SHACL rules to infer domain-level triples

**Key benefits of the One-Eyed Graph:**
- Improved performance
- Better semantic readability
- Reduced complexity compared to navigating RDF lists

The intermediate graph was confirmed to be primarily a performance and maintainability optimization.

---

## 5. General Discussion
- Creating a single monolithic query to transform XML directly into the domain graph was considered infeasible due to scale and complexity.
- Multi-step transformations were deemed necessary.
- Suggestions included:
  - Supporting query composition within SPARQL Anything
  - Considering the One-Eyed Graph as a potential generic alternative facade
- Participants expressed interest in defining additional FacadeX schemas tailored to specific needs.

---

## 6. FacadeX Specification Update
- Luigi Asprino presented a [draft specification of the FacadeX metamodel](https://w3c-facade-x.github.io/facade-x-metamodel/), derived from previous academic work.
- The specification:
  - Defines core concepts such as resource, data source, container, slots, and values
  - Uses first-order logic to express constraints in a technology-agnostic manner
- Feedback was requested on:
  - Improving readability
  - Alternative ways to express constraints
  - Missing patterns or required extensions
- Participants were invited to provide feedback via the GitHub issue tracker.

---

## 7. Experiment with RML Mappings
Els de Vleeschauwer reported on [an experiment](https://github.com/w3c-facade-x/meetings/tree/main/meetings/2026-01-12/RML-experiment) mapping CSV data to a FacadeX-style RDF representation using RML.

**Findings:**
- RML is suitable for mapping concrete data sources.
- RML is not currently suitable for defining a fully generic mapping applicable to all CSV, JSON, or XML sources.
- Logical Views help but do not fully address generic transformation needs.

**Discussion:**
- RML may still be useful for structure-specific mappings.
- Alternative approaches discussed:
  - [Mapping algebra approaches](https://arxiv.org/abs/2503.10385)
  - [Pattern-based knowledge graph generation](https://link.springer.com/book/10.1007/978-3-031-01916-6)
- No immediate decision was made; further exploration is required.

---

## 8. Decisions and Action Items
- SPARQL Anything was confirmed as suitable for the PPDS transformation pipeline.
- Luigi Asprino and Enrico Daga will continue refining the FacadeX specification.
- A potential future community task was identified to explore additional facade schemas (e.g., One-Eyed Graph).
- RML remains a possible option, but no immediate extensions will be pursued.

---

## 9. Next Meeting
- **Date:** February 9  
- **Time:** 15:00 CET
- **Planned agenda:**
  - Review of the updated FacadeX specification
  - Presentation by Mathias Vanden Auweele on a SPARQL Anything case study in the railway domain

---

## 10. Closing
Luigi Asprino thanked all participants for the productive and insightful discussion. The meeting was adjourned. 

