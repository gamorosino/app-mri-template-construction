[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brainlife/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-brainlife.app.XXX-blue.svg)](https://doi.org/10.25663/brainlife.app.XXX)

# MRI Template Construction (ANTs)

This application builds a **group-wise anatomical template** from a set of MRI volumes using **ANTs**.

The workflow supports a wide range of volumetric MRI inputs, including:

* T1-weighted (T1w)
* T2-weighted (T2w)
* Diffusion-derived scalar maps (e.g., FA, MD)
* Other quantitative MRI contrasts

Template construction is performed using an iterative **nonlinear registration framework**, producing:

* A **population-average template**
* Subject-to-template transformations
* Warped images in template space

All relevant ANTs parameters are exposed to the user for flexible configuration.

---

## Authors

* Gabriele Amorosino — [g.amorosino@gmail.com](mailto:g.amorosino@gmail.com)

---

### Funding

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)
[![NIH-NINDS-US24NS140384](https://img.shields.io/badge/NIH_NINDS-US24NS140384-green.svg)](https://reporter.nih.gov/search/OwP3wLYKIkGQ3uS2ODPUYw/project-details/11033905)
[![NIH-NINDS-UM1NS122207](https://img.shields.io/badge/NIH_NINDS-UM1NS1222074-green.svg)](https://reporter.nih.gov/search/pRbkN1_vbUmz-GTMYDjwMw/project-details/10664257)

---

Please cite the following foundational works when publishing results generated using this application.

1. Avants, B. B., Tustison, N. J., ..., & Gee, J. C. (2011). A reproducible evaluation of ANTs similarity metric performance in brain image registration. Neuroimage, 54(3), 2033-2044. 
2. Hayashi S., Caron B. A., ... & Pestilli F. (2024). brainlife.io: A decentralized and open-source cloud platform to support neuroscience research. Nature Methods, 21(5), 809–813.

---

# Overview of the Processing Workflow

The pipeline performs the following steps:

1. **Initialization**

   * Selects an initial reference (or computes an initial average).
   * Optionally performs rigid and affine alignment.

2. **Iterative template construction**

   * Registers all subjects to the current template using **SyN nonlinear registration**.
   * Warps images into template space.
   * Updates the template as the average of aligned images.

3. **Template refinement**

   * Repeats registration and averaging for a user-defined number of iterations.
   * Produces a final unbiased population template.

---

# Running the App

## On Brainlife.io

Execute directly via:

[https://doi.org/10.25663/brainlife.app.XXX](https://doi.org/10.25663/brainlife.app.XXX)

All dependencies are handled automatically by the platform.

---

## Running Locally

### 1. Clone the repository

```bash
git clone https://github.com/gamorosino/app-mri-template-construction.git
cd app-mri-template-construction
```

---

### 2. Create `config.json`

Example minimal configuration:

```json
{
  "images": [
    "testdata/sub-01.nii.gz",
    "testdata/sub-02.nii.gz",
    "testdata/sub-03.nii.gz"
  ]
}
```

Optional parameters:

```json
{
  "images": ["sub-01.nii.gz", "sub-02.nii.gz"],

  "iterations": 2,
  "metric": "CC",
  "transform": "SyN",
  "cores": 4
}
```

---

### 3. Run the pipeline

```bash
./main
```

---

# Inputs

### Required

* List of MRI volumes (`images`)

  * Must be in **NIfTI format**
  * Must have consistent orientation and voxel size

### Optional

* Template construction parameters:

  * Number of iterations
  * Registration metric (e.g., CC, MI)
  * Transform model (e.g., SyN)
  * Parallelization settings

---

# Outputs

### Main outputs

* Final group template

  ```
  template/template.nii.gz
  ```

* Intermediate templates (per iteration)

  ```
  template/template_iter*.nii.gz
  ```

* Warped subject images

  ```
  warped/sub-*/image.nii.gz
  ```

* Subject-to-template transforms

  ```
  transforms/sub-*/
  ```

---

# Dependencies

## Brainlife execution

All software dependencies are managed automatically.

---

## Local execution

Only the following is required:

* **Singularity / Apptainer**

All neuroimaging tools — including **ANTs** — are bundled inside the container.
No local installation is required.

---

# Reproducibility

This application is fully containerized, ensuring:

* Deterministic software environment
* Version-controlled template construction
* Cross-platform reproducibility

across Brainlife and local execution.