# LOF-ODP-003: Correlative Deontic Positions

| | |
|---|---|
| **Status** | Draft 0.1.0, for review by the LOF founding coordinators |
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
| legal position | OPEN: specifically dependent continuant (BFO:0000020) in Option A; generically dependent continuant (BFO:0000031) in Option B | To be decided by the founding coordinators. See the specification. |

## Alignment (LOF-P-015)

The eight positions follow Hohfeld's Fundamental Legal Conceptions (1913, 1917). LKIF-Core and LegalRuleML represent obligations, permissions, and rights as deontic modalities or rule elements rather than as entities with holders; the mapping is therefore partial and will be documented once the placement question is settled.

## Two placement options

This pattern is maintained in two parallel versions pending a decision by the
founding coordinators:

- **Option A** (`src/ontology/lof-odp-003-edit.owl`): legal positions are
  specifically dependent continuants that inhere in their holders.
- **Option B** (`src/ontology/alternatives/lof-odp-003-option-b.owl`): legal
  positions are generically dependent continuants, related to holders by
  `held by`.

`make test-options ODP002_BASE=<path to lof-odp-002-base.owl>` runs the lease
example and the claim-assignment test under both. Option A is inconsistent
when the same claim acquires a new holder; Option B is not.

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
make release VERSION=0.1.0   # writes the two release files to the repo root
```

Requires Java 11 or later; ROBOT is downloaded on first use. The same checks
run on every pull request (`.github/workflows/qc.yml`).

## Citation

Koepsell, D. R. (2026). *LOF-ODP-003: Correlative Deontic Positions*, version 0.1.0 (draft). Legal Ontologies
Foundry. https://w3id.org/lof/odp-003/releases/0.1.0/lof-odp-003.owl
