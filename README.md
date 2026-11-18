# UCG-LineFit

**UCG-LineFit** is a robust line-segment extraction method for sparse 2D LiDAR point clouds. The method combines sensor-uncertainty calibration, locally adaptive robust fitting, and graph-based fragment recovery to improve line extraction under measurement noise and sparse sampling.

This package contains the source materials and reproduction workflow for Tables 1-5 and Figures 1-15. It recomputes the experimental results in Tables 3-5 and Figures 9-15 and imports Tables 1-2 and Figures 1-8 from the supplied manuscript assets. The comparison experiments include UCG-LineFit, RANSAC, IEPF, SRG, and WCTLS. The ablation study compares Full, Heuristic, Fixed-Huber, and Graph-off.

## Download

[Download Reproduction_Data.zip (v1.1.0)](https://github.com/liuyu-up/UCG-LineFit/releases/download/v1.1.0/Reproduction_Data.zip)

[Release v1.1.0](https://github.com/liuyu-up/UCG-LineFit/releases/tag/v1.1.0) | [Previous release v1.0.0](https://github.com/liuyu-up/UCG-LineFit/releases/tag/v1.0.0)

Download the `Reproduction_Data.zip` release asset for the complete reproduction package. Extract it and open a terminal in the extracted `Reproduction_Data/` directory. All file paths and execution commands below refer to that directory. The original `v1.0.0` release remains available unchanged.

## Start here: reproduce all tables and figures

**Main entry point: `reproduce_all.py`.** Configure the Python environment, install the dependencies, and run the single command below to produce Tables 1-5 and all fifteen figures.

### 1. Configure the environment

Use Python 3.10, 3.11, or 3.12 on Windows, Linux, or macOS where the pinned dependencies in `requirements.txt` can be installed and run successfully. The required packages are NumPy, SciPy, Matplotlib, Numba, and PyMuPDF. The scripts read the supplied scan files directly; ROS/ROS2 and a GPU are not required.

Open a terminal in the directory containing this README. Use an existing Python, venv, or Conda environment, or create a virtual environment with:

```text
python -m venv .venv
```

Activate the virtual environment using the command for your terminal:

| Terminal | Activation command |
| --- | --- |
| Windows PowerShell | `.\.venv\Scripts\Activate.ps1` |
| Windows Command Prompt | `.venv\Scripts\activate.bat` |
| Linux/macOS Bash or Zsh | `source .venv/bin/activate` |

With the intended environment active, install the pinned dependencies:

```text
python -m pip install -r requirements.txt
```

If the interpreter is available as `python3`, replace `python` with `python3` in the environment-creation, installation, and execution commands. Use the same interpreter for installation and reproduction.

### 2. Run the complete reproduction

```text
python reproduce_all.py
```

The script recomputes Tables 3-5 and Figures 9-15. It imports Tables 1-2 from `manuscript_assets/tables1_2.txt` and Figures 1-8 directly from `manuscript_assets/`. These illustrations, photographs, and input point-cloud views are source artwork outside the experimental result figures; they are copied without changing their appearance.

The source text records Table 1, Experimental platform and sample information for the real scenes, and Table 2, Reference values of the core UCG-LineFit parameters. Table 1 describes the original platform reported in the manuscript. Its operating-system and hardware entries are retained as manuscript content; they do not restrict the supported reproduction environments listed above.

All fifteen figures are collected in `figs_new/`. Figures 6 and 8 retain their PNG format as `fig6_new.png` and `fig8_new.png`; the remaining figures use `fig1_new.pdf` through `fig15_new.pdf` with the corresponding figure numbers. The experimental figures remain vector PDFs with their original layouts.

The root-level `experiment_log_new.txt` begins with the imported Table 1 and Table 2 content, followed by the complete Table 3, Table 4, and Table 5 records, including summary tables, measured runtimes, per-scan matching results, errors, and complete predicted segment endpoints. The individual reproduction scripts also retain their own outputs in the locations listed below.

The timing experiments run sequentially. Figure 11 reuses the Table 3 and Table 4 logs produced earlier in the same command and performs fresh stage profiling. The sensitivity experiment recomputes all settings and does not reuse a checkpoint by default. To use multiple workers for sensitivity only:

```bash
python reproduce_all.py --workers 3
```

The complete package can be moved without editing any paths. The script resolves its inputs and outputs from its own location and can also be invoked from another working directory.

## Archive contents

This archive includes the input scans, annotations, method implementations, reproduction scripts, instruction files, original experiment records, original reference figures, and manuscript assets. Generated experiment logs, figures, segment exports, sensitivity result files, checkpoints, and caches are excluded. The reproduction commands create these outputs after dependency installation.

The original `experiment_log.txt` and `experiment_log_wctls.txt` files, reference and annotation PDFs, `figure_reference.json`, the three original sensitivity PDF templates, and `manuscript_assets/tables1_2.txt` are retained. These files provide the recorded references or inputs required for reproduction.

## Package contents

| Path | Contents |
| --- | --- |
| `Open_Dataset_Experiments/` | Forty annotated Intel scans, five method implementations, the Table 3 script, and original experiment logs |
| `Open_Dataset_Experiments/Figure_9/` | Two-scan point cloud, reference and layout data, five methods, and the complete Figure 9 reproduction script |
| `Real_World_Experiments/` | Twenty annotated R2000 scans, available source recordings and scan files, five methods, the Table 4 script, and original experiment logs |
| `Real_World_Experiments/Figure_10/` | Laboratory point cloud, reference and layout data, five methods, and the complete Figure 10 reproduction script |
| `Runtime_Analysis/` | Figure 11 stage-profiling script, runtime summaries, and vector figures |
| `Ablation_Study/` | Four method variants, thirty annotated scans, and the Table 5 script |
| `Ablation_Study/Figure_12_Local_Point_Clouds/` | Three local-view manifests and the complete Figure 12 reproduction script |
| `Sensitivity_Analysis/` | Two groups of thirty annotated scans, the shared method, original PDF style templates, and the reproduction script for Figures 13-15 |
| `error_calibration/Inter/` and `error_calibration/R2000/` | Sensor calibration reports |
| `parameter_list.docx` | Sensor and algorithm parameter reference |
| `requirements.txt` | Central dependency installation requirements |
| `manuscript_assets/` | Original figure artwork and `tables1_2.txt`; the complete reproduction script imports Figures 1-8 and Tables 1-2 from this directory |
| `reproduce_all.py` | Single-command reproduction and collection of all fifteen figures and Tables 1-5 |

Each of the eight reproduction-script directories contains an English `reproduction_instructions.txt` with the applicable requirements, inputs, outputs, a single reproduction instruction, and detailed execution steps.

WCTLS implementations are provided as `wctls.py` in both dataset experiment directories and in their Figure 9 or Figure 10 subdirectories. Original standalone WCTLS logs use `experiment_log_wctls.txt`. The Figure 9 and Figure 10 reproduction commands include WCTLS in panel (f) of the complete six-panel PDF and export the combined figure, segment JSON, and endpoint text log by default. The two figure directories also contain `figure_reference.json` files describing the reference segments and layout. The Figure 9 `requirements.txt` forwards to the central dependency file.

## WCTLS comparison implementation

The original WCTLS implementation is not publicly available as open-source software. The WCTLS code included in this package is an independent reimplementation based on the method described in the original publication. This reimplementation was developed for the comparisons reported in this study and is provided solely for academic research and reproducibility.

## Reproduction commands

Run the following commands from the directory containing this README. Each row requires one reproduction command after dependency installation.

| Result | Command | Detailed instructions |
| --- | --- | --- |
| Table 3: five methods | `python Open_Dataset_Experiments/reproduce_table3.py` | `Open_Dataset_Experiments/reproduction_instructions.txt` |
| Table 4: five methods | `python Real_World_Experiments/reproduce_table4.py` | `Real_World_Experiments/reproduction_instructions.txt` |
| Table 5: four ablation variants | `python Ablation_Study/reproduce_table5.py` | `Ablation_Study/reproduction_instructions.txt` |
| Figure 9: all six panels | `python Open_Dataset_Experiments/Figure_9/reproduce_fig9.py` | `Open_Dataset_Experiments/Figure_9/reproduction_instructions.txt` |
| Figure 10: all six panels | `python Real_World_Experiments/Figure_10/reproduce_fig10.py` | `Real_World_Experiments/Figure_10/reproduction_instructions.txt` |
| Figure 11: runtime allocation | `python Runtime_Analysis/reproduce_fig11.py` | `Runtime_Analysis/reproduction_instructions.txt` |
| Figure 12: all six panels | `python Ablation_Study/Figure_12_Local_Point_Clouds/reproduce_fig12.py` | `Ablation_Study/Figure_12_Local_Point_Clouds/reproduction_instructions.txt` |
| Figures 13-15: all sensitivity settings | `python Sensitivity_Analysis/reproduce_fig13_15.py` | `Sensitivity_Analysis/reproduction_instructions.txt` |

Alternatively, open a terminal in an individual script directory and run `python` followed by the script filename. Inputs and default outputs are resolved relative to the script location. Preserve the complete directory structure when moving or sharing the package.

## Outputs generated by reproduction

The following outputs are not included in the archive. Each corresponding command generates them; subsequent runs overwrite files with the same names. Generated JSON and CSV files and checkpoints are also excluded, including those whose names do not contain `_new`. The complete reproduction command additionally creates `figs_new/` and the combined `experiment_log_new.txt` at the package root.

| Experiment directory | Generated files |
| --- | --- |
| `Open_Dataset_Experiments/` | `experiment_log_new.txt` |
| `Real_World_Experiments/` | `experiment_log_new.txt` |
| `Ablation_Study/` | `experiment_log_new.txt` for Table 5; `fig12_new.pdf` and the identical `ablation_combined_new.pdf` for Figure 12 |
| `Open_Dataset_Experiments/Figure_9/` | `fig9_new.pdf`, `fig9_segments_new.json`, `experiment_log_new.txt` |
| `Real_World_Experiments/Figure_10/` | `fig10_new.pdf`, `fig10_segments_new.json`, `experiment_log_new.txt` |
| `Ablation_Study/Figure_12_Local_Point_Clouds/` | `experiment_log_new.txt` |
| `Runtime_Analysis/` | `experiment_log_new.txt`, `fig11_new.pdf` |
| `Sensitivity_Analysis/` | `fig13_new.pdf`, `fig14_new.pdf`, `fig15_new.pdf`; the descriptive PDF names listed below; `sensitivity_results.json`, `sensitivity_results.csv`, `sensitivity_checkpoint.json`, `experiment_log_new.txt` |

The sensitivity figures also retain the original descriptive filename stems with `_new` appended: `fig13_sensitivity_confidence_new.pdf`, `fig14_sensitivity_huber_gain_new.pdf`, and `fig15_sensitivity_graph_angle_new.pdf`. Each is identical to its corresponding short-name PDF.

Tables 3-5 write summary metrics, detailed per-frame matches and errors, measured runtimes, and complete predicted segment endpoints to `experiment_log_new.txt`. Figure 11 writes final runtime summaries and per-scan UCG-LineFit endpoints to its log. Figures 9, 10, and 12 write their predicted endpoints to local `experiment_log_new.txt` files. The sensitivity log contains all setting summaries and per-scan predictions, matching results, and errors. Coordinates use 17 significant digits and retain the output IDs used by the matching records, including unmatched predictions. Figure outputs preserve the original layouts as vector PDFs. The Figure 12 PDFs are written to the parent `Ablation_Study/` directory. Original logs and original PDFs are retained.

## Workflow details

### Tables 3-5 and Figure 11

Table 3 processes forty Intel scans with five timed repetitions per frame and method. Table 4 processes twenty R2000 scans with ten repetitions. Table 5 processes twenty Intel scans and ten R2000 scans using the respective dataset-specific parameter configurations; the matching JSON and PDF annotation files in `Annotations_and_Raw_Data/` must remain together. Its default output is the text log; `--export-data` additionally writes `table5_new.json` and `table5_new.csv`.

The default Figure 11 command first reruns Tables 3 and 4 and updates their `experiment_log_new.txt` files. It then profiles UCG-LineFit ten times per frame for both datasets, giving 400 instrumented Intel calls and 200 instrumented R2000 calls. The measured exclusive stage shares are normalized using the newly generated table runtimes. Run timing experiments sequentially, without concurrent reproduction jobs.

If Tables 3 and 4 have just been completed in the same measurement session, their new logs can be reused with one command:

```bash
python Runtime_Analysis/reproduce_fig11.py --reuse-table-results
```

Stage profiling still runs with this option. A missing table log causes its experiment to run automatically.

### Figures 9, 10, and 12

Figures 9 and 10 recompute all five methods and combine them with numbered reference segments in complete six-panel figures. Their point-cloud JSON files, `figure_reference.json`, and local method files must remain in their respective directories.

Figure 12 reads the three `Fig12_*.json` manifests, fits the referenced complete scans in `Ablation_Study/Annotations_and_Raw_Data/`, and then displays the specified local windows. Keep the manifests, full scans, and parent method files in their supplied relative locations.

### Figures 13-15

The default sensitivity command recomputes all 57 parameter settings and 1,710 fits. Both thirty-frame data groups and the three original PDFs are required. The originals `fig13_sensitivity_confidence.pdf`, `fig14_sensitivity_huber_gain.pdf`, and `fig15_sensitivity_graph_angle.pdf` serve as vector style templates; all data curves and markers are replaced using the computed results.

No sensitivity checkpoint is supplied in the archive. Start with the default command to recompute the full experiment. After a run creates a checkpoint, an interrupted run can be continued with:

```bash
python Sensitivity_Analysis/reproduce_fig13_15.py --resume
```

The checkpoint is accepted only when its source and input hashes match the current files. A complete checkpoint allows aggregation and figure regeneration without repeating the fits. Omit `--resume` to recompute the full experiment.

## Citation

If you use this reproduction package, cite the associated UCG-LineFit work and identify the exact release version used.

```bibtex
@misc{ucg_linefit_reproduction_2026,
  title        = {{UCG-LineFit} Reproduction Package},
  year         = {2026},
  version      = {1.1.0},
  howpublished = {GitHub release},
  url          = {https://github.com/liuyu-up/UCG-LineFit/releases/tag/v1.1.0}
}
```

## License

See [LICENSE](LICENSE) for the repository license and the source files for accompanying notices.
