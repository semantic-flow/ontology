---
id: 0vwvn9y166clpweuy52ob10
title: Core Ontology Summary
desc: ''
updated: 1773756893178
created: 1773756686863
---

## Purpose

This note is a compact mental model for the current `semantic-flow-core-ontology.ttl`.

Use it to stay aligned with the live core vocabulary and to avoid reintroducing older model layers that have already been removed.

## Core Mental Model

- A `Mesh` is the Semantic Flow surface, not the entire host filesystem or workspace.
- A public designator like `D/` denotes the referent.
- `D/_knop/` denotes the mesh-managed `Knop` associated with that designator.
- A `Knop` is the identifier handle/container. It may be purely referential, or it may also host a payload artifact.
- Not every file in a host hierarchy is part of the mesh. Only explicitly integrated mesh resources are.

## Artifact Facet Model

`DigitalArtifact` is the umbrella class.

The main facet chain is:

`AbstractArtifact -> HistoricalState -> ArtifactManifestation -> LocatedFile`

Interpretation:

- `AbstractArtifact`: the whole-artifact, over-time identity
- `HistoricalState`: an immutable published snapshot
- `ArtifactManifestation`: a concrete variant of the artifact or state
- `LocatedFile`: retrievable bytes at some location

Sparse cases are explicitly supported:

- an `AbstractArtifact` may have a `hasWorkingLocatedFile` without materializing a working state
- an `AbstractArtifact` may link directly to an `ArtifactManifestation` when no `HistoricalState` is materialized

Use `narrowerFacet` / `broaderFacet` for artifact-facet structure.

## Mesh Structure

- `Mesh` contains `Knop`s and mesh-level support resources.
- `Knop` contains support resources and may contain one primary payload resource.
- `containsSemanticFlowResource` is semantic containment/inventory, not a raw filesystem listing.

Current path conventions:

- `_mesh/` denotes the mesh surface
- `D/_knop/` denotes the Knop for designator `D/`
- historical material lives under `D/_knop/_history/`

## Role Classes

These are role classes over `DigitalArtifact` facets, not separate artifact species:

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
- `ResourcePage` is a `LocatedFile` subclass for human-facing HTML resource pages.
- `payloadSlug` is the stable identifier path segment for an `AbstractArtifact`.
- `preferredPayloadFileSlug` is the mutable filename preference.
- `preferredPayloadSlug` is deprecated.

## Things To Not Reintroduce

These are not part of the current core surface:

- `Nomen`
- `ArtifactFlow`
- `WorkingState`
- `CurrentState`
- `ArtifactState`
- `AbstractFile`
- `FileExpression`

Also do not assume:

- every `Knop` has a payload
- every integrated artifact has a fully materialized artifact complex
- every mesh is backed by a literal filesystem tree

## Example

For a concrete sketch of the current model, see [use-cases.alice-bio.md](./use-cases.alice-bio.md).
