# Predictive State Quotients and Recurrence

This repository studies exact recurrence and state reconstruction under incomplete observation. Its main question is:

> When does an observation contain enough information to determine the complete future of a deterministic system?

A repeated complete state fixes the subsequent future. A repeated density field, image, conserved quantity, or position snapshot may not. The project combines analytical results, exhaustive finite-state enumeration, and reproducible numerical examples for reversible cellular automata, lattice gases, exact hard-particle dynamics, irrational torus flow, Poincare recurrence, and quantum recurrence.

## Principal HPP result

The full four-particle, zero-momentum sector of the `3x3` reversible HPP lattice gas contains 9,153 microscopic states. Exact partition refinement under site-density observation gives:

```text
Present-density classes:       495
Predictive classes:          9,126
Predictive singletons:       9,099
Predictive doubletons:          27
Ambiguous microscopic states:   54
```

The exceptional set has the exact structure

\[
54=18\times3=9\times2\times3.
\]

- 18 distinct microscopic cycles have least period 3.
- The cycles form 9 time-reversal pairs.
- Each pair contains 2 different microscopic time orientations.
- Each cycle has 3 aligned phases.
- The 9 pairs therefore produce 27 predictive doubletons containing 54 states.

Density sees where particles are but not which way they are moving. For these special period-three trajectories, the forward and velocity-reversed microscopic movies cast the same density shadow forever.

The analytical theorem explains the mechanism. The exhaustive computation discovers and certifies that these 54 states are the complete exceptional set in the stated sector.

## Repository contents

- [`src/recurrence_dynamics/`](src/recurrence_dynamics/) - exact model implementations and finite-state orbit utilities.
- [`tests/`](tests/) - unit, reversibility, exhaustive HPP, catalog, and integrity tests.
- [`scripts/validate.py`](scripts/validate.py) - deterministic generation and repository validation entry point.
- [`scripts/verify_ambiguity_certificate.py`](scripts/verify_ambiguity_certificate.py) - independent self-contained HPP verifier.
- [`data/square_lattice_gas/ambiguity_cycles.csv`](data/square_lattice_gas/ambiguity_cycles.csv) - machine-checkable 54-state certificate.
- [`data/square_lattice_gas/ambiguity_cycle_pair.png`](data/square_lattice_gas/ambiguity_cycle_pair.png) - aligned paired-cycle figure.
- [`paper/exact_and_near_recurrence.tex`](paper/exact_and_near_recurrence.tex) - manuscript source.
- [`paper/exact_and_near_recurrence.pdf`](paper/exact_and_near_recurrence.pdf) - compiled manuscript.
- [`docs/mathematical_framework.md`](docs/mathematical_framework.md) - definitions and theorem summary.
- [`docs/reproducibility.md`](docs/reproducibility.md) - detailed reproducibility procedure.
- [`MANIFEST.sha256`](MANIFEST.sha256) - integrity manifest for committed files.
- [`LICENSE.md`](LICENSE.md) - split-license scope and attribution terms.
- [`LICENSES/`](LICENSES/) - complete Apache-2.0 and CC-BY-4.0 license texts.

## Supported environment

The repository is tested with CPython 3.13. The package metadata requires `>=3.13,<3.14`.

Validated Python dependencies:

- matplotlib 3.10.8
- Pillow 12.2.0
- pytest 9.0.2 for testing
- setuptools 80.9.0 for the editable build

The paper build was tested with pdfTeX 1.40.26 from TeX Live 2025.

## Installation

From the repository root:

```bash
python -m venv .venv
. .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt -r requirements-dev.txt
python -m pip install -e . --no-deps
```

On Windows, activate with `.venv\Scripts\activate`.

## Run the tests

```bash
python -m pytest
```

Expected result:

```text
25 passed
```

The exact pass count may increase when tests are added, but there must be no failures or skipped scientific checks.

## Verify the ambiguity certificate independently

```bash
python scripts/verify_ambiguity_certificate.py
```

Expected successful output:

```text
Sector states: 9153
Predictive singletons: 9099
Predictive doubletons: 27
Ambiguous states: 54
Period-3 cycles: 18
Time-reversal pairs: 9
Validation: PASS
```

The verifier does not import the project package or the high-level classification code. It independently defines the local HPP collision, streaming, inverse, time reversal, sector enumeration, and predictive partition refinement, then checks the committed CSV certificate.

## Regenerate data and figures

Regenerate every deterministic CSV and PNG under `data/`, then refresh the manuscript figures:

```bash
python scripts/validate.py reproduce
```

Successful completion prints:

```text
Data and figures regenerated from source code.
```

This command regenerates the exact HPP ambiguity catalog and paired-cycle figure as well as the other numerical studies.

## Validate exact claims

```bash
python scripts/validate.py data
```

Expected final lines:

```text
Exact predictive-state results validated.
Data validation passed.
```

## Check clean regeneration

Regenerate deterministic outputs in a temporary directory and compare them with the committed files:

```bash
python scripts/validate.py reproduction
```

Expected output:

```text
Independent reproduction matched all committed tables and figures.
```

CSV files are compared byte-for-byte. PNG files are stripped of textual metadata and compared by pixel content with a strict tolerance.

## Rebuild the manuscript

With `pdflatex` available:

```bash
python scripts/validate.py paper
```

The command compiles [`paper/exact_and_near_recurrence.tex`](paper/exact_and_near_recurrence.tex) twice and writes [`paper/exact_and_near_recurrence.pdf`](paper/exact_and_near_recurrence.pdf). Build-time metadata is suppressed and temporary TeX files are removed.

## Validate repository hygiene

```bash
python scripts/validate.py repository
```

This checks relative links, figure paths, line endings, temporary files, nested archives, local paths, PNG text fields, and PDF metadata.

## Check file integrity

```bash
python scripts/validate.py integrity
```

To rewrite the manifest after an intentional, validated change:

```bash
python scripts/validate.py manifest
```

Do not update the manifest merely to conceal an unexplained difference.

## Complete local verification

```bash
python scripts/validate.py all
```

This runs the test suite, independent certificate verifier, clean regeneration comparison, exact-data checks, manuscript build, repository hygiene validation, and integrity validation.

## Continuous integration

Routine pushes and pull requests run the test suite, independent certificate verifier, exact-data validation, repository hygiene checks, and manifest verification through [`.github/workflows/tests.yml`](.github/workflows/tests.yml).

The clean regeneration comparison is intentionally excluded from routine CI because it reruns every deterministic experiment and can be computationally expensive. [`.github/workflows/reproduction.yml`](.github/workflows/reproduction.yml) runs that check only when manually dispatched or when a GitHub release is published, with a 90-minute job timeout.

## Certificate format

[`ambiguity_cycles.csv`](data/square_lattice_gas/ambiguity_cycles.csv) contains one row for each exceptional microscopic state. Each row records:

- pair, cycle, and aligned phase identifiers;
- fixed-width state and velocity-reversed encodings;
- site-density encoding;
- successor and predecessor encodings;
- velocity-reversal partner phase;
- observation-word reversal shift;
- least period;
- active collision count.

The verifier rejects missing rows, duplicate assignments, invalid successors or predecessors, wrong periods, collision sites, incomplete reversal pairs, and any mismatch between the catalog and the independently recomputed predictive doubletons.

## Reproducibility model

Authoritative source files are the Python model implementations, validation scripts, tests, LaTeX source, and fixed experiment parameters. Generated scientific data are committed under `data/`. Generated manuscript figures are copied into `paper/figures/`. The PDF is a compiled output.

The exhaustive HPP claim is computer-assisted but finite and exact: every state in the stated sector is enumerated, no sampling is used, and the independent verifier reconstructs the classification from lower-level definitions.

## Scope and limitations

- The exhaustive HPP classification applies only to the `3x3`, four-particle, zero-momentum sector and the site-density observation map.
- Observational indistinguishability under density is not physical identity and does not imply indistinguishability under richer measurements.
- The lattice models are mathematical test systems, not proposed microscopic laws of nature.
- Finite-time Hamming damage is not a Lyapunov exponent.
- A capped cycle search is censored, not evidence of nonrecurrence.
- Poincare neighborhood recurrence and quantum near recurrence are distinct from exact finite-state recurrence.

## Citation

Citation metadata are provided in [`CITATION.cff`](CITATION.cff). No DOI or publication status is claimed.

## License

This repository uses a split license:

- software is licensed under the [Apache License 2.0](LICENSES/Apache-2.0.txt);
- the manuscript, documentation, figures, and generated data are licensed under [Creative Commons Attribution 4.0 International](LICENSES/CC-BY-4.0.txt).

See [`LICENSE.md`](LICENSE.md) for the exact scope, attribution requirements, and treatment of mixed files.
