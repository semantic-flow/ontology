---
id: ls7gy1x4oj43fpgctvsqbp3
title: Biographical Data Publishing
desc: 'publishing stable linked-data identifiers and versioned artifacts for people and biographies'
updated: 1777703078298
created: 1773730267525
---

## Purpose

Biographical data publishing is the use case where a person, organization, community, archive, or project publishes stable identifiers and structured public information about people. The public surface needs to work for both readers and data consumers: human-readable pages, machine-readable RDF, clear references, durable identifiers, and inspectable versions of published artifacts.

This is not just "a profile page." A person identifier, a biography document, a profile page definition, a current RDF description, a reference catalog, and historical snapshots can all be separate resources with different lifecycles.

## Typical Publishers

* an individual maintaining an authoritative public profile and biography
* a research group or nonprofit publishing profiles for members, contributors, interview subjects, or participants
* an archive describing people, relationships, source materials, and evidence
* an organization publishing durable staff or contributor identities with source-backed metadata

## User Needs

* mint a stable identifier for a person, such as `/alice`
* distinguish the person from artifacts about the person, such as `/alice/bio` or `/alice/page-main`
* publish human-readable profile or biography pages
* publish RDF descriptions of names, relationships, affiliations, roles, alternate identifiers, cited sources, and related works
* cite external references without making an external authority the owner of the local identifier
* version published biographical artifacts so prior public statements can be inspected
* keep generated support metadata, inventories, histories, and pages understandable without exposing unrelated authoring files

## Semantic Flow Shape

The person is normally the primary referent. In RDF terms it may be typed with vocabularies such as `schema:Person` or `foaf:Person`, but the person is not itself a `DigitalArtifact`.

Biographical files, profile pages, RDF descriptions, and generated page definitions are artifacts about the person. They can be modeled as `PayloadArtifact`s or more specific artifact classes when the ontology has one. Their current working bytes may live in an authoring location, while woven historical states should preserve the published bytes that readers or machines may have consumed.

External sources and identifiers belong in reference-oriented structures. A local mesh might say that `/alice` is related to an ORCID, Wikidata entity, homepage, publication, or cited source, but those links should not collapse the local identifier into the external one. `ReferenceCatalog` is the natural support artifact when the mesh needs to record these links explicitly.

Resource pages are part of the publishing surface, not the identity model itself. A profile page should make the public facts easy to inspect, but the durable distinction between the person, artifacts about the person, located files, and historical states should remain visible in the mesh data.

## Modeling Boundaries

The key distinction is between a person and artifacts about that person. A path like `/alice` can identify the person. A path like `/alice/bio` can identify a biography artifact. That artifact may have current located files, historical states, manifestations, and generated pages, but those support resources do not become the person.

Publishing biographical data also has a social boundary. Stable identifiers and historical snapshots are useful, but living people, corrections, retractions, and privacy-sensitive facts require care. A mesh is a public commitment surface; it should only carry facts and historical records the publisher is prepared to expose.

## Repository Shape

A standalone public dataset can be a whole-repo semantic mesh. That is a good shape for a small reference publication or a fixture where the mesh structure itself is the thing being inspected.

Most real biographical publishing projects are likely better as sidecar meshes. The authoring repository may contain notes, drafts, source data, scripts, editorial workflow, or private working material that should not become the public identifier surface. In that shape, the sidecar mesh carries stable public identifiers, generated pages, and historical snapshots while source files remain in project-appropriate locations. See [[wu.repository-options]] for the repository-topology distinction.

## Alice Bio Fixture

The Alice Bio fixture is a small whole-repo reference mesh that exercises this use case for Semantic Flow tooling. It is useful as an implementation fixture, not as the canonical information architecture for all biographical publishing projects.

Use the fixture notes when implementation detail matters, especially [[wd.task.2026.2026-03-25-mesh-alice-bio]] and [[sf.task.2026.2026-03-29-conformance-for-mesh-alice-bio]]. This use-case note intentionally avoids carrying the fixture ladder or its full designator table.
