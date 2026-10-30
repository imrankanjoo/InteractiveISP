# interactiveISP

**interactiveISP** is an interactive RAW image-signal-processing research tool
for inspecting ISP stages, comparing tone-mapping operators (TMOs), and
analyzing image-quality metrics (IQMs). It is built with **PySide6** and
**PyTorch** and is designed to keep processing data separate from display-only
inspection controls.

> Current research build: **v6.9 — tabbed IQM / modular TMO architecture**

## Highlights

- Inspect the RAW pipeline stage-by-stage: decompanding, optional BLC, RAW/RGB
  denoising, demosaic, highlight tools, white balance, CCM, tone mapping, and
  sRGB.
- Independent gray/green stage bypass switches without changing the inspected
  stage.
- Faithful intermediate-stage display plus explicit display-bit/window controls
  for exploring linear/HDR signal ranges.
- Multiple TMOs: Reinhard, Hable, Durand, ACES, logarithmic, TMOz-inspired, and
  native PyTorch IQATM inference using the bundled research checkpoint.
- IQM research tools including TMQI components, SSIM maps, naturalness analysis,
  TMO comparison, and predicting-image-difference diagnostics.
- Closable center tabs for IQM/feature maps while the main **ISP Image** tab
  remains available.
- Recent files/folders, stage export, GPU/CPU device selection, and shared YAML
  configuration for the Qt application and reference GPU pipeline.

## Project layout

```text
interactiveISP/
├── config/
│   └── interactive_isp.yml
├── modules/
│   ├── tone_mapping/
│   │   ├── module.py
│   │   ├── operators/
│   │   └── iqatm/
│   ├── iqm/
│   └── folders/
├── third_party/
│   └── IQATM/
├── docs/
├── validation/
├── interactive_isp_app.py
├── interactiveISP_gpu_v3.py
└── run_interactive_isp.py
```

See [`docs/PROJECT_LAYOUT.md`](docs/PROJECT_LAYOUT.md) for more detail.

## Requirements

The current environment is based on Python, PySide6, NumPy, Pillow, PyYAML, and
PyTorch. See [`requirements.txt`](requirements.txt) for the versions used by the
research build.

For GPU acceleration, install a PyTorch build compatible with your CUDA driver.
The repository's pinned Torch build reflects the development machine and may
need to be adjusted for another workstation.

## Run

```bash
python run_interactive_isp.py
```

On Windows you can also use:

```text
RUN_INTERACTIVEISP.cmd
```

### VS Code

Open the repository folder and use the included launch configuration:

```text
interactiveISP (current workspace folder)
```

The committed `.vscode/settings.json` intentionally contains no user-specific
Python path. Select your own interpreter/Conda environment in VS Code.

## Configuration

The Qt application and reference GPU implementation share:

```text
config/interactive_isp.yml
```

Keep calibration and research-analysis settings here rather than hard-coding
machine-specific values.

## Validation

Useful checks include:

```bash
python -m compileall -q .
python verify_build_static.py
python smoke_test_backend.py
python test_iqatm_runtime.py
```

`validation/` contains research-build regression data. Large rendered validation
images are intentionally ignored by the Git configuration.

## IQATM

interactiveISP includes a native PyTorch inference path for the supplied IQATM
checkpoint. **TensorFlow and OpenCV are not required** for this native path.
Third-party model assets retain their original license; see
[`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) and
[`third_party/IQATM/LICENSE`](third_party/IQATM/LICENSE).

## Research metrics

Metric names should be interpreted carefully:

- **TMQI** is used with an HDR/linear reference and tone-mapped result.
- **SSIM** is meaningful when the compared images share a suitable display
  domain/reference definition.
- **BTMQI** and **HDR-VDP-3** should only be labelled as official scores when
  their published trained/reference components are actually present.
- Difference/feature maps are analysis views and never modify cached ISP data.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md). New TMOs should generally be placed in
`modules/tone_mapping/operators/`; new metrics belong under `modules/iqm/`.

## License

No open-source license has yet been selected for the interactiveISP source code.
See [`LICENSE`](LICENSE). Third-party components have separate licenses.

## Citation

A preliminary [`CITATION.cff`](CITATION.cff) is included for research use. Before
publishing the repository, replace the placeholder GitHub repository URL and add
project author information if desired. Please also cite the original papers for
any TMO/IQM used in published experiments.
