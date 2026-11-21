# LOF-ODP-003: Correlative Deontic Positions

| | |
|---|---|
| **Status** | Draft 0.2.0, for review by the LOF founding coordinators |
| **IRI** | https://w3id.org/lof/odp-003.owl |
| **Specification** | https://legal-ontologies-foundry.github.io/odp/odp-003/ |
| **License** | [CC BY 4.0](LICENSE) |
| **Contact** | David R. Koepsell (drkoepsell@tamu.edu) |

## Scope

Covers Hohfeld's eight fundamental legal positions and their correlative pairings (claim and duty, liberty and no-claim, power and liability, immunity and disability), their holders, the legal content that grounds them, and the legal acts in which powers are exercised. Excludes the action types that positions concern, which remain an open modeling question, and Hohfeld's jural opposites.

**Depends on:** [LOF-ODP-001](https://w3id.org/lof/odp-001.owl), imported from its base release.

## Competency questions

1. What duties does this person hold, and to whom are they owed?
2. What is the correlative of this claim, and who holds it?
3. In virtue of what legal content does this position exist?
4. Which legal acts were exercises of this power?
5. Does this position survive the assignment of the claim, or the death of its holder?

## BFO grounding (LOF-P-004)

| Term | BFO category | Rationale |
|---|---|---|
| legal position | role (BFO:0000023) | Inheres in its holder because of the holder's legal circumstances, not physical make-up, and is realized when the holder complies, acts, or exercises it. Exactly one holder; a change of holder is a new position that `succeeds` the old. Adopted in 0.2.0 (issue #9). |

## Alignment (LOF-P-015)

The eight positions follow Hohfeld's Fundamental Legal Conceptions (1913, 1917). LKIF-Core and LegalRuleML represent obligations, permissions, and rights as deontic modalities or rule elements rather than as entities with holders; the mapping is therefore partial. The Document Act Ontology (D-Acts) models four of the eight positions as deontic roles; claim, duty, liberty, and power are aligned with its claimant, duty holder, privilege holder, and power roles in the LOF to D-Acts bridge, which also aligns powers exercised in role conferral and termination acts with the D-Acts deontic declaration performer role. ODP-003 adds the four positions D-Acts lacks: no-claim, liability, immunity, and disability. Positions are grounded in legal norms (LOF-ODP-001), which correspond to the CCO process requirement, process prohibition, and action permission.

## Placement (decided in 0.2.0)

Legal positions are BFO roles. Version 0.1.0 kept two options open: Option A
(specifically dependent continuant) and Option B (generically dependent
continuant, so that the same claim could pass from holder to holder). 0.2.0
adopts Option A in its role form, following W. Mandrick's proposal to align
with the D-Acts deontic roles (issue #9).

What Option B was meant to capture, a claim surviving its assignment, is kept
by the new relation `succeeds`: the assignee's claim succeeds the assignor's.
`src/examples/assignment-succession.ttl` models an assignment this way and is
consistent; `assignment-test.ttl` (one claim, two holders) stays inconsistent
by design. Option B is archived in `src/ontology/alternatives/`.

## Files

| Path | Contents |
|---|---|
| `lof-odp-003.owl` | Full release: merged with imports and reasoned |
| `lof-odp-003-base.owl` | Base release: this pattern's own axioms only (import this) |
| `src/ontology/lof-odp-003-edit.owl` | Editors' file (OWL functional syntax; edit in Protégé) |
| `src/ontology/lof-odp-003-idranges.owl` | Term ID ranges (ODP003_0001000 to 0001999: first editor) |
| `src/sparql/` | QC checks run by `make test` |

## Building and testing

```
cd src/ontology
make test                    # HermiT consistency + seven SPARQL QC checks + ROBOT report
make release VERSION=0.2.0   # writes the two release files to the repo root
```

Requires Java 11 or later; ROBOT is downloaded on first use. The same checks
run on every pull request (`.github/workflows/qc.yml`).

## Citation

Koepsell, D. R. (2026). *LOF-ODP-003: Correlative Deontic Positions*, version 0.2.0 (draft). Legal Ontologies
Foundry. https://w3id.org/lof/odp-003/releases/0.2.0/lof-odp-003.owl
