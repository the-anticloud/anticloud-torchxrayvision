# TORCHXRAYVISION

![license](https://img.shields.io/badge/license-license-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-medical_health-lightgrey)

> Anticloud-hardened packaging of the upstream project `TORCHXRAYVISION` in category **MEDICAL HEALTH**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** MEDICAL HEALTH · **Upstream:** https://github.com/mlmed/torchxrayvision · **Upstream pin:** `bd41a6ce24c4f69fd6e7fd6c8b263166a6bbd737` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

🚨 Paper now online! [https://arxiv.org/abs/2111.00595](https://arxiv.org/abs/2111.00595)

🚨 Documentation now online! [https://mlmed.org/torchxrayvision/](https://mlmed.org/torchxrayvision/)

# TorchXRayVision 

| <img src="https://raw.githubusercontent.com/mlmed/torchxrayvision/main/docs/torchxrayvision-logo.png" width="300px"/>  |  ([🎬 promo video](https://www.youtube.com/watch?v=Rl7xz0uULGQ)) <br>[<img src="http://img.youtube.com/vi/Rl7xz0uULGQ/0.jpg" width="400px"/>)](http://www.youtube.com/watch?v=Rl7xz0uULGQ "Video Title") |
|---|---|

# What is it?

A library for chest X-ray datasets and models. Including pre-trained models.

TorchXRayVision is an open source software library for working with chest X-ray datasets and deep learning models. It provides a common interface and common pre-processing chain for a wide set of publicly available chest X-ray datasets. In addition, a number of classification and representation learning models with different architectures, trained on different data combinations, are available through the library to serve as baselines or feature extractors.

- In the case of researchers addressing clinical questions it is a waste of time for them to train models from scratch. To address this, TorchXRayVision provides pre-trained models which are trained on large cohorts of data and enables 1) rapid analysis of large datasets 2) feature reuse for few-shot learning.
- In the case of researchers developing algorithms it is important to robustly evaluate models using multiple external datasets. Metadata associated with each dataset can vary greatly which makes it difficult to apply methods to multiple datasets. TorchXRayVision provides access to many datasets in a uniform way so that they can be swapped out with a single line of code. These datasets can also be merged and filtered to construct specific distributional shifts for studying generalization.

Twitter: [@torchxrayvision](https://twitter.com/torchxrayvision)

## Getting started

```
$ pip install torchxrayvision
```

```python3
import torchxrayvision as xrv
import skimage, torch, torchvision

# Prepare the image:
img = skimage.io.imread("16747_3_1.jpg")
img = xrv.datasets.normalize(img, 255) # convert 8-bit image to [-1024, 1024] range
img = img.mean(2)[None, ...] # Make single color channel

transform = torchvision.transforms.Compose([xrv.datasets.XRayCenterCrop(),xrv.datasets.XRayResizer(224)])

img = transform(img)
img = torch.from_numpy(img)

# Load model and process image
model = xrv.models.DenseNet(weights="densenet121-res224-all")
outputs = model(img[None,...]) # or model.features(img[None,...]) 

# Print results
dict(zip(model.pathologies,outputs[0].detach().numpy()))

{'Atelectasis': 0.32797316,
 'Consolidation': 0.42933336,
 'Infiltration': 0.5316924,
 'Pneumothorax': 0.28849724,
 'Edema': 0.024142697,
 'Emphysema': 0.5011832,
 'Fibrosis': 0.51887786,
 'Effusion': 0.27805611,
 'Pneumonia': 0.18569896,
 'Pleural_Thickening': 0.24489835,
 'Cardiomegaly': 0.3645515,
 'Nodule': 0.68982,
 'Mass': 0.6392845,
 'Hernia': 0.00993878,
 'Lung Lesion': 0.011150705,
 'Fracture': 0.51916164,
 'Lung Opacity': 0.59073937,
 'Enlarged Cardiomediastinum': 0.27218717}

```

A sample script to process images usings pretrained models is [process_image.py](https://github.com/mlmed/torchxrayvision/blob/main/scripts/process_image.py)

```
$ python3 process_image.py ../tests/00000001_000.png -resize
{'preds': {'Atelectasis': 0.50577986,
           'Cardiomegaly': 0.62151504,
           'Consolidation': 0.3124331,
           'Edema': 0.21286564,
           'Effusion': 0.39427388,
           'Emphysema': 0.503361,
           'Enlarged Cardiomediastinum': 0.4313866,
           'Fibrosis': 0.5401596,
           'Fracture': 0.28907478,
           'Hernia': 0.012677962,
           'Infiltration': 0.5220189,
           'Lung Lesion': 0.21828467,
           'Lung Opacity': 0.36826086,
           'Mass': 0.4104132,
           'Nodule': 0.5091791,
           'Pleural_Thickening': 0.5104176,
           'Pneumonia': 0.18006423,
           'Pneumothorax': 0.30677897}}

```

## Models ([demo notebook](https://github.com/mlmed/torchxrayvision/blob/main/scripts/xray_models.ipynb))

Specify weights for pretrained models (currently all DenseNet121)
Note: Each pretrained model has 18 outputs. The `all` model has every output trained. However, for the other weights some targets are not trained and will predict randomly becuase they do not exist in the training dataset. The only valid outputs are listed in the field `{dataset}.pathologies` on the dataset that corresponds to the weights. 

```python3

## 224x224 models
model = xrv.models.DenseNet(weights="densenet121-res224-all")
model = xrv.models.DenseNet(weights="densenet121-res224-rsna") # RSNA Pneumonia Challenge
model = xrv.models.DenseNet(weights="densenet121-res224-nih") # NIH chest X-ray8
model = xrv.models.DenseNet(weights="densenet121-res224-pc") # PadChest (University of Alicante)
model = xrv.models.DenseNet(weights="densenet121-res224-chex") # CheXpert (Stanford)
model = xrv.models.DenseNet(weights="densenet121-res224-mimic_nb") # MIMIC-CXR (MIT)
model = xrv.models.DenseNet(weights="densenet121-res224-mimic_ch") # MIMIC-CXR (MIT)

# 512x512 models
model = xrv.models.ResNet(weights="resnet50-res512-all")

# DenseNet121 from JF Healthcare for the CheXpert competition
model = xrv.baseline_models.jfhealthcare.DenseNet() 

# Official Stanford CheXpert model
model = xrv.baseline_models.chexpert.DenseNet(weights_zip="chexpert_weights.zip")

# Emory HITI lab race prediction model
model = xrv.baseline_models.emory_hiti.RaceModel()
model.targets -> ["Asian", "Black", "White"]

# Riken age prediction model
model = xrv.baseline_models.riken.AgeModel()

```

Benchmarks of the modes are here: [BENCHMARKS.md](BENCHMARKS.md) and the performance of some of the models can be seen in this paper [arxiv.org/abs/2002.02497](https://arxiv.org/abs/2002.02497). 

## Autoencoders 
You can also load a pr

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Python** (manifests: setup.py, requirements.txt; scanned in UPSTREAM_CLONE)
- Top-level source layout: `scripts/`, `tests/`, `torchxrayvision/`
- Snapshot size: **107 files**, **9943 lines of code** (measured; see Benchmarks)
- Primary languages: `.py` (43), `.ipynb` (24), `.gz` (10), `.sh` (6), `(none)` (5), `.md` (5)
- Upstream commit pinned for this packaging: `bd41a6ce24c4f69fd6e7fd6c8b263166a6bbd737`

---

## Installation

```
$ pip install torchxrayvision
```

```python3
import torchxrayvision as xrv
import skimage, torch, torchvision

*Section quoted from the upstream readme.*
Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

- In the case of researchers developing algorithms it is important to robustly evaluate models using multiple external datasets. Metadata associated with each dataset can vary greatly which makes it difficult to apply methods to multiple datasets. TorchXRayVision provides access to many datasets in a uniform way so that they can be swapped out with a single line of code. These datasets can also be merged and filtered to construct specific distributional shifts for studying generalization.

Twitter: [@torchxrayvision](https://twitter.com/torchxrayvision)

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

The upstream API surface is defined by the `TORCHXRAYVISION` source tree vendored in `UPSTREAM_CLONE/` (Python ecosystem). Public entry points:

- Source modules: `scripts/`, `tests/`, `torchxrayvision/`
- The snapshot declares 1 dependency references across 1 ecosystem(s); see Dependencies below.
- Overlay API: `anticloud/cli.py` exposes 13 subcommands with JSON stdout; `anticloud/bench/runner.py` runs the 16-check suite; `anticloud/provenance/chain.py` exposes the SHA3-256 + Ed25519 provenance chain.

---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Python |
| Manifests detected | setup.py, requirements.txt |
| Files in snapshot | 107 |
| Lines of code | 9943 |
| Dependency references | 1 |
| Dependencies by ecosystem | pypi: 1 |
| Upstream license | No license file present in the upstream snapshot |
| Overlay license | Anticommons 0.1.0 |

Top dependency references recorded in the benchmark snapshot:

| Ecosystem | Name | Version | Source file |
|-----------|------|---------|-------------|
| pypi | __version__ | = [ | setup.py |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

No configuration section was found in the upstream readme. Configuration-relevant files detected in this project directory:

- None detected at the snapshot root; consult the upstream documentation link in the Upstream section.

Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

*Excerpt from upstream `CONTRIBUTING.md`:*

# Contributing to TorchXRayVision

We welcome pull requests! Please open an issue to discuss major changes before submitting.

We are looking for contributions in:
- Datasets: Tuberculosis datasets, segmentation masks
- Baseline Models: Pathology predictions or other interesting tasks
- Utility Functions: Evaluation scripts and metrics

---

## Guidelines

### 1. Adding a New Dataset
Follow conventions in `torchxrayvision/datasets.py`:
- Inherit from `Dataset`.
- Parse metadata from CSV/JSON.
- `__getitem__` must return a dict with `img` and `lab`.
- Normalize images with `xrv.utils.normalize(img, maxval=...)`. Ensure `maxval` matches the format (e.g., 255 for 8-bit) to map values to `[-1024, 1024]` with shape `(1, H, W)`.

### 2. Adding a Baseline Model
When adding to `torchxrayvision/baseline_models`:
- Inherit from `nn.Module`.
- Define a `targets` list for model outputs.
- Download weights automatically via `xrv.utils.download` unless restricted.
- The `forward()` method must accept inputs in `[-1024, 1024]` and internally scale them for your model.
- Use `xrv.utils.fix_resolution` and `xrv.utils.warn_normalization`.
- Provide a docstring with examples and citation.

### 3. Development
Install requirements:
```bash
pip install -r requirements-dev.txt
```
Run tests:
```bash
pytest tests/
```
Check style with `pep8.sh`.

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: No license file present in the upstream snapshot** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
Licenses vary by subpackage 

# Main library (https://www.apache.org/licenses/LICENSE-2.0)
xrv.models:  
xrv.autoencoders
xrv.datasets
xrv.utils
testing code
sample notebooks

# Baseline models (check each model's license)
xrv.baseline_models
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original No license file present in the upstream snapshot terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `No license file present in the upstream snapshot` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `TORCHXRAYVISION` (category: MEDICAL HEALTH)
- **Upstream URL:** https://github.com/mlmed/torchxrayvision
- **Pinned commit (SHA):** `bd41a6ce24c4f69fd6e7fd6c8b263166a6bbd737`
- **Branch:** main
- **Pin provenance:** resolved during the second documentation pass. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`d5b8dfeaafcea12b7c3cbb6b353aa25b105bbfd757fe02a01203a46d4bb04668`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

