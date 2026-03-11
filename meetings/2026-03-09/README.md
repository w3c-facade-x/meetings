# Agenda

Date: [March 09th 2026, h 15:00 (CET)](https://everytimezone.com/s/883dbca2)

- Case study presentation by Magnus Bakken ([Data Treehouse](https://www.data-treehouse.com/))
- Review of the updated FacadeX specification
- AOB

[Link to join the meeting](https://teams.microsoft.com/meet/35034569292381?p=a73VfVUMmD3KoPle45)
[Timezone](https://everytimezone.com/s/883dbca2)

# Data Facades Community Group – Meeting Minutes

**Date:** 9 March 2026  
**Duration:** 1h 00m  
**Chair:** Enrico Daga  

---

# Minutes of the meeting

**Date:** March 9, 2026  
**Time:** 15:00–16:00 (CET)  
**Chair:** Enrico Daga 
**Presenter:** Mathias Vanden Auweele  
**Participants:**  
Enrico Daga, Luigi Asprino, Mathias Vanden Auweele, Ivo Velitchkov,  Salvador González Gerpe, Els de Vleeschauwer, Magnus Bakken, Ryan Shaw and other community members.

**Video recording** : TBA

# 1. Case Study Presentation – Magnus Bakken (Data Treehouse)

Magnus Bakken presented the work of **Data Treehouse**, a company focused on building high-performance tooling for knowledge graph engineering, primarily applied in industrial sectors such as electrical grids, oil and gas, rail, maritime, and public administration.

The company’s technology stack is centered on transforming existing enterprise data models and data storage systems into **knowledge graphs based on open standards**. Much of their work focuses on lifting structured and semi-structured data into RDF-based representations that can be integrated with domain ontologies.

Key characteristics of their approach include:

- A **high-performance data stack** implemented largely in Rust.
- Use of **Apache Arrow** and **Polars** for columnar, in-memory data processing.
- Integration of knowledge graph processing with modern analytics infrastructure.
- An **open-core software model**, where the core functionality remains open source while additional enterprise functionality may be proprietary.

Bakken introduced **MapLib**, a core component of their platform. MapLib provides:

- Mapping capabilities for transforming data into RDF.
- A SPARQL query engine.
- A Datalog engine.
- A SHACL validation engine.
- Data ingestion and transformation from multiple formats.

MapLib is designed to operate on data frames and allows knowledge graph operations to integrate with common data engineering workflows. The system currently operates primarily in memory, although work is underway to support larger graphs and disk-based processing.

A demonstration was provided showing how **JSON data can be automatically lifted into RDF using the FacadeX approach**. The demo illustrated:

- Loading a JSON file.
- Converting it into a knowledge graph representation consistent with the FacadeX model.
- Querying the resulting graph and returning triples as data frames.
- Exporting the resulting data as Turtle.

The demonstration also highlighted how the generated triples can be further processed using standard data frame operations for analytics.

Discussion followed on datatype mapping issues during JSON lifting, particularly regarding numeric values and whether `xsd:int`, `xsd:long`, or `xsd:integer` should be used when translating JSON numbers to RDF literals. It was noted that this may require clarification within the FacadeX specification.

---

# 2. Review of the Updated FacadeX Specification

Following the presentation, the group discussed aspects of the **FacadeX specification** and its implementation.

The discussion focused on implementation considerations observed during the demo and other ongoing development efforts. Particular attention was given to:

- The practical workflow of lifting structured data (e.g., JSON) into RDF using FacadeX.
- How FacadeX graphs may be queried and transformed after the lifting process.
- How FacadeX representations can serve as an intermediate representation before mapping data into domain ontologies.

One specific issue raised concerned **datatype choices for numeric values extracted from JSON**. Participants noted that JSON itself does not clearly distinguish integer subtypes, which raises questions about which XML Schema datatypes should be used when generating RDF literals.

It was agreed that:

- This issue requires clarification within the specification.
- Implementation experiences from different tool builders will help guide the decision.
- Further discussion will occur during specification refinement.

The group also noted that multiple implementations of FacadeX are emerging, which will help validate the specification against real-world use cases and large datasets.

---

# 3. AOB (Any Other Business)

No additional topics were formally raised.

Participants acknowledged the value of industry-driven implementations such as the one presented by Data Treehouse and expressed interest in continued collaboration between tool developers and the community group.

The meeting concluded with appreciation for the guest presentation and continued work on the FacadeX specification and implementations.
