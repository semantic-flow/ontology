---
id: ls7gy1x4oj43fpgctvsqbp3
title: Alice Bio
desc: ''
updated: 1774529384808
created: 1773730267525
---

## Designator Table

This sketch assumes:

* `alice` and `alice/bio` are the only non-supporting designators.
* `alice` denotes the person Alice.
* `alice/bio` denotes the bio `DigitalArtifact` directly.
* `alice/_knop` and `alice/bio/_knop` denote the corresponding `Knop`s.
* the `Knop`s for `alice` and `alice/bio` carry designatorPath values `alice` and `alice/bio` respectively.
* `_mesh` denotes the mesh surface itself.
* explicit histories, when materialized, are represented as `ArtifactHistory` resources directly under the artifact path using `_historyNNN`.
* historical states are explicit within an `ArtifactHistory`, while a working `LocatedFile` may still hang directly off the `DigitalArtifact` without requiring separate working-state resources.
* the `surface path` column is a mesh-relative serialization sketch, not a claim that every row carries a `designatorPath`; in the current core model, `designatorPath` is carried by `Knop`s and used only to form public Semantic Flow identifiers.

| surface path | referent | classes |
| --- | --- | --- |
| `_mesh` | The mesh surface for this Alice example | `sflo:SemanticMesh` |
| `_mesh/_meta` | Mesh metadata artifact for this mesh | `sflo:MeshMetadata`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `_mesh/_meta/meta.ttl` | Located Turtle file providing bytes for `_mesh/_meta` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `_mesh/_inventory` | Mesh inventory artifact for this mesh | `sflo:MeshInventory`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `_mesh/_inventory/inventory.ttl` | Located Turtle file providing bytes for `_mesh/_inventory` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice` | Alice the person | `schema:Person` |
| `alice/_knop` | The Knop for `alice` | `sflo:Knop` |
| `alice/_knop/_meta` | Knop metadata artifact for `alice` | `sflo:KnopMetadata`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/_knop/_meta/meta.ttl` | Located Turtle file providing bytes for `alice/_knop/_meta` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/_knop/_inventory` | Knop inventory artifact for `alice` | `sflo:KnopInventory`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/_knop/_inventory/inventory.ttl` | Located Turtle file providing bytes for `alice/_knop/_inventory` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/bio` | Alice bio as the primary artifact identified by this designator | `sflo:PayloadArtifact`, `sflo:DigitalArtifact` |
| `alice/bio/_knop` | The Knop for `alice/bio` | `sflo:Knop` |
| `alice/bio/_knop/_meta` | Knop metadata artifact for `alice/bio` | `sflo:KnopMetadata`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/bio/_knop/_meta/meta.ttl` | Located Turtle file providing bytes for `alice/bio/_knop/_meta` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/bio/_knop/_inventory` | Knop inventory artifact for `alice/bio` | `sflo:KnopInventory`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/bio/_knop/_inventory/inventory.ttl` | Located Turtle file providing bytes for `alice/bio/_knop/_inventory` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/bio/_history001` | Current explicit history for the bio artifact | `sflo:ArtifactHistory` |
| `alice/bio/_history001/index.html` | ResourcePage for `alice/bio/_history001` | `sflo:ResourcePage`, `sflo:LocatedFile` |
| `alice/bio/_history001/_s0001` | First published historical state in the current bio history | `sflo:HistoricalState` |
| `alice/bio/_history001/_s0001/bio-md` | Markdown artifact manifestation for historical state `_s0001` | `sflo:ArtifactManifestation` |
| `alice/bio/_history001/_s0001/bio-md/bio.md` | Located markdown file providing bytes for `alice/bio/_history001/_s0001/bio-md` | `sflo:LocatedFile` |
| `alice-bio.md` | Working located markdown file currently associated directly with `alice/bio` | `sflo:LocatedFile` |
| `alice/bio.md` | one (of infinite) less-preferred alternate located markdown file locations that could be associated with `alice/bio` | `sflo:LocatedFile` |
