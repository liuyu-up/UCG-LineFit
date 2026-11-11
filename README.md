# UCG-LineFit

**UCG-LineFit** is a robust line-segment extraction method for sparse 2D LiDAR point clouds. The method combines sensor-uncertainty calibration, locally adaptive robust fitting, and graph-based fragment recovery to improve line extraction under measurement noise and sparse sampling.

## Download

[Download Reproduction_Data.zip](https://github.com/liuyu-up/UCG-LineFit/raw/refs/heads/main/Reproduction_Data.zip)

Extract the archive and open a Bash terminal in the `Reproduction_Data/` directory. The directory includes its own `README.md`.

## Archive Contents

The archive contains point clouds, manual annotations, reproduction scripts, figures, and experiment logs for the manuscript.

| Path | Contents |
| --- | --- |
| `Open_Dataset_Experiments/` | 40 annotated Intel scans, Figure 9 materials, and the Table 3 experiment log |
| `Real_World_Experiments/` | R2000 recordings, 20 annotated scans, Figure 10 materials, and the Table 4 experiment log |
| `Runtime_Analysis/` | Figure 11 and its stage-by-stage timing log |
| `Ablation_Study/` | Ablation methods, 30 annotated scans, and materials for Figure 12 and Table 5 |
| `Sensitivity_Analysis/` | Two sets of 30 scans and materials for Figures 13–15 |
| `error_calibration/` and `parameter_list.docx` | Sensor calibration reports and parameter settings |

## Linux Setup

Use Ubuntu 22.04.3 LTS and Python 3.10.12. The scripts read exported JSON point clouds directly. Numba is optional and accelerates UCG-LineFit. Replace the path below with the extracted directory.

```bash
cd '/path/to/Reproduction_Data'
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install numpy==1.24.4 matplotlib==3.7.5 scipy==1.10.1

export MPLBACKEND=Agg
OUT="$PWD/reproduced"
mkdir -p "$OUT"
```

## Reproduction Commands

```bash
# Figures 9 and 10
python3 -B Open_Dataset_Experiments/Figure_9/reproduce_fig9.py \
  --methods UCG-LineFit RANSAC IEPF SRG --output-dir "$OUT/fig9"
python3 -B Real_World_Experiments/Figure_10/reproduce_fig10.py \
  --methods UCG-LineFit RANSAC IEPF SRG --output-dir "$OUT/fig10"

# Figure 12 and Table 5
python3 -B Ablation_Study/Figure_12_Local_Point_Clouds/reproduce_fig12.py --output "$OUT/fig12.pdf"
python3 -B Ablation_Study/reproduce_table5.py --output-dir "$OUT/table5"

# Figures 13–15
python3 -B Sensitivity_Analysis/reproduce_fig13_15.py --output-dir "$OUT/sensitivity"
```

Figures 9 and 10 produce a PDF per method and save the extracted segments as JSON. Table 5 writes CSV and JSON results. To continue an interrupted sweep for Figures 13–15, rerun the command with `--resume` and the same output directory.

## Recorded Results

- Figure 11. PDF at `Runtime_Analysis/fig11.pdf` and stage timing log at `Runtime_Analysis/experiment_log.txt`.
- Tables 3 and 4. Intel experiment log at `Open_Dataset_Experiments/experiment_log.txt` and R2000 experiment log at `Real_World_Experiments/experiment_log.txt`.

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
