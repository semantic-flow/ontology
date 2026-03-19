---
id: p9g0h6xqn3igqbawtjwr8rm
title: Semantic Flow identifier
desc: ''
updated: 1773890827253
created: 1773890727274
---

- aka: "Semantic Flow IRI"

## Definition

A `Semantic Flow identifier` is a public IRI minted in the context of a `SemanticMesh` from:

- the mesh's `meshBase`
- a `Nomen`'s `designatorPath`

That public IRI denotes the referent.

## Why It Matters

A Semantic Flow identifier is the thing people cite, dereference, and use in RDF statements about the referent.

It is the public-facing identifier in the model, while the paired `Nomen` and `Knop` provide supporting structure around it.

## Formation

In the current core model:

- a `Nomen` carries a mesh-relative `designatorPath`
- a `SemanticMesh` carries a `meshBase`
- the Semantic Flow identifier is formed from `meshBase + designatorPath`

Example:

- `meshBase`: `https://example.org/`
- `Nomen.designatorPath`: `alice/bio`
- Semantic Flow identifier: `https://example.org/alice/bio`

## Related Concepts

The Semantic Flow identifier is not the same thing as its paired support resources:

- `Nomen`
  - the thin mesh-relative naming resource
  - conventionally exposed at `D/_nomen`
  - carries `designatorPath` and optional `designates`
- `Knop`
  - the mesh-managed support object
  - conventionally exposed at `D/_knop`
  - carries payload, metadata, inventory, and other support structure

Each Semantic Flow identifier is associated one-to-one with:

- one `Nomen`
- one `Knop`

## What It Is Not

- It is not just any IRI.
- It is not the same thing as a `Nomen`.
- It is not the same thing as a `Knop`.
- It is not just a raw string path; the modeled path value is `designatorPath`.

In prose, `designator` can be used as shorthand for the mesh-relative naming form, but the ontology-level node is `Nomen` and the modeled value is `designatorPath`.

## Dereferencing

If you put a Semantic Flow identifier into a browser, it should resolve to a useful human-facing resource page.

In hierarchy-backed serializations, non-file referents may still be served from folder-backed `index.html` pages even when the canonical Semantic Flow identifier is slashless.

## See Also

- [[ont.summary.core]]
- [[ont.use-cases.alice-bio]]
- [[principles]]
