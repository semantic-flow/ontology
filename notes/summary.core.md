---
id: 0vwvn9y166clpweuy52ob10
title: Core Ontology Summary
desc: ''
updated: 1773881981246
created: 1773756686863
---

## Purpose

This note is a compact mental model for the current `semantic-flow-core-ontology.ttl`.

Use it to stay aligned with the live core vocabulary and to avoid reintroducing older model layers that have already been removed.

## Core Mental Model

- A `SemanticMesh` is a namespace region together with its supporting resources, typically overlaid onto a file tree or filesystem region.
- A public Semantic Flow identifier IRI like `D/` denotes the referent.
- A `Knop` is the mesh-managed handle-container associated one-to-one with a Semantic Flow identifier. It has a `designatorPath`, may optionally `designates` its referent explicitly, and may also host a payload artifact.
  - `D/_knop/` denotes the mesh-managed `Knop` associated with that designator.
- Not every file in a host hierarchy is part of the mesh. Only explicitly integrated mesh resources are.

## Designators And IRIs

Current terminology in these notes:

- `IRI` means any IRI.
- A `designator` is the mesh-relative designating form, typically path-like.
- A `Semantic Flow identifier` is a special kind of IRI: an IRI formed in mesh context from a mesh base plus a designator, and associated one-to-one with a `Knop`.
- In the current core direction, the Knop carries the explicit `designatorPath`; there is no separate generic identifier-handle class.

Examples:

- designator: `alice/bio/`
- Semantic Flow identifier IRI: `https://example.org/alice/bio/`
- corresponding Knop IRI: `https://example.org/alice/bio/_knop/`

Use `designator` when you mean the softer mesh-relative form.
Use `Semantic Flow identifier` when you mean the full IRI that participates in the one-Knop-per-identifier rule.

## Artifact Facet Model

`DigitalArtifact` is the umbrella class.

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

Use `narrowerFacet` / `broaderFacet` for artifact-facet structure.

## Mesh Structure

- `SemanticMesh` has `Knop`s and mesh-level support resources.
- `Knop` has support resources and may have one primary payload resource.
- The current slot vocabulary uses explicit properties such as `hasKnop`, `hasPayloadArtifact`, `hasKnopMetadata`, and `hasKnopInventory` rather than a generic `containsSemanticFlowResource`.

Current path conventions:

- `_mesh/` denotes the mesh surface
- `D/_knop/` denotes the Knop for designator `D/`
- historical material lives under `D/_knop/_history/`

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
- `ResourcePage` is a `LocatedFile` subclass for the human-facing HTML resource pages that should accompany every `SemanticFlowResource`
- `designatorPath` is the mesh-relative path-like designator carried by a `Knop`.
- `designates` is an optional explicit link from a `Knop` to the referent of its associated Semantic Flow identifier.
- `preferredPayloadFileSlug` is the mutable filename preference.

## Things To Not Reintroduce

These are not part of the current core surface:

- `Nomen`
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
