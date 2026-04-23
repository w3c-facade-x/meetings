# W3C Facade-X Community Group — Meeting Minutes

Date: [April 13th 2026, h 15:00 (CET)](https://everytimezone.com/s/1991b851)

[Link to join the meeting](https://teams.microsoft.com/l/meetup-join/19%3ameeting_NjE5ZWEwN2ItYzJjNC00MTVmLWJkYjItN2I0OWNhYTAwNGNi%40thread.v2/0?context=%7b%22Tid%22%3a%220e2ed455-96af-4100-bed3-a8e5fd981685%22%2c%22Oid%22%3a%2250e9ca98-1350-44d3-9737-b43b637c9ecf%22%7d)
[Timezone](https://everytimezone.com/s/1991b851)

**Date:** Monday, 13 April 2026  
**FX Specs Issues tracker:** [w3c-facade-x/facade-x-specs](https://github.com/w3c-facade-x/facade-x-specs/issues)

**Attendees:** Enrico Daga, Els de Vleeschauwer (imec), Ryan Benjamin Shaw, Mathias Vanden Auweele

**Recording:** https://youtu.be/47r4xOGPgeE?si=labgpNLncPC4SJh5

---

## 1. Workflow & Collaboration *(~20 min)*

*Housekeeping and process issues that affect how the group works together on the specs.*

- [#4 — Collaboration on specifications](https://github.com/w3c-facade-x/facade-x-specs/issues/4) *(kvistgaard, Feb 9 2026)*
- [#5 — Standardise diagrams](https://github.com/w3c-facade-x/facade-x-specs/issues/5) *(kvistgaard, Feb 9 2026)*
- [#16 — Markdown editing workflow](https://github.com/w3c-facade-x/facade-x-specs/issues/16) *(enridaga, Apr 2 2026)*
- [#15 — Move sparql.xyz URIs to point to the specs](https://github.com/w3c-facade-x/facade-x-specs/issues/15) *(enridaga, Mar 31 2026)*

### Minutes

**Markdown-based workflow.** The group discussed moving to a Markdown-based authoring workflow so that spec text lives directly on GitHub and can be annotated and edited through pull/merge requests. Els confirmed that the RML group follows the same approach: specifications are written in Markdown, converted to HTML via ReSpec and a JavaScript pipeline, with changes managed through merge requests and versioned output folders placed in versioned directories. The group agreed to adopt the same strategy.

**Repository structure.** It was agreed to keep all specs in a single repository for now to make cross-spec synchronisation easier. The module-per-repository model used by RML (separate repos for spec, ontology, and test cases per module) was noted as a future option.

**Test suite.** Enrico reported having created a dedicated repository for a test suite. The proposed structure organises examples by format: each format has a folder containing sample source files, their expected RDF translations, and the associated configuration properties (since different configurations can yield different transformations). Els expressed interest in collaborating on the test suite, noting it would be a useful exercise to check which Facade-X features cover RML inputs and outputs. The group agreed to include SPARQL query test cases (source + query + expected result) alongside the transformation tests, and to extend the structure to cover standard functions and magic properties supported by a Facade engine. A dedicated agenda item will be added to a future meeting.

**Communication channels.** The group confirmed the following norms:
- **GitHub issues** for focused, trackable discussions.
- **Public mailing list** for broader or more discursive exchanges; private e-mail only when there is a specific reason not to publish.
- **Meeting minutes** to record decisions made on any channel, ensuring there is always an authoritative written record.

---

## 2. Conceptual Foundations of the Metamodel *(~30 min)*

*Open questions about the scope, rationale, and core primitives of the Facade-X metamodel — including whether "metamodel" is the right framing at all. These should be resolved before vocabulary work can proceed.*

- [#3 — Why meta-model?](https://github.com/w3c-facade-x/facade-x-specs/issues/3) *(kvistgaard, Feb 9 2026)*
- [#12 — Do we really need the notion of "Slot"?](https://github.com/w3c-facade-x/facade-x-specs/issues/12) *(enridaga, Mar 31 2026)*
- [#13 — Shall formal properties of FX be included in the specification?](https://github.com/w3c-facade-x/facade-x-specs/issues/13) *(enridaga, Mar 31 2026)*

### Minutes

**Why "metamodel"?** Luigi presented the rationale: a *model* is the data model that structures a source (e.g., a graph representing a JSON file); Facade-X is *more abstract* — it imposes constraints on how models are structured — hence *metamodel*. The analogy offered was UML: a Java class is a model element; UML itself (with its rules for classes, associations, cardinalities) is the metamodel. The group accepted this framing.

**Core entities.** The metamodel defines the following entity types: Resources, Data Sources, Containers, Slots, Values, Root, and Types.

- A **Container** is the primary data structure, representing a node in a hierarchical source (XML element, HTML DOM node, JSON object or array, BibTeX entry, etc.). Containers may have a **Type** (e.g., the XML tag name) and may contain **Slots**.
- A **Slot** reifies the relation between a Container and its values or inner Containers, allowing properties (such as keys or ordinal positions) to be attached to that relation. There are two sub-types: **Slot Number** (for list/array positions) and **Slot String** (for map/object keys). A key constraint is that a given slot identifier is unique within a container — no two slots with the same key or index can coexist.
- A **Data Source** corresponds to the content of a file. A **Resource** (identified by URI/URL) may contain one or more Data Sources (e.g., an Excel workbook, a ZIP archive).
- The **Root Container** is the unique top-level container within a hierarchical data source.

**Do we need Slots? (#12).** The group discussed the utility of the Slot concept. Two motivations were identified: (1) it provides a typed anchor — an entity with its own URI — for pointing to a specific location in the source (e.g., a JSON property that becomes an RDF predicate); (2) it unifies list indices and map keys under a single abstraction, while keeping the two varieties distinguishable. The group was broadly satisfied with the concept, noting that renaming may be considered if better terminology emerges.

**Container types.** In response to a question from Ryan Shaw, it was clarified that container types originate from XML/HTML tag names and from BibTeX entry types. Types represent unary predicates that can be expressed about a container without requiring a full slot.

**Formal properties (#13).** Luigi walked through the first-order logic formalisation of Facade-X constraints, covering:
- Domain/range constraints for each relation.
- Cardinality constraints (at least one, at most one).
- Disjointness (e.g., a container cannot simultaneously be a slot or a value).
- An acyclicity constraint ensuring that no container indirectly contains itself — enforcing the strictly hierarchical nature of the model.

The group agreed that the formalisation is valuable for implementers writing execution algorithms (e.g., to rule out impossible query solutions), but is not the right entry point for all readers. The proposed resolution is to **add a new introductory section** to the spec with an informal, by-example presentation of Facade-X concepts, keeping the first-order formalisation as a separate, clearly labelled section.

**Three target audiences (raised by Ryan Shaw).** A productive framing emerged for structuring the documentation around three distinct audiences:
1. **Users** — people who use a Facade-X-powered tool (e.g., SPARQL Anything) to build knowledge graphs; they need worked examples of typical queries and data shapes.
2. **Format developers** — people who want to implement support for a new data format; they need a precise definition of Facade-X and how to apply it.
3. **Engine implementers** — people who want to write a new Facade-X engine in another language or product; they need the full formal specification.

(Academics studying the theory were added as a fourth audience.) It was agreed that the **landing page** of the spec repository should guide readers to the right document based on their role.

---

## 3. RDF Vocabulary Design *(~25 min)*

*Concrete vocabulary-level decisions that depend on the metamodel discussion above: which terms to define or reuse, how to represent constraints, and how to handle URI generation.*

- [#14 — Facade-x properties](https://github.com/w3c-facade-x/facade-x-specs/issues/14) *(enridaga, Mar 31 2026)*
- [#7 — rdfs:member](https://github.com/w3c-facade-x/facade-x-specs/issues/7) *(enridaga, Jan 22 2026)*
- [#8 — How to represent constraints?](https://github.com/w3c-facade-x/facade-x-specs/issues/8) *(enridaga, Jan 8 2026)*
- [#9 — How to specify URI generation for Slots?](https://github.com/w3c-facade-x/facade-x-specs/issues/9) *(enridaga, Jan 8 2026)*
- [#10 — How to deal with data source → named graph?](https://github.com/w3c-facade-x/facade-x-specs/issues/10) *(enridaga, Jan 8 2026)*

### Minutes

Enrico gave a brief overview of the current draft RDF vocabulary. Due to time constraints, a full discussion was deferred to the next meeting; the following points were noted.

**Classes defined so far:** `Container`, `Slot` (a class of properties, with domain `Container` and range `Container` or `Value`), `SlotNumber` (intersection of `Slot` and `rdfs:ContainerMembershipProperty`, for list positions), `SlotString` (for map keys), `Type`, and `Root`.

**URI generation (#9).** A design question was raised: the vocabulary needs to specify how URIs are generated for Containers and Slots from the surface form of the source (string keys or ordinal positions). This is a requirement for the mapping specifications, but it is not clear where this belongs — not in the metamodel, and possibly not in the vocabulary either. A tentative resolution is to document URI generation rules **within the individual format mapping documents**, and to enumerate general URI properties (e.g., uniqueness of container identifiers) in an appropriate shared section. A fix was also noted: Values should not carry a URI generator — the surface form is used directly.

**Named graphs (#10).** A note in the vocabulary draft states that each Data Source should correspond to a named graph, with the root container being the unique root within that graph. The placement of this discussion (vocabulary vs. a separate document vs. implementation guidance) remains open.

**OWL/SHACL formalisation (#8).** The vocabulary includes an OWL axiomatisation (disjointness, domain/range, cardinality). The group discussed adding a **SHACL implementation** of the model to support testing. Mathias Vanden Auweele confirmed experience with SHACL and volunteered to draft SHACL shapes for valid Facade-X output. This will be followed up before the next meeting.

**Namespaces.** The current draft uses `sparql.xyz` namespaces as a placeholder; these will be redirected to the Facade-X CG namespace once it is established. No decision required at this stage.


---

## Next Meeting

**Date:** Monday, 4 May 2026 
**Guest presentation:** Triply (Netherlands) — applying and implementing Facade-X in their product. 