# MERSCOPE Spatial Transcriptomics Analysis

Analysis pipeline for targeted spatial transcriptomics data generated with the Vizgen MERSCOPE platform. Developed for tomato (*Solanum lycopersicum*) root tissue sections.

## Biological context

This pipeline was developed to analyze spatial gene expression in tomato root tissue under mock and treatment conditions. The gene panel includes scRNAseq-derived genes of interest.

## Workflow

| Notebook | Description |
|---|---|
| `01_load_data.ipynb` | Load MERSCOPE output per region, build SpatialData zarr stores, add PolyT image |
| `02_generate_plots_allregions.ipynb` | Define regions of interest, generate per-gene transcript overlay plots across all regions |

## Data

Raw data not included. Expected input structure:

```
.../output/<experiment_id>/
    region_R*/
        detected_transcripts.csv
        images/
            mosaic_PolyT_z3.tif
            mosaic_DAPI_z3.tif
            mosaic_<gene>_z3.tif   # sequential/high-abundance genes
            ...
```

Processed SpatialData zarr stores are written to `.../analysis/<experiment_id>/` mirroring the output directory structure.

## Output

Per-gene plots are organized as:

```
plots/genes/<gene_id>/
    <gene_id>_<region>_<root>.png       # individual root plots
    <gene_id>_all_regions.png           # multi-panel summary across all regions
```

## Installation

Requires Python 3.12 and [uv](https://github.com/astral-sh/uv).

```bash
uv venv --python=3.12 harpy_env
source harpy_env/bin/activate
uv pip install "harpy-analysis[extra]"
uv pip install "setuptools<81" rioxarray ipykernel
python -m ipykernel install --user --name harpy --display-name "harpy"
```

See `requirements.txt` for the full pinned dependency list.

## Notes

- Tested on Rocky Linux 9 (HPC environment)
- PolyT z3 mosaic image is used as background for transcript overlay plots
- Sequential/high-abundance genes are imaged as dedicated fluorescence channels rather than detected as transcripts; these are handled separately in the plotting pipeline

## Author

Nikolaos Ntelkis 
PhD candidate
VIB-UGent Center for Plant Systems Biology