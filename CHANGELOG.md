# DCM Self-Test Corpus — Changelog

## Unreleased

### Added

- Six additional hand-authored use cases expanding Phase 1a coverage
  across new domains, profiles, and failure shapes:
  - `data/persistent-volume-provision` (uc-seed-004a, dev profile) —
    happy-path storage provisioning; introduces the `data` domain.
  - `governance/policy-override-approval` (uc-seed-005a, prod profile)
    — soft-policy override workflow with human-in-loop approval;
    exercises the override_requests machinery.
  - `compute/vm-provision-with-provider-failure` (uc-seed-006a,
    standard profile) — recovery policy path when a service provider
    becomes unreachable mid-realization; first use of the
    `recovery_policy` dimension value.
  - `governance/audit-merkle-tree-verification` (uc-seed-007a, sovereign
    profile) — cryptographic verification of historical audit events
    via Merkle inclusion and consistency proofs with sovereignty-scoped
    key material.
  - `cross_domain/tenant-onboarding` (uc-seed-008a, fsi profile) —
    atomic onboarding of a new FSI tenant across identity, policy,
    data, and provider domains; first use of `compound_service` and
    `compliance_gated` in the corpus.
  - `governance/minimal-profile-policy-scope-boundary` (uc-seed-009a,
    minimal profile) — negative architectural claim: FSI-scoped
    policies (data-residency, dual-approval, encryption-at-rest-mandatory)
    must NOT evaluate against minimal-profile requests. Exercises
    the profile inheritance boundary at the bottom of the profile
    chain.

- Two new domain directories: `use_cases/data/` and `use_cases/governance/`.

- Profile coverage expanded: `dev`, `fsi`, and `minimal` now represented
  (previously only `standard`, `prod`, `sovereign`).

- Controlled-vocabulary cell coverage expanded:
  - `lifecycle_phase`: adds `modification` (override and audit-verification
    flows).
  - `resource_complexity`: adds `compound_service` (tenant onboarding).
  - `policy_complexity`: adds `recovery_policy`, `system_defaults_only`.
  - `provider_landscape`: adds `multiple_eligible`, `mixed`.
  - `governance_context`: adds `audit_heavy`, `compliance_gated`,
    `no_governance`.
  - `failure_mode`: adds `provider_failure`.

### Initial seed (previously released)

- Initial seed with 3 hand-authored use cases covering foundational
  scenarios across three domains:
  - `compute/vm-standard-provision` — happy-path VM creation in the
    standard profile. Foundational baseline.
  - `cross_domain/sovereign-decommission-with-peer` — edge case
    exercising peer_dcm coordination during sovereign-profile
    decommission with peer disconnection failure mode.
  - `identity/auth-provider-drift-detection` — drift detection
    between DCM's identity cache and the authoritative IdP, with
    human-escalation policy path.

- Schema definitions (`schema/use_case.schema.json`,
  `schema/analysis.schema.json`) matching DCM self-test engine
  v1.0 canonical schemas.

- Governance documentation: contribution workflow, review criteria,
  retirement policy, controlled-vocabulary reference.

### Notes

- No baselines yet. Baselines will be generated when the stage 2
  engine first runs these cases against the current DCM spec.
- Repository currently at `croadfeldt/dcm-self-test-corpus` during
  the validation phase. Migration to `dcm-project/dcm-self-test-corpus`
  is planned once the process has accumulated ~25 validated cases
  and demonstrated its value through a full PR cycle.
