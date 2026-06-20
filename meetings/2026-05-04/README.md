# Agenda

Date: [May 4th 2026, h 15:00 (CET)](https://everytimezone.com/s/d7d51a00)

- Presentation from Laurens Rietvald and Martin van Harmelen - [Triply](http://triply.cc)
- Review of the updated FacadeX specification
- AOB

[Link to join the meeting](https://teams.microsoft.com/l/meetup-join/19%3ameeting_OGI0MTE4YjUtYThhOC00NzJlLWE3NjAtODE4ODM3YTk2YjFj%40thread.v2/0?context=%7b%22Tid%22%3a%220e2ed455-96af-4100-bed3-a8e5fd981685%22%2c%22Oid%22%3a%2250e9ca98-1350-44d3-9737-b43b637c9ecf%22%7d)
[Timezone](https://everytimezone.com/s/d7d51a00)

# Issues opened from this meeting

- [#17 Specify deterministic URI generation for containers](https://github.com/w3c-facade-x/facade-x-specs/issues/17)
- [#18 Standardise SPARQL helper functions for Façade-X graph patterns](https://github.com/w3c-facade-x/facade-x-specs/issues/18)
- [#19 Discuss named-graph partitioning strategy](https://github.com/w3c-facade-x/facade-x-specs/issues/19)
- [#20 Investigate BNF-grammar-based formalisation of format mappings](https://github.com/w3c-facade-x/facade-x-specs/issues/20)
- [#21 Specify how Façade-X maps relational/SQL databases](https://github.com/w3c-facade-x/facade-x-specs/issues/21)

# Meeting Minutes

**Date:** May 4th 2026, h 15:00 (CET)  
**Video recording:** https://youtu.be/6AMJKPeVysI
**Recording duration:** 52m 49s  

**Attendees:** Enrico Daga, Luigi Asprino, Laurens (Triply, co-founder), Martin van Harmelen (Triply, software developer), Vanden Auweele Mathias, Ivo Velitchkov, Ryan Benjamin Shaw, Andy Seaborne.


[Link to join the meeting](https://teams.microsoft.com/l/meetup-join/19%3ameeting_OGI0MTE4YjUtYThhOC00NzJlLWE3NjAtODE4ODM3YTk2YjFj%40thread.v2/0?context=%7b%22Tid%22%3a%220e2ed455-96af-4100-bed3-a8e5fd981685%22%2c%22Oid%22%3a%2250e9ca98-1350-44d3-9737-b43b637c9ecf%22%7d)  
[Timezone](https://everytimezone.com/s/d7d51a00)

## Presentation from Laurens Rietveld and Martin van Harmelen (Triply)

### About Triply

Laurens opened by introducing Triply and its main product **TriplyDB**, a cloud-based linked data database. Triply is currently the market leader in linked data and triple store products in the Netherlands, with a strong presence in highly regulated industries, including the Ministry of Finance, land registry agencies, the Free University Amsterdam, and Flemish Government Institutes. Their ambition is to be a top-five EU player in linked data and triple stores, and a global service provider for linked data databases.

### Current ETL Challenges

Laurens outlined the challenges of typical ETL pipelines for getting data from source systems (SQL, CSV, XML) into linked data. Current approaches require knowledge of Git, GitLab CI, TypeScript, ETL configuration interfaces, and linked data — a stack that is difficult for regular data scientists and is maintained separately from TriplyDB.

### Towards an ELT Approach with Facade X

Triply is moving from an ETL (Extract–Transform–Load) approach to an **ELT** (Extract–Load–Transform) approach, where data is first extracted and loaded into TriplyDB as quickly as possible, and then transformed post-load using RDF standards (e.g. SPARQL queries). Facade X plays a central role in the **load** step, converting any non-RDF format into an RDF representation for import into TriplyDB.

This is realised in TriplyDB as **Triply Flows** — data pipelines where, for example, an XML source is imported via URL (extract + load using Facade X), then a transform facade applies SPARQL queries to go from one linked data representation to another, followed by a SHACL validation step.

### What Triply Needs from Facade X

Laurens identified the key requirements for the load-oriented use of Facade X:

- **Losslessness:** No information should be lost in the transformation, since no configuration wizards or options are available during load — all customisation happens post-load via RDF standards.
- **Non-interpretation of data:** Data should not be interpreted beyond what the source format defines. For example, a string that looks like a date in CSV should remain a string in RDF, since CSV has no concept of dates.
- **Easily queryable RDF schema:** The structure of the output RDF should be straightforward to navigate.
- **Minimal implementation-dependent behaviour:** The standard should specify exactly what to expect, leaving little room for diverging implementations — important for customer trust and product interoperability.

### Discussion: Query Complexity and UX Challenges

Enrico noted a recurring pattern in industry feedback: practitioners want to load data first and defer transformation to RDF-native tools, which aligns with the ELT direction. He also highlighted that knowledge graph practitioners are often domain experts rather than software engineers, making a low-complexity toolchain essential.

Laurens acknowledged that the main challenges are on the **user experience** side. Users constructing SPARQL queries over Facade X representations can encounter significant complexity — for instance, extracting the last column of a CSV or iterating over rows (skipping a header) requires elaborate `FILTER NOT EXISTS` constructs. This makes Facade X representations harder to query than simpler, record-oriented representations for common CSV patterns.

**Proposed directions:**
- Developing a collection of **SPARQL functions** (potentially standardised as part of the Facade X specification suite) to help users work with common Facade X graph patterns, such as navigating container membership properties or working with ordered sequences. Enrico noted that SPARQL Anything already implements many such functions (e.g. `next`/`previous` for sequence traversal). Laurens stressed the importance of standardising these — proprietary functions in a vendor namespace create lock-in, whereas standardised functions provide portability.
- Luigi described a general-purpose SPARQL query he developed to get an overview of the containers and slots in a Facade X graph, intended for use in a visualisation tool. A [tutorial](https://colab.research.google.com/drive/1R5zeIx4IutF0cc4oTc45CrXjdTSGP4eW?usp=sharing#scrollTo=MgVgJ526sd3e) is available in the SPARQL Anything repository.
- Enrico added that LLMs (e.g. Claude) already produce reasonably accurate SPARQL queries over Facade X representations, which may reduce the urgency of this UX problem over time.

### Discussion: Unsupported Formats

Luigi asked whether Triply had encountered formats not yet supported by the Facade X metamodel. Laurens identified **relational/SQL databases** as the main gap. He also noted a general pattern: formats with more inherent data types (beyond CSV's plain text or XML's two types) are progressively harder to handle — JSON is already more complex than CSV or XML due to its native data types — and having a standard for these mappings would spare implementors from making ad hoc decisions independently.

Enrico confirmed that relational databases have only been explored theoretically so far and never addressed in practice within the group.

---

## Review of the Updated FacadeX Specification

### What Specifications Are Needed?

Enrico asked participants — particularly the Triply guests — what kinds of specification documents they would expect from the group by autumn (targeting September or later). The planned output includes:

- An **RDF vocabulary** for Facade X
- A **metamodel specification** (format-independent conceptual model)
- **Format-specific mapping specifications** for each addressed format

Laurens suggested that reusing existing formalisms where possible (rather than defining new formal systems) reduces overhead, and that **test suites with concrete input/output examples** ("these inputs should produce these outputs") are extremely valuable for implementors — arguably more immediately useful than formal specifications alone. Enrico agreed that example-driven presentation makes specifications much more accessible to developers.

### URI Generation and Blank Nodes

Enrico raised the question of whether and how to specify URI generation for containers. Currently, SPARQL Anything generates deterministic URIs by starting from the resource locator and appending path segments (slot strings or slot numbers) according to the depth and position in the source structure — though the approach is not yet fully consistent across all format transformers.

Laurens strongly endorsed specifying URI generation, arguing that choosing node names is a genuinely hard problem in ETL work: it determines identity, and is especially important for multi-file resources where the same real-world entity must receive the same URI across files. He characterised blank nodes as an avoidance tactic that must eventually be resolved, since published data needs stable identities.

Key points raised:
- URIs should be **deterministic within a file** (e.g. based on X/Y coordinates for CSV) but must **not collide across files** — the source file locator (URL, filesystem URI, S3 path, JDBC URI) can serve as the base.
- There may be value in encoding **versioning information** (e.g. a hash or timestamp) into URIs when a source file is updated in place, so that all versions are preserved.
- **Named graphs** in SPARQL Anything currently partition data by resource (one graph per resource, sometimes more — e.g. one per sheet in Excel). The group noted this deserves more careful discussion; the current approach may not be optimal.
- URIs can encode significant information in a compact form, which can be exploited for classification and exploratory queries.

### Formalising Format Mappings: BNF Grammars

Enrico proposed using **BNF grammars** (already used to specify file format parsers) as a basis for formally expressing the Facade X mappings — annotating grammar tokens to indicate whether they produce a slot, a type, a data value, and which RDF data type applies. This would have the advantage of reusing a well-known formalism, handling edge cases systematically, and — in principle — enabling automatic generation of parsers from the specification.

Laurens supported the idea of reusing BNF grammars as an existing formalism to build on.

Luigi raised a concern about whether non-terminal elements of a grammar map one-to-one with RDF nodes (i.e. whether every non-terminal logical term maps cleanly to an RDF term), which would need to be verified format by format. Enrico acknowledged a known limitation: the grammar alone cannot determine whether something should become a relation or a container — that is a design decision. However, once those decisions are made, BNF annotations could express them deterministically and formally, supporting rigorous W3C-quality specifications.

---

## AOB

### ISWC 2026 Tutorial Proposal

Enrico and Luigi announced their intention to submit a **tutorial proposal for ISWC 2026** (to be held in Bari in November), to showcase the maturity of the Facade X approach. The submission deadline is **June 9th**. All group members were invited to contribute or co-present; Enrico will follow up by email.

### Next Meeting

The next meeting is scheduled for **June 15th**. Enrico noted that the group will continue alternating between sessions with external presenters and working sessions focused on the actual specifications — recent months having been heavy on industry input with less time for spec work.
