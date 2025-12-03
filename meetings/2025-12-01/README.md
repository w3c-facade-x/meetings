# Agenda

Date: [December 1st 2025, h 16:00 (CET)](https://everytimezone.com/s/434e7f38)

- Case study presentation by Prof. Ryan Shaw (University of North Carolina)
- Metamodel draft
- Industry network discussion
- Website
- AOB

[Link to join the meeting]( https://teams.microsoft.com/l/meetup-join/19%3ameeting_ZWFiNWNiMDktZTAzMi00ZWE1LWFmMDAtZTU1NmQzMjkyNjk2%40thread.v2/0?context=%7b%22Tid%22%3a%220e2ed455-96af-4100-bed3-a8e5fd981685%22%2c%22Oid%22%3a%2250e9ca98-1350-44d3-9737-b43b637c9ecf%22%7d)
[Timezone]( https://everytimezone.com/s/434e7f38)


# Minutes

Below is a concise but complete summary of the meeting based on the transcript of the recording.

---

# **Summary of the W3C Data Façades Community Group Meeting (Dec 1, 2025)**

**Chair:** Enrico Daga, Luigi Asprino

**Participants mentioned:** Ryan Shaw, Ivo Velitchkov, Salvador González Gerpe, Els de Vleeschauwer, Andy Seaborne.

## **1. Presentation: Teaching SPARQL/Facade-X – Ryan Shaw**

Ryan Shaw presented how he teaches knowledge-graph concepts to library/information-science students with little programming experience.
Key points:

* Previously used **Tarql** with CSVs; transitioned to **SPARQL Anything / Facade-X** for a two-semester course.
* Students learn RDF, RDFs, SHACL, OWL, then use Facade-X to build real projects (e.g., a digital gazetteer).
* Facade-X works well because **students already learn SPARQL**, avoiding a second mapping language.
* Students successfully handled CSV and GeoJSON using SPARQL Anything.
* Ryan is prototyping **interactive mapping tools** to make building SPARQL construct queries more approachable.
* Challenges for beginners: The “facade” abstraction is initially hard to grasp; interactive feedback helps.

## **2. Specification Work – GitHub Setup (Luigi)**

* A dedicated **GitHub organization** is now created for all CG work.
* Two repositories:

  1. Meeting notes & materials
  2. **Facade-X Metamodel Specification** (with Respec + GitHub pages already scaffolded)
* Members encouraged to join the GitHub org to contribute issues and drafts.

## **3. Main Discussion: How to Structure the Facade-X Specification**

The group debated how to write the official specification.

### **Two Possible Approaches**

1. **Single RDF vocabulary only**, with examples for each source format (CSV, JSON, XML…).
2. **Two-part specification**:

   * an **abstract, format-independent metamodel**, and
   * a concrete **RDF vocabulary** + format-specific profiles.

### **Arguments raised**

* *Pro two-spec approach:*

  * Allows clean mapping from any format (CSV, JSON, XML…) to a Facade-X representation.
  * Future-proof: new formats can be supported by defining mappings to the metamodel.
 
* *Concerns:*

  * The group is RDF-focused; unclear whether non-RDF metamodel specification is necessary.
  * The possibility of creating ontologies for each format with the purpose of expressing the mappings was discarded.

### **Outcome:**

The group leans toward **starting with the RDF vocabulary** and exploring mappings by example first, keeping the metamodel question open.

## **4. Format Profiles and Possible Use of RML**

* Future goal: **Facade-X profiles per format** (CSV, JSON, XML, Markdown, PPTX, etc.).
* **Salvador** suggests modelling each source format with its own ontology to define mappings clearly.
* **Els** introduces how **RML** could help:

  * RML already has **abstract source descriptions**, reference formulations for CSV/JSON/SQL, iterators, etc.
  * Could serve as a language to express the mapping from source-format metamodel → Facade-X RDF.
* Agreement that RML is promising; an exploration is needed.

**Action:**
Els will prepare draft RML mappings for CSV and JSON examples.
Enrico/Luigi will provide simple Facade-X examples for her to work from.
Enrico/Luigi to prepare a first draft of the RDF vocabulary for Facade-X

## **5. Additional Technical Points**

* A brief discussion on SPARQL performance when navigating nested structures in Facade-X representations.
  → Suggested to move this into an issue for SPARQL Anything.

* Ivo highlights the importance of identifying truly **generic constructs** in Facade-X (e.g., container, literal) and specifying them clearly.

## **6. Planning Next Steps**

* Next meeting proposed: **January 12** (avoid holidays).
* Ivo may present his own experience using Facade-X in a proof-of-concept.
