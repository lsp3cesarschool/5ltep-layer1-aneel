# 5LTEP-L1 · ANEEL instance (control experiment)

[![Tests](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/actions/workflows/tests.yml/badge.svg)](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/actions/workflows/tests.yml) [![Layer 1](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer1-aneel%2Fmain%2Fdocs%2Fdata%2Fstatus.json)](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/actions/workflows/layer1.yml) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**English** · [Português](LEIAME.md)

**5L-TEP Layer 1 (structural contracts) applied to the open data portal of ANEEL, Brazil's electricity
regulator: a second instance of [5ltep-layer1](https://github.com/lsp3cesarschool/5ltep-layer1), set up
by the author as a control case for the IBAMA study.**

| Resource | What you find there |
|---|---|
| 📊 **Dashboard** | [lsp3cesarschool.github.io/5ltep-layer1-aneel](https://lsp3cesarschool.github.io/5ltep-layer1-aneel/?lang=en): maturity, documentation findings, every dataset and file |
| 🔀 **Schema drift** | [![drift issues](https://img.shields.io/github/issues/lsp3cesarschool/5ltep-layer1-aneel/layer1?label=drift%20issues&color=0366d6)](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/issues?q=is%3Aissue+label%3Alayer1) |
| 🧑‍⚖️ **Suggested schemas** | [pull requests](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/pulls?q=is%3Apr+schemas+suggested) with schemas extracted from PDF dictionaries, waiting for a person |
| 🏛️ **Main instance** | [5ltep-layer1](https://github.com/lsp3cesarschool/5ltep-layer1): IBAMA, and the full documentation |
| 🔁 **Other control** | [5ltep-layer1-recife](https://github.com/lsp3cesarschool/5ltep-layer1-recife): the Recife city portal |

> **Status: research demonstration.** This repository is not operated by, affiliated with or endorsed
> by ANEEL; it only reads ANEEL's open data. It shows that the toolkit can be reused on another portal.
> It does not assume that ANEEL will review its results or adopt it. Suggested schemas and drift
> issues demonstrate the flow; the author does not act as their reviewer.

## Use case in one paragraph

ANEEL publishes its open data on a CKAN portal, almost every dataset with a data dictionary. Suppose
someone who reuses these data wants to know, before trusting a file, what it should contain and
whether it does. This instance reads every dictionary the portal publishes, turns the PDF ones into
machine-readable schemas (first by reading their tables, then with a local AI model, and always with
people deciding what the file's own header does not confirm), and checks every published CSV against
its schema, week after week.

## Why a control experiment

The Layer 1 toolkit was built on IBAMA's portal. A method that works on one portal may only have
learned that portal's habits. This repository runs **the same code** on another agency and portal,
following the steps of *Running your own instance* of the main README: only `portal.json` (the
portal's URL) and the texts of this README changed. Where ANEEL's documentation differs from IBAMA's,
the difference shows up in the results, not in the code.

## How ANEEL documents its data

As observed when this instance was set up (01/10/2026); the dashboard has the current numbers.

- Dictionaries are almost always **PDF** files that follow one template ("Dicionário de Metadados do
  Conjunto de Dados"), one per dataset, with a table of field name, type, size and description. A PDF
  is readable only by people (level 1), so this instance depends on the PDF stages: deterministic
  extraction, the local LLM where needed, and pull requests for review.
- Many CSV files are loaded in the CKAN **DataStore**, but every field is typed `text`: the API exposes
  the columns, not their types, so the DataStore does not raise the maturity level.
- Data are also published as Parquet and ZIP; only CSV (and zips holding CSV) are validated.

## What changed from the main instance

| File | Change |
|---|---|
| `portal.json` | `portal_url` = `https://dadosabertos.aneel.gov.br` |
| `README.md`, `LEIAME.md`, `CITATION.cff` | this text and the citation of this repository |

Everything else is the code of `5ltep-layer1` at commit `7efb995`
([7efb995e748e8523396644d5833e7e9c326af678](https://github.com/lsp3cesarschool/5ltep-layer1/commit/7efb995e748e8523396644d5833e7e9c326af678)).

## Running it, and adapting it again

The workflow *5L-TEP Layer 1 Structural Contracts* runs every Monday and can be started by hand
(*Actions → Run workflow*). Locally:

```bash
pip install -r requirements.txt
python main.py run --minutes 10
```

To point it at yet another CKAN portal, change `portal_url` in `portal.json`; see the main README.

## Reproducibility

Each result records the SHA-256 of the file it validated, the fingerprint of the schema, the method
parameters and, for PDF extractions, the model, its digest and the prompt version. The code is the
upstream commit above; the schemas and results are committed by the workflow.

## Limitations

The limitations of the main instance apply, including its [size and time limits](https://github.com/lsp3cesarschool/5ltep-layer1#size-and-time-limits) (what GitHub and the system accept). Specific to ANEEL: PDF dictionaries whose tables do not
follow the template, or that have no text layer, depend on the LLM stage or stay at level 1.

## Documentation and references

The full documentation (maturity scale, readers, validation, PDF stages, drift, findings, security,
configuration and references) is in the [main repository](https://github.com/lsp3cesarschool/5ltep-layer1#readme).

## License

MIT for the code ([LICENSE](LICENSE)). ANEEL's data are published under the Open Database License
(ODbL); this repository stores only schemas and aggregate results derived from them.
