---
id: b6t6wu1wdwh7ssite71l3yr
title: Decision Log
desc: ''
updated: 1773896780320
created: 1773896763313
---

## Decisions

Superseded decisions are intentionally retained for traceability. When a decision is reversed or replaced, mark it explicitly rather than deleting it.

### 2026-03-26: Introduce explicit `ArtifactHistory` and remove `ArtifactContainer`

- Status: Active
- Decision: Reintroduce a narrow explicit `ArtifactHistory` resource as the lineage handle for published artifact history, while keeping `ArtifactFlow` out of the active core. A `DigitalArtifact` relates to one or more histories through `hasArtifactHistory`, may identify the active one through functional `currentArtifactHistory`, and may track `nextHistoryOrdinal`. An `ArtifactHistory` owns `hasHistoricalState` and `latestHistoricalState`, may carry `historyOrdinal` and `nextStateOrdinal`, and is typed only as a `SemanticFlowResource`. Remove `ArtifactContainer` from the active core.
- References: [[ont.task.2026.2026-03-26-ArtifactHistory]], [[sf.conv.2026.2026-03-25_1413-title-mesh-alice-bio-codex]]
- Why:
  - a history landing page should correspond to an explicit resource rather than an exceptional page with no paired resource
  - once multiple histories exist, the current/default lineage must be identified explicitly rather than inferred indirectly from lineage-local latest states
  - history allocation metadata belongs on the artifact/history pair, not in current-surface inventory
  - `ArtifactContainer` was too vague and unused to justify keeping it in the active core
- Notes:
  - the default generated naming direction is `_historyNNN` for histories and `_sNNNN` for states
  - history-level metadata remains in `KnopMetadata` for now; do not introduce a dedicated history metadata artifact in this pass
  - this refines the older simplified artifact model by inserting `ArtifactHistory` between `DigitalArtifact` and `HistoricalState`

### 2026-03-25: Distinguish mesh metadata and mesh inventory document roles

- Status: Active
- Decision: In hierarchy-backed serializations, `_mesh/_meta/meta.ttl` carries the mesh identity/description and may point to the mesh inventory with `hasMeshInventory`, while `_mesh/_inventory/inventory.ttl` is the self-contained current-surface map and may repeat both `hasMeshMetadata` and `hasMeshInventory`.
- References: [[wd.task.2026.2026-03-25-mesh-alice-bio]], [[sf.conv.2026.2026-03-25_1413-title-mesh-alice-bio-codex]]
- Why:
  - mesh metadata and mesh inventory serve different document roles and should not be treated as interchangeable
  - inventory benefits from being self-contained because it is the document most directly concerned with the currently present managed surface
  - metadata should stay lighter and less repetitive while still pointing readers and tools toward the inventory document
  - this asymmetry is intended as a reusable serialization convention, not just a one-off detail of the Alice Bio fixture

### 2026-03-21: Adopt a Knop-only naming model

- Decision: Remove `Nomen`, `hasNomen`, `_nomen`, and `designates` from the active core. Each `Knop` carries exactly one `designatorPath`, and a Semantic Flow identifier is the public IRI formed from `meshBase + Knop.designatorPath`.
- Why:
  - this avoids keeping a separate naming-handle layer when the active model only needs one resource-side handle
  - it makes `Knop` the single active naming anchor, support object, and operational handle
  - it keeps rebasing/composability by preserving `designatorPath` as a mesh-relative value rather than a globally unique identifier
- Notes:
  - this supersedes the 2026-03-18 thin-`Nomen` decision
  - explicit referent linkage via `designates` is dropped from the active core for now
  - in hierarchy-backed serializations, `D/_knop` remains the active support-resource convention and no `_nomen` handle is part of the active model

### 2026-03-18: Use `SemanticMesh` as the core mesh concept

- Decision: The core mesh class is `SemanticMesh`, understood as a namespace region together with its supporting resources, conventionally exposed via `_mesh` in hierarchy-backed serializations.
- Why:
  - `Mesh` by itself is too generic in ontology-facing docs.
  - the mesh is the Semantic Flow surface, not the entire surrounding workspace or filesystem tree
  - this wording still allows generated or otherwise non-filesystem-backed meshes

### 2026-03-18: Reintroduce a thin `Nomen` and keep `Knop` as the paired support object

- Status: Superseded on 2026-03-21 by the Knop-only naming model
- Decision: Use a thin `Nomen` as the mesh-relative naming resource, keep `designatorPath` and optional `designates` on `Nomen`, and link each `Knop` to exactly one `Nomen` with `hasNomen`.
- Why:
  - this keeps naming semantics separate from support/container semantics without reviving the older heavier `Nomen` model
  - it preserves composability: changing `meshBase` can mint different public IRIs while reusing the same `Nomen` plus `Knop` structure
  - it keeps `Knop` focused on payload/support hosting and mesh management
- Notes:
  - in hierarchy-backed serializations, `D/_nomen` denotes the `Nomen` and `D/_knop` denotes the `Knop`
  - `designator` is prose shorthand only; the modeled value is `Nomen.designatorPath`

### 2026-03-18: Define Semantic Flow identifiers as slashless canonical IRIs

- Status: Superseded on 2026-03-21 by the Knop-only naming model
- Decision: A Semantic Flow identifier is the public IRI formed from `meshBase + Nomen.designatorPath`, and canonical examples should use slashless non-file identifiers.
- Why:
  - slashless canonical identifiers are easier to use consistently in Turtle, SPARQL, and examples
  - the identifier should stay distinct from the delivery URL of a static-host resource page
- Notes:
  - the formation detail was superseded on 2026-03-21; the active model now uses `meshBase + Knop.designatorPath`
  - hierarchy-backed pages may still be served from folders with `index.html`
  - on static hosts such as GitHub Pages, a trailing-slash URL caused by folder serving is treated as a delivery artifact, not as the canonical identifier
  - generated resource pages should emit canonical links and may use `history.replaceState(...)` to restore the slashless canonical URL in the browser

### 2026-03-18: Use the simplified artifact model centered on `DigitalArtifact`

- Status: Partially superseded on 2026-03-26 by the explicit `ArtifactHistory` model
- Decision: `DigitalArtifact` is the governing artifact-level resource; `HistoricalState`, `ArtifactManifestation`, and `LocatedFile` are facet-side classes; `AbstractArtifact`, `ArtifactFlow`, `ArtifactState`, `WorkingState`, and `CurrentState` are not part of the current core.
- Why:
  - this keeps the core model smaller and easier to resolve in application code
  - it supports sparse cases without requiring every integrated artifact to materialize the full structure
  - it avoids treating artifact facets as ordinary artifact kinds
- Notes:
  - the main explicit structure was originally `DigitalArtifact -> HistoricalState -> ArtifactManifestation -> LocatedFile`
  - the 2026-03-26 ArtifactHistory decision refines that to `DigitalArtifact -> ArtifactHistory -> HistoricalState -> ArtifactManifestation -> LocatedFile`
  - sparse cases may also use `DigitalArtifact -> hasManifestation -> ArtifactManifestation`
  - working bytes are modeled with `hasWorkingLocatedFile`

### 2026-03-18: Prefer explicit structural relations over a generic facet lattice

- Decision: Use explicit structural relations such as `hasHistoricalState`, `hasManifestation`, `hasLocatedFile`, `hasWorkingLocatedFile`, and optional `locatedFileForState`; do not keep `narrowerFacet` / `broaderFacet` in the active core.
- Why:
  - the generic facet lattice was becoming more confusing than helpful
  - the important model structure is specific and directional
  - explicit links are easier to validate, document, and resolve operationally

### 2026-03-18: Treat payload, metadata, and inventory as artifact-level role classes

- Decision: `PayloadArtifact`, `KnopMetadata`, `KnopInventory`, `MeshMetadata`, and `MeshInventory` classify `DigitalArtifact`s directly rather than living in a separate artifact species hierarchy or in the facet lattice.
- Why:
  - this cleanly separates mesh role from artifact structure
  - support artifacts can still have their own states, manifestations, and located files because those hang off the governing `DigitalArtifact`
  - it fits sparse and partially materialized cases better than forcing everything through a dedicated support-artifact complex
- Notes:
  - `preferredPayloadFileSlug` applies to `PayloadArtifact`
  - `hasMeshMetadata`, `hasKnopMetadata`, `hasMeshInventory`, and `hasKnopInventory` canonically point to artifact-level resources

### 2026-03-18: Keep `RdfDocument` as an orthogonal content-kind classification

- Decision: `RdfDocument` is not restricted to only artifact-level resources or only facet-side resources; it may classify either a `DigitalArtifact` or a specific `DigitalArtifactFacet`.
- Why:
  - an artifact may have an RDF version without every facet needing to be RDF
  - a specific manifestation or located file may be RDF even when the governing artifact is more general
  - this avoids reopening the broader unresolved question of what should count as a `DigitalArtifact`

### 2026-03-18: Loose meshes only include explicitly integrated resources

- Decision: A host hierarchy or workspace may contain many files, but only explicitly integrated Semantic Flow resources are part of the mesh.
- Why:
  - this preserves filesystem sparseness
  - it avoids accidentally treating ambient files, caches, build outputs, or unrelated working material as mesh content
  - it allows meshes to be generated or partially overlaid rather than tied one-to-one to a literal directory tree

### 2026-03-18: Nested `_mesh` boundaries are sub-meshes by default

- Decision: If a `_mesh` occurs within another `_mesh` region, treat it as a sub-mesh boundary by default. The parent mesh inventory identifies the sub-mesh itself, not all of the sub-mesh's internal Semantic Flow resources.
- Why:
  - this gives composability a clean default
  - it allows a sub-mesh to remain a reusable unit under a different `meshBase`
  - flattening, rebasing, or re-export of child resources should be explicit rather than implied by nesting
