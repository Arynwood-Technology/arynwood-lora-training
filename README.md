# Cross-Checkpoint Portability of Subject-Identity LoRA Fine-Tunes in SDXL

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21282393.svg)](https://doi.org/10.5281/zenodo.21282393)

A technical study of subject-identity LoRA portability across SDXL checkpoints.

The study asks whether a subject-identity LoRA trained against
one SDXL-derivative checkpoint (Juggernaut XL) transfers to other SDXL
derivatives (e.g. DreamShaper XL Turbo) without retraining, and why it
architecturally cannot transfer to SD1.5.

**Read the write-up:** [arynwood.com/research/sdxl-lora-portability](https://arynwood.com/research/sdxl-lora-portability/)

Full write-up: [docs/methodology.md](docs/methodology.md)

## Contents

- `scripts/prepare_lora_dataset.py` — dataset preprocessing + WD14 auto-captioning
- `scripts/run_lora_training.py` — kohya_ss `sdxl_train_network.py` wrapper used for the training run
- `docs/methodology.md` — dataset, training config, and the cross-checkpoint portability analysis
- `weights/` — pointer to the two trained `.safetensors` files (hosted on Zenodo, not in git — see [`weights/README.md`](weights/README.md))

Training images are not included (personal photo dataset).

## Reproducing

This reproduces the pipeline, not the published weights: the personal photo dataset is not public.
Both scripts hardcode `KOHYA_SS_DIR = /home/lorelei/tools/kohya_ss`; edit that constant in
both scripts to your own kohya_ss checkout **before running**. The scripts expect kohya_ss's
virtual environment and SDXL training dependencies to be installed there.
TODO(Lorelei): record the kohya_ss version used for the published run.

From this repository root, substituting your own paths:

```bash
python3 scripts/prepare_lora_dataset.py <photos> --trigger-word Arynwood --repeats 10 --output-root <dir> --resolution 1024
python3 scripts/run_lora_training.py --base_model <path>/Juggernaut-XL-v9.safetensors --train_data_dir <dir> --output_dir <out> --output_name Arynwood_lora --resolution 1024 --network_dim 32 --network_alpha 16 --train_batch_size 1 --max_train_epochs 10 --learning_rate 1e-4
```

Replace angle-bracket placeholders before executing: they are path notation, not shell syntax.
Use the generated dataset parent containing `10_Arynwood` as the training data directory.
These options match the scripts' argparse and [methodology](docs/methodology.md).
There are no automated tests or workflows; these commands run actual preparation and GPU training.

## Citing this work

See [CITATION.cff](CITATION.cff). Licensed under [CC-BY-4.0](LICENSE).

Author ORCID iD: [0009-0007-8723-2857](https://orcid.org/0009-0007-8723-2857)
— set in both `CITATION.cff` and `.zenodo.json`, so a published DOI will
link back to the ORCID profile as a "work" automatically.

## Releases & DOIs

TODO(Lorelei): reconcile GitHub’s latest release 0.1.1 with the v0.2.0 DOI label below;
they currently disagree. This page does not treat the DOI label as a GitHub release.

GitHub detects the abbreviated CC BY 4.0 LICENSE as Other.
TODO(Lorelei): decide whether to include the verbatim legal code; the license file is unchanged.

This repository is connected to Zenodo's GitHub integration, which is
release-triggered rather than push-triggered: a DOI is minted only when a
GitHub Release is published here, never automatically on a commit.

- **Concept DOI** (always resolves to the latest version, use this one for
  citing the project generally): [10.5281/zenodo.21282393](https://doi.org/10.5281/zenodo.21282393)
- **Version DOI labeled v0.2.0**: [10.5281/zenodo.21282597](https://doi.org/10.5281/zenodo.21282597)

v0.1.1 was code/methodology only. v0.2.0 adds both trained LoRA checkpoints
(Juggernaut XL and DreamShaper XL Turbo) as direct uploads to the Zenodo
deposit — see [`weights/README.md`](weights/README.md) for file details and
checksums. As with the code, this was a manual, deliberate publish, not
something that happens automatically on push.

Publishing a further version: draft a GitHub Release (tagged, e.g.
`v0.3.0`) and publish it — Zenodo archives the repository at that tag and
adds a new version under the same concept DOI. Note that a GitHub Release
only archives tracked git content; any additional large files (like the
weights) still need a separate manual upload directly to that version's
Zenodo deposit.

## Scope

This repository contains the LoRA research artifact only — training
scripts, configuration, and methodology. It does not include the private
platform this pipeline was originally developed inside, nor the personal
photo dataset used for training.

## How this is built

I decide the scope and architecture, review changes, run the available checks and ship releases.
Claude Code and Codex work with me as pair programmers; some assisted commits carry
co-author trailers, and others do not. [How we build](https://github.com/Arynwood-Technology/.github/blob/main/HOW-WE-BUILD.md).
