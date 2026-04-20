# DCM Self-Test Corpus — Changelog

## Unreleased

### Added

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
