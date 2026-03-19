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
- A `Nomen` is the thin mesh-relative naming resource for a Semantic Flow identifier.
  - `D/_nomen` denotes the `Nomen` associated with that identifier in hierarchy-backed serializations.
- A `Knop` is the mesh-managed support object linked to that same identifier through its `Nomen`.
  - `D/_knop` denotes the mesh-managed `Knop` associated with that identifier.
- Not every file in a host hierarchy is part of the mesh. Only explicitly integrated mesh resources are.

## Nomen And IRIs

Current terminology in these notes:

- `IRI` means any IRI.
- A `Nomen` is the thin mesh-relative naming resource for a Semantic Flow identifier.
- `designatorPath` is the mesh-relative path-like naming value carried by a `Nomen`.
- A `Semantic Flow identifier` is a special kind of IRI: an IRI formed in mesh context from a mesh base plus a `Nomen`'s `designatorPath`, and associated one-to-one with both a `Nomen` and a `Knop`.
- In prose, `designator` is just shorthand for a `Nomen`'s `designatorPath`, not a separate ontology class or first-class core node.

Examples:

- Nomen designatorPath: `alice/bio`
- Semantic Flow identifier/IRI: `https://example.org/alice/bio`
- corresponding Nomen IRI: `https://example.org/alice/bio/_nomen`
- corresponding Knop IRI: `https://example.org/alice/bio/_knop`

Use `designatorPath` when you mean the modeled mesh-relative naming value on a `Nomen`.
Use `designator` only as informal prose shorthand for that value.
Use `Semantic Flow identifier` when you mean the full IRI minted from a mesh base plus a `Nomen`'s `designatorPath` and paired one-to-one with a `Nomen` and a `Knop`.

### Canonical URLs On Static Hosts

- Prefer slashless canonical Semantic Flow identifiers for non-file referents.
- In hierarchy-backed serializations, those identifiers may still be served from folders with `index.html` resource pages.
- On static hosts such as GitHub Pages, a request for the slashless form may be redirected to a trailing-slash page URL. Treat that trailing-slash URL as a delivery artifact, not as the canonical identifier.
- Resource pages should emit a canonical link for the slashless identifier and may use `history.replaceState(...)` after load so users copying from the browser location bar get the canonical slashless form rather than the redirected trailing-slash form.

## Artifact Facet Model

`DigitalArtifact` is the conceptual umbrella class.

`DigitalArtifactFacet` is the instantiated facet-side superclass in the current model.

The main facet chain is:

`AbstractArtifact -> HistoricalState -> ArtifactManifestation -> LocatedFile`

Interpretation:

- `AbstractArtifact`: the whole-artifact, over-time identity
- `HistoricalState`: an immutable version representing the content of the artifact at a particular point
- `ArtifactManifestation`: a concrete variant of the artifact or state whose bytes may be provided by one or more `LocatedFile`s
- `LocatedFile`: retrievable bytes at some location

Sparse cases are explicitly supported:

- an `AbstractArtifact` may have a `hasWorkingLocatedFile` without materializing a working state
- an `AbstractArtifact` may link directly to an `ArtifactManifestation` when no `HistoricalState` is materialized

Use `narrowerFacet` / `broaderFacet` for artifact-facet structure, and `hasAbstractFacet` when you want a direct discoverability link back to the governing `AbstractArtifact`.

## Mesh Structure

- `SemanticMesh` has `Knop`s and mesh-level support resources.
- `Knop` has exactly one associated `Nomen`.
- `Knop` has support resources and may have one primary payload resource.
- The current slot vocabulary uses explicit properties such as `hasKnop`, `hasPayloadArtifact`, `hasKnopMetadata`, and `hasKnopInventory` rather than a generic `containsSemanticFlowResource`.

Current path conventions:

- `_mesh` denotes the mesh surface
- `D/_nomen` denotes the Nomen associated with identifier `D`
- `D/_knop` denotes the Knop associated with identifier `D`
- historical material lives under `D/_knop/_history`

## Role Classes

These are "Mesh role" classes over `DigitalArtifact` facets, not separate artifact species:

- `PayloadArtifact`
- `KnopMetadata`
- `KnopInventory`
- `MeshMetadata`
- `MeshInventory`

Important consequence:

- a role may be carried by an `AbstractArtifact`, an `ArtifactManifestation`, or a `LocatedFile`
- support artifacts can have artifact structure without needing their own `Knop`

## Historical And Working Model

- `HistoricalState` is the only explicit state class in the current core.
- `latestHistoricalState` is a convenience pointer from `AbstractArtifact`.
- `hasWorkingLocatedFile` is the sparse working-surface hook.
- `locatedFileForState` is an optional shortcut that should agree with `hasManifestation / hasLocatedFile`.

## Other Important Vocabulary

- `RdfDocument` is a content-kind class that can apply at multiple artifact-facet levels.
- `DigitalArtifactFacet` is the common superclass for `AbstractArtifact`, `HistoricalState`, `ArtifactManifestation`, and `LocatedFile`.
- `ResourcePage` is a `LocatedFile` subclass for the human-facing HTML resource pages that should accompany every `SemanticFlowResource`
- `Nomen` is the thin mesh-relative naming class associated with a Semantic Flow identifier.
- `hasNomen` links a `Knop` to its `Nomen`.
- `designatorPath` is the mesh-relative path-like naming value carried by a `Nomen`.
- `designates` is an optional explicit link from a `Nomen` to the referent of its associated Semantic Flow identifier.
- `preferredPayloadFileSlug` is the mutable filename preference.

## Things To Not Reintroduce

These are not part of the current core surface:

- the old heavier `Nomen` model that duplicated Knop responsibilities
- `ArtifactFlow`
- `WorkingState`
- `CurrentState`
- `ArtifactState`
- `AbstractFile`
- `FileExpression`
- `payloadSlug`

Also do not assume:

- every `Knop` has a payload
- every integrated artifact has a fully materialized artifact complex
- every mesh is backed by a literal filesystem tree

## Example

For a concrete sketch of the current model, see [use-cases.alice-bio.md](./use-cases.alice-bio.md).
