---
id: ls7gy1x4oj43fpgctvsqbp3
title: Alice Bio
desc: ''
updated: 1773891622424
created: 1773730267525
---

## Nomen Table

This sketch assumes:

* `alice` and `alice/bio` are the only non-supporting designators.
* `alice` denotes the person Alice.
* `alice/bio` denotes the bio `DigitalArtifact` directly.
* `alice/_nomen` and `alice/bio/_nomen` denote the corresponding `Nomen`s.
* `alice/_knop` and `alice/bio/_knop` denote the corresponding `Knop`s.
* `_mesh` denotes the mesh surface itself.
* historical states are explicit, while a working `LocatedFile` may hang directly off the `DigitalArtifact` without requiring separate working-state resources.

| designatorPath | referent | classes |
| --- | --- | --- |
| `_mesh` | The mesh surface for this Alice example | `sflo:SemanticMesh` |
| `_mesh/.semantic-mesh-metadata` | Mesh metadata artifact for this mesh | `sflo:MeshMetadata`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `_mesh/.semantic-mesh-metadata.ttl` | Located Turtle file providing bytes for `_mesh/.semantic-mesh-metadata` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `_mesh/.semantic-mesh-inventory` | Mesh inventory artifact for this mesh | `sflo:MeshInventory`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `_mesh/.semantic-mesh-inventory.ttl` | Located Turtle file providing bytes for `_mesh/.semantic-mesh-inventory` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice` | Alice the person | `schema:Person` |
| `alice/_nomen` | The Nomen for `alice` | `sflo:Nomen` |
| `alice/_knop` | The Knop for `alice` | `sflo:Knop` |
| `alice/_knop/_meta` | Knop metadata artifact for `alice` | `sflo:KnopMetadata`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/_knop/_meta.ttl` | Located Turtle file providing bytes for `alice/_knop/_meta` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/_knop/_inventory` | Knop inventory artifact for `alice` | `sflo:KnopInventory`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/_knop/_inventory.ttl` | Located Turtle file providing bytes for `alice/_knop/_inventory` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/bio` | Alice bio as the primary artifact identified by this designator | `sflo:PayloadArtifact`, `sflo:DigitalArtifact` |
| `alice/bio/_nomen` | The Nomen for `alice/bio` | `sflo:Nomen` |
| `alice/bio/_knop` | The Knop for `alice/bio` | `sflo:Knop` |
| `alice/bio/_knop/_meta` | Knop metadata artifact for `alice/bio` | `sflo:KnopMetadata`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/bio/_knop/_meta.ttl` | Located Turtle file providing bytes for `alice/bio/_knop/_meta` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/bio/_knop/_inventory` | Knop inventory artifact for `alice/bio` | `sflo:KnopInventory`, `sflo:DigitalArtifact`, `sflo:RdfDocument` |
| `alice/bio/_knop/_inventory.ttl` | Located Turtle file providing bytes for `alice/bio/_knop/_inventory` | `sflo:LocatedFile`, `sflo:RdfDocument` |
| `alice/bio/_knop/_history/h1` | First published historical state of the bio artifact | `sflo:HistoricalState` |
| `alice/bio/_knop/_history/h1/bio-md` | Markdown artifact manifestation for historical state `h1` | `sflo:ArtifactManifestation` |
| `alice/bio/_knop/_history/h1/bio-md/bio.md` | Located markdown file providing bytes for `alice/bio/_knop/_history/h1/bio-md` | `sflo:LocatedFile` |
| `alice/bio/bio.md` | Working located markdown file currently associated directly with `alice/bio` | `sflo:LocatedFile` |
| `alice/bio.md` | one (of infinite) less-preferred alternate located markdown file locations that could be associated with `alice/bio` | `sflo:LocatedFile` |
