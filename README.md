# UCG-LineFit

UCG-LineFit is a line-segment extraction method for sparse 2D LiDAR point clouds. It combines sensor-uncertainty calibration, locally adaptive Huber fitting, and graph-based fragment recovery.

This repository provides the Python implementation, experimental datasets, algorithm parameters, and ablation resources in a single downloadable archive.

## Download

**[Download UCG-LineFit.zip](./UCG-LineFit.zip?raw=true)**

Extract the archive and open a terminal in the extracted `UCG-LineFit` folder containing `core_method`, `datasets`, and `ablation`. The README inside the archive provides additional usage details and a Python API example.

## Included resources

| Location inside the archive | Contents |
| --- | --- |
| `core_method/UCG.py` | Main UCG-LineFit implementation. |
| `core_method/feature.py`, `feature_tour_batch.py`, `feature_tour_native.py` | Required support modules for geometry, plotting, batch fitting, and optional acceleration. Keep these files beside `UCG.py`. |
| `core_method/Algorithm_Parameters.docx` | All 148 command-line parameters and their effective defaults, presented in three-line tables. |
| `datasets/Lab/` | Laboratory dataset and visualization, including `Lab_points.json`, `Lab_points.png`, and `single_frame.json`. |
| `datasets/map_points.json` | Single-frame real-world point cloud with 1,166 points. |
| `datasets/test.json` | Synthetic stress-test scene with 167 points and ground-truth annotations. |
| `ablation/` | Three ablation scripts, a copy of `test.json`, and the latest experimental records in `latest_records/`. |

## Quick start

Python 3.10 or later is recommended. The package was checked with Python 3.12, NumPy 2.4.4, and Matplotlib 3.10.9.

Install the required packages:

```bash
python -m pip install numpy matplotlib
```

Run the following commands from the extracted `UCG-LineFit` folder:

```bash
# Single-frame real-world data
python core_method/UCG.py --input datasets/map_points.json --output-png real_result.png --no-native-enable

# Synthetic data
python core_method/UCG.py --input datasets/test.json --output-png synthetic_result.png --no-native-enable

# Laboratory data
python core_method/UCG.py --input datasets/Lab/Lab_points.json --output-png lab_result.png --no-native-enable

# All available options
python core_method/UCG.py --help
```

Each run saves a fitted-line visualization and a `points.png` image, and prints segment statistics, runtime, and diagnostics. Repeated runs in the same output directory overwrite `points.png`.

Input JSON must contain a `points` list of objects with `x` and `y` coordinates in metres. Preserve the input point order. Use `--angle-increment` to specify the LiDAR angular increment in radians; otherwise, the CLI reads a supported metadata hint or estimates it from the points. The supplied synthetic scene includes a `0.0098` rad hint.

For optional Numba acceleration, install `numba` and `scipy`, then omit `--no-native-enable`. The NumPy implementation remains available without these optional packages.

## Ablation experiments

Run the full method and all three ablations:

```bash
python ablation/ablation_no_uncertainty.py --all
```

The three conditions remove uncertainty calibration and sparsity compensation, replace the adaptive Huber scale with a fixed scale, and disable graph recovery together with gap bridging in other merge stages, respectively. The third condition evaluates the broader gap-recovery pipeline.

Results are saved to `ablation/runs/`, including six visualizations, a JSON report, and Markdown/HTML comparison tables. Original experimental records from October 11, 2026 are preserved in `ablation/latest_records/`.

The synthetic scene was selected from 316 candidate configurations to illustrate specific mechanism differences. It is a designed stress test, not an independent unbiased benchmark. The archived records document this design and provide the support and matching statistics needed to interpret the results.

## Citation

If you use these resources, cite the [UCG-LineFit repository](https://github.com/liuyu-up/UCG-LineFit) and record the release tag or commit hash used in your experiments. A basic repository citation is:

```bibtex
@misc{ucg_linefit_2026,
  title        = {{UCG-LineFit}: Line-Segment Extraction for Sparse 2D LiDAR Point Clouds},
  year         = {2026},
  howpublished = {GitHub repository},
  url          = {https://github.com/liuyu-up/UCG-LineFit}
}
```

## License

The source code is released under the [MIT License](./LICENSE).
