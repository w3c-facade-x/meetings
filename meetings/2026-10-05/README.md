# Agenda

Date: [Monday, October 5, h 15:00 (CEST)](https://everytimezone.com/s/6cc12f60)

- ISWC Tutorial preparations
- Review of the updated FacadeX specification
- Discuss timeline and plans towards v0.1 -> v0.5 -> v1.0
- AOB

[Link to join the meeting](https://teams.microsoft.com/meet/346935162820662?p=2drEhkWrdsCINDCC6j)
[Timezone](https://everytimezone.com/s/6cc12f60)


---

# Minutes

**Attendees:** Enrico Daga, Luigi Asprino, Ryan Benjamin Shaw, Els de Vleeschauwer

**Video recording**: https://youtu.be/k95AdAbuAoU

Enrico Daga chaired the meeting, which was meant to be a quick run through the usual agenda: an update on the ISWC tutorial, a walkthrough of the current state of the Facade-X specifications together with the open issues, the release timeline (v0.1 → v0.5 → v1.0) and any other business (the CG logo, ISWC attendance and the next meetings).

---

## ISWC Tutorial preparations

The speakers of the ISWC tutorial will coordinate offline (asynchronously) to prepare the tutorial. Once ready, the material will be shared with the CG, both for feedback and so that members can reuse it for their own purposes. The tutorial website (https://w3c-facade-x.github.io/iswc2026-tutorial/) shows the agenda, which is now considered settled:

1. **The Facade-X problem and idea** — introductory session; Enrico and Luigi still need to decide who takes which slot.
2. **Hands-on**, in three steps: (i) live querying; (ii) functions and magic properties; (iii) a couple of examples of concrete transformation workflows.
3. **Clinic** during the coffee break.
4. **Industry session** (one hour, three slots of about 20 minutes): it opens with the **interactive hands-on by Martin (TriplyDB)**, followed by the lightning talks/industry perspective reports by **Mathias** and **Ivo** (participants who are already playing with the data can keep doing so while listening). Ivo already has titles, as mentioned in the tutorial submission.
5. **Teaching SPARQL Anything** — a 20-minute session by Ryan.
6. **Closing discussion** — Enrico will use the last 20 minutes to present the Community Group, show the timeline and invite participants to join.

**Materials.** All slides and materials must be ready **at least one week before the tutorial**. They will be published (as PDF or downloadable PowerPoint) on the tutorial GitHub repository, since the organisers ask that all content be shared openly, so that it can be reused also by people who do not attend (this may not be possible for all of it, e.g. the TriplyDB material, but should be done as much as possible). A section summarising all the materials will be added to the tutorial website. Enrico will send an e-mail to all speakers about this.

Answering Ryan, Enrico confirmed the deadline for the slides is a week before the tutorial; speakers can send them over and Enrico will update the programme and link them. Provisional titles are fine and can be changed later.

---

## Review of the updated FacadeX specification

Draft: https://w3c-facade-x.github.io/facade-x-specs/#set-of-documents

The specifications are now in an almost stable setting; a few things need to be completed before a first stable release. Enrico invited everybody to read the documents independently: they are deliberately concise, especially the vocabularies.

### Set of documents

There are now seven documents:

- **Primer** — an overview of the set of specifications, showing the building blocks intuitively with examples (e.g. CSV).
- **Schema vocabulary** — the Facade-X primitives: containers, slots, types, etc.
- **Engine vocabulary** — how to describe and configure the execution of a Facade-X engine. Enrico stressed an important distinction: the engine is about **producing a Facade-X representation** (an RDF graph) of a resource, and does not assume that execution happens within a SPARQL engine (e.g. an engine may simply generate Facade-X RDF from CSV or XML).
- **Facade-X in SPARQL** — about the SPARQL engine that executes SPARQL queries on Facade-X resources.
- **Concepts** — the theoretical specification.
- **Format mappings** — the new drafts for **CSV, JSON and XML**.

### Mapping principles

The newest part is the set of general principles that guide the mappings, which the group is distilling while curating the first three mappings:

- **Everything is a string**, unless the format specifies a value type. Engines must **not infer types** from lexical forms (e.g. the string `0` stays a string, unless the format can express numbers, as JSON does with an unquoted `0`).
- **Keep the lexical form** as much as possible, without normalising it (e.g. the number `-0` is not translated to `0`, even though the value is equivalent).
- Each mapping maps the datatypes of the source format to **XSD datatypes** where possible, with a priority when more than one applies; if the lexical form does not respect the datatype in the mapping, it **falls back to string**. Mappings try to cover all datatypes of the source format with some XSD datatype.
- **Ordering is preserved.**
- **Names** in the source tend to become **slots**.
- **Containers are blank nodes** — the discussion on IRIs is postponed.
- The options of the engine vocabulary apply to all formats and must be taken into account.

The mappings started from how things are implemented in SPARQL Anything, trying to make things neater; in some cases the behaviour of SPARQL Anything was adjusted accordingly.

### JSON mapping

JSON has a single numeric type (`number`), which does not map one-to-one to an XSD datatype: its values fall within the value space of `xsd:decimal`, but its lexical space includes the exponent notation, supported by `xsd:double` and not by `xsd:decimal`. Resolution: every number is mapped to **`xsd:decimal`** after checking that the numeral is consistent with it; otherwise **`xsd:double`**; if neither applies (malformed JSON, very unlikely since parsers should enforce the number grammar), it **falls back to string**. This has been implemented in SPARQL Anything, fixing the inconsistencies so that the reference implementation is consistent; the issue will be closed.

The JSON document also describes the mapping, the format-specific options extending the engine vocabulary, and examples. Currently **`null` does not generate triples**; an open issue discusses adding null to the Facade-X model.

### Open issues planned for v0.1

- **Whitespace-only strings / preserving whitespace** (XML): easy to add; probably as a general option of the engine vocabulary.
- **`xml:lang`**: to be supported as soon as possible. At the moment it is not specified, so engines generate an `xml:lang` slot with a value. Its semantics is that every string within the scope of the element carrying the attribute is a string in that language: the output RDF should bring these semantics over by annotating those literals with the language tag. Enrico's feeling is that this should be the **default behaviour**, with an option to disable it. To double check: whether the range of `xml:lang` matches that of language tags in RDF.
- **Null values**: probably handled a bit later.
- **`rdfs:member`**: to be explicitly mentioned in the schema vocabulary, explaining that engines may not generate `rdfs:member` triples, but `rdfs:member` may be supported (e.g. by the SPARQL engine) as an abstraction over lists, to make querying lists easier — as SPARQL Anything already does.
  - Luigi suggested this belongs to a section on magic properties, in the Facade-X in SPARQL document. It turned out such a section (with `rdfs:member` and the functions already identified in SPARQL Anything, e.g. cardinal functions) **already exists**; Enrico will make sure the issue is correctly referenced.
  - On the schema side, Luigi pointed out that declaring the slot-number class as a subclass of `rdfs:member` would be wrong (it would make e.g. `rdf:_12` of type `rdfs:member`). Agreed formulation: **instances of the slot-number class are container membership properties, hence sub-properties of `rdfs:member`** — without redefining RDF's container membership properties or `rdfs:member`. The container membership property rule is missing and must be added.
  - Luigi argued that magic properties and functions are engine-related; Enrico replied that the engine vocabulary is about how resources are transformed into triples, not about how queries are executed. Agreed: magic properties and functions belong to **Facade-X in SPARQL**.
- **RDF serialisation of the vocabularies**: the schema and engine vocabularies have no RDF file yet; this must be added.
- **Materialisation of schema triples**: the specification should **not require** implementations to materialise triples about the schema (e.g. that nodes are containers, properties are slots, `rdfs:member` triples), since they can all be inferred from the graph (every subject is a container; properties are container membership properties if numbered, otherwise string slots; objects of `rdf:type` other than the root are types; etc.). Fewer triples mean more efficient processing, and SPARQL Anything does not generate them. For v0.1 the schema vocabulary will clarify that generating schema triples is not mandatory and is left to implementations; a *verbose* engine option to generate all schema elements may be discussed later.
- **Alignment with RDF/RDFS**: the vocabulary is aligned with RDF and RDFS (container membership properties and the implied `rdfs:member`; types are subclasses of `rdfs:Class`; literals). Further reflection is needed on other implications of the RDF/RDFS semantics.
- **Named graphs**: the current note may not be sufficient. An open issue proposes changing the way SPARQL Anything treats spreadsheet tabs: instead of separate graphs, a macro container holding all tabs, giving a one-to-one mapping between a resource (e.g. a URL) and a graph, which seems more robust. Other cases (e.g. archives/zip files, materialising file-system metadata beyond file locations) concern the mapping of the file system and are left for the future.

The plan is to close these issues over this week and the next, while preparing the tutorial material, and to make the next SPARQL Anything release (1.3), to be used in the tutorial, consistent with the latest specification, so that a first set of features is both specified and supported by the reference implementation.

### Issues mapped to v0.5

- More specific features of the format mappings (XML, CSV): many CSV cases are not yet covered by the spec but should be easy to add.
- Whether **HTML and XML** should be mapped together, and which reference specification to use for HTML (DOM, HTML or XHTML).
- Named graphs (see above).
- Public URIs, redirects and name resolution.
- How far to go with the formal theory. An issue by Ivo questions the use of the term **metamodel** instead of **model**, since Facade-X is not used to represent schemas. Luigi noted that Facade-X also generates the schema of the source (e.g. classes); Enrico agreed, but noted it is not used to represent schemas. Both agreed the term is not crucial and that things should be presented in the simplest possible way, avoiding theoretical details (people should not have to wonder what a metamodel is). Everybody is invited to join the discussion on the issue; a resolution is expected in the next round.

---

## Discuss timeline and plans towards v0.1 -> v0.5 -> v1.0

- **v0.1**: to be released very soon, starting a cycle of releases and more structured feedback, from CG members and the wider community.
- **v0.5**: planned approximately for **March 2027**. Correcting an earlier statement, Enrico proposed that **v0.5** (rather than v1.0) be circulated for feedback, e.g. on the Semantic Web mailing list and to the RDF & SPARQL community group, to understand whether the group is going in the right direction and adjust towards v1.0.
- **v1.0**: a first release, targeted for **early autumn 2027**. Nothing has been mapped to the 1.0 milestone yet.

---

## AOB

### Community Group logo

Luigi recalled the discussion about a logo for the CG. He and Enrico generated a couple of candidates with AI tools, and Luigi showed them: the idea is a facade in front of a building, since the term comes from architecture. Enrico does not like any of them yet: they are too literal; the logo should be **very essential** — e.g. a stylised building shape with blocks, very few lines, recognisable even at very small sizes. Concepts that emerged:

- the basic building blocks of Facade-X (lists, maps, types);
- you can map anything / do any mapping — a sort of Swiss knife;
- a unique abstraction (more of a proxy than a building facade);
- simplicity, conciseness, agility and applicability.

Els noted that what stands out about the group is that it manages to describe everything with a relatively simple and concise specification (she has been reading the version available until the end of August), and agreed the logo should be as simple as possible. Ryan will think about it.

Everybody is invited to bring ideas or proposals (the transcript of this discussion can feed the next round of generation), to be discussed at a future meeting. There is no rush: no logo is better than one that does not work. The CG has no budget for a professional designer, and members should not ask designer friends to work for free; if budget becomes available (e.g. next year, once the first specification is out), it may be allocated to a stronger brand.

### ISWC

Els confirmed she will attend ISWC, but will probably miss the tutorial because of a conflicting working-group meeting on the same day. The group plans a dinner after the tutorial (that evening or another one) and will find a way to meet. Enrico will stay the entire week; Ryan the first two days.

---

## Actions

| Who | Action |
|-----|--------|
| Enrico | E-mail all tutorial speakers about preparing slides and materials at least one week before the tutorial, to be published openly on the tutorial GitHub repository |
| Enrico | Add a materials section to the tutorial website and update the programme with titles and slides |
| Enrico, Luigi | Decide who covers which slot of the introductory session |
| Tutorial speakers | Send slides/materials (and titles) one week before the tutorial |
| Enrico | Add a general option for whitespace-only strings and specify `xml:lang` handling (default on, with an option to disable); check the `xml:lang` range against RDF language tags |
| Enrico | Add `rdfs:member` and the container membership property rule to the schema vocabulary; check the issue is referenced in the magic properties section of Facade-X in SPARQL |
| Enrico | Add RDF serialisations of the schema and engine vocabularies |
| Enrico | Clarify in the schema vocabulary that materialising schema triples is not mandatory |
| Enrico, Luigi | Close the v0.1 issues, release spec v0.1 and align the next SPARQL Anything release for the tutorial |
| All | Review the specification documents and comment on the open issues (e.g. model vs metamodel) |
| All | Bring logo ideas/proposals |

---

## Next meeting

The November meeting is skipped because of ISWC. Whether a public meeting can be arranged during ISWC is still to be decided: updates will be posted on the mailing list. Otherwise, the next meeting will be in **December** (planned for Monday, 7 December 2026), and will be advertised on the list.
