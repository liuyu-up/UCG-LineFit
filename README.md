# UCG-LineFit

**UCG-LineFit** is a robust line-segment extraction method for sparse 2D LiDAR point clouds. The method combines sensor-uncertainty calibration, locally adaptive robust fitting, and graph-based fragment recovery to improve line extraction under measurement noise and sparse sampling.

This repository provides the reproduction package for Tables 3-5 and Figures 9-15 of the associated manuscript. The comparison experiments include UCG-LineFit, RANSAC, IEPF, SRG, and WCTLS. The ablation study evaluates Full, Heuristic, Fixed-Huber, and Graph-off variants.

## Download

[Download Reproduction_Data.zip (v1.0.0)](https://github.com/liuyu-up/UCG-LineFit/raw/refs/tags/v1.0.0/Reproduction_Data.zip)

Version `v1.0.0` identifies the reproduction package accompanying this manuscript revision. The same package is provided as `Reproduction_Data.zip` on the default branch.

Extract the archive and open a terminal in the extracted `Reproduction_Data/` directory. All paths and commands below refer to this directory. The archive includes its own `README.md` and a `reproduction_instructions.txt` file alongside each of the eight reproduction scripts.

## Archive Contents

The archive contains input scans, reference annotations, method implementations, reproduction scripts, original experiment records, and original reference figures. Regenerated logs, figures, segment exports, sensitivity result files, and checkpoints are excluded; the reproduction commands generate these files after dependency installation.

| Path | Contents |
| --- | --- |
| `Open_Dataset_Experiments/` | Forty annotated Intel scans, five method implementations, the Table 3 reproduction script, and original comparison logs |
| `Open_Dataset_Experiments/Figure_9/` | Two-scan point cloud, reference segments and layout data, five methods, and the complete Figure 9 reproduction script |
| `Real_World_Experiments/` | Twenty annotated R2000 scans, available source recordings and scan files, five method implementations, the Table 4 reproduction script, and original comparison logs |
| `Real_World_Experiments/Figure_10/` | Laboratory point cloud, reference segments and layout data, five methods, and the complete Figure 10 reproduction script |
| `Runtime_Analysis/` | Figure 11 stage-profiling script, original timing summary, and reference figure |
| `Ablation_Study/` | Four ablation variants, twenty Intel and ten R2000 annotated scans, the Table 5 reproduction script, and original experiment records |
| `Ablation_Study/Figure_12_Local_Point_Clouds/` | Three local-view manifests and the complete Figure 12 reproduction script |
| `Sensitivity_Analysis/` | Two groups of thirty annotated scans, the shared method implementation, original PDF style templates, original experiment records, and the Figures 13-15 reproduction script |
| `error_calibration/Inter/` and `error_calibration/R2000/` | Original sensor-calibration reports |
| `parameter_list.docx` | Sensor and algorithm parameter reference |
| `requirements.txt`, `README.md`, and eight `reproduction_instructions.txt` files | Pinned dependencies, package documentation, and experiment-specific requirements, inputs, outputs, and execution instructions |

The original `experiment_log.txt` and `experiment_log_wctls.txt` files, annotation and reference PDFs, `figure_reference.json` files, and three original sensitivity PDF templates are retained. These files provide recorded references or inputs used by the reproduction scripts.

## Setup

Use Python 3.10, 3.11, or 3.12 on Windows, Linux, or macOS where the pinned dependencies in `requirements.txt` can be installed and run successfully. An existing Python environment, a `venv` environment, or a Conda environment may be used. The scripts read the supplied scan files directly; ROS/ROS2 and a GPU are not required.

With the intended Python environment active and the terminal in the extracted `Reproduction_Data/` directory, install the required NumPy, SciPy, Matplotlib, Numba, and PyMuPDF packages:

```text
python -m pip install -r requirements.txt
```

The dependency file selects the appropriate pinned versions for the supported Python versions. If the interpreter is available as `python3`, replace `python` with `python3` in both installation and execution commands. Use the same interpreter for both steps.

## Reproduction Commands

After dependency installation, each row below requires one command. Run the commands from the extracted `Reproduction_Data/` directory. Alternatively, open a terminal in a script's own directory and run `python` followed by its filename. Inputs and default outputs are resolved relative to the script location, so preserve the complete directory structure when moving the package.

| Result | Command |
| --- | --- |
| Table 3: five comparison methods | `python Open_Dataset_Experiments/reproduce_table3.py` |
| Table 4: five comparison methods | `python Real_World_Experiments/reproduce_table4.py` |
| Table 5: four ablation variants | `python Ablation_Study/reproduce_table5.py` |
| Figure 9: all six panels | `python Open_Dataset_Experiments/Figure_9/reproduce_fig9.py` |
| Figure 10: all six panels | `python Real_World_Experiments/Figure_10/reproduce_fig10.py` |
| Figure 11: runtime allocation | `python Runtime_Analysis/reproduce_fig11.py` |
| Figure 12: all six panels | `python Ablation_Study/Figure_12_Local_Point_Clouds/reproduce_fig12.py` |
| Figures 13-15: complete sensitivity experiments | `python Sensitivity_Analysis/reproduce_fig13_15.py` |

The commands generate the following files. Subsequent runs overwrite files with the same names.

| Output directory | Generated files |
| --- | --- |
| `Open_Dataset_Experiments/` | `experiment_log_new.txt` for Table 3 |
| `Real_World_Experiments/` | `experiment_log_new.txt` for Table 4 |
| `Ablation_Study/` | `experiment_log_new.txt` for Table 5; `fig12_new.pdf` and the identical `ablation_combined_new.pdf` for Figure 12 |
| `Open_Dataset_Experiments/Figure_9/` | `fig9_new.pdf`, `fig9_segments_new.json` |
| `Real_World_Experiments/Figure_10/` | `fig10_new.pdf`, `fig10_segments_new.json` |
| `Runtime_Analysis/` | `experiment_log_new.txt`, `fig11_new.pdf` |
| `Sensitivity_Analysis/` | `fig13_new.pdf`, `fig14_new.pdf`, `fig15_new.pdf`, equivalent descriptive-name PDFs, `sensitivity_results.json`, `sensitivity_results.csv`, and `sensitivity_checkpoint.json` |

Tables 3-5 produce summary metrics, detailed per-frame matching results and errors, and measured runtimes. Figure 11 writes only final runtime summaries to its log. Figures are generated as vector PDFs using the original layouts. Figures 9 and 10 include WCTLS in panel (f) of the complete six-panel figure; the default commands do not generate separate WCTLS PDFs. Figure 12 writes its PDFs to the parent `Ablation_Study/` directory.

Table 3 processes forty Intel scans with five timed repetitions per frame and method. Table 4 processes twenty R2000 scans with ten repetitions. Table 5 processes twenty Intel scans and ten R2000 scans using the respective dataset-specific parameter configurations. Its default output is the text log; `--export-data` additionally writes `table5_new.json` and `table5_new.csv`.

The default Figure 11 command first reruns Tables 3 and 4 and updates their new logs. It then profiles UCG-LineFit ten times per frame for both datasets, producing 400 Intel and 200 R2000 instrumented calls. Exclusive stage shares are normalized using the newly measured table runtimes. Run timing experiments sequentially, without concurrent reproduction jobs. If Tables 3 and 4 have just been completed in the same measurement session, their new logs can be reused with:

```text
python Runtime_Analysis/reproduce_fig11.py --reuse-table-results
```

The sensitivity command recomputes all 57 parameter settings and 1,710 fits. It uses the two supplied thirty-frame data groups and the original PDFs as vector style templates, replacing all data curves and markers with computed results. The descriptive output names are `fig13_sensitivity_confidence_new.pdf`, `fig14_sensitivity_huber_gain_new.pdf`, and `fig15_sensitivity_graph_angle_new.pdf`; each is identical to its corresponding short-name PDF.

No sensitivity checkpoint is included in the archive. Use the default command for the initial full run. After a run creates a checkpoint, continue an interrupted experiment with:

```text
python Sensitivity_Analysis/reproduce_fig13_15.py --resume
```

The checkpoint is reused only when its source and input hashes match the current files. Omit `--resume` to recompute the full experiment.

## Recorded Results

Original records remain available separately from newly generated outputs:

- Tables 3 and 4: `Open_Dataset_Experiments/experiment_log.txt` and `Real_World_Experiments/experiment_log.txt`, with the respective WCTLS records in `experiment_log_wctls.txt`.
- Table 5: `Ablation_Study/experiment_log.txt`, including separate Intel and R2000 parameter configurations in Appendix II.
- Figure 11: `Runtime_Analysis/fig11.pdf` and `Runtime_Analysis/experiment_log.txt`.
- Figures 9, 10, and 12: original reference figures, scan data, and reference or local-view definitions in the corresponding directories.
- Figures 13-15: `Sensitivity_Analysis/experiment_log.txt` and the three original sensitivity PDFs used as style templates.

New outputs use the names documented above and do not replace the original reference logs or figures. Keep the matching scan JSON and annotation PDF files together, along with the supplied local method files and relative directory structure.

## Citation

If you use this code or reproduction data, please cite the associated UCG-LineFit work and this repository. For reproducibility, record the repository release tag or commit hash used in your experiments.

A basic BibTeX entry for the repository is:

```bibtex
@misc{ucg_linefit_2026,
  title        = {{UCG-LineFit}: Robust Line-Segment Extraction for Sparse 2D LiDAR Point Clouds},
  year         = {2026},
  howpublished = {GitHub repository},
  url          = {https://github.com/liuyu-up/UCG-LineFit}
}
```

## License

The source code is released under the [MIT License](LICENSE). Please check the license and any accompanying notices before redistributing third-party datasets, recordings, annotations, or other included materials.
