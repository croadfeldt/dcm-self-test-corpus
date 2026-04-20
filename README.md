# DCM Self-Test Corpus

The curated body of use cases that define what the DCM (Data Center
Management) architecture is contractually required to support.

This corpus is the foundation of the DCM self-test system's Mode A
(Regression) gating — every PR to the DCM architecture spec is
validated against each use case here. It's also the destination
for use cases promoted from Mode B (PR-targeted) and Mode C
(Authoring) runs.

**Status:** Validation phase. Lives in `croadfeldt/dcm-self-test-corpus`
(Chris's personal repo) while the process is being proven. Will
migrate to `dcm-project/dcm-self-test-corpus` once the pipeline,
promotion workflow, and regression gating demonstrate real value.

See the DCM self-test canonical spec for full context:
`dcm-self-test-ansible-integrated/docs/DCM_SELF_TEST_REQUIREMENTS.md`

## Layout

```
.
├── README.md                           (this file)
├── CHANGELOG.md                        (human-readable corpus evolution)
├── schema/
│   ├── use_case.schema.json            (JSON Schema for use case YAMLs)
│   └── analysis.schema.json            (JSON Schema for stage 2 analyses)
├── use_cases/
│   ├── compute/                        (VMs, bare metal, containers)
│   ├── networking/                     (VLANs, VPCs, routing)
│   ├── identity/                       (authn/authz, IdP integration)
│   ├── data/                           (data services, storage)
│   ├── governance/                     (policy, compliance, audit)
│   └── cross_domain/                   (scenarios crossing 2+ domains)
└── baselines/
    └── dcm-<version>/
        └── <use_case_uuid>.baseline.yaml
```

## Use case format

Every use case is a YAML file conforming to `schema/use_case.schema.json`.

Filename convention: `<trailing-handle-segment>.yaml` — e.g. a use case
with handle `compute/vm-standard-provision` lives at
`use_cases/compute/vm-standard-provision.yaml`.

UUID convention: `uc-<hex>` generated at creation time; stable for the
life of the use case.

## Baseline format

Each use case has zero or more baseline analyses — one per DCM spec
version it has been validated against. Baselines are the stored
stage 2 output that regression runs diff against.

When a use case is admitted to the corpus, its initial baseline is
generated and committed at `baselines/dcm-<current-version>/<uuid>.baseline.yaml`.

Baselines are re-generated on major DCM version bumps; any unexpected
delta flags the case for architect review.

## Contribution workflow

Use cases enter the corpus via one of three paths:

1. **Direct hand authoring.** Architect writes a YAML, validates it
   (schema + dimension vocabularies), opens a PR to this repo.
2. **Promotion from a Mode B/C run.** An architect reviews a
   generated or authored case in the Review Console, clicks "promote
   to corpus," which opens a PR against this repo with the case YAML
   and initial baseline pre-filled.
3. **Bulk seeding.** Initial population from the seed set shipped
   in the DCM self-test Ansible deployment repo under
   `dcm-self-test-corpus/`.

## Review criteria for corpus admission

A use case gets merged when:

- ✅ Schema-valid
- ✅ Dimensions make architectural sense (not contradictory)
- ✅ Scenario is concrete enough that stage 2 can analyze it
- ✅ Success criteria are testable
- ✅ Tags are useful (domain, complexity, edge flags)
- ✅ Not a near-duplicate of an existing case
- ✅ Initial baseline passes architect spot-check (rationales are
  spec-grounded, not fabricated)

## Retirement

A use case can be retired (removed from the corpus) only via a PR
with explicit rationale:

- The architecture has genuinely moved past supporting it (not "we
  broke it and didn't fix the spec")
- It was superseded by a more precise or more comprehensive case
- It was admitted in error (e.g., duplicates an existing case)

Retirement moves the YAML and its baselines to `retired/<date>/`
rather than deleting them, so the historical record is preserved.

## Controlled vocabularies

Dimension values are constrained. Valid values as of schema v1.0:

- **profile**: `minimal | dev | standard | prod | fsi | sovereign`
- **lifecycle_phase**: `new_request | modification | decommission |
  drift_detection | brownfield_ingestion | rehydration_faithful |
  rehydration_provider_portable | rehydration_historical_exact |
  rehydration_historical_portable | expiry_enforcement`
- **resource_complexity**: `single_no_deps | hard_dependencies |
  compound_service | conditional_soft_deps | process_resource |
  cross_dependency_payload`
- **policy_complexity**: `system_defaults_only | single_gatekeeper |
  multi_policy_chain | conflicting_policies | orchestration_flow_static |
  dynamic_conditional_flow | cross_domain_constraint |
  human_escalation_required | governance_matrix_enforcement |
  recovery_policy`
- **provider_landscape**: `single_eligible | multiple_eligible |
  none_eligible | peer_dcm_required | meta_provider_composed |
  process_provider | mixed`
- **governance_context**: `no_governance | standard_governance |
  audit_heavy | compliance_gated | sovereignty_enforced`
- **failure_mode**: `happy_path | provider_failure | policy_violation |
  peer_dcm_disconnect | data_inconsistency | rollback_required |
  partial_fulfillment | timeout | resource_exhaustion`

These vocabularies evolve alongside DCM itself. Additions require a
schema bump and updates in the engine's `core/use_case_schema.py`.

## License

Apache 2.0 (matches the DCM project itself).
