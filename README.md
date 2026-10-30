# ☄️ NASA Meteorite Landings: Witnessed Falls (FAIRified)

**A FAIR, machine-actionable knowledge graph of the 1,107 meteorite falls that people actually saw happen.**

[![Licence: CC BY 4.0](https://img.shields.io/badge/licence-CC%20BY%204.0-blue.svg)](https://creativecommons.org/licenses/by/4.0/)
[![F-UJI FAIR score](https://img.shields.io/badge/F--UJI%20FAIR%20score-100%25-brightgreen.svg)](https://www.f-uji.net)
[![Persistent IDs: W3ID](https://img.shields.io/badge/PIDs-W3ID-orange.svg)](https://w3id.org/FAIR-course-UT/2026/group01/)
[![Format: RDF/Turtle](https://img.shields.io/badge/format-RDF%2FTurtle-lightgrey.svg)](https://w3id.org/FAIR-course-UT/2026/group01/data/rdf)
[![SHACL: conforms](https://img.shields.io/badge/SHACL-conforms-success.svg)](#-validation)

This repository contains the complete output of a FAIRification project carried out for the course **FAIR Data Engineering** at the **University of Twente** (2026). We took the NASA *Meteorite Landings* dataset, selected the witnessed falls, cleaned and documented them, modelled them with an OWL ontology, converted them to RDF, described them with standards-based metadata, gave every resource a persistent identifier, and measured the result with an automated FAIR assessment.

> **In one sentence:** we started with a table that people could download, and ended with data that explains itself: every value has a defined meaning, every resource has a permanent address, and both people and machines can find and use it.

---

## 📑 Contents

- [Quick start](#-quick-start)
- [Persistent identifiers](#-persistent-identifiers)
- [Repository structure](#-repository-structure)
- [The dataset](#-the-dataset)
- [Data preparation](#-data-preparation)
- [Semantic data model (ontology)](#-semantic-data-model-ontology)
- [The RDF knowledge graph](#-the-rdf-knowledge-graph)
- [Metadata](#-metadata)
- [Validation](#-validation)
- [FAIR assessment](#-fair-assessment)
- [Querying the data](#-querying-the-data)
- [Reproducing the work](#-reproducing-the-work)
- [Known limitations](#-known-limitations)
- [Licence and rights](#-licence-and-rights)
- [How to cite](#-how-to-cite)
- [Authors](#-authors)
- [Acknowledgements](#-acknowledgements)

---

## 🚀 Quick start

| I want to… | Go to |
|---|---|
| Read about the dataset in a browser | https://w3id.org/FAIR-course-UT/2026/group01/meta/dataset/nasa-meteorite-landings-witnessed-falls-fairified |
| Download the knowledge graph (Turtle) | https://w3id.org/FAIR-course-UT/2026/group01/data/rdf |
| Download the cleaned table (CSV) | https://w3id.org/FAIR-course-UT/2026/group01/data/csv |
| Look up what a term means | https://w3id.org/FAIR-course-UT/2026/group01/ont |
| Get the machine-readable metadata | Request the dataset identifier with `Accept: text/turtle` (see below) |

```bash
# Human view: a browser gets the landing page
open https://w3id.org/FAIR-course-UT/2026/group01/meta/dataset/nasa-meteorite-landings-witnessed-falls-fairified

# Machine view: the same identifier returns DCAT metadata in Turtle
curl -L -H "Accept: text/turtle" \
  https://w3id.org/FAIR-course-UT/2026/group01/meta/dataset/nasa-meteorite-landings-witnessed-falls-fairified

# Download the full knowledge graph
curl -L -o Meteorite_Falls_RDF.ttl https://w3id.org/FAIR-course-UT/2026/group01/data/rdf
```

---

## 🔗 Persistent identifiers

Every resource in this project has a **W3ID** persistent identifier under the namespace:

```
https://w3id.org/FAIR-course-UT/2026/group01/
```

W3ID forwards each identifier to the course redirect service ([`luizbonino/pid-relay`](https://github.com/luizbonino/pid-relay/tree/main/rules/FAIR-course-UT/2026/group01)), where our redirect rules live in an Apache `.htaccess` file together with a `tests.yaml` that declares the expected result of every identifier. Changes are submitted as pull requests and only go live after all tests pass, so **every identifier listed here is tested to resolve**.

Because identifiers are separate from where files are stored, the files can move (for example to a long-term repository) without breaking a single identifier: only the redirect target changes.

| Identifier | Resolves to | Redirect |
|---|---|---|
| [`…/group01/`](https://w3id.org/FAIR-course-UT/2026/group01/) | This GitHub repository | 302 |
| [`…/group01/repo`](https://w3id.org/FAIR-course-UT/2026/group01/repo) | This GitHub repository | 302 |
| [`…/group01/ont`](https://w3id.org/FAIR-course-UT/2026/group01/ont) | `ontology/Meteorite_Ontology.owl` | 302 |
| [`…/group01/data`](https://w3id.org/FAIR-course-UT/2026/group01/data) | `rdf/Meteorite_Falls_RDF.ttl` | 302 |
| [`…/group01/data/rdf`](https://w3id.org/FAIR-course-UT/2026/group01/data/rdf) | `rdf/Meteorite_Falls_RDF.ttl` | 302 |
| [`…/group01/data/csv`](https://w3id.org/FAIR-course-UT/2026/group01/data/csv) | `data/prepared/Meteorite_Falls_prepared.csv` | 302 |
| [`…/group01/meta/dataset/nasa-meteorite-landings-witnessed-falls-fairified`](https://w3id.org/FAIR-course-UT/2026/group01/meta/dataset/nasa-meteorite-landings-witnessed-falls-fairified) | Landing page **or** `metadata/records/dataset_metadata.ttl` | 303 (content negotiation) |
| [`…/group01/meta/distribution/meteorite-witnessed-falls-original-csv`](https://w3id.org/FAIR-course-UT/2026/group01/meta/distribution/meteorite-witnessed-falls-original-csv) | `metadata/records/distribution_csv.ttl` | 302 |
| [`…/group01/meta/distribution/meteorite-witnessed-falls-fairified-rdf`](https://w3id.org/FAIR-course-UT/2026/group01/meta/distribution/meteorite-witnessed-falls-fairified-rdf) | `metadata/records/distribution_rdf.ttl` | 302 |

### Content negotiation

The dataset identifier serves two audiences from one address:

```
                                        ┌─► Accept: text/turtle ──► dataset_metadata.ttl   (machines)
w3id.org/…/meta/dataset/… ──► pid-relay ┤
                                        └─► anything else ──────► landing page (HTML)     (people)
```

---

## 📁 Repository structure

```
FAIR-Meteorites-group1/
├── README.md                                   ← you are here
├── data/
│   ├── original/
│   │   └── Meteorite_Landings.csv              ← unmodified NASA source (45,716 records)
│   └── prepared/
│       ├── Meteorite_Falls_prepared.csv        ← cleaned subset: 1,107 witnessed falls
│       ├── Meteorite_Correction_Log.csv        ← every corrected value, before and after
│       └── Meteorite_Data_Dictionary.csv       ← meaning, type and unit of each column
├── ontology/
│   └── Meteorite_Ontology.owl                  ← OWL ontology (v0.2), RDF/XML
├── rdf/
│   └── Meteorite_Falls_RDF.ttl                 ← the knowledge graph (13,608 triples)
├── metadata/
│   ├── schemas/                                ← SHACL shapes for the metadata (built in Contour)
│   └── records/
│       ├── dataset_metadata.ttl                ← DCAT description of the dataset
│       ├── distribution_csv.ttl                ← DCAT description of the CSV download
│       └── distribution_rdf.ttl                ← DCAT description of the RDF download
├── validation/                                 ← SHACL shapes and validation material for the data
└── docs/
    └── index.html                              ← landing page with schema.org JSON-LD (GitHub Pages)
```

---

## 🌠 The dataset

### Source

**NASA Open Data Portal, *Meteorite Landings*:** https://data.nasa.gov/dataset/meteorite-landings
Compiled from data of The Meteoritical Society. The full source lists 45,716 meteorites, both those seen falling and those found later.

### Scope: witnessed falls only

We selected the **1,107 records with fall status `Fell`**: meteorites whose descent to Earth was observed and recorded. These form a consistent set of observations with a known event behind each one, which makes them well suited to modelling falls as events. The oldest fall in the set is **Nogata, Japan (year 861)**, widely cited as the oldest witnessed fall whose stone still exists.

### Attributes

| Column | Meaning | Type | Unit / values |
|---|---|---|---|
| `id` | Original NASA record identifier | string | e.g. `1` |
| `name` | Meteorite name, usually the place it fell | string | e.g. `Aachen` |
| `recclass` | Meteorite classification | string | e.g. `L5`, `H5`, `Eucrite` (126 distinct) |
| `mass (g)` | Recovered mass | decimal | grams |
| `fall` | Fall status | string | `Fell` for every record in this subset |
| `year` | Year the fall was recorded | integer | e.g. `1880` |
| `reclat` | Recorded latitude | decimal | decimal degrees |
| `reclong` | Recorded longitude | decimal | decimal degrees |

The full data dictionary is in [`data/prepared/Meteorite_Data_Dictionary.csv`](data/prepared/Meteorite_Data_Dictionary.csv).

### What do codes like `L5` mean?

Most witnessed falls are **ordinary chondrites**. Their code is a chemical group plus a petrologic type:

- **Letter = iron content:** `H` = high iron, `L` = low iron, `LL` = low iron, low metal
- **Number = how strongly the rock was heated** in its parent asteroid: `3` barely, `6` strongly

So **`L5`** is a low-iron ordinary chondrite, moderately heated. Other codes include achondrites such as `Eucrite` and `Diogenite`, and labels such as `Achondrite-ung` (ungrouped) and `Stone-uncl` (unclassified).

| Most common classes | Meteorites |
|---|---|
| L6 | 262 |
| H5 | 163 |
| H6 | 91 |
| L5 | 76 |
| H4 | 50 |
| LL6 | 42 |

---

## 🧹 Data preparation

A literal conversion would have turned errors in the source into false statements in the knowledge graph. Data preparation was done in a Python pipeline (Google Colab), and **every change is recorded** in [`Meteorite_Correction_Log.csv`](data/prepared/Meteorite_Correction_Log.csv) so that each corrected value can be traced back to the original.

| Issue | Example | Treatment |
|---|---|---|
| Placeholder coordinates | `0.000000` used for "unknown" (e.g. Mason Gully) | Not treated as a real location at 0°, 0° |
| Formatting artefacts in years | Nogata recorded as `0860` | Corrected to `861`, logged |
| Missing mass | 32 records | Left out: no value invented |
| Missing coordinates | 11 records | Left out: no value invented |

**Principle:** values that are genuinely unknown are *absent* from the knowledge graph, never filled with placeholders. Absence is honest; a fake value is a false fact.

---

## 🧠 Semantic data model (ontology)

**Meteorite Witnessed Falls Ontology** · version 0.2 · [`ontology/Meteorite_Ontology.owl`](ontology/Meteorite_Ontology.owl)
**IRI:** https://w3id.org/FAIR-course-UT/2026/group01/ont · **Namespace:** `https://w3id.org/FAIR-course-UT/2026/group01/ont#` (prefix `ont:`)

Built in Protégé, imports SKOS, and checked with the **HermiT reasoner: consistent**.

```
                      ont:hasFallEvent
   ┌──────────────────┐ ───────────────────────► ┌──────────────────────────────┐
   │  ont:Meteorite   │                          │  ont:FallEvent               │
   │──────────────────│                          │──────────────────────────────│
   │ meteoriteName    │                          │ recordedFallStatus  : string │
   │ sourceIdentifier │                          │ recordedFallYear    : integer│
   │ hasMassGrams     │                          │ recordedLatitude    : decimal│
   └──────────────────┘                          │ recordedLongitude   : decimal│
            │                                    └──────────────────────────────┘
            │ ont:hasClassification
            ▼
   ┌────────────────────────────┐  rdfs:subClassOf  ┌──────────────┐
   │ ont:MeteoriteClassification│ ────────────────► │ skos:Concept │
   │  skos:prefLabel            │                   └──────────────┘
   └────────────────────────────┘
   (disjoint with ont:Meteorite)
```

### Classes

| Class | Definition |
|---|---|
| `ont:Meteorite` | A meteorite represented by a record in the Meteorite Landings dataset |
| `ont:FallEvent` | A recorded witnessed fall, described by its year and landing coordinates |
| `ont:MeteoriteClassification` | A classification category from the `recclass` column; subclass of `skos:Concept` |

### Properties

| Property | Kind | Domain → Range |
|---|---|---|
| `ont:hasFallEvent` | object | Meteorite → FallEvent |
| `ont:hasClassification` | object | Meteorite → MeteoriteClassification |
| `ont:meteoriteName` | data | Meteorite → `xsd:string` |
| `ont:sourceIdentifier` | data | Meteorite → `xsd:string` |
| `ont:hasMassGrams` | data | Meteorite → `xsd:decimal` (grams) |
| `ont:recordedFallStatus` | data | FallEvent → `xsd:string` |
| `ont:recordedFallYear` | data | FallEvent → `xsd:integer` |
| `ont:recordedLatitude` | data | FallEvent → `xsd:decimal` (decimal degrees) |
| `ont:recordedLongitude` | data | FallEvent → `xsd:decimal` (decimal degrees) |

### Design decisions

1. **The rock and its fall are separate things.** A meteorite is an object; its fall is an event with a time and a place. Year and coordinates belong to `ont:FallEvent`, so the event can be enriched later (witness reports, time of day) without touching the meteorite.
2. **Classifications are concepts, not text.** Each `recclass` value becomes one shared `skos:Concept` (e.g. `data:classification_L5`) that all matching meteorites point to: no duplicated strings, no typos, and ready for a hierarchy (`L5 → L chondrite → ordinary chondrite`) via `skos:broader`.
3. **Disjointness.** `ont:Meteorite` and `ont:MeteoriteClassification` are disjoint, so a reasoner catches any modelling mistake that confuses the two.
4. **Recorded, not verified.** Properties are named `recorded…` because they hold what the NASA catalogue *reports*. The mass property carries its unit in its name (`hasMassGrams`), so it cannot be misread.
5. **Deliberately small.** Three classes, two object properties, seven data properties: exactly what the data contains, no speculative modelling.

---

## 🕸️ The RDF knowledge graph

[`rdf/Meteorite_Falls_RDF.ttl`](rdf/Meteorite_Falls_RDF.ttl) · Turtle · instance namespace `https://w3id.org/FAIR-course-UT/2026/group01/data#` (prefix `data:`)

| Resource | Count | IRI pattern |
|---|---|---|
| Meteorites | 1,107 | `data:meteorite_{id}` |
| Fall events | 1,107 | `data:fall_{id}` |
| Classification concepts | 126 | `data:classification_{recclass}` |
| **Triples** | **13,608** | |

IRIs are minted from the stable NASA record identifier, so they stay the same every time the graph is regenerated.

### One record, before and after

**CSV row:**
```
1, Aachen, L5, 21, Fell, 1880, 50.775, 6.08333
```

**RDF:**
```turtle
@prefix data: <https://w3id.org/FAIR-course-UT/2026/group01/data#> .
@prefix ont:  <https://w3id.org/FAIR-course-UT/2026/group01/ont#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

data:meteorite_1 a ont:Meteorite ;
    rdfs:label "Aachen" ;
    ont:meteoriteName "Aachen"^^xsd:string ;
    ont:sourceIdentifier "1"^^xsd:string ;
    ont:hasMassGrams 21.0 ;
    ont:hasClassification data:classification_L5 ;
    ont:hasFallEvent data:fall_1 .

data:fall_1 a ont:FallEvent ;
    ont:recordedFallStatus "Fell"^^xsd:string ;
    ont:recordedFallYear 1880 ;
    ont:recordedLatitude 50.775 ;
    ont:recordedLongitude 6.08333 .

data:classification_L5 a ont:MeteoriteClassification, skos:Concept ;
    skos:prefLabel "L5"@en .
```

Every term in the second version links to a definition on the web. That is the whole point.

---

## 🏷️ Metadata

The dataset and its two downloads are described with established, registered vocabularies, so any FAIR-aware tool can understand the description without knowing anything about this project.

### Vocabularies used

| Prefix | Vocabulary | Used for |
|---|---|---|
| `ont:` | Meteorite Ontology (ours) | Domain terms |
| `skos:` | SKOS | Classifications as concepts |
| `rdfs:` | RDF Schema | Labels and definitions |
| `xsd:` | XML Schema Datatypes | Value types |
| `owl:` | OWL | The ontology itself |
| `dcat:` | DCAT | Dataset, distributions, download URLs, keywords, landing page |
| `dct:` | Dublin Core Terms | Title, description, creator, publisher, licence, access rights, source |
| `prov:` | PROV-O | Derivation from NASA, attribution, generation time |
| `foaf:` | FOAF | People and organisations |
| `schema:` | schema.org | Measured variables; landing-page JSON-LD for search engines |
| `sh:` | SHACL | Validation shapes |

### What the dataset record contains

- **Identification:** title, description, keywords, its own W3ID (`dct:identifier`)
- **Responsibility:** creators, publisher (University of Twente), issue date
- **Access and reuse:** access rights (EU vocabulary: `PUBLIC`), licence (CC BY 4.0), a rights note on the source
- **Provenance and links:** `prov:wasDerivedFrom` and `dct:source` → NASA dataset; `dct:conformsTo` → our ontology; `prov:wasAttributedTo`, `prov:generatedAtTime`
- **Content:** observation type (witnessed falls), eight measured variables
- **Distributions:** RDF/Turtle and CSV, each with access URL, download URL (both W3IDs), IANA media type, format and byte size

### Landing page

[`docs/index.html`](docs/index.html), served by GitHub Pages at https://rishiraghavendran.github.io/FAIR-Meteorites-group1/, embeds the same description as **schema.org `Dataset` JSON-LD**, so search engines such as Google Dataset Search can index it.

---

## ✅ Validation

Both the data and the metadata are checked against **SHACL** shapes.

| What | Shapes | Tool | Result |
|---|---|---|---|
| Knowledge graph | MeteoriteShape, FallEventShape, ClassificationShape | SHACL Playground | Conforms |
| Dataset metadata | `ont:DatasetShape` (targets `dcat:Dataset`) | Contour | Conforms |
| Distribution metadata | `ont:DistributionShape` (targets `dcat:Distribution`) | Contour | Conforms |

`ont:DistributionShape` includes a pattern constraint that download URLs must start with `https://w3id.org/FAIR-course-UT/2026/group01/`, so a raw storage link can never be published by mistake.

The ontology itself was checked for logical consistency with the **HermiT** reasoner.

> **OWL vs SHACL in one line:** OWL says what things *mean*; SHACL checks that the data has the right *shape*.

---

## 📊 FAIR assessment

Assessed with **[F-UJI](https://www.f-uji.net)** (software v4.0.1, metric v0.8) on the dataset identifier.

| Principle | Before | After |
|---|---|---|
| Findable | 3 / 7 | **7 / 7** |
| Accessible | 6 / 7 | **7 / 7** |
| Interoperable | 4 / 6 | **6 / 6** |
| Reusable | 5 / 6 | **6 / 6** |
| **Total** | **18 / 26 (69%)** | **26 / 26 (100%)** |

What closed the gap: an identifier, publisher and keywords in the metadata; explicit access rights; file types and sizes for each distribution; qualified links to the source and the ontology; a machine-readable licence; and a landing page with embedded schema.org metadata.

### How each FAIR principle is addressed

| Principle | How |
|---|---|
| **F1** unique, persistent identifiers | W3ID for every resource; IRIs for every meteorite, event and classification |
| **F2** rich metadata | DCAT + Dublin Core record with all core elements |
| **F3** metadata names the data | `dcat:distribution` → `dcat:downloadURL` |
| **F4** searchable | schema.org JSON-LD on the landing page |
| **A1** standard protocol | HTTPS; content negotiation for people and machines |
| **A2** metadata outlives data | Metadata has its own identifier and file, separate from the data |
| **I1** formal language | RDF, OWL |
| **I2** FAIR vocabularies | SKOS, DCAT, Dublin Core, PROV-O, FOAF, schema.org |
| **I3** qualified references | Typed links between entities; links to NASA source and ontology |
| **R1** rich attributes | Defined properties with datatypes and units; data dictionary |
| **R1.1** clear licence | CC BY 4.0 as a machine-readable URI |
| **R1.2** provenance | PROV-O, original NASA IDs kept, correction log |
| **R1.3** community standards | W3C standards, validated with SHACL |

> A 100% score means the **metadata is complete**. It does not mean the data is correct; that is what the correction log and SHACL validation are for. See [Known limitations](#-known-limitations).

---

## 🔍 Querying the data

### Python (rdflib)

```python
from rdflib import Graph

g = Graph()
g.parse("https://w3id.org/FAIR-course-UT/2026/group01/data/rdf", format="turtle")
print(len(g))  # 13608
```

### SPARQL examples

**Witnessed falls in the 20th century**

```sparql
PREFIX ont: <https://w3id.org/FAIR-course-UT/2026/group01/ont#>

SELECT (COUNT(?m) AS ?falls) WHERE {
  ?m ont:hasFallEvent ?e .
  ?e ont:recordedFallYear ?year .
  FILTER (?year >= 1900 && ?year <= 1999)
}
# → 635
```

**Number of meteorites per classification**

```sparql
PREFIX ont:  <https://w3id.org/FAIR-course-UT/2026/group01/ont#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT ?class (COUNT(?m) AS ?n) WHERE {
  ?m ont:hasClassification ?c .
  ?c skos:prefLabel ?class .
}
GROUP BY ?class
ORDER BY DESC(?n)
```

**The ten heaviest witnessed falls**

```sparql
PREFIX ont: <https://w3id.org/FAIR-course-UT/2026/group01/ont#>

SELECT ?name ?mass ?year WHERE {
  ?m ont:meteoriteName ?name ;
     ont:hasMassGrams ?mass ;
     ont:hasFallEvent ?e .
  ?e ont:recordedFallYear ?year .
}
ORDER BY DESC(?mass)
LIMIT 10
```

**Records with missing information**

```sparql
PREFIX ont: <https://w3id.org/FAIR-course-UT/2026/group01/ont#>

SELECT ?name WHERE {
  ?m a ont:Meteorite ;
     ont:meteoriteName ?name .
  FILTER NOT EXISTS { ?m ont:hasMassGrams ?mass }
}
```

---

## 🔁 Reproducing the work

| Step | Tool | Input → Output |
|---|---|---|
| 1. Select and clean | Python (Google Colab) | `data/original/Meteorite_Landings.csv` → `data/prepared/*.csv` |
| 2. Model | Protégé + HermiT | → `ontology/Meteorite_Ontology.owl` |
| 3. Triplify | Python + RDFLib | prepared CSV → `rdf/Meteorite_Falls_RDF.ttl` |
| 4. Validate data | SHACL Playground | RDF + data shapes → conforms |
| 5. Describe | Contour (SHACL editor) | shapes → `metadata/records/*.ttl` |
| 6. Publish | GitHub, GitHub Pages | files, landing page |
| 7. Identify | W3ID + pid-relay | `.htaccess` + `tests.yaml` (pull request) |
| 8. Assess | F-UJI | dataset identifier → FAIR score |

The pipeline is scripted, so the whole knowledge graph regenerates in one run. When the course namespace changed from `2025-2026/group1` to `2026/group01`, every IRI was regenerated by changing two namespace constants, with identical validation results before and after.

---

## ⚠️ Known limitations

We prefer to be honest about these rather than hide them behind a score.

- **Long-term preservation (A2).** GitHub is not an archive; there is no formal retention guarantee. → *Planned:* deposit in Zenodo for a DOI.
- **Record-level external links (I3).** Individual meteorites are not yet linked to external sources such as the Meteoritical Bulletin Database.
- **Domain vocabulary (R1.3).** Classification concepts follow the Meteoritical Society's naming but are not yet linked to a published classification scheme, and have no `skos:broader` hierarchy yet.
- **Recorded values.** Values reflect the NASA catalogue after documented corrections; they are not independently verified.
- **Geography.** Coordinates use project properties rather than WGS84; aligning them is a possible improvement.

---

## ⚖️ Licence and rights

Our contributions (the ontology, the RDF transformation, the cleaning and corrections, the metadata and the landing page) are released under the **[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)** licence. You may share and adapt them for any purpose, including commercially, as long as you give appropriate credit.

**The licence does not cover the underlying source records.** The NASA catalogue specifies no licence for the original data, and its reuse conditions remain those set by the original provider. This distinction is also recorded in the metadata (`dct:rights`).

---

## 📝 How to cite

> Shah, K. R., & Raghavendran, R. (2026). *NASA Meteorite Landings: Witnessed Falls (FAIRified)* [Data set]. University of Twente. https://w3id.org/FAIR-course-UT/2026/group01/meta/dataset/nasa-meteorite-landings-witnessed-falls-fairified

Please also credit the original source:

> NASA Open Data Portal. *Meteorite Landings*. Data from The Meteoritical Society. https://data.nasa.gov/dataset/meteorite-landings

```bibtex
@misc{shah_raghavendran_2026_meteorites,
  author    = {Shah, Kavisha Rajesh and Raghavendran, Rishi},
  title     = {{NASA Meteorite Landings: Witnessed Falls (FAIRified)}},
  year      = {2026},
  publisher = {University of Twente},
  howpublished = {\url{https://w3id.org/FAIR-course-UT/2026/group01/meta/dataset/nasa-meteorite-landings-witnessed-falls-fairified}},
  note      = {Data set, CC BY 4.0}
}
```

---

## 👥 Authors

| Name | Role |
|---|---|
| **Kavisha Rajesh Shah** | Data preparation, semantic modelling, triplification |
| **Rishi Raghavendran** | Metadata, persistent identifiers, publication, FAIR assessment |

MSc Computer Science (Data Science), University of Twente · FAIR Data Engineering, 2026 · Group 01

---

## 🙏 Acknowledgements

- **NASA Open Data Portal** and **The Meteoritical Society** for the source data
- The **FAIR Data Engineering** teaching team at the University of Twente, and the `pid-relay` service
- **[W3ID](https://w3id.org)** for the persistent identifier infrastructure
- **[F-UJI](https://www.f-uji.net)** (FAIRsFAIR) for the automated FAIR assessment
- **[Protégé](https://protege.stanford.edu)**, **[RDFLib](https://rdflib.readthedocs.io)** and **Contour** for the tooling

---

<sub>Questions or corrections? Open an issue in this repository.</sub>
