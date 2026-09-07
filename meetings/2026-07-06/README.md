# Agenda

Date: [July 6th 2026, h 15:00 (CET)](https://everytimezone.com/s/c2dbedae)

- Review of the updated FacadeX specification
- ISWC Tutorial
- Dissemination of the CG
- Timeline and plans for the Autumn
- AOB

[Link to join the meeting](https://teams.microsoft.com/l/meetup-join/19%3ameeting_OGI0MTE4YjUtYThhOC00NzJlLWE3NjAtODE4ODM3YTk2YjFj%40thread.v2/0?context=%7b%22Tid%22%3a%220e2ed455-96af-4100-bed3-a8e5fd981685%22%2c%22Oid%22%3a%2250e9ca98-1350-44d3-9737-b43b637c9ecf%22%7d)
[Timezone](https://everytimezone.com/s/c2dbedae)

[Link to video](https://youtu.be/_SEM4Jbmndo)

---

# Minutes

**Attendees:** Enrico Daga, Luigi Asprino, Ivo Velitchkov, Ryan Benjamin Shaw, Els de Vleeschauwer (imec), Peter Winstanley

Enrico Daga chaired the meeting and shared the agenda. As usual, plans and minutes are kept on GitHub and the recording is published on YouTube. Since Luigi Asprino could only attend for the first 30 minutes, the agenda was reshuffled on the fly: the ISWC tutorial and dissemination items were taken first, followed by the specification review, the autumn plans and AOB. The minutes below follow the order of the published agenda.

---

## Review of the updated FacadeX specification

### Markdown-based editing workflow

Enrico reported on the work done on the specification, mostly on the management side. As discussed in previous meetings, the specifications are now authored in **Markdown**: the actual content of each document lives in a `content` folder, one page per document, and the ReSpec HTML is generated from the Markdown and a template by the build script. This enables several ways of proposing changes and comments:

- editing directly on GitHub, with permalinks to specific lines;
- comments on commit messages;
- GitHub issues to manage the overall workflow.

### Automatic embedding of open issues

The build system has been changed so that the build script (run through a GitHub Action) queries the GitHub API for the **open issues carrying a given label** (e.g. the label for the FacadeX vocabulary) and embeds them in the corresponding document. Previously, each issue had to be referenced by hand in the document. Now the list of active issues is refreshed at every build: once an issue is fixed and the content is edited, the issue is closed and disappears from the document at the next build. Issues opened after a build only show up at the following build.

### Current set of documents

Several pages were generated and still require proper review. The current version of the specs comprises:

- **Primer** — a new, informal description of how the FacadeX concepts work. Structure: general use case for FacadeX; an intuitive description of the model and its components; examples from JSON and CSV; a small section on the intuitive relationship between computer science primitives and FacadeX components; comments on the structure of the specification and the main properties of FacadeX data; a very simple example query; pointers to the other specifications. The Primer should also point to the FacadeX-in-SPARQL document.
- **Concepts and Metamodel** — unchanged apart from the Markdown workflow.
- **Vocabulary** — unchanged apart from the Markdown workflow; open issues are now embedded automatically.
- **FacadeX in SPARQL** — a newly drafted document about the `SERVICE` clause and how FacadeX is used within SPARQL.

### Vocabulary: open points

- **`rdfs:member`**: the vocabulary must be updated to include `rdfs:member`, both in the Vocabulary document and in the FacadeX-in-SPARQL document, where the old `fx:anySlot` is still referenced.
- **Representation of constraints**: the vocabulary currently expresses constraints informally, with the Manchester syntax version below. The question is how clear this is and whether an informal textual clarification should be added (e.g., a container can have any number of predicates, provided each is an instance of Slot or is `rdf:type`, which is a union / anonymous class), possibly embedding the Manchester syntax. In the classes/slot description, slot properties have Container as domain and Container or Value as range. Luigi suggested that **SHACL** may be a better fit for some of these constraints. Enrico agreed: containers cannot even be defined logically as "classes that have slots", because there can be empty containers, so SHACL shapes could be used to describe some of the properties; at least a study of how those SHACL shapes would look is worthwhile.
- **Root is a singleton**: another example of a property whose expressibility in OWL is unclear.

In summary, the vocabulary description (in particular around `rdfs:member`) needs to be enriched before a call for feedback/review can be issued.

### IRI generation for slots

Enrico raised the question of whether and how the generation of IRIs for slots should be specified. Ryan asked what was needed beyond the existing `fx:entity` function, since he had not run into anything the function could not handle. Enrico clarified that the point is not the function itself but the **general strategy for going from source strings to IRIs**, which is currently nowhere in the specification: for example, a JSON key is appended to the `xyz` namespace or to a namespace provided by the user. Some sources (e.g. XML with qualified names) already provide URIs which can be reused, but in many cases source strings are not URIs, and a proper IRI is still needed for the slot property. The specification should therefore have a place that discusses how slot IRIs are minted, and within it the available options: so far only URL encoding is applied (spaces in CSV column names become `%20`), but users may want a **slug-style normalization** of the string as an alternative — as discussed with GraphWise at the previous meeting. Enrico is unsure whether IRI minting deserves a separate document or belongs to the Vocabulary; there is already an issue for IRI generation.

### FacadeX in SPARQL

The document currently shows, mainly by example, how FacadeX is used within SPARQL. Open points raised:

- **Formalization**: it must be understood how much and how to formalize the `SERVICE` clause and the way properties are encoded in it.
- **Protocol name**: the `x-sparql-anything:` scheme should be replaced with something more generic. Options discussed: `x-facade-x` (which "doesn't sound great"), `fx:` (a protocol extension should normally start with `x`, but a registered scheme could avoid that), or, as Luigi proposed, swapping to `x-facade` / `xfacade`. The decision is to keep an `x-` prefixed, FacadeX-based scheme unless somebody challenges it; in any case it is more generic than `x-sparql-anything`.
- **Options encoding**: options can be embedded in the `SERVICE` IRI or expressed as triples; both are currently described by example, and a better way of specifying this is needed. Ivo Velitchkov noted that currently several things can be done in two different ways (e.g., the location in the `SERVICE` IRI or as the value of a specific property) and asked whether the spec should keep both or standardize one. Enrico expects the current draft to describe both; both he and Ivo personally prefer the **triple-based** form, which is clearer syntactically and less error-prone, but there is no reason not to support both. The description will stay informal for the moment, since formalizing the IRI form would require specifying a URI-building pattern (as originally done with Luigi).
- **Namespace for options**: the document currently uses the `fx:` namespace for the options, the same as the vocabulary. Enrico is not sure this is right: perhaps a different namespace is needed for the properties, and a place where they are described as a vocabulary (or they may simply be described informally).
- **Which options belong to the specification**: the options listed are taken from SPARQL Anything. Enrico considers most of them relevant to a specification: media type, namespace, whether to mint IRIs for containers or use blank nodes, the IRI of the root container (reused for all containers in the data source), whether to trim strings, the null string, etc. **Execution options** (e.g., on-disk, slice, and other options about how the query is executed) will be removed, since they only make sense for specific implementations. `fx:anySlot` should be replaced with `rdfs:member`. An open question is the `use-rdfs-member` option: `rdfs:member` will be kept as a magic property, but the idea is whether implementations should be allowed to generate only `rdfs:member` instead of the container membership properties.

Ivo strongly supported **standardizing the options**, since the whole effort is about interoperability: a user of one FacadeX implementation should be able to reuse all their SPARQL queries when moving to another tool, which won't be the case if each tool has its own options. Tools can add their own on top, but there should be a core of options that is part of the vocabulary. Enrico agreed: all the main options (properties, location, media type, ...) are meant to be standardized; only the implementation-specific execution options are being removed. Ivo added that, once this is agreed, the **SPARQL Anything documentation** should be updated to clearly distinguish which options follow the specification and which are SPARQL Anything-specific.

### Reviewers and editors

Enrico proposed that a **reviewer be assigned to each document**, preferably someone other than Enrico and Luigi; reviewers would be invited to become editors of the specific document. Members are invited to come forward. Everybody is invited to take time during the holidays, before the next meeting in September, to read the documents, open issues and ask questions, so that the documents are as robust as possible before they are shared with the wider communities, in particular the SPARQL community.

### Additional documents needed

- **Function extensions**: a document listing the function extensions (e.g., `fx:entity`, string manipulation functions, functions operating on container membership properties, ...). SPARQL Anything has a long list of functions, some of which are worth including as extensions that FacadeX implementations may support through the `fx:` namespace. An issue needs to be opened for this document.
- **IRI generation** (see above; issue already open).
- **Format-specific mappings** (see Timeline below).
- **Resource access protocol / HTTP** (raised by Ivo, see below).
- **Cookbook** (raised by Ryan, see AOB).

Enrico also noted that a general principle should be stated in the Primer: **every literal is interpreted as a string unless the source format defines specific types, in which case they are mapped to XSD datatypes** (e.g., JSON booleans or decimals). This will become clearer when working on the actual mappings for each format.

---

## ISWC Tutorial

The tutorial proposal submitted to **ISWC 2026** by a group of the most active members of the CG has been **accepted**. The tutorial will be a half-day event at the conference, at the end of October. There is a deadline for the tutorial website, so Enrico and/or Luigi will work on it; once ready, the programme will be shared with the whole group for feedback. Members attending ISWC are very welcome to join.

Luigi proposed that, if other members are physically present at ISWC, a small in-person meeting could be organized there. Enrico agreed that a **hybrid CG meeting** at ISWC would be a very good idea; the organizers can be asked whether a room or a slot could be reused for this.

---

## Dissemination of the CG

Enrico noted that dissemination so far has consisted of just one LinkedIn post. Since the end of the academic season is approaching, he proposed:

- a **recap blog post on the W3C platform**, summarizing the work done so far: the invited speakers, the feedback collected, the evidence of adoption. It is worth having these things on record somewhere;
- a further **LinkedIn post** to make some noise around the CG work.

Enrico has also been invited to give a **talk on FacadeX at the GOBLIN COST Action series**, at the end of August (exact date to be confirmed). The details will be shared on the mailing list as soon as published; everybody is welcome to join and take part in the discussion.

---

## Timeline and plans for the Autumn

Apart from the ISWC tutorial, the goal is to **publish the first version of all the specifications before the ISWC conference** (end of October), so that they can be announced a week or a few days before the conference and during the tutorial, also to attract participants and exploit the conference for dissemination. This is considered doable with roughly two working months (September and October). If a full set of documents can be reached sooner, the **format-specific specifications** can be added: at least CSV, JSON and XML; Markdown should also be doable. A **test set** is more work but worth it. Ideally, by October/November the first set of specs will be available and feedback can start being collected from the industry sector and the **RDF/SPARQL Working Group**.

### Resource access protocol and HTTP

Ivo asked whether the protocol side needs special care: the resource to be accessed could be in local storage, in the cloud, or behind a REST API, in a few different modes. Enrico observed that the only requirement is that a resource has a URL, so in principle any valid URL should work (e.g., an S3 URL or even a JDBC-style URL), and URL resolution should be enough for resource access; however, the point deserves consideration. Ivo added the question of **how to communicate authentication**: he is using SPARQL Anything to interrogate REST APIs, some of which require authentication. Enrico acknowledged that this refers to the HTTP functionality of SPARQL Anything, which exposes a number of features of the HTTP client library (authentication, extending request headers, etc.). An **issue will be opened**; the topic can start as a section of the FacadeX-in-SPARQL document and, if it becomes too difficult to manage, become a separate document about HTTP support for resource access (i.e., how the HTTP protocol is exposed so that requests can be customized through the configuration).

### Format mappings: DOM vs. separate XML and HTML specs

Enrico raised the question of whether XML and HTML should have separate specifications, or whether the **Document Object Model (DOM)**, which is a W3C specification and can be used to access XML, HTML, XHTML and SVG, should be the reference: with the DOM there would be a single document to write, with examples covering both XML and HTML. Points raised:

- Ryan found this useful, since he currently has to convert HTML to XHTML, which is sometimes painful. He also noted that he had been using the XML support for HTML, unaware of the HTML support; the HTML format in SPARQL Anything also includes features that go beyond the format (browser control, screenshots). Enrico explained that the headless-browser feature is an extension that renders the page in memory before querying (useful when JavaScript modifies the content), and that it should not be considered in the spec for now.
- SPARQL Anything currently has two parsers: a SAX-based one for XML and a DOM-based one for HTML. There are also other differences between the XML and HTML triplifiers (e.g., HTML uses the XHTML default namespace while XML uses none when no namespace is defined); **harmonizing the two** would be a good idea in any case.
- Ivo pointed out that going for DOM would break the mapping given by the **`fx:media-type` property**, which acts as the entry point for activating one or another format, and that this is relevant for all implementers. Enrico agreed, adding that even with one document per format multiple MIME types must be supported anyway ("the MIME type is a jungle"), and that some notions are much clearer in the terminology of XML or HTML rather than in DOM terms.
- No other DOM-based formats were identified besides XML, HTML and SVG (an XML application).

The DOM-vs-separate-specs question remains **open**.

---

## AOB

### Querying a folder of files (Ryan Shaw)

Ryan is working with **MARC XML** records from library catalogs, currently combined into a single collection file in order to query them, and asked whether it is possible to query a directory of records directly. Enrico explained the technique: **nest two `SERVICE` clauses** — the outer one queries the folder (using the archive triplifier, pointing the location at a folder path, even `.`), which yields the list of file paths; the file paths are then used to build the location IRI passed to the inner `SERVICE` clause, which is executed once for each binding. Anything reachable by the user running the Java process is accessible. Enrico shared an example from the SPARQL Anything documentation (a bit convoluted, but showing the technique). Ryan suggested renaming the "Archive" documentation section to "Folders or archives", since he assumed it only concerned tarballs; Enrico agreed. Any problems encountered should be reported as issues.

Ryan also suggested that a **cookbook** of good practices would be a good complement to the Primer. Enrico agreed that this must be added, possibly together with the test set.

### Next meeting

The next meeting will be on **Monday, 7 September 2026**, same time (second Monday-ish of the month, as much as possible). Els will not be able to attend. The proposal will be circulated on the list in case of objections.

---

## Action items

- **Enrico/Luigi**: set up the ISWC tutorial website before the deadline and share the programme with the group.
- **Enrico**: ask the ISWC organizers about a room/slot for a hybrid CG meeting.
- **Enrico**: write a recap blog post on the W3C platform and a LinkedIn post.
- **Enrico**: share the details of the GOBLIN COST Action talk on the mailing list.
- **All**: review the specification documents (Primer, Concepts and Metamodel, Vocabulary, FacadeX in SPARQL) before the September meeting; open issues and volunteer as reviewers/editors.
- **Enrico**: add `rdfs:member` to the vocabulary and replace `fx:anySlot` in the FacadeX-in-SPARQL document; remove execution options from the spec.
- **Enrico**: open issues for the function extensions document and for HTTP/resource access (protocol and authentication).
- **Enrico**: add the literal-typing principle (strings unless typed by the source format, mapped to XSD) to the Primer.
- **Enrico**: add a cookbook to the planned deliverables, together with the test set.
- **Enrico**: rename the "Archive" section in the SPARQL Anything documentation and share the folder-query example with Ryan.
- **Enrico**: pencil in 7 September 2026 for the next meeting and announce it on the list.
