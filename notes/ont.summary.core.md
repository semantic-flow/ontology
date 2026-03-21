---
id: 0vwvn9y166clpweuy52ob10
title: Core Ontology Summary
desc: ''
updated: 1773890897593
created: 1773756686863
---

## Purpose

This note is a compact mental model for the current `semantic-flow-core-ontology.ttl`.

Use it to stay aligned with the live core vocabulary and to avoid reintroducing older model layers that have already been removed.

## Core Mental Model

- A `SemanticMesh` is a namespace region together with its supporting resources, typically overlaid onto a file tree or filesystem region.
- A public Semantic Flow identifier IRI like `D` denotes the referent.
- A `Knop` is the mesh-managed support object and naming anchor for a Semantic Flow identifier.
  - `D/_knop` denotes the mesh-managed `Knop` associated with that identifier.
- A `Knop` carries a mesh-relative `designatorPath` from which the public identifier IRI is formed in mesh context.
- Not every file in a host hierarchy is part of the mesh. Only explicitly integrated mesh resources are.

## Designator Paths And IRIs

Current terminology in these notes:

- `IRI` means any IRI.
- `designatorPath` is the mesh-relative path-like naming value carried by a `Knop`.
- A `Semantic Flow identifier` is a special kind of IRI: an IRI formed in mesh context from a mesh base plus a `Knop`'s `designatorPath`.
- In prose, `designator` is just shorthand for a `Knop`'s `designatorPath`, not a separate ontology class or first-class core node.

Examples:

- Knop designatorPath: `alice/bio`
- Semantic Flow identifier/IRI: `https://example.org/alice/bio`
- corresponding Knop IRI: `https://example.org/alice/bio/_knop`

Use `designatorPath` when you mean the modeled mesh-relative naming value on a `Knop`.
Use `designator` only as informal prose shorthand for that value.
Use `Semantic Flow identifier` when you mean the full IRI formed from a mesh base plus a `Knop`'s `designatorPath`.

### Canonical URLs On Static Hosts

- Prefer slashless canonical Semantic Flow identifiers for non-file referents.
- In hierarchy-backed serializations, those identifiers may still be served from folders with `index.html` resource pages.
- On static hosts such as GitHub Pages, a request for the slashless form may be redirected to a trailing-slash page URL. Treat that trailing-slash URL as a delivery artifact, not as the canonical identifier.
- Resource pages should emit a canonical link for the slashless identifier and may use `history.replaceState(...)` after load so users copying from the browser location bar get the canonical slashless form rather than the redirected trailing-slash form.

## Artifact Facet Model

`DigitalArtifact` is the governing artifact-level class in the current model.

`DigitalArtifactFacet` is the facet-side superclass for states, manifestations, and retrievable files.

The main facet chain is:

`DigitalArtifact -> HistoricalState -> ArtifactManifestation -> LocatedFile`

Interpretation:

- `DigitalArtifact`: the governing over-time artifact-level resource
- `HistoricalState`: an immutable version representing the content of the artifact at a particular point
- `ArtifactManifestation`: a concrete variant of the artifact or state whose bytes may be provided by one or more `LocatedFile`s
- `LocatedFile`: retrievable bytes at some location

Sparse cases are explicitly supported:

- a `DigitalArtifact` may have a `hasWorkingLocatedFile` without materializing a working state
- a `DigitalArtifact` may link directly to an `ArtifactManifestation` when no `HistoricalState` is materialized

Use the explicit structural relations `hasHistoricalState`, `hasManifestation`, `hasLocatedFile`, and `hasWorkingLocatedFile` for artifact/facet structure.

## Mesh Structure

- `SemanticMesh` has `Knop`s and mesh-level support resources.
- `Knop` has exactly one `designatorPath`.
- `Knop` has support resources and may have one primary payload resource.
- The current slot vocabulary uses explicit properties such as `hasKnop`, `hasPayloadArtifact`, `hasKnopMetadata`, and `hasKnopInventory` rather than a generic `containsSemanticFlowResource`.

Current path conventions:

- `_mesh` denotes the mesh surface
- `D/_knop` denotes the Knop associated with identifier `D`
- historical material lives under `D/_knop/_history`

## Role Classes

These are artifact-level mesh role classes, not members of the facet lattice:

- `PayloadArtifact`
- `KnopMetadata`
- `KnopInventory`
- `MeshMetadata`
- `MeshInventory`

Important consequence:

- a role classifies a `DigitalArtifact`
- a support artifact can still have its own historical states, manifestations, and located files because those hang off the `DigitalArtifact`, not off the role class and not off a separate `Knop`

## Historical And Working Model

- `HistoricalState` is the only explicit state class in the current core.
- `latestHistoricalState` is a convenience pointer from `DigitalArtifact`.
- `hasWorkingLocatedFile` is the sparse working-surface hook.
- `locatedFileForState` is an optional shortcut that should agree with `hasManifestation / hasLocatedFile`.

## Other Important Vocabulary

- `RdfDocument` is an orthogonal content-kind classification that may be applied selectively to a `DigitalArtifact` or to a specific `DigitalArtifactFacet`.
- `DigitalArtifactFacet` is the common superclass for `HistoricalState`, `ArtifactManifestation`, and `LocatedFile`.
- `ResourcePage` is a `LocatedFile` subclass for the human-facing HTML resource pages that should accompany every `SemanticFlowResource`
- `designatorPath` is the mesh-relative path-like naming value carried by a `Knop`.
- `preferredPayloadFileSlug` is the mutable filename preference.

## Things To Not Reintroduce

These are not part of the current core surface:

- a separate naming-handle layer distinct from `Knop`
- `ArtifactFlow`
- `WorkingState`
- `CurrentState`
- `ArtifactState`
- `AbstractArtifact`
- `AbstractFile`
- `FileExpression`
- `payloadSlug`

Also do not assume:

- every `Knop` has a payload
- every integrated artifact has a fully materialized artifact complex
- every mesh is backed by a literal filesystem tree

## Example

For a concrete sketch of the current model, see [use-cases.alice-bio.md](./use-cases.alice-bio.md).
