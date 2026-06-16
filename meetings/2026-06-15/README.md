# Agenda

Date: [June 15th 2026, h 15:00 (CET)](https://everytimezone.com/s/6b0a9bc5)

- Invited presentation: Vladimir Alexiev (Graphwise)
- Review of the updated FacadeX specification
- AOB

[Link to join the meeting](https://teams.microsoft.com/l/meetup-join/19%3ameeting_OGI0MTE4YjUtYThhOC00NzJlLWE3NjAtODE4ODM3YTk2YjFj%40thread.v2/0?context=%7b%22Tid%22%3a%220e2ed455-96af-4100-bed3-a8e5fd981685%22%2c%22Oid%22%3a%2250e9ca98-1350-44d3-9737-b43b637c9ecf%22%7d)
[Timezone](https://everytimezone.com/s/6b0a9bc5)

Recording: https://youtu.be/1SGPL9Ye-CI

---

# Minutes

**Attendees:** Enrico Daga, Luigi Asprino, Vladimir Alexiev (GraphWise, Chief Data Architect), Arkadiusz Chadzynski (GraphWise, Knowledge Graph Engineer), Ryan Benjamin Shaw, Els de Vleeschauwer  and others

---

## Invited Presentation: Vladimir Alexiev (GraphWise)

### Declarative Turtle-based Mapping Models

Vladimir Alexiev presented GraphWise's approach to data integration using simple declarative models expressed in Turtle, with a case study on the **Crunchbase dataset** (18 CSV files, ~40 million rows). The core idea is to write the target RDF shape directly as a Turtle template — a concise, human-readable representation from which SPARQL queries are generated automatically via a C-preprocessor macro system.

Key features of the approach:

- **Template-based Turtle models**: Each CSV table has a corresponding Turtle file where field values appear as template variables (e.g., `<org/{uid}>`) and transformations are expressed as macros rather than raw SPARQL binds.
- **Macros for common operations**: Macros encapsulate frequent data operations such as date normalization (`fixed_date`), string splitting (`split`), URI-safe string construction (`ulify`), and void-value elimination. These expand into SPARQL `BIND` expressions targeting specific engines (e.g., Tarql, OntoRefine).
- **Incremental updates via named graphs**: The Crunchbase conversion uses one named graph per row (40 million graphs total). A global timestamp is stored in GraphDB, and SPARQL `DELETE/INSERT` update queries selectively replace only graphs whose timestamp is newer than the last ingestion run. This reduces a 40-minute full conversion to roughly a minute for weekly delta updates.
- **Multi-table model composition**: Individual per-table Turtle models can be concatenated to generate an overall schema diagram, making it easy to validate URI consistency across related tables and detect foreign-key mismatches before running transformations.
- **Readability for domain experts**: Unlike SPARQL queries, the Turtle model closely resembles the target output and can be reviewed by domain experts without SPARQL knowledge. Vladimir noted similar approaches in YARML, dot-OBDA (Ontop), and Stardog's simple mapping format.

Vladimir also demonstrated a second example involving a method ontology CSV, where multi-valued class columns are split and property-shape URIs are computed via CURIE expansion using active prefixes declared in the query.

### Discussion

**Declarative models and modularization (Arkadiusz Chadzynski)**

Arkadiusz asked whether SPARQL Anything supports separating and modularizing the SPARQL construct from the Facade-X service call, to make large queries easier to reuse and debug. Enrico explained two mechanisms currently available:

1. **SERVICE clause reuse**: SPARQL Anything can invoke itself recursively via a `SERVICE` call, allowing sub-queries to be composed into a wrapping query.
2. **Variable substitution in pipelines**: The command-line tool supports variable injection from a CSV input, enabling step-by-step workflows where each step consumes the output of the previous one.

Arkadiusz noted this would greatly improve debugging of large transformation pipelines when migrating from Tarql to SPARQL Anything.

**Named graphs / CONSTRUCT for quads (Vladimir Alexiev & Enrico Daga)**

Vladimir raised that Tarql (SPARQL CONSTRUCT) cannot produce named graphs, which is a significant limitation for the update scenario described. Enrico noted that recent releases of Jena already support CONSTRUCT for quads (producing `.nq` output with `GRAPH` patterns), and that SPARQL Anything uses Jena and therefore also supports this. Vladimir confirmed he was aware of the relevant SPARQL Dev GitHub issue (with Andy Seaborne's contributions) and will add a note that SPARQL Anything supports it.

**Streaming and memory footprint (Vladimir Alexiev)**

Vladimir highlighted that Tarql is streaming — it processes rows and emits triples without accumulating the full result in memory — which is critical for datasets with hundreds of millions of rows. He noted that SPARQL Anything's full-materialization approach is a limitation for very large CSVs, and asked about plans to address this.

Enrico responded that:
- There is an existing **slicing option** in SPARQL Anything (open GitHub issue from three weeks prior) that materializes the source in configurable windows (e.g., by row range or XPath/JSONPath expression).
- The current CSV slicing (by individual row) is too granular and may degrade performance; a windowed approach (e.g., 1000 rows at a time) is under active research.
- A prototype for low-memory-footprint evaluation of basic graph patterns against streamed sources is in progress, though not yet part of SPARQL Anything.

Luigi and Enrico confirmed that streaming is a priority research area.

**Multi-valued columns and Cartesian products**

Vladimir raised the question of how SPARQL Anything handles two multi-valued columns in the same row — specifically whether splitting both independently produces a Cartesian product of bindings. Enrico confirmed that yes, two separate `BIND`/`split` operations on the same row would yield a Cartesian product, and suggested a workaround of nesting two separate SPARQL Anything calls (one per column) to avoid it. Luigi noted that using two bindings per row per column would be the natural result.

**CURIE expansion function**

Vladimir mentioned a function he uses to expand CURIE (prefixed name) strings into full URIs using the active prefix declarations in the query. Enrico confirmed SPARQL Anything has an `expand prefix name` / `FX.` function for this purpose but noted that the IRI() function alone would not work on string variables containing prefixed names. Vladimir offered to open a GitHub issue to make this feature more discoverable.

**Turtle syntax for mappings (Luigi Asprino)**

Luigi proposed that the group consider defining a Turtle-based mapping syntax as a complementary definition language alongside SPARQL. Vladimir's presentation demonstrated that Turtle templates are more readable for non-experts, and since both Turtle and SPARQL are W3C standards, the community group could specify the mapping model in a way that admits multiple serializations.

---

## Review of the Updated FacadeX Specification

This agenda item was not reached during the meeting due to time spent on the invited presentation and discussion.

---

## AOB

Enrico closed the meeting by:

- Thanking Vladimir Alexiev and Arkadiusz Chadzynski for the presentation and discussion.
- Encouraging attendees to **register with the W3C Data Facades Community Group** and subscribe to future meetings.
- Reaffirming the goal of **completing a specification before the end of 2026**.
- Inviting follow-up questions and proposals via the **community group mailing list** or **GitHub issues**.
