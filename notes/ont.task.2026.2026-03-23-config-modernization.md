---
id: 0cm22gs24b5ufl01qdtw1fg
title: 2026 03 23 Config Modernization
desc: ''
updated: 1774330976647
created: 1774330976647
---

## Context

The old config work in `old/sflo-config-ontology.jsonld` was designed around ontology structures that no longer exist in the active model.

The main mismatches are:

- `sflo:Mesh` should now be `sflo:SemanticMesh`
- `sflo:AbstractArtifact` should now be `sflo:DigitalArtifact`
- old Flow / State / Distribution framing should not be carried forward directly
- old template-mapping naming should be revisited, but likely in a more ResourcePage-specific direction than the broad `OutputTemplateMapping`

The primary current use case is figuring out a `Knop`'s config.

Secondarily, the same modernization should probably support mesh-level config, especially where a mesh provides defaults or broader output-generation policy.

## Working Goal

Define a modern config model that fits the current core ontology and can answer:

- what config applies to this `Knop`?
- what config applies to this `SemanticMesh`?
- what explicitly authored config artifacts exist?
- what effective config was computed for debugging or runtime inspection?

## Current Direction

The modernized config model should be grounded in current active classes:

- `sflo:SemanticMesh`
- `sflo:Knop`
- `sflo:DigitalArtifact`
- `sflo:RdfDocument`

The likely shape is:

- `Config` remains an RDF configuration resource
- named reusable config should be modeled as a `DigitalArtifact`, probably also an `RdfDocument`
- config can attach directly to a `SemanticMesh`, `Knop`, and possibly a `DigitalArtifact`
- computed effective config remains explicitly non-authoritative runtime/debugging data

This means the old “config as Flow/Distribution” framing should be dropped rather than translated literally. The current core decision is that `DigitalArtifact` is the governing artifact-level resource and that `AbstractArtifact`, `ArtifactFlow`, `ArtifactState`, `WorkingState`, and `CurrentState` are not part of the active core.

## Likely Term Mapping

- `sflo:Mesh` -> `sflo:SemanticMesh`
- `sflo:AbstractArtifact` -> `sflo:DigitalArtifact`
- `hasAbstractArtifactConfig` -> likely `hasDigitalArtifactConfig`
- `TemplateMappingSet` -> maybe `ResourcePageTemplateMappingSet`, unless the scope truly broadens beyond ResourcePages
- `TemplateMapping` -> maybe `ResourcePageTemplateMapping`, unless the scope truly broadens beyond ResourcePages
- `InnerTemplate` -> probably `InnerResourcePageTemplate` or `ResourcePageBodyTemplate`
- `OuterTemplate` -> probably `OuterResourcePageTemplate` or `ResourcePageShellTemplate`
- `mappingTargetSlugRegex` -> probably something closer to `mappingTargetDesignatorPathRegex`, or a broader selector term if regex-only targeting feels too narrow

## ResourcePage vs Browser Layer

Recent ResourcePage discussion suggests a useful boundary for the config work:

- woven `index.html` pages should remain real standalone HTML with meaningful content
- shared chrome like global nav/search/header can be emitted separately and loaded by the browser
- Turbo is promising as a browser-layer enhancement for smoother navigation, not as the thing that defines page identity or page structure

That suggests template vocabulary should primarily describe woven ResourcePage composition, not the separately loaded browser shell.

So for now, the “inner/outer template” lineage still seems useful, but it should probably be renamed in a more explicit ResourcePage direction rather than generalized too quickly into “output templates” if the actual semantics are still page-specific.

## Scope For This Task

This task should focus first on modernizing the configuration ontology for current active concepts.

That likely includes:

- config attached to `SemanticMesh`
- config attached to `Knop`
- possibly config attached to `DigitalArtifact`
- template-mapping vocabulary, likely still ResourcePage-focused in this pass
- effective-config vocabulary

This task should not automatically assume that integration-template modeling belongs in exactly the same ontology pass.

It also should not automatically conflate:

- ResourcePage composition rules
- browser-layer chrome/navigation behavior
- integration-template logic for mapping arbitrary artifacts into `Knop`s

## Integration Templates

There is a related but somewhat different need: describing how candidate `DigitalArtifact`s should be integrated into `Knop`s.

That probably wants vocabulary for things like:

- integration pattern or template
- target mesh or scope
- candidate artifact selector
- designator-path mapping
- default metadata or reference behavior
- optional generation or validation policy

This feels adjacent to config, but not obviously identical to output-template mapping.

Current instinct: treat integration-template design as a linked second phase unless a very small shared core falls out naturally.

## Open Questions

- Should `Config` itself be a first-class class distinct from `DigitalArtifact`, with named reusable configs modeled as a config role on `DigitalArtifact`?
- Should mesh config and knop config share the same generic attachment property plus subproperties, or have more explicit separate properties from the start?
- Should output-template mapping live under config, or should it become its own small ontology/module?
- Should the mapping vocabulary stay explicitly ResourcePage-oriented for now, with broader “output” terms deferred until there is a second concrete output family?
- Should config targeting stay regex-oriented, or move toward more semantic selectors aligned with current API targeting ideas?
- How much of integration-template modeling belongs in this task versus a follow-up task?

## Candidate Deliverables

- a modern `semantic-flow-config-ontology.ttl`
- a migration note from old config terms to new terms
- one worked example showing mesh config plus knop config
- optional follow-up task for integration-template modeling

## Suggested Plan

1. Audit old config terms and mark each as keep, rename, replace, or drop.
2. Decide whether named config should be modeled as a role on `DigitalArtifact` or as a parallel class hierarchy.
3. Define the minimal modern config ontology for `SemanticMesh`, `Knop`, and `DigitalArtifact`.
4. Decide whether template vocabulary should stay ResourcePage-specific, and rename `InnerTemplate` / `OuterTemplate` accordingly.
5. Decide whether integration templates are in scope for this pass or spun into a second task.
