# Third-party software, models and data

The MIT licence in `LICENSE` covers **only the original code and text in this repository** (the notebooks, results tables and documentation). It does **not** relicense anything below. The notebooks download these components at run time; none are redistributed here.

| Component | Used for | Licence / terms |
|---|---|---|
| [Roblox/cube](https://github.com/Roblox/cube) code and `Roblox/cube3d-v0.5` weights (Hugging Face) | Experiment A | Roblox "Cube3D Research-Only RAIL-MS" licence. The permitted purpose is academic or research use only. Read the `LICENSE` in the Roblox repo and the model card for the exact version you use before doing anything beyond coursework. |
| [VAST-AI-Research/TripoSR](https://github.com/VAST-AI-Research/TripoSR) code and `stabilityai/TripoSR` weights | Experiment B | MIT, Copyright (c) 2024 Tripo AI & Stability AI. |
| PyTorch, Transformers, trimesh, scikit-image, rembg, plotly, etc. | Both | Their own open-source licences. |

## Modification of TripoSR code

Notebook 02 overwrites TripoSR's `tsr/models/isosurface.py` at run time with a version that swaps the `torchmcubes` call for `scikit-image` Marching Cubes. That patch is derived from TripoSR's MIT-licensed file, so TripoSR's copyright and permission notice (above) applies to it. The neural network and weights are unchanged.

## Input images and outputs

- The three images used as TripoSR inputs in the original run are **not** included, because their redistribution rights have not been cleared. Use your own images or ones you are licensed to use.
- Generated meshes are not included. Outputs of the Cube model remain subject to the Roblox licence terms above.
