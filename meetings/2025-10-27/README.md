# 🧩 Facade-X Community Group – Kick-off Meeting Minutes
**Date:** 27 October 2025, 15:00 UTC  
**Location:** Online  
**Duration:** ~1 hour  
**Recorded by:** Enrico Daga  

---

## 1. Attendees

**Chairs:**  
- Enrico Daga (Open University)  
- Luigi Asprino (CNR, Italy)  

**Participants:**  
- Marco Ratta (Open University)  
- Ivo Velitchkov (Independent Consultant)  
- Els de Vleeschauwer (UGent-imec)  
- Ryan Shaw (University of North Carolina)  
- Lennert Van de Velde (Meemoo)  
- Salvador Gonzalez Gerpe (Entity Data Company / PPD Project)  
- Cristian Vasquez (Software Engineer)  

---

## 2. Meeting Objectives
- Launch the **W3C Data Facade Community Group (Facade-X)**  
- Introduce members and affiliations  
- Present background and goals of the project  
- Define initial objectives, roadmap, and collaboration setup  

---

## 3. Background Presentation

**Presenter:** Luigi Asprino  

**Summary:**
- **Origins:** Facade-X originated from the EU *SPICE* project (2020) on cultural heritage data integration.  
- **Motivation:** To solve integration challenges across heterogeneous data formats (JSON, CSV, XML, SQL, etc.).  
- **Approach:** Create *data facades* — homogeneous RDF views of diverse data sources, enabling SPARQL querying.  

**Key Achievements:**
- *2021:* Introduced data facade concept at Semantics Conference.  
- *2023:* ACM Transactions on Internet Technology paper formalizing the method for general data formats.  
- Developed the **SPARQL Anything** open-source tool.  

**Community Group Goals:**
1. Standardize the **data-facade approach** and meta-model.  
2. Define **mappings** for different data formats.  
3. Specify **SPARQL dialect** for façade access.  
4. Build a collaborative and interoperable community ecosystem.  

---

## 4. Discussion Summary

### 4.1 Specification & Meta-Model
- First priority: develop a **Facade-X specification** describing the meta-model.  
- Possibly use **RDF and SHACL** for formal definitions.  
- Establish ontology for describing properties and classes extracted from the source.  
- Ensure alignment with existing W3C standards (RDF, SHACL, RML).  

---

### 4.2 Mappings and Profiles
- Define mappings from various data formats (CSV, JSON, XML, etc.) to the Facade-X model.  
---

### 4.3 SPARQL Integration
- Current practice: use of `SERVICE` clauses with custom parameters.  
- Goal: maintain full **SPARQL compatibility** without changing syntax.  
- Explore standardized parameter passing (possibly via HTTP requests).  
- Potential collaboration with W3C RDF/SPARQL Working Groups.  
---

### 4.4 Collaboration Infrastructure
- **GitHub** chosen as the main collaboration platform.  
- Specifications will use **ReSpec** for W3C-style publishing.  
- A **GitHub Pages** website will follow once specs are ready.  
---

## 5. Next Meetings
- **Frequency:** Monthly (recorded; minutes circulated).  
- **Next meeting:** **Monday, 1 December 2025** (rescheduled from 24 Nov due to Thanksgiving).  
- **Focus:**  
  - Begin drafting the meta-model.  
  - Member use-case presentations (**Ryan Shaw** volunteered).  

---

## 6. Closing Remarks
- Chairs thanked participants for their contributions and enthusiasm.  
- Emphasis on collaboration, open standardization, and community growth.  
- Meeting adjourned at approximately **16:00 UTC**.

---
