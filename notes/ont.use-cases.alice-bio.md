---
id: ls7gy1x4oj43fpgctvsqbp3
title: Alice Bio
desc: ''
updated: 1774417187131
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
* historical states are explicit, while a working `LocatedFile` may hang directly off the `DigitalArtifact` without requiring separate working-state resources.

| designatorPath | referent | classes |
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
| `alice/bio/_knop/_history/v1` | First published historical state of the bio artifact | `sflo:HistoricalState` |
| `alice/bio/_knop/_history/v1/bio-md` | Markdown artifact manifestation for historical state `v1` | `sflo:ArtifactManifestation` |
| `alice/bio/_knop/_history/v1/bio-md/bio.md` | Located markdown file providing bytes for `alice/bio/_knop/_history/v1/bio-md` | `sflo:LocatedFile` |
| `alice/bio/bio.md` | Working located markdown file currently associated directly with `alice/bio` | `sflo:LocatedFile` |
| `alice/bio.md` | one (of infinite) less-preferred alternate located markdown file locations that could be associated with `alice/bio` | `sflo:LocatedFile` |
