# MD-LD

**Markdown-Linked Data (MD-LD)** — a deterministic, streaming-friendly RDF authoring format that extends Markdown with explicit `{...}` annotations.

[![NPM](https://img.shields.io/npm/v/mdld-parse)](https://www.npmjs.com/package/mdld-parse)

[Demo](https://mdld.js.org) | [Repository](https://github.com/davay42/mdld-parse) 

## What is MD-LD?

MD-LD allows you to author RDF graphs directly in Markdown using explicit `{...}` annotations:

```markdown
[my] <tag:alice@example.com,2026:>

# 2024-07-18 {=my:journal-2024-07-18 .my:Event my:date ^^xsd:date}

## A good day {label}

Mood: [Happy] {my:mood}
Energy level: [8] {my:energyLevel ^^xsd:integer}

Met [Sam] {+my:sam .my:Person ?my:attendee} on my regular walk at [Central Park] {+my:central-park ?my:location .my:Place label @en} and talked about [Sunny] {my:weather} weather. 

Activities:

- **Walking** {+ex:walking ?my:hasActivity .my:Activity label}
- **Reading** {+ex:reading ?my:hasActivity .my:Activity label}

```

Generates valid RDF triples:

```turtle
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>.
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>.
@prefix xsd: <http://www.w3.org/2001/XMLSchema#>.
@prefix sh: <http://www.w3.org/ns/shacl#>.
@prefix prov: <http://www.w3.org/ns/prov#>.
@prefix ex: <http://example.org/>.
@prefix my: <tag:alice@example.com,2026:>.

my:journal-2024-07-18 a my:Event;
    my:date "2024-07-18"^^xsd:date;
    rdfs:label "A good day";
    my:mood "Happy";
    my:energyLevel 8;
    my:attendee my:sam;
    my:location my:central-park;
    my:weather "Sunny";
    my:hasActivity <tag:alice@example.com,2026:journal-2024-07-18#walking>, <tag:alice@example.com,2026:journal-2024-07-18#reading>.
my:sam a my:Person.
my:central-park a my:Place;
    rdfs:label "Central Park"@en.
<tag:alice@example.com,2026:journal-2024-07-18#walking> a my:Activity;
    rdfs:label "Walking".
<tag:alice@example.com,2026:journal-2024-07-18#reading> a my:Activity;
    rdfs:label "Reading".

```

## Core Features

- **Prefix folding**: Build hierarchical namespaces with lightweight IRI authoring
- **Subject declarations**: `{=IRI}` and `{=#fragment}` for context setting
- **Object IRIs**: `{+IRI}` and `{+#fragment}` for temporary object declarations  
- **Four predicate forms**: `p` (S→L), `?p` (S→O), `!p` (O→S)
- **Type declarations**: `.Class` for rdf:type triples
- **Datatypes & language**: `^^xsd:date` and `@en` support
- **Fragments**: Built-in document structuring with `{=#fragment}`
- **Round-trip serialization**: Markdown ↔ RDF ↔ Markdown preserves structure
