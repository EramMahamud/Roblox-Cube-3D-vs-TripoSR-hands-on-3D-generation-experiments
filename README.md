# Roblox Cube 3D vs TripoSR: hands-on 3D generation experiments

Two small, reproducible Google Colab experiments (single NVIDIA Tesla T4, 14.56 GB VRAM) written for the Macquarie University COMP8410 *Generative AI System Case Study*. The case study analyses Roblox's GenerationService and its Cube 3D foundation model. These notebooks supply the hands-on evidence.

| | Experiment A: Cube 3D v0.5 | Experiment B: TripoSR |
|---|---|---|
| Task | text + bounding box → mesh | one RGB image → mesh |
| Paradigm | autoregressive generation of discrete shape tokens (VQ-VAE) | feed-forward transformer → triplane NeRF |
| Notebook | [`01_experiment_a_cube3d_v0.5.ipynb`](notebooks/01_experiment_a_cube3d_v0.5.ipynb) | [`02_experiment_b_triposr.ipynb`](notebooks/02_experiment_b_triposr.ipynb) |
| Open in Colab | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_GITHUB_USERNAME/comp8410-roblox-cube3d-triposr/blob/main/notebooks/01_experiment_a_cube3d_v0.5.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_GITHUB_USERNAME/comp8410-roblox-cube3d-triposr/blob/main/notebooks/02_experiment_b_triposr.ipynb) |

> Replace `YOUR_GITHUB_USERNAME` in the badge links after you push (a find-and-replace in this file).

## Results at a glance

Single run, single seed, three inputs each. Indicative only. Full tables are in [`results/`](results/).

| | Cube 3D v0.5 (res-base 4.0) | TripoSR (192³ grid) |
|---|---|---|
| Time per mesh | 191.75–316.94 s | 1.91–3.54 s |
| Vertices / faces | ~400 / ~780 | 27,913–45,050 / 55,838–90,088 |
| Peak GPU memory | not measured | 1.89 GB (allocated) |
| Watertight | 3 of 3 | 3 of 3 |
| Connected components | 3–5 | 2–13 |

**Main observation:** every mesh was watertight yet contained detached fragments, so a watertightness check alone is not a sufficient quality gate for generated 3D assets. Both systems extract a surface from an implicit field with Marching Cubes. Cube's slower time reflects serial decoding of 1,024 shape tokens (about 8.9 tokens/s in the log); TripoSR reconstructs in one forward pass but can only infer what the image does not show.

## Repository layout

```
.
├── notebooks/
│   ├── 01_experiment_a_cube3d_v0.5.ipynb   # Cube 3D: 3 text prompts with bounding boxes
│   └── 02_experiment_b_triposr.ipynb       # TripoSR: 2–3 uploaded images
├── results/                                # CSV measurements + notes
├── requirements-cube.txt                   # reference version pins (documentation)
├── requirements-triposr.txt
├── NOTICE.md                               # third-party licences and what is NOT included
├── LICENSE                                 # MIT, original code only
└── README.md
```

Notebook outputs are cleared so the repository stays small and renders on GitHub. Run the notebooks to regenerate them.

## How to run

Both notebooks are written for Google Colab. Use **Runtime → Change runtime type → T4 GPU**, then run the cells in order.

**Experiment A (Cube 3D)**
- Clones `Roblox/cube`, installs `transformers==5.16.1` and `huggingface-hub==1.33.0`, and downloads about 7.7 GB of `Roblox/cube3d-v0.5` weights.
- Do not upgrade `huggingface-hub` to 2.x; it conflicts with this Transformers build.
- Uses the normal engine (not `--fast-inference`) at `resolution-base 4.0` to fit a T4. Allow roughly 15 minutes.

**Experiment B (TripoSR)**
- Clones `VAST-AI-Research/TripoSR` and builds an isolated Python 3.11 virtual environment, because Colab's Python 3.13 breaks TripoSR's older dependencies.
- Replaces only TripoSR's `torchmcubes` helper with scikit-image Marching Cubes; the network and `stabilityai/TripoSR` weights are unchanged.
- Asks you to upload 2–3 single-object images. **Use your own or properly licensed images** and record their source and licence. The original test images are not included here.
- Mesh extraction with the scikit-image helper runs on the CPU and may order axes differently from the reference code. This affects exported orientation, not counts, topology or timing.

## Limitations

- Three inputs, one run and one seed per experiment, so no confidence intervals.
- The systems solve different tasks (text-to-shape vs image-to-mesh), so mesh quality is not directly comparable.
- Cube was run at a deliberately low resolution on a T4, and its GPU memory was not measured.
- Results are consistent with, but do not prove, the architectural explanations given for them.

## Licence and third-party terms

Original code and text: MIT (see [`LICENSE`](LICENSE)). Cube 3D code and weights are under Roblox's **research-only** RAIL-MS licence; TripoSR is MIT. See [`NOTICE.md`](NOTICE.md) for details and for what this repository deliberately does not redistribute (weights, meshes, input images).

## References

- Foundation AI Team Roblox. (2025). *Cube: A Roblox view of 3D intelligence*. arXiv. https://arxiv.org/abs/2503.15475
- Tochilkin, D., Pankratz, D., Liu, Z., Huang, Z., Letts, A., Li, Y., Liang, D., Laforte, C., Jampani, V., & Cao, Y.-P. (2024). *TripoSR: Fast 3D object reconstruction from a single image*. arXiv. https://arxiv.org/abs/2403.02151
- Roblox. (2025). *Cube* [Computer software]. GitHub. https://github.com/Roblox/cube
- VAST-AI-Research. (2024). *TripoSR* [Computer software]. GitHub. https://github.com/VAST-AI-Research/TripoSR

## Academic-integrity note

This code supports a university assessment in which generative-AI use was permitted subject to disclosure. AI tools helped with parts of the notebooks and write-up; see the AI Use Declaration in the submitted report for details. Reuse for your own coursework should follow your institution's policies.
