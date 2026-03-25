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

There is also a distinct but closely related `IntegrationConfig` aspect: configuration that governs how raw candidate artifacts are recognized, claimed, mapped into `Knop`s, and initialized during `weave integrate`.

## Working Goal

Define a modern config model that fits the current core ontology and can answer:

- what config applies to this `Knop`?
- what config applies to this `SemanticMesh`?
- what explicitly authored config artifacts exist?
- what effective config was computed for debugging or runtime inspection?
- what integration config applies when a candidate artifact is being considered for mesh integration?

## Current Direction

The modernized config model should be grounded in current active classes:

- `sflo:SemanticMesh`
- `sflo:Knop`
- `sflo:DigitalArtifact`
- `sflo:RdfDocument`

The likely shape is:

- `Config` remains an RDF configuration resource
- named reusable config should be modeled as a `DigitalArtifact`, probably also an `RdfDocument`
- config can attach directly to a `SemanticMesh` and `Knop`
- if config needs its own persistent identity, history, or reuse, the config resource itself may be modeled as a `DigitalArtifact`
- computed effective config remains explicitly non-authoritative runtime/debugging data

It may be useful to distinguish at least two major config families:

- presentation or publication-oriented config, especially for ResourcePage generation
- integration-oriented config, governing how external or in-tree artifacts become mesh-managed `Knop`s and `DigitalArtifact`s

This means the old “config as Flow/Distribution” framing should be dropped rather than translated literally. The [current core decision](ont.decision-log.md) is that `DigitalArtifact` is the governing artifact-level resource and that the old named classes `AbstractArtifact`, `ArtifactFlow`, `ArtifactState`, `WorkingState`, and `CurrentState` are not part of the active core as such. Their concerns are now handled more explicitly through the current structure: `ArtifactState` is roughly replaced by `HistoricalState`, `CurrentState` is approximated operationally by `latestHistoricalState`, and `WorkingState` is replaced by `hasWorkingLocatedFile`.

## Likely Term Mapping

- `sflo:Mesh` -> `sflo:SemanticMesh`
- `sflo:AbstractArtifact` -> `sflo:DigitalArtifact`
- `hasAbstractArtifactConfig` -> probably drop rather than rename directly, unless a narrower non-`Knop` attachment point reemerges
- `TemplateMappingSet` -> maybe `ResourcePageTemplateMappingSet`, unless the scope truly broadens beyond ResourcePages
- `TemplateMapping` -> maybe `ResourcePageTemplateMapping`, unless the scope truly broadens beyond ResourcePages
- `InnerTemplate` -> probably `InnerResourcePageTemplate` or `ResourcePageBodyTemplate`
- `OuterTemplate` -> probably `OuterResourcePageTemplate` or `ResourcePageShellTemplate`
- `mappingTargetSlugRegex` -> probably something closer to `mappingTargetDesignatorPathRegex`, or a broader selector term if regex-only targeting feels too narrow

## ResourcePage vs Browser Layer

[Recent ResourcePage discussion](../../sflo-dendron-notes/sflo.conv.2026.2026-03-13_2159-chrome-on-static-sites-codex-2.md) suggests a useful boundary for the config work:

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
- template-mapping vocabulary, likely still ResourcePage-focused in this pass
- integration-config vocabulary, whether in the same ontology or a closely related module
- effective-config vocabulary

This task should not automatically assume that integration-template modeling belongs in exactly the same ontology pass.

It also should not automatically conflate:

- ResourcePage composition rules
- browser-layer chrome/navigation behavior
- integration-template logic for mapping arbitrary artifacts into `Knop`s

## Integration Templates

There is a related but somewhat different need: describing how candidate `DigitalArtifact`s should be integrated into `Knop`s.

It may be better to speak not only of "integration templates" but of an `IntegrationConfig` family, because the concern is broader than a single reusable template object. This area seems to include both:

- reusable integration patterns or templates
- resolved effective config used by `weave integrate` for a specific target, scope, or candidate

The [March 14 `weave integrate` discussion](../../sflo-dendron-notes/sflo.conv.2026.2026-03-14_0958-existing-solutions-that-could-be-extended-with-weave-codex.md) points toward config that can drive:

- scoped subtree or target resolution
- candidate matching and claim rules
- designator-path derivation
- knop creation rules
- metadata-template assignment
- handling of external working sources like paths outside the tree or URLs
- history vs no-history behavior at integration time

That probably wants vocabulary for things like:

- integration pattern or template
- target mesh or scope
- candidate artifact selector
- designator-path mapping
- default metadata or reference behavior
- optional generation or validation policy

This feels adjacent to config, but not obviously identical to output-template mapping.

Current instinct: treat `IntegrationConfig` as a real part of the config modernization problem, even if the full integration-pattern vocabulary lands as a linked second phase.

## Open Questions

- Should `Config` itself be a first-class class distinct from `DigitalArtifact`, with named reusable configs modeled as a config role on `DigitalArtifact`?
- Should mesh config and knop config share the same generic attachment property plus subproperties, or have more explicit separate properties from the start?
- If some artifact-directed policy still feels necessary, should it attach to the `Knop` as publication or integration policy rather than directly to a `DigitalArtifact`?
- Should output-template mapping live under config, or should it become its own small ontology/module?
- Should the mapping vocabulary stay explicitly ResourcePage-oriented for now, with broader “output” terms deferred until there is a second concrete output family?
- Should `IntegrationConfig` be a named subclass or role of `Config`, or just a recognized concern addressed by particular config properties?
- Should integration-oriented config live in the same ontology as presentation config, or in a closely related companion module?
- Should config targeting stay regex-oriented, or move toward more semantic selectors aligned with current API targeting ideas?
- How much of integration-template modeling belongs in this task versus a follow-up task?

## Candidate Deliverables

- a modern `semantic-flow-config-ontology.ttl`
- a migration note from old config terms to new terms
- one worked example showing mesh config plus knop config
- one worked example showing enough `IntegrationConfig` to drive a realistic `weave integrate`
- optional follow-up task for integration-template modeling

## Suggested Plan

1. Audit old config terms and mark each as keep, rename, replace, or drop.
2. Decide whether named config should be modeled as a role on `DigitalArtifact` or as a parallel class hierarchy.
3. Define the minimal modern config ontology for `SemanticMesh` and `Knop`, while deciding how named reusable config artifacts relate to `DigitalArtifact`.
4. Decide whether template vocabulary should stay ResourcePage-specific, and rename `InnerTemplate` / `OuterTemplate` accordingly.
5. Decide the minimal shape of `IntegrationConfig`, even if the richer integration-pattern vocabulary becomes a second task.
