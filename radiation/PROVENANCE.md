# Radiation payload provenance

This file records where the payload came from, which reference variant each
family uses, how regeneration is demonstrated, and the licensing and accuracy
caveats that accompany publication. The payload contents, record counts,
layouts, and data checksums are in `README.md`.

## Producers and generations

### Current curated corpus

The 27 binary fixtures ship uncompressed beside ordinary paired JSON/TSV/CSV
files with descriptive family prefixes at this directory's root. Per-record `origin` and `family`
labels are serialized in each binary fixture;
they include `current_official_common_prepared_transport` (four-point
transport shards), `external_thcm1_fragment`, `external_thcm1_source_fragment`,
`external_thcm1_direct_fragment`, `external_thcm1_direct_grid`,
`seeded`, `source_campaign`, `independent_corrected_THC_caller`,
`independent_analytical_minerbo`, `independent_closed_form_helpers`,
`analytic_declared_TEH78_approximation`, and
`independent_analytical_collision`. No expected output is replaced by a
GRHayL prediction anywhere in the payload.

The paired text corpus stores shared inputs once. The pointwise ordinary and
mapped-xi captures share `pointwise_fixtures_seeded_packets.tsv`, and the
source roundoff and timestep-refinement captures share
`source_update_fixtures_roundoff_seeded_packets.tsv`. Repository-owned
materialization supplies the historical family-relative input aliases at
replay time; the removed duplicate copies added no cases or independent
reference answers. The distinct captures and refinement levels are retained.

The corrected-reference generator is
`Unit_Tests/radiation_pairs/corrected_reference.py` in the matching GRHayL
checkout. It compiles THC/CPPUtils/GSL without linking GRHayL and reuses the
reviewed correction patchers; the existing Jacobian witness checks the
compiled corrected derivatives against analytical and finite-difference
derivatives. The decision and regeneration procedure are documented in the
matching checkout's `docs/raw/Corrected_THC_reference.md`.

### Independent helper and raw-rate fixtures

`helpers_independent.bin` (107 direct-input records: 9 pair-aware transport
opacity, 15 comoving number density including the closure-reuse companion,
7 split LU factorize/solve, 28 dense-solver contract guards (34 inputs /
1 status output), and 48 optional diffusion-flux contracts (34 inputs /
3 outputs); every record is input-invariant, with the scenario carried by
the per-record perturbation label) is produced by
`Unit_Tests/radiation_pairs/generate_helper_fixtures.py` in the matching
checkout. Expected values are independent of the production code paths:
exact rational (`fractions.Fraction`) evaluation of the documented
opacity/extinction formulas, 90-digit closed-form Lorentz scalar
contractions `n = N J / [W(E-F.V)]` for number density, exact rational
Gaussian elimination for the LU systems, and a 90-digit evaluation of the
advective/Fick blending equation for the diffusion-flux contracts (an
equation-contract reference, not an exact continuum solution or a THC
accuracy ranking). Dense-solver negative-input records carry explicit finite
uint32 words that reconstruct nonfinite operands at the public boundary;
they are data inputs, not code interposition. The helper reference and
corruption checks pass with HDF5 enabled and disabled.

`collision_independent.bin` (49 direct-input records, all input-invariant
with the scenario carried by the perturbation label: 5 rest RHS, 30 rest
Jacobian, 2 moving finite-difference consistency, 12 rejection guards; 94
inputs per record, outputs 33/34/15/1 by family) is produced by
`Unit_Tests/radiation_pairs/generate_collision_fixtures.py` in the matching
checkout. Rest-reference Jacobian components are checked by exact-rational
central differences of the producer's rest RHS polynomial — reference
derivative algebra, not an independent proof of the RHS physics. Moving
columns are finite-difference consistency checks of the production RHS, not
an independent physical moving solution or a THC accuracy ranking.
Rejection guards expect exact API statuses. Native replay passes with HDF5
enabled and disabled. Corrupted baseline/perturbed answers, masks/statuses,
normalization, record removal and oversized species tags are rejected.
Companion checks independently evaluate the folded rest RHS and exact-rational
derivatives. This member contains no fault-injection records.

`raw_rates_independent.bin` (171 records) is produced by
`Unit_Tests/radiation_pairs/generate_rate_fixtures.py` via high-precision
evaluation of the DECLARED TEH78 moment approximation. It checks the
approximation GRHayL promises, NOT exact physical Fermi-Dirac integrals.
169 records are direct-input (including expected-rejection bounds and 20
input-invariant controls); the remaining 2 records (`rates-0169`,
`rates-0170`) are fault-injection records executed only by the Linux
`--injection-fixture-dir` ELF-interposition mode and are never replayed as
direct input, keeping ordinary and injection-assisted coverage separate.
Fresh on/off builds pass the three rate-generator tests and the native
consumer value/status/empty/truncation mutation controls, including the
separately selected injection mode.

### Independent source and validation contracts

`source_contracts_independent.bin` is produced by
`generate_source_contract_fixtures.py`: 84 ordinary records and nine separate
Linux injection records. Rest scalar endpoints use 90-digit integrating-factor
ODE solutions (70/110-digit rounding agrees), while dyadic fixed-four-step
endpoints and CC integrals use exact-rational BE recurrences. An independent
integrating-factor quadrature checks the exponential reference. Rest stiff
branches use the scalar BE equation, not the continuous-time answer. Pointer,
range, diagnostic and rollback contracts include all eleven exchange fields.
The five Python tests and native corruption controls pass in both builds.
The dyadic solves do not test nonzero Newton work; no terminal-fallback fixture
is supplied, and moving source-free identities are not moving-source ODE proofs.

`validation_contracts_independent.bin` is produced by
`generate_validation_fixtures.py`: 95 direct-input records. Cone boundaries
use high-precision quadratic/floor inequalities; flux norms and repair use
high-precision metric contractions. All numerical references round identically
at 70/110 digits. Retained-face callback contracts have analytical half-space
examples; probe counts check the specified search procedure. The overflow-blend
case tests the documented safe-anchor policy, not greatest-factor accuracy.
Both endpoints and rejection rollback are checked, with three Python tests,
native corruption controls and an actual no-op-repair mutation rejected.
Callback failures are public caller-input contracts, not ELF interposition.
Initialization is covered by the source member rather than duplicated here.
Both generators live under `Unit_Tests/radiation_pairs/` in the matching checkout;
the validation producer imports the source producer's error-enum reader.



## Corrected THC reference policy

Default comparisons use the reviewed corrected THC_M1 numerical
implementation where the fixes apply. The fixes are exact-rest arithmetic and
analytic closure-root evaluation, lapse-consistent boost prediction and
optical-depth branch selection, and the implicit Jacobian's opacity argument
order and y-row derivative indexing. They do not harmonize angular closure or
fallback models.

Applicability (the producer's policy receipt is emitted outside the candidate
payload; capture sidecars are maintained with GRHayL's test support):

- Recaptured with corrected THC_M1: the paired `pointwise/`,
  `pointwise_extended/`, and `source_update/` references (including the
  roundoff cohort), on their original seeded packets. The extended
  prepared-pressure operands are re-exported from the corrected THC tensor.
  The separate xi quantity mapping is unchanged.
- `source_relaxation_thcm1.bin` is recaptured through the corrected caller
  using its original initial states, rates, timestep sequence, final time,
  identifiers, and normalizations; its comparison policy is unchanged.
- Already corrected and retained unchanged: the `time_refinement/` cohorts
  and `source_relaxation_directed_thcm1.bin`.
- Unaffected: the raw pointwise and instantaneous-source packets contain no
  exact fluid-rest endpoints, and the prepared/fixed-state kernels (Rusanov
  family, covariant stress-energy, geometry sources, matter/lepton/number
  increments, scalar floors, physical E/F flux, Jthick, and the four-point
  transport shards) do not invoke the corrected integration path, so their
  reference values are retained rather than recaptured. Seeded inputs and the
  comparison gates are unchanged everywhere.

## Retired varying-controls corpus (`transport_four_point_varying_controls`)

The pre-migration corpus paired 38,880 changed-input variable-volume pairs
(12,960 per direction shard) with 23,112 input-invariant control records in a
separate `transport_four_point_varying_controls*` member. Independent decode
of the removed member shows every control record carries byte-identical
complete 50-field baseline and perturbed input vectors, byte-identical
baseline and perturbed output vectors, and `sensitivity_count == 0`.

What removal changed:

- The removed member added broad fixed-input sampling and repeated-input
  consistency evidence (replaying the same consumed operands through GRHayL
  against the stored identical reference outputs). It contained no
  changed-input perturbation-sensitivity evidence, so no response or
  sensitivity obligation is retired with it.
- The shipped shards `transport_four_point_varying_d{0,1,2}.bin` carry 58,
  63, and 65 changed-input pairs (186 total); each pair still supplies a
  fixed-input reference check at each endpoint plus the common physical-flux
  transform checks. This is explicitly reduced sampling: this document does
  not claim the 186 pairs preserve every old sampled state, and no
  equivalence with the retired 23,112-sample control coverage is claimed.
- No current consumer requires the standalone controls file (it detects any
  control records per record via `sensitivity_count == 0`, and the shipped
  shards carry none). Because the removed evidence class was sampling breadth
  rather than a distinct obligation, the member is not restored here. If
  broad-scale control sampling is wanted again, re-export it from the
  campaign tooling rather than repointing consumers at the retired bytes,
  which survive in this repository's pre-migration commit history (the pinned
  pre-migration revision's tree and related feature branches).

## Paired data and regeneration evidence

The paired files are ordinary JSON, TSV, or CSV data — original/perturbed
inputs, captured THC results, independent references, mapped-input data, and
refinement references. No executable code, command receipts, saved GRHayL
replay outputs, or separate campaign manifests are distributed here.
The copy layer in the matching checkout validates paths, member types,
collisions and integrity, and combines the data with
repository-owned trusted sources in a fresh result directory.

Original-corpus regeneration, verified before the five independent members
were added: a fresh run of the corrected-reference producer route
(`Unit_Tests/radiation_pairs/corrected_reference.py` and its family build/run
receipts) reproduced all 22 original binary fixtures byte-identically and all
48 ZIP members with the same names and member-for-member identical bytes;
only the ZIP container bytes differed (packaging metadata such as
timestamps/order). The numerical input/reference members are now stored
directly with unchanged bytes and flat prefixed filenames. Test-owned schemas,
capture sidecars and execution controls live in GRHayL's
`Unit_Tests/radiation_pairs/fixture_support/`; historical command receipts,
saved GRHayL outputs and the redundant JSON view of prepared CSV inputs live
in `Verification/Radiation/corrected_reference_migration/`. The original
relative paths needed by the consumers are reconstructed only in temporary
replay/regeneration output. The ZIP and folder-wide JSON manifest are retired.
The five added binary members are independent references,
not corrected-THC captures: fresh runs of `generate_helper_fixtures.py`,
`generate_rate_fixtures.py`, `generate_collision_fixtures.py`,
`generate_source_contract_fixtures.py`, and `generate_validation_fixtures.py` reproduce
their binaries byte-identically: 107 helper, 171 rate, 49 collision,
93 source-contract and 95 validation records.
The full producer emits `corrected_reference_policy.json`, regenerated capture
sidecars, and command receipts outside its `payload/` directory. Preserve these
as regeneration evidence, not as public reference datasets; do not silently
replace or relabel historical captures.
Replay retains raw THC disagreement ledgers separately from independent
mathematical-correctness results; no closure or comparator is altered by
packaging.

## Publication pairing and retained evidence

Publish this data with the matching GRHayL consumer, materializer, reference
generators and corrected-THC support. A source revision lacking those paths
cannot reproduce this payload merely because it has a Radiation library.
Select the published TestData revision in GRHayL's
`.github/radiation-testdata-ref` only after that revision exists; a supplied
`M1_FIXTURE_DIR` is the local validation route before publication.

Replay receipts, capture-policy receipts, regeneration logs and numerical
accuracy reports are GRHayL verification evidence, not TestData payload.
Retain them outside temporary build trees, with the data-file inventory and
the tested source revision/dirty-state information. Reused completed command
groups must remain identified as reused; their original logs and receipts
must accompany them. A preserved receipt documents what ran, not a fresh
rebuild of a later source revision. Uncommitted source state cannot be given
an immutable published revision until publication is authorized.

Mandatory mathematical-correctness results, raw cross-code disagreements and
supplementary diagnostic failures must remain separately visible. Symmetric
checks compare each code's own supplied E/F with an independently evaluated
number-to-lepton chain. GRHayL's local exchange can instead consume its
already rounded published number, so reproducing that expression can explain
a diagnostic difference without certifying the upstream number or E/F.
THC's internal number increment is not serialized; an exact cancellation
attribution for its analogous failures is not established by the stored
number alone. THC-method residual checks are also distinct from a comparison
to a common continuous-time physical solution. Their failures are retained,
not reclassified as shared-target accuracy wins or silently suppressed.

## Licensing and attribution caveats

- TestData (this repository) and GRHayL (the matching checkout) are BSD
  2-Clause licensed, copyright (c) 2022 Samuel Cupp; see the repository-root
  `LICENSE` files.
- The THC-derived references were produced with THC_M1 (Templated Hydrodynamics
  Code, copyright David Radice, GPL-3.0-or-later). THC/CPPUtils producer
  sources appear only as snapshots inside the matching GRHayL checkout's
  producer and fixture-support paths; they are NOT distributed in this
  TestData publication, which ships numerical data and JSON/TSV/CSV files
  with no executable code. Redistribution and attribution obligations for
  those producer sources follow the matching checkout's own notices.
- Fixture numbers are data, not code; nevertheless the payload's producer
  provenance above should be retained so downstream users preserve correct
  attribution of the THC_M1 reference implementation and the GRHayL test
  infrastructure.

## Accuracy limits

- Fixtures measure discrete cross-code agreement for the recorded inputs and
  comparison policies; replay does not prove host-level evolution or
  continuum validation, and passing independent correctness checks is not a
  claim of universal raw equality with THC_M1.
- Raw THC agreement ledgers and disagreement histories are retained; closure/
  model differences and known THC issues are covered by independent
  references (for example the analytical-Minerbo stress-energy reference and
  the analytic frozen-rate relaxation ranking) without changing the original
  perturbation acceptance rule.
- The corrected reference repairs specific numerical defects; it does not
  harmonize closure/fallback models, and applicability is recorded per family
  as above. Prepared-kernel comparisons do not cover THC Cactus scheduling or
  the cell-to-face reconstruction pipeline, and the direction labels in the
  four-point corpora reuse one spacing value, so they do not validate
  differing axis geometry.
- Timestep-refinement attributions are reported conditionally beside the
  unchanged gate; they are accuracy attributions, not safe-production
  remedies or tolerance changes.
- Agreement fixtures do not generate trusted expected M1 outputs from GRHayL,
  and an absent reference, unexplained API error, or unvalidated solver
  failure remains a correctness failure in replay.
