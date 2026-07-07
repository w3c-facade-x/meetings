# Agenda

Date: [July 6th 2026, h 15:00 (CET)](https://everytimezone.com/s/6b0a9bc5)

- Dissemination update
- Review of the updated FacadeX specification
- Timeline and plans for the autumn
- AOB

[Link to join the meeting](https://teams.microsoft.com/l/meetup-join/19%3ameeting_OGI0MTE4YjUtYThhOC00NzJlLWE3NjAtODE4ODM3YTk2YjFj%40thread.v2/0?context=%7b%22Tid%22%3a%220e2ed455-96af-4100-bed3-a8e5fd981685%22%2c%22Oid%22%3a%2250e9ca98-1350-44d3-9737-b43b637c9ecf%22%7d)
[Timezone](https://everytimezone.com/s/6b0a9bc5)

[Recording](https://youtu.be/_SEM4Jbmndo)

# Issues opened from this meeting

- Function list / SPARQL function extensions document (`FX:` entity and string-manipulation functions)
- HTTP support for resource access (authentication, custom request headers) under the SPARQL access spec

# Minutes

**Date:** July 6th 2026, h 15:00 (CET)
**Recording duration:** 53m 9s

**Attendees:** Enrico Daga, Luigi Asprino, Ryan Benjamin Shaw, Ivo Velitchkov, Els de Vleeschauwer (imec), Peter Winstanley and others

---

## Dissemination

- **ISWC 2026 tutorial accepted.** The tutorial submitted by the more active members of the group has been accepted for ISWC 2026 (Bari, end of October) and will run as a half-day session. A website with the programme is needed against a deadline; Enrico or Luigi will work on it and share the draft with the group for feedback. Attendees are welcome to join or co-present.
- **Hybrid meeting at ISWC (Luigi Asprino).** Luigi suggested holding a small in-person gathering for members attending ISWC in person. Enrico proposed running a hybrid community group meeting there and reaching out to the organisers about a room or slot.
- **W3C recap blog post.** Enrico proposed a recap post on the W3C platform summarising the work done so far — invited speakers, feedback collected, and adoption evidence — to put it on record, followed by a LinkedIn post to raise the group's visibility as the academic season closes.
- **COST Action talk.** Enrico has been invited to give a talk on Façade-X in the Goblin COST Action series, at the end of August. The date will be shared on the mailing list once published; attendees are welcome to join.

---

## Review of the Updated FacadeX Specification

Enrico gave a walkthrough of the current state of the specs following recent work on the authoring workflow and content.

### Authoring workflow and build system

- **Markdown-based editing.** Spec content now lives as one Markdown page per document in the `content` folder; changes, edits, annotations and comments can be made through GitHub (pull requests, commit messages, or issues) rather than editing generated HTML.
- **Automatic issue embedding.** The build system now queries GitHub for open issues carrying a given label and embeds them into the relevant spec automatically at build time (via the GitHub Action), so specific issues no longer have to be referenced by hand in the content. Only open issues are shown: once an issue is fixed and the content rebuilt, it drops off the list. Issues opened after a build only appear on the next build.

### Draft documents

New draft material was generated (with AI assistance) and needs proper human review; the prose is generally correct but some titles and passages read as too "AI-generated" and should be revised.

- **Primer (new).** An informal description of how Façade-X works: general use case, an intuitive/informal description of the model and its components, worked examples from JSON and CSV, a short section relating computer-science primitives to Façade-X components, a simple query example, and links to the other specs. Enrico noted the SPARQL access document link should also be added.
- **Concepts and metamodel / vocabulary.** Largely unchanged apart from the Markdown workflow. Open points discussed:
  - Add the `rdfs:member` (DFS member) treatment to the vocabulary.
  - How to represent **constraints**: the Manchester-syntax version is formal but may be unclear; some explanatory text about what the syntax means (e.g. a container may have any predicate that is an instance of `Slot` or `rdf:type`, i.e. a subclass of a union / anonymous class) is likely needed.
  - Luigi suggested **SHACL** to describe some properties of the model, since containers cannot logically be defined as classes that have slots (empty containers exist). At minimum the group should study what such SHACL shapes would look like.
- **SPARQL access document (new draft).** Shows how Façade-X is used within SPARQL (SERVICE clause, options). Discussion points:
  - Naming: whether to keep `facade-x`/`x-facade-x`; kept as-is for now unless challenged.
  - Options can be embedded in the service IRI or expressed as triples. Enrico's (and Ivo's) preference is triples, as clearer and less error-prone, but both forms will be documented for now.
  - The properties currently use the `fx:` namespace (same as the vocabulary); whether a separate namespace and a proper vocabulary description are needed is open.
  - **Execution options** tied to specific implementations (on-disk, slicing, etc.) will be removed from the spec, as they concern back-end behaviour rather than the standard.

### Discussion

- **URI/IRI generation** Enrico clarified the question is whether, and where, to specify how IRIs are minted for slots/properties from source strings that are not themselves URIs (e.g. a JSON key appended to a namespace; qualified XML names could reuse the source URI). Current behaviour URL-encodes strings (spaces become `%20`); alternatives such as slug-based normalisation should be offered (there were discussions about this [on SPARQL Anything issues](https://github.com/SPARQL-Anything/sparql.anything/issues/631)). A dedicated document on IRI generation is likely warranted.
- **Standardising options (Ivo Velitchkov).** Ivo argued in favour of standardising the core set of options for interoperability, so that SPARQL queries port across tools; tool-specific options can sit on top. Enrico agreed — implementation-specific execution should not be included. Ivo added that the SPARQL Anything documentation should later distinguish which options are standard versus SPARQL-Anything-specific.
- **`rdfs:member` vs container membership properties.** Open question whether implementations should be allowed to generate only `rdfs:member` instead of numbered container membership properties.
- **Assigning reviewers.** Enrico proposed assigning, for each spec, a reviewer who is not one of the current editors, and inviting them to join as an editor of that document. Members are encouraged to volunteer.


---

## Timeline and Plans for the Autumn

- **Additional documents needed**, beyond the format-specific mappings:
  - **Function extensions** document (e.g. `FX:` entity, string manipulation, container-membership functions) — an issue to be opened.
  - **IRI generation** document.
- **Format mappings.** Start with CSV, JSON, XML; Markdown is also doable, plus a test set. 
- **Literal typing principle.** To be stated in the primer: every literal is interpreted as a string unless the source format specifies a type, in which case it is mapped to the corresponding XSD type (with JSON examples for Boolean/decimal). 
- **Protocol / resource access** Open an issue to track the discussion on either a section in the access spec, or a separate HTTP-support document if it grows.
- **Release target.** Aim to have a first version of all the specs (vocabulary, metamodel, CSV/JSON/XML/Markdown mappings, plus a test set) ready before ISWC at the end of October, ideally announced during the tutorial and around the ISWC conference. Roughly two working months (September–October) are available. Feedback from industry and the RDF SPARQL working group would then be sought in October–November.

---

## AOB

- **XML/HTML mappings or W3C DOM?** An open design question: whether to write separate specs for XML and HTML or target the **DOM** (a W3C spec) as the reference model. Ryan supported DOM, as he currently has to convert HTML to XHTML. Targeting DOM means a single document with examples covering XML and HTML; Ivo cautioned this changes the `fx` media-type entry point that implementers rely on, and Enrico noted some notions are clearer in XML/HTML terminology. No format beyond XML/HTML (SVG being XML) was identified as needing DOM. To be decided. Existing XML vs HTML parsers (SAX vs DOM; XHTML default namespace handling) should be harmonised for consistency.
- **Protocol / resource access (Ivo Velitchkov).** Ivo asked whether the protocol side (local storage, cloud, REST API, different access modes) needs special treatment, and in particular how **authentication** is communicated (he uses SPARQL Anything against authenticated REST APIs). In principle any valid URL (S3, JDBC, etc.) should suffice for resource access, but HTTP features such as authentication and custom request headers need coverage. Ivo also noted that some things (e.g. location) can currently be encoded in two ways (in the service or as a property value), and the group may wish to prefer one as standard. Enrico agreed to open an issue; a section in the access spec, or a separate HTTP-support document if it grows, will cover it.
- **Cookbook (Ryan Benjamin Shaw).** In response to Ryan's use case — querying a directory of MARC XML records directly rather than combining them into a single file — Enrico described nesting two SERVICE clauses: the first queries the folder (via the archive triplifier, pointed at a folder or even `.`) to obtain the file paths, and the second substitutes each path into the location of a nested SERVICE call, querying each file in turn. Ryan suggested a **cookbook** of good practices as a complement to the primer; Enrico agreed to add one, possibly alongside the test set. It was also noted the archive triplifier could be renamed (e.g. "folders/archives") to make folder support more discoverable.
- **Next meeting.** Provisionally **Monday 7th September 2026**, keeping Mondays and the same time, aiming for the second week of the month where possible.

---

## Actions

- **Chairs:** build the ISWC tutorial website against the submission deadline and share the draft programme with the group for feedback.
- **Chairs:** explore a hybrid community group meeting / in-person gathering at ISWC (contact organisers about a room or slot).
- **Chairs:** draft a W3C recap blog post (invited speakers, feedback, adoption evidence) and a follow-up LinkedIn post.
- **Enrico:** share the date of the Goblin COST Action talk (end of August) on the mailing list once published.
- **Group:** review the draft specs (primer, concepts/metamodel, vocabulary, SPARQL access) over the summer, open issues and raise questions before the next meeting.
- **Group:** volunteer as reviewer for a spec; each spec to get a non-editor reviewer invited to join as editor of that document.
- **Editors:** open an issue for the function-list / function-extensions document.
- **Editors:** open an issue for HTTP support in resource access (authentication, custom headers) under the SPARQL access spec.
- **Editors:** enrich the vocabulary description (`rdfs:member`, constraints, SHACL shapes study); remove implementation-specific execution options from the specs.
- **Editors:** revise AI-generated titles/prose in the new drafts.
- **Group:** decide whether format mappings target the DOM or separate XML/HTML specs; harmonise XML/HTML parser behaviour.
- **Editors:** add the typing principle (string-by-default, XSD when the source types) to the FX primer, with JSON examples.
- **Editors:** add a cookbook of good practices as a complement to the primer (possibly with the test set); consider renaming the archive triplifier to surface folder support.
- **Enrico:** pencil the next meeting for Monday 7th September 2026 and confirm on the list.