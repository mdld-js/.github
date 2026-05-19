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

- **Walking** {+#walking ?my:hasActivity .my:Activity label}
- **Reading** {+#reading ?my:hasActivity .my:Activity label}

```

Generates valid RDF quads - one per each annotation term.

## Core Features

- **Prefix folding**: Build hierarchical namespaces with lightweight IRI authoring
- **Subject declarations**: `{=IRI}` and `{=#fragment}` for context setting
- **Object IRIs**: `{+IRI}` and `{+#fragment}` for temporary object declarations  
- **Four predicate forms**: `p` (S→L), `?p` (S→O), `!p` (O→S)
- **Type declarations**: `.Class` for rdf:type triples
- **Datatypes & language**: `^^xsd:date` and `@en` support
- **Fragments**: Built-in document structuring with `{=#fragment}`
- **Round-trip serialization**: Markdown ↔ RDF ↔ Markdown preserves structure

