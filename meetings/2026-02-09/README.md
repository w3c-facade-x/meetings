# Agenda

Date: [February 09th 2026, h 15:00 (CET)](https://everytimezone.com/s/22a3a004)

- Case study presentation by Mathias Vanden Auweele
- Review of the updated FacadeX specification
- AOB

[Link to join the meeting](https://teams.microsoft.com/meet/37520956560868?p=WxgBU2fEilSkGU5k3G)
[Timezone](https://everytimezone.com/s/22a3a004)

# Minutes of the Meeting  
**Data Facades Community Group**

**Date:** February 9, 2026  
**Time:** 15:00–16:00 (CET)  
**Chair:** Enrico Daga  
**Presenter:** Mathias Vanden Auweele  
**Participants:**  
Enrico Daga, Luigi Asprino, Mathias Vanden Auweele, Ivo Velitchkov, Emidio Stani, Marco Ratta,  Salvador González Gerpe, Els de Vleeschauwer,  Ryan Shaw, Lennert Van de Velde  and other community members.

**Video recording** https://youtu.be/jg1fPEnrfcA

---

## 1. Case Study: Knowledge Graph for Railway Infrastructure (SPARQL Anything)

This section covers the **entire presentation and related discussion**, corresponding to the main agenda item [Slides](https://github.com/Matdata-eu/Slides-W3C-Facade-X/releases/download/v1.1/W3C-facade-x-ppt-v1.1.pdf) 

### 1.1 Background and Business Motivation
- Mathias Vanden Auweele introduced his background in railway infrastructure data and semantic technologies.
- The project focuses on building a **knowledge graph for railway infrastructure** in collaboration with a railway infrastructure manager.
- Railway infrastructure is a highly complex system involving multiple domains:
  - Asset maintenance (tracks, catenary, signalling)
  - Infrastructure projects
  - Operations and capacity management
  - Stakeholder communication
  - Corporate functions (finance, HR, procurement)
- Historically, these domains operate in **data silos**, each with its own representation of infrastructure.
- The long-term vision is a **unified reference model** of railway infrastructure that supports all use cases consistently.

### 1.2 Role of RailML and Long-Term Vision
- RailML is an established XML-based standard used by existing tools.
- The project uses RailML as a **bridge technology**:
  - Short term: generate RailML files from the knowledge graph to support legacy tools.
  - Long term: enable direct RDF consumption from the knowledge graph.
- The knowledge graph acts as both an integration layer and a source for RailML and ERA-compliant outputs.

### 1.3 Technical Architecture and Evolution
- Source systems are heterogeneous, including XML, CSV, and SQL/SQLite-based formats.
- SPARQL Anything was selected because:
  - The team needed to learn SPARQL anyway
  - It avoids custom code
  - It supports XML and CSV well
- Initial pipeline:
  - Source files → SPARQL Anything → RDF (Turtle) → Apache Jena TDB
- Infrastructure:
  - Kubernetes-based deployment
  - Hot-swappable Jena datasets to avoid downtime during ETL processes

### 1.4 Importance of Topology
- RailML lacks explicit topology, which is essential for:
  - Unambiguous positioning of assets
  - Navigation and routing
  - Linking heterogeneous source systems
- An alternative format (OpenTNF) was introduced:
  - Based on GeoSPARQL and SQLite
  - Provides topology as a core concept
- A **rail topology model (RTM)** underpins the approach:
  - Supports micro- and macro-level network views
  - Enables linear referencing and precise asset positioning

### 1.5 Mapping Strategy and Post-Processing
- Asset-by-asset mappings are implemented using SPARQL CONSTRUCT queries.
- Approximately 40 asset types are currently mapped.
- Post-processing was introduced to handle:
  - Cross-file dependencies
  - Ontology alignment (RailML ↔ ERA)
  - Reusable transformations
- About 50 ordered SPARQL UPDATE queries are applied on the Jena Fuseki dataset.
- Execution order is currently managed manually, which was identified as a limitation.

### 1.6 SQL Pre-processing and Data Patches
- Some required spatial operations (e.g. azimuth, linear referencing) are not supported by GeoSPARQL/Jena.
- These computations are performed in SQL (Spatialite/PostGIS) prior to RDF ingestion.
- Additional pre-processing includes:
  - Fixing upstream data quality issues
  - Applying “data patches” early to reduce downstream complexity
- Example discussed:
  - Resolving “double switch nodes” by restructuring topology
- Roughly 40 such patches are currently applied.

### 1.7 Splitting the Knowledge Graph
- The full knowledge graph is too large for downstream tools when exported as RailML.
- The network is divided into **sub-networks** defined by polygon-based regions.
- Each sub-network is extracted using complex SPARQL queries.
- Initial performance issues were addressed by:
  - Dataset copying
  - Parallel execution
  - Use of Apache Airflow for orchestration
- Current performance:
  - ~20–30 sub-networks processed in parallel
  - Total runtime ~25 minutes

### 1.8 Scale and Performance
- Current knowledge graph size: ~8.4 million triples.
- SPARQL Anything:
  - Testing environment: ~8 GB RAM
  - Production ETL: ~24 GB RAM
- Most memory usage occurs during post-processing rather than ingestion.
- Jena TDB’s limited parallelism required workarounds via dataset-level parallelisation.

### 1.9 Future Directions and Wishes
Key improvement areas identified:
- Adoption of a **one-eyed graph + SHACL rules** approach to simplify the pipeline.
- Better support for:
  - Linear referencing and routing in GeoSPARQL
  - SHACL rules and validation
  - Data lineage and provenance
- Improved validation reporting and dashboards.
- URI dereferencing for human-readable exploration.
- Long-term goal:
  - Direct access to source systems
  - Elimination of intermediate CSV and SQL steps
  - Unified rule-based transformation pipeline.

### 1.10 Demonstration
- A web application was demonstrated that:
  - Queries a SPARQL endpoint directly considered (no custom backend API)
  - Allows users to download RailML files
  - Supports exploration via tools such as SPARQL Natural
- Demonstrates feasibility of building applications directly on top of SPARQL endpoints.

### 1.11 Discussion
- Questions focused on:
  - RDF-to-RDF transformations between RailML and ERA ontologies
  - Whether spatial functions should be contributed upstream to Apache Jena
  - Scale, memory usage, and JVM configuration
- The presenter clarified that:
  - Maintaining both vocabularies in the graph supports interoperability needs
  - Contributions to Jena’s spatial capabilities are planned
  - Kubernetes-based scaling is essential for managing resource constraints

---

## 2. Specification Process
- No major progress was reported on the FacadeX specification.
- Discussion focused on collaborative review mechanisms.
- Options considered:
  1. GitHub pull requests with inline comments
  2. Collaborative document tools
  3. Hypothes.is-based web annotation (Ivo opened a group on Hypothesis, [Link to Join the group](https://hypothes.is/groups/x5wgwMGK/facade-x))
- No decision was taken; all options remain open. Options can be discussed in [this issue](https://github.com/w3c-facade-x/facade-x-metamodel/issues/4).

---

## 3. Next Meeting
- **Date:** March 9, 2026  15:00 - 16:00 (CET)
- **Presenter:** To be announced  
- Invitation already circulated.

