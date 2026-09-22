# Agenda

Date: [Monday, September 21, h 15:00 (CEST)](https://everytimezone.com/s/9fcdcbea)

- ISWC Tutorial
- Review of the updated FacadeX specification
- Timeline and plans for the autumn
- AOB

[Link to join the meeting](https://teams.microsoft.com/meet/3928304209427?p=s6iPG5dk3opq14bQVW)
[Timezone](https://everytimezone.com/s/9fcdcbea)

---

# Minutes

**Attendees:** Enrico Daga, Luigi Asprino, Ryan Benjamin Shaw, Mathias Vanden Auweele, Els de Vleeschauwer

**Video recording**: https://www.youtube.com/watch?v=Zvlxgc2zmcg

Enrico Daga chaired the meeting and shared the agenda, published as usual on the CG GitHub organization. The meeting covered the status of the ISWC tutorial preparation, a review of the updated Facade-X specifications (the new engine vocabulary and the format-specific mappings for CSV, JSON and XML), and the plans for the next meetings.

---

## ISWC Tutorial

The tutorial website is up and running (https://w3c-facade-x.github.io/iswc2026-tutorial/). The tutorial is scheduled for **Sunday, 25 October 2026, in the afternoon**.

### Current structure

The current structure is the one originally agreed among the main organizers, with breaks between the slots:

1. **Introduction** — a general introduction and a part on the theory of data access, with some modelling exercises.
2. **Hands-on** — CSV, JSON and XML, followed by functions and magic properties (the ways data can be processed).
3. **Industry perspectives** — lightning talks.
4. **Education** perspective.
5. **The Community Group** — a closing recap of what the CG is doing: the specifications, their status and the plans ahead. Rather than starting from the specifications, the idea is to finish with them, to invite participants to contribute and show that the approach has a future.

Enrico is not sure about the current time split of the hands-on part and proposed to make it more interactive: instead of a presentation followed by a hands-on, a sequence of exercises/examples that incrementally cover the capabilities of the approach, each introduced by a short presentation of a feature. A group of organizers would move around the room to help participants troubleshoot. Case studies brought by the organizers (with their own data) are very welcome as hands-on material; this will be coordinated by e-mail.

### Triply session

Martin (Triply) proposed a session showing how the **Triply platform** can be used to import non-RDF data and query it. Martin could not attend; Enrico will follow up with him by e-mail to decide where it fits (as part of slot 2, or at the beginning of slot 3 together with the industry perspectives) and how much time it needs. The schedule will be adjusted accordingly.

### Industry perspectives

The industry session lasts one hour, currently split into three slots of about **20 minutes each** (including Ivo and Mathias). Mathias presented last week at the SEMANTiCS conference in Ghent on his industry experience with SPARQL and GeoSPARQL (a knowledge graph in production in Norway, what it is used for, with a focus on the railway domain). He can reuse that presentation, shifting the focus towards SPARQL Anything. As an indication, that talk took 30 minutes delivered quickly, and was very interactive, so timing depends on the number of questions. Enrico confirmed 20 minutes as the current plan, with some flexibility.

Ryan and Mathias are fine with the current structure. Ryan offered to help with other sessions beyond his own presentation.

---

## Review of the updated FacadeX specification

### Engine vocabulary and namespaces

The current set of documents comprised a sketch of the vocabulary, the Primer, Concepts and Metamodel, and Facade-X in SPARQL. Enrico's main change was to create a **new vocabulary covering the configuration options of a Facade-X engine**. SPARQL Anything uses the same namespace for the vocabulary and for the configuration, which is not ideal, so the two have been split. There are now three namespaces:

- the **schema vocabulary** (the Facade-X model; unchanged, with a few open issues still pending);
- the **data vocabulary**, i.e. the one generated on the fly (as before);
- the new **engine vocabulary**, representing the general-purpose configuration properties of a Facade-X engine.

Each document now also has a **"Set of documents"** subsection recapping all the documents of the specification. The configuration also has a type, which engines may simply ignore. None of these changes is supported by SPARQL Anything yet: once agreed, the tool will be updated.

An open issue concerns **where extensions of the engine vocabulary live**, since format-specific configurations extend it with dedicated properties (e.g. CSV headers). Enrico would rather not have a different engine namespace for each format, since developers would dislike juggling many namespaces; implementations may also be tolerant with respect to the namespace used for options.

### Which options belong to the core specification

The common properties were taken as they are from SPARQL Anything and were reviewed one by one. Luigi raised the general question of what standardizing an option means: every tool complying with the specification must implement it. Options could therefore be classified into **mandatory (MUST)** and **optional (MAY)** ones. Enrico, while not fond of specifications that leave things open, agreed that marking options as MUST or MAY is a good starting point, with the final decision informed by feedback from other working/community groups. He stressed the importance of reaching a **core set of specifications on which it is easy to agree**, leaving the more sophisticated features for later. Ryan observed that some options assume the engine runs as a command-line tool, and would not make sense for, e.g., a Facade-X engine running in the browser.

Outcome of the review:

- **location, content, media-type, charset**: core options.
- **command, read-from-stdin, query**: CLI-oriented or implementation-specific; they may be kept, but the spec will state that a compliant engine MAY not support them. `command` is tricky (e.g. different operating systems); for `query`, providing a mechanism for query partitioning is arguably outside the remit of the spec.
- **from-archive, archive format**: to be removed from the engine vocabulary; they probably belong to a mapping extension (e.g. for the file system/archives).
- **use-rdfs-member**: Luigi argued it is needed, since it switches between materializing `rdfs:member` and using container membership properties (with `rdfs:member` matched as a magic property). Enrico would focus on the core ones first: if `rdfs:member` is supported as a magic property, materializing it may open inconsistencies with the way the facade is represented. To be discussed further.
- **blank-nodes** and **root**: removed. Proposal (agreed): for the first round of specifications, the spec is **agnostic with respect to IRI minting** — resource identifiers may all be blank nodes, and implementations are free to generate blank nodes or named entities. A specification of IRI minting may come at a later stage (e.g. next year), once the core is robust; blank nodes are used extensively with SPARQL Anything anyway.
- **namespace**: kept; it indicates the namespace the engine assigns to the generated data elements (in place of the default `xyz:`).
- **trim-strings** and **null-string**: kept. `null-string` is not about the notion of null in a given format, but about users wanting to skip specific values (e.g. empty CSV cells producing empty literals).
- **metadata** and **audit**: Ryan asked whether there is a specification of what the metadata and audit graphs look like. Enrico explained that `metadata` materializes resource metadata (e.g. location) in a separate graph, but its behaviour is not consistent across formats and access methods even in SPARQL Anything: both are removed for now. Metadata in general makes sense (e.g. HTTP response headers, resource size, EXIF annotations or other embedded metadata) but is too big a topic for now.
- **generate-predicate-labels**: cut for now. It generates an `rdfs:label` for each slot from the key. Enrico proposed to generalize it into an option for **generating schema annotations** (e.g. typing nodes as containers or slots, plus slot labels). SPARQL Anything currently only generates data, which is appropriate since most schema types can be derived from the position of a node in the graph pattern; for efficiency this should not be the default. Whether engines should generate schema annotations will be discussed later — not dropped from the spec, but not in the first version of the draft.

Enrico also noted an issue with **location**: SPARQL Anything currently always generates a full file-system URI even when the location is relative to the folder where the command is launched. If the location is relative, the generated IRIs/pointers should be relative too. This requires a change request to SPARQL Anything.

The Facade-X in SPARQL document is unchanged, apart from moving some properties to the engine vocabulary.

### CSV mapping

The CSV document explains how CSV is mapped, the fields and how values are handled, the extension of the engine vocabulary, and examples taken from the test suite. The CSV options all make sense; `null-string` will be removed as redundant with the general option.

- **TSV**: CSV is also mapped to TSV/tab formats, in which case the delimiter must default to tab; the document should clarify this, keeping the CSV prefix for all properties. Luigi suggested naming the format more generally (e.g. *tabular*), but this would imply mostly cosmetic changes (all options becoming `tab.`, defaults mapped to the media type) and was set aside.
- **Empty lines**: the spec does not yet say what happens with empty lines.
- **Duplicate header names**: SPARQL Anything appends a suffix (`_1`, `_2`, `_3`, ...). Reusing the same header is discouraged, since slot property names in the metamodel must be distinct. The proposal is to replicate the SPARQL Anything behaviour and see whether anyone raises an issue. Ryan (issues 37 and 38) pointed out that generated names — whether to make headers unique or to give a header to a column that has none — could clash with existing headers. Agreed: the spec should require **uniqueness of slots**; how uniqueness is achieved is secondary. Stress tests are needed. A similar case exists in JSON (duplicate keys): the usual behaviour is that the last value overrides the previous ones, but Facade-X should not override values, since duplicate column names are legitimate in formats like CSV; suffixing is the least of the evils.
- **Header/column count mismatch**: behaviour when headers are enabled and the number of columns differs from the number of headers is to be checked in SPARQL Anything; the goal is to retain all data.
- **Header row other than the first**: common in CSVs exported from spreadsheets. SPARQL Anything uses that row for the headers, but what happens to the preceding data is not specified.
- One issue about redundancy was closed.

Enrico stressed that these are only the issues he found while drafting the specs (using the SPARQL Anything documentation, some AI assistance and his knowledge of the tool), and invited everybody to review the documents in detail.

### JSON mapping

The main open issue concerns **numeric datatypes**. JSON types are mapped one-to-one to XSD datatypes; numbers were initially typed as `xsd:double`, then moved to `xsd:decimal` following a discussion on the SPARQL Anything issue tracker. `xsd:decimal` is correct in terms of value, but does not cover all surface forms: e.g. **exponent notation** is not a valid `xsd:decimal` lexical form (and the sign of **-0** is lost). Since a design principle of Facade-X is to keep the surface form of source values as much as possible, rewriting the value to make it valid is not desirable. Enrico's preferred option: use **`xsd:decimal` whenever possible, fall back to `xsd:double`** when the surface form cannot be kept with decimal, and **fall back to string** if it is not a valid double either. This reflects a general Facade-X principle: keep the data as close as possible to the source, avoiding value conversions.

Ryan asked how **JSON-LD** handles this, since it had to define a mapping between JSON and RDF. Enrico will look into it (e.g. a JSON-LD context applied to JSON with exponent notation or -0) and update the issue.

### XML mapping

Same structure as the other documents: elements are types, attributes are keys, everything else is a value. XPath (and JSONPath, for JSON) options allow transforming only a specific section of a document. Several issues concern design decisions in SPARQL Anything that need review:

- **XPath semantics**: which specification to refer to (left aside for now).
- **DTDs and declared entities**: not covered so far. Proposal: support them following XML semantics (including external DTDs), without reifying the entity declarations.
- **`xml:lang`**: currently treated as any other attribute. Proposal: honour it as a language tag on the generated literals (removing the attribute), consistently with the principle of bringing over whatever semantics the format establishes (as is done for namespaces).
- **Whitespace**: empty elements and elements with empty strings are currently skipped, which is inconsistent with CSV and other formats — probably what XML users want, but to be discussed. Mixed content (inline strings next to sub-elements) raises tricky cases. Supporting `xml:space` (`preserve`/`default`) should be considered.
- **Namespace delimiters**: most XML namespaces do not end with a delimiter, and SPARQL Anything adds one, unlike typical XML parsers. Enrico is inclined to keep this fix and specify it, but is open to discussion.

### Test suite

There is now a draft **Facade-X tests repository** covering the features described in the specs (README still to be written). A `run-tests` script downloads the latest SPARQL Anything release, or uses a jar passed as argument, and runs the tests. A few tests currently fail because of changes (e.g. to the engine namespace) not yet ported to SPARQL Anything.

---

## Timeline and plans for the autumn

- The next update meeting is planned for **Monday, 5 October 2026**.
- Since many members will attend ISWC, a **hybrid CG meeting at ISWC** is being considered; it must not clash with the tutorial, and a venue needs to be found. If it takes place, the November meeting will be skipped.
- **Monday, 7 December 2026**: meeting with Ilaria Tiddi and the other presentations.

---

## Next meeting

**Monday, 5 October 2026**, h 15:00 (CEST).
