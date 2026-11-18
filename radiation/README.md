# Radiation fixtures

This directory is data-only: flat binary reference fixtures,
flat paired JSON/TSV/CSV files, and documentation. Trusted replay,
adapter, producer, and test sources belong to the matching GRHayL checkout
under `Unit_Tests/radiation_pairs/` and are not distributed here. This README
documents the inventory and binary layout; `PROVENANCE.md` records fixture
origins, the corrected-THC policy, regeneration evidence, licensing caveats,
and accuracy limits.

- Payload: 27 raw `.bin` fixtures and 28 paired JSON/TSV/CSV files. See "Independent fixture
  families" for the analytical and contract checks.
- Default reference captures use the reviewed corrected THC_M1 numerical
  implementation wherever its fixes apply; see `PROVENANCE.md`. Comparison
  gates, seeded inputs, and closure/fallback models are unchanged, and no
  expected output is replaced by a GRHayL prediction.
- `transport_four_point_varying_controls.bin` is intentionally absent from
  this layout; see "Retired varying-controls corpus" in `PROVENANCE.md`.

## Payload inventory, records, and checksums

SHA-256 digests below cover the payload data files. They are data checksums,
not source hashes. Operation labels, comparison policies and family/origin
labels are serialized in the fixtures. There is no folder-wide JSON manifest.

| Member | Content | Records | Bytes | SHA-256 |
| --- | --- | --- | --- | --- |
| `source_contracts_independent.bin` | Analytical source/update contracts (84 direct + 9 separately selected injection records) | 93 | 264989 | `aa96c156fd0f542e81e9a5169a6e089392ab1eb5b4e6da479089f50d9c57e43b` |
| `validation_contracts_independent.bin` | Analytical validation/transport and public callback contracts (95 direct records) | 95 | 86226 | `0076dcbbaf7ee59705c95f8fc1e1bb33451438df970f1d5507f3b3dc15d3f63c` |
| `collision_independent.bin` | Independent collision RHS/Jacobian references: rest RHS (5), rest Jacobian (30), moving FD consistency (2), rejection guards (12); all records input-invariant | 49 | 119834 | `6211776a16a9eefc3f7a3be1e3bed9f1f99825ab933b74b9e3042415fc0d6714` |
| `geometry_sources_thcm1.bin` | Geometric source pairs (metric/lapse/shift gradients, curvature, seeded) | 64 | 94909 | `2c0e23bbb9c96a8b1174b96ab63fe316fad56652f091211b0ed986f2cd2d134b` |
| `helpers_independent.bin` | Independent closed-form helper references: transport opacity (9), number density (15), split LU (7), dense-solver contracts (28), diffusion-flux contracts (48); all records input-invariant | 107 | 93506 | `987b8511ff80aba6dea3212cff323cd4414675c08fb3e37974bb6c31cc5d9ce7` |
| `jthick_thcm1.bin` | Jthick diffusion scalar references | 6 | 3758 | `3e8ebdebbc94ac5a335d25dd0065c1722b47b297db061d710d0ffe4c36ab0615` |
| `lepton_increment_thcm1.bin` | Lepton-number increment source pairs | 99 | 37910 | `961747400d42f82f93894da95d2ff0453682c269f26912a91d5c2598575817e3` |
| `m1_thcm1_instantaneous_sources.bin` | Instantaneous frozen-rate interaction-source pairs (incl. 48 input-invariant controls) | 168 | 189648 | `b9af5ec09baca2dd30caed4c84d543b7ba4efe26e0f508a6ed6f8b49f1fab9c0` |
| `matter_increment_thcm1.bin` | Matter energy-momentum increment source pairs | 96 | 48290 | `e2d27a23873d9ceda9bc9625cdf98f567c02641b8856d7845e0f502068e8a630` |
| `number_update_thcm1.bin` | Radiation number-update source pairs | 96 | 63926 | `3c3ef454f6c8d536582cab6be86c0684d708b59d32c3e89d73c2285883f30235` |
| `physical_ef_flux_thcm1.bin` | Physical E/F flux source pairs | 96 | 71891 | `c8899e0c5c86245eeb6b25ebf64d567d18dfbc728d549fe6a76d7283900bf9ad` |
| `pointwise_closure_moments.bin` | Pointwise closure/moment pairs (incl. 25 input-invariant controls) | 56 | 128237 | `dd96fad7e6e9e1a6ca540c8ebd988146febb36e529246f245f259b3632262eb1` |
| `raw_rates_independent.bin` | Independent raw rate/provider references from the declared TEH78 approximation (169 direct-input + 2 injection-assisted records; not exact Fermi integrals) | 171 | 180981 | `e0b41666ce4db6619ae4dc37c3f5ff4c82343c8f93e81a1ad3f942bcbe0b3546` |
| `rusanov_generic.bin` | Generic Rusanov scalar flux pairs | 72 | 46456 | `05b74ab8b5f9eaae00a889de710f0f5f3d75b65b44d3ed7adb8a529c00cdc529` |
| `rusanov_neutrino.bin` | Neutrino Rusanov flux pairs | 72 | 91633 | `3dd3addb14de296cec309715f46e8fb0f1b45d7058b8fd64cfa320dae9a97eb8` |
| `rusanov_neutrino_current.bin` | Number-current Rusanov flux pairs (`_current_v2`) | 72 | 93020 | `21566f9eaa988ec4820dea1e3d0f94641d4aeb5b37b87cc437de27d9010c3f18` |
| `scalar_floors_thcm1.bin` | Scalar floor / common-representation pairs | 96 | 37935 | `f00df46b978880ac79c7906c531be895d81c86cc21ea669da1f141697915873a` |
| `source_relaxation_thcm1.bin` | Frozen-rate relaxation trajectory, recaptured via corrected THC caller | 7 | 5018 | `b1441613970f6c4d26ee02ae756e8f2693389fd265d843074506f05dfc55abae` |
| `source_relaxation_directed_thcm1.bin` | Directed relaxation states, corrected THC_M1 | 7 | 5340 | `27e286d60095595785b018204127e7ca9179fb40962445bf49b50dbfcd7fe23c` |
| `stress_energy.bin` | Covariant stress-energy paired grid references | 512 | 430681 | `3c4b63e97126a1098d3630c1749619f9b02ad8ae71a003db719e05ff177b4ea9` |
| `stress_energy_independent.bin` | Independent analytical-Minerbo stress-energy references | 512 | 432730 | `36f98c758ab85a801d8bb1eca33c893109f7b93b4c61e71650a644cb6eb99535` |
| `transport_four_point_d0.bin` | Constant-volume four-point transport prepared pairs, direction d0 | 58 | 83515 | `8583722ddde19e1e0bb42323d36f64659c5f525cd0f7ef1502637108f1728fb8` |
| `transport_four_point_d1.bin` | Constant-volume four-point transport prepared pairs, direction d1 | 63 | 90715 | `238b2258283b132a309a0d793943d9a2b996992c28f28cca175425e50b06eb24` |
| `transport_four_point_d2.bin` | Constant-volume four-point transport prepared pairs, direction d2 | 65 | 93614 | `f6dfc0a9099a0ad38dfcf402d16947015b4812bf038a47da8441d5b8fe235dd7` |
| `transport_four_point_varying_d0.bin` | Variable-volume prepared transport pairs, direction d0 | 58 | 86714 | `c6bb071991f814b403f6c92e506e6598585e88789127036c608bac00cc23a936` |
| `transport_four_point_varying_d1.bin` | Variable-volume prepared transport pairs, direction d1 | 63 | 94189 | `28ad7487c54aef02e61059ede907801f8ae566481dc24defb264219e11bfe0e2` |
| `transport_four_point_varying_d2.bin` | Variable-volume prepared transport pairs, direction d2 | 65 | 97198 | `f39e4f7c1c4ad9b0631e2ba4bdf13976f19636ae780f828f6bfa45deb2672c30` |

## Binary layout (`.bin` fixtures)

Each binary fixture uses the indexed-vector stream below. This specifies the
complete byte framing; it does not name every operation-specific vector entry.
Those semantic field orders are defined in the matching checkout's
`Unit_Tests/m1_helpers/m1_thcm1_*_fixture.{h,c}` and producer sources.
The authoritative stream reader is
`Unit_Tests/m1_helpers/m1_thcm1_fixture_utils.c` in that checkout.

- Header: 8-byte magic `M1THCMB1`; strings `operation`, `policy`;
  unsigned 64-bit little-endian `record_count`.
- Per record: strings `case_id`, `pair_id`, `origin`, `seed_id`, `family`,
  `perturbation`; two u64 integers `sensitivity_start`, `sensitivity_count`;
  five length-prefixed IEEE-754 binary64 vectors `baseline_input`,
  `perturbed_input`, `baseline_output`, `perturbed_output`, `normalization`;
  six u64 status fields `baseline_available`, `perturbed_available`,
  `baseline_status`, `perturbed_status`, `baseline_reference_status`,
  `perturbed_reference_status`.
- Strings are a u64 byte length then bytes (no terminator); vectors are a u64
  element count then that many binary64 elements, little-endian; there is no
  padding and no trailing data. Duplicate pair IDs, unusable statuses,
  truncated streams, and trailing bytes fail the loader; no consumer
  fabricates missing trusted outputs.
- Pair records are baseline/perturbed; records with
  `sensitivity_count == 0` are input-invariant controls, not
  perturbation-sensitivity evidence.

## Paired data files

These flat files use descriptive family prefixes. They contain inputs,
corrected THC captures, independent
references and refinement cohorts.
No executable code, command receipts, saved GRHayL replay results, or separate
campaign manifests are distributed here. Schemas, replay settings and capture
sidecars belong to the matching GRHayL test tooling, as do local test programs.
The source-owned mapping in `Unit_Tests/radiation_pairs/materialize.py`
reconstructs family directories only in temporary replay/regeneration output;
this TestData directory has no subdirectories.

Input packets are stored once even when multiple replay families use them.
The mapped-flux-factor replay reuses `pointwise_fixtures_seeded_packets.tsv`;
timestep refinement reuses `source_update_fixtures_roundoff_seeded_packets.tsv`.
The materializer supplies their family-relative aliases in scratch output.
Their reference answers remain separate because the diagnostic conventions
or timestep subdivisions differ. There are no duplicate physical input files
for these aliases in this directory.

Counts below describe serialized packets, pairs, component rows or reference
records, as labelled. They are not numbers of independent scientific tests or
the replay's scalar-comparison denominator. Captures can include diagnostics
that are not part of the raw numerical agreement tally. Unsupported reference
records retain their classifications rather than supplying invented answers.
The endpoint-recipe file includes a declared eight-step backward-Euler
diagnostic target; that reference does not select the production substep policy.
Likewise, retaining timestep-refinement data does not enable an adaptive
integrator in GRHayL.

| Member | Content | Records | Bytes | SHA-256 |
| --- | --- | --- | --- | --- |
| `kinetic_accuracy_reference.json` | Independent angular-transport solutions, including boosted equilibrium and anisotropic distributions | 12 cases | 15521 | `f4337e4d44c5ffc3c1de6ca09cfa13cef5b5e9f78233a9180917f948a2fe062a` |
| `pointwise_fixtures_seeded_packets.tsv` | Shared baseline/perturbed closure and mapped-flux-factor input packets | 256 packets | 275370 | `607ecdaebcea954549b80d79a5025a1b86bc88caecf532fa52e9dd7edadb0980` |
| `pointwise_fixtures_thc_reference.tsv` | Corrected THC closure/moment component captures, including diagnostics | 10240 rows | 573746 | `d3aa91f31c3e182815c9ef5ca0182c899f768ba17f376f4e544ec6938c62b59f` |
| `pointwise_fixtures_mapped_xi_thc_reference.tsv` | THC component captures with squared flux factor mapped to GRHayL convention | 10240 rows | 573632 | `9a48f7bd44500fa6249ffdc658ac0a470d78bd63df2d925612da9bdc40a8ca0a` |
| `pointwise_extended_fixtures_prepared_packets.tsv` | Extended input packets with corrected THC prepared-pressure operands | 256 packets | 594645 | `afe041ad6a346b01cb60adfa69d9115aaeebd576911070d9e278db8def04e7f8` |
| `pointwise_extended_fixtures_seeded_packets.tsv` | Original baseline/perturbed extended tensor/current/geometry inputs | 256 packets | 535471 | `c9521703a3e22a95efc30b9382f31aaae950de7e7ce7b38131a2da5f8ab89c21` |
| `pointwise_extended_fixtures_thc_reference.tsv` | Corrected THC extended component/status captures | 67584 rows | 5007662 | `214117df6b3e5423db1094817dde1c9fab4a47985b2d7c4b274a3eb11cd4cdaa` |
| `prepared_transport_fixtures_prepared_flux_inputs.csv` | Prepared four-point transport input pairs | 1584 pairs | 1397174 | `91a29078d58522bde610e855469b51b4cca788d751bd70e37e959afae6161cb8` |
| `prepared_transport_fixtures_thc_reference.csv` | THC prepared-transport reference pairs | 1584 pairs | 1000372 | `2db406988ba2ed34067ac1971c6020031018adea0d487d7ac2174d3fec956399` |
| `recipe_endpoint_accuracy_expected.json` | Independent boosted closure and eight-step moving backward-Euler targets, at two precisions | 2 closure + 2 endpoint cases | 29310 | `b567a4413c4df496c8b9b3b2447c85aabdb2376cd602b98bd1e9c78899f99ffe` |
| `scalar_sources.json` | Scalar number/lepton input pairs and THC endpoint answers | 768 pairs | 755833 | `fa29dc8425f13e5f5259d3ac0cdfc3bbd83d2acafef76f4da229254a573eeab3` |
| `source_update_fixtures_seeded_packets.tsv` | Original frozen-rate source/update input pairs | 448 packets | 464938 | `eccd380f837a6126bf0d2817a4acad4353b7b8cd5697534f8436a30688c0ae04` |
| `source_update_fixtures_thc_reference.tsv` | Corrected THC source/update component captures and diagnostics | 69888 rows | 4322312 | `b21e2f0189dfcda93f503e998c78dfac88cf60eafe539e3a692accfb880886ac` |
| `source_update_fixtures_roundoff_seeded_packets.tsv` | Roundoff source/update inputs, also shared by timestep refinement | 448 packets | 464958 | `4a239f764b270bb7103a0f148d816b98dca43c81035e43e80b0dba542742fb73` |
| `source_update_fixtures_roundoff_thc_reference.tsv` | Corrected THC source/update captures for the roundoff cohort | 69888 rows | 4322280 | `f47db60b78373fe3cf85378904b6d1149af4228c28819f4ddaea65b4bcaae7aa` |
| `speeds_floors_fixtures_seeded_packets.tsv` | Characteristic-speed, floor and limiter input pairs | 266 packets | 238738 | `7aeeb8daa30307f0b5b6f19370366228f5993e3a6eb9be045ea256026e619aab` |
| `speeds_floors_fixtures_thc_reference.tsv` | THC speed/floor component captures | 9044 rows | 516546 | `b57f1609f347a6704eadf4424793e5f484ba37d742e6a0967d0994dd0293ef91` |
| `time_refinement_fixtures_cohort.tsv` | Case/species/endpoint keys for the full refinement cohort | 2688 keys | 41026 | `d902a06a5ec9e4c032f001871c8cc4637c0fa98d493f66c82170ee663339f527` |
| `time_refinement_fixtures_thc_1.tsv` | Corrected THC endpoint captures after 1 equal timestep subdivisions | 2688 endpoints | 282639 | `bb16ab509a7e14fecb8e420833f25cd42cbd38b0a3512f67016b5c02719a7dce` |
| `time_refinement_fixtures_thc_128.tsv` | Corrected THC endpoint captures after 128 equal timestep subdivisions | 2688 endpoints | 297212 | `b237c91d8fd7d955a0291a674cd97b4f3ccef8f74dad6b82d77062baf2f981d2` |
| `time_refinement_fixtures_thc_16.tsv` | Corrected THC endpoint captures after 16 equal timestep subdivisions | 2688 endpoints | 290565 | `cdc081bd28347b73b48e63bbce9db82580f90fb220b5c03d26f16136b4006201` |
| `time_refinement_fixtures_thc_2.tsv` | Corrected THC endpoint captures after 2 equal timestep subdivisions | 2688 endpoints | 283653 | `2c332205fcae29dd2e7a11236f326c1a3eaa5a3cf5803c761e1bd08b660dfdff` |
| `time_refinement_fixtures_thc_32.tsv` | Corrected THC endpoint captures after 32 equal timestep subdivisions | 2688 endpoints | 290725 | `b692247e1eeaccca73f2e4a74e1d97f12f8825664199b44c71b4818a96453a87` |
| `time_refinement_fixtures_thc_4.tsv` | Corrected THC endpoint captures after 4 equal timestep subdivisions | 2688 endpoints | 283941 | `6f8d71bdc790aee8b520451540874b0c9522f0206d2ca00389d6c0c5e3941e3a` |
| `time_refinement_fixtures_thc_64.tsv` | Corrected THC endpoint captures after 64 equal timestep subdivisions | 2688 endpoints | 290864 | `a151c15a154db7714c624204814d12dd3a76e592c1f0917baded08d3d65a127e` |
| `time_refinement_fixtures_thc_8.tsv` | Corrected THC endpoint captures after 8 equal timestep subdivisions | 2688 endpoints | 284433 | `48002ccb166bb7ef482bb54d3648fb4e942fc41ffd3d6c9b8c272c831513992a` |
| `time_refinement_reference_cohort_cohort.tsv` | Keys for the separately classified continuum-reference cohort | 384 keys | 5974 | `08529582bfc978e2244ff8810de6f9cefd29fed25428621887ab153699d045e3` |
| `time_refinement_reference_cohort_references.json` | Continuum references and explicit unsupported-model classifications; not all keys rankable | 384 records | 235209 | `acb02aeb5be74537f4d19b17d04c7423fd13edd53d6eea6b4c5e2d7123f3d5d6` |

## Independent fixture families

The helper, collision, rate, source-contract and validation members contain
515 records: 504 direct-input records and 11 separately selected injection
records. The rate member contributes two injection records and the source
member nine; Linux interposition replay is never counted as direct coverage.
Public retained-face callback failures are declared caller-input contracts,
not ELF interposition. Full layouts and per-family counts are defined by the
matching `generate_*_fixtures.py` scripts and native consumers.

These five members do not use THC_M1 outputs as their expected answers. They
check independently evaluated equations, analytical special cases and declared
API contracts. The older THC comparison corpus remains alongside them,
unchanged by their addition. Moving collision finite differences test derivative
consistency rather than an independent physical solution; raw-rate answers
evaluate the declared TEH78 approximation, not exact Fermi-Dirac integrals.

Source initialization is 8 inputs / 14 outputs, ordinary source updates
92/55, and injected source updates 94/55. The eleven-component exchange
includes matter dYe. Validation uses 32 inputs; cone/norm outputs have length
2, retained-face outputs 8, floor outputs 3, and repair outputs 5.
Both endpoints are replayed. These fixtures do not imply complete solver-path
coverage or a code-wide accuracy ranking; moving collision finite differences
are consistency checks, not independent moving-fluid physical solutions.

## Replay

From the matching GRHayL checkout:

```sh
python3 -m pip install -r Unit_Tests/radiation_pairs/requirements.txt
M1_FIXTURE_DIR=/path/to/TestData/radiation \
M1_RESULTS_DIR=/path/to/results \
  .github/run_tests.sh m1 --noomp --disable-hdf5 --builddir=/path/to/build
```

- The runner materializes family-relative data beside repository-owned
  support code in a fresh results directory; it does not modify this
  directory. See `Unit_Tests/radiation_pairs/README.md` for replay recipes
  and result meanings.
- Without `M1_FIXTURE_DIR`, the runner downloads the payload members from
  `https://raw.githubusercontent.com/GRHayL/TestData/<revision>/radiation/`
  where `<revision>` is the full commit SHA pinned in `.github/radiation-testdata-ref`
  of the matching checkout. Missing members fail before replay; a
  caller-supplied complete directory bypasses downloads and stays
  caller-owned.

## Validation and regeneration

All commands below run from the matching GRHayL checkout, not TestData.
The metadata checker is read-only and validates actual payload bytes against
the complete inventory, record layouts/counts and data checksums. It also
rejects byte-identical data files under different names:

```sh
python3 Verification/Radiation/fixture_coverage/verify_payload.py \
  --fixture-dir /path/to/TestData/radiation
```

To reproduce a candidate, install the Python requirements above, a C++17
compiler, GSL development libraries and `pkg-config`, then run:

```sh
python3 Unit_Tests/radiation_pairs/corrected_reference.py \
  --fixtures /path/to/TestData/radiation \
  --output /path/to/new-regeneration
```

The output path must not exist. The command preserves the supplied corpus,
creates `payload/`, command logs and `replacement_manifest.json` below that
path, and refreshes candidate data checksums. It regenerates the corrected-THC
captures to which fixes apply and the five independent members; unaffected
members and already-corrected refinement captures are retained. This is not
fresh generation of every historical producer. Compare the ordinary data
files byte-for-byte and validate the candidate with the same checker before
considering replacement. The generator emits its independent-generation audit
in `corrected_reference_policy.json` beside, not inside, `payload/`. Retain
that receipt and regenerated capture sidecars as verification evidence in GRHayL.
Full original producer recipes and retained-generation limits are described in
`PROVENANCE.md` and `Unit_Tests/radiation_pairs/README.md` in GRHayL.

To retain compact evidence from a completed replay outside temporary build
storage, use the source-owned utility (the destination must be new):

```sh
python3 Unit_Tests/radiation_pairs/evidence.py \
  --run-dir /path/to/completed-results \
  --fixture-dir /path/to/TestData/radiation \
  --output-dir /path/to/retained-evidence
```

It checks the supplied operands against the saved runtime, identifies the
numerical data, and preserves command logs, receipts and family summaries.
Reused commands retain their actual earlier logs and are labelled reused.
The evidence belongs with GRHayL verification, not in this directory. It is
not a fresh replay or a complete source/build archive; mathematical ledgers
and stage dumps must be regenerated by replay when needed.

## Scope and evidence limits

Passing independent correctness checks is not a claim of universal raw
equality with THC_M1. Original comparisons and disagreements remain in result
ledgers; closure/model differences and known THC issues use independent
references, without changing the original perturbation acceptance rule. The
corrected reference fixes exact-rest arithmetic, lapse handling and the
implicit Jacobian; it does not harmonize closure/fallback models. The
producer's policy receipt and source-owned capture sidecars identify the
reference variant. Original pre-correction data are retained in the
matching GRHayL checkout's `Verification/Radiation/corrected_reference_migration/`.
See [PROVENANCE.md](PROVENANCE.md) for payload and migration provenance,
licensing/attribution caveats, and accuracy limits.

Interpret the replay's evidence in separate layers:

- **Cross-code agreement:** the original perturbation gate compares the
  recorded THC answer and the current GRHayL answer. Different closure or
  fallback policies can disagree without demonstrating an arithmetic error.
- **Equation and contract correctness:** independent analytical, rational or
  high-precision references check the declared equation or API behavior.
  A discrete-stage check verifies that stage, not the continuous-time solution;
  a number/lepton check using supplied E/F does not certify those upstream E/F.
- **Shared-target accuracy:** both codes must be compared with the same
  independently calculated target, initial physical state and final time.
  Only applicable shared-model cases support an accuracy ordering; a chosen
  closure is not proven physically superior merely by reproducing its formula.
- **Supplementary diagnostics:** symmetric own-condition and THC-method
  checks retain their own failure counts even when mandatory replay passes.
  In particular, forming lepton exchange from an already rounded number
  publication can differ from an unrounded number-to-lepton chain. Reproducing
  that boundary explains the local arithmetic; it does not erase upstream
  error or establish cross-code superiority. Other diagnostic failures must
  retain their evidence and unresolved causes.

The matching checkout's
`Verification/Radiation/disagreement_review/shared_accuracy/README.md`
documents the supplementary disagreement-accounting commands. Their reports
are generated evidence, not additional TestData fixtures. Timestep-refinement
tracks overlap the primary corpus and must not be added to its disagreement
denominator.
The same source-owned audit includes whole-applicable moving E/F controls
over passing as well as disagreeing records. They reuse the stored common
references and require verified input identity; no new fixture copies are
needed. Exact analytical moving special cases are separate local validation
controls, not substitutes for a missing shared trajectory reference.
