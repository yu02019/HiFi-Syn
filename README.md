# HiFi-Syn: Hierarchical Granularity Discrimination for High-Fidelity Synthesis of MR Images

## Introduction
HiFi-Syn is a framework for medical image synthesis that aims to maintain structural consistency through hierarchical granularity discrimination, especially for MRI images involving structure preservation and lesion handling. Our model ensures high-fidelity conversion between multi-modal MRI images using pixel-level, structure-level, and global-level discrimination mechanisms.

This project includes the implementation of the methods described in our paper. **The code is currently being organized and will be released soon.** 

<!-- If you are interested in our work, feel free to follow this project and check for updates. -->

## Paper
* HiFi-Syn: Hierarchical Granularity Discrimination for High-Fidelity Synthesis of MR Images with Structure Preservation (Under Revision)

## File Structure
- `README.md` - Project introduction and installation guide.
- `data/` - Dataset and preprocessing scripts.
  - `download_data.sh` - Script for downloading MRI datasets from public databases.
  - `preprocessing.py` - Data preprocessing script, including image normalization, cropping, and other necessary steps.
- `models/` - HiFi-Syn model components and training code.
  - `generator.py` - Image generator model.
  - `discriminator.py` - Discriminator model.
  - `memory_bank.py` - Brain memory bank module for storing and retrieving pixel-level features.
  - `model.py` -  HiFi-Syn model.
- `weights/` - Pre-trained model weights.
  - `README.md` - Instructions on how to download pre-trained weights.
- `notebooks/` - Example notebooks demonstrating how to run the model and perform experiments.
  - `example_usage.ipynb` - Example of how to train and test the model using preprocessed data.
- `scripts/` - Training and testing scripts.
  - `train.py` - Training script for training the HiFi-Syn model.
  - `test.py` - Testing script for evaluating model performance.
- `utils/` - Utility functions, including data loading and evaluation metrics.
  - `data_loader.py` - Data loading utility.
  - `metrics.py` - Implementation of evaluation metrics, such as PSNR and SSIM.



## Installation Guide
1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/hifi-syn.git
   ```
2. Navigate to the project directory and install the dependencies:
   ```bash
   cd hifi-syn
   pip install -r requirements.txt
   ```


## Using Different Toolboxes for Annotation Labels
Existing neuroimaging toolboxes can provide high-quality automated segmentation labels, serving as an alternative when expert manual annotations are unavailable. Below are the commands and post-processing steps for using  [SPM](https://www.fil.ion.ucl.ac.uk/spm), [FSL](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki), and [FreeSurfer](https://surfer.nmr.mgh.harvard.edu/).

### SPM (Statistical Parametric Mapping)
SPM is a popular toolbox for analyzing brain imaging data, especially functional MRI. To generate segmentation labels with SPM:
1. Launch SPM in MATLAB and navigate to the segmentation tool (`SPM12 > Segment`).
2. Load the MRI image you want to segment.
3. Run the segmentation to obtain tissue classes (typically gray matter, white matter, and cerebrospinal fluid).
4. The output will include segmented tissue probability maps (`c1`, `c2`, `c3` for gray matter, white matter, and CSF respectively).

**Post-processing Steps:**
- Convert the tissue probability maps to binary masks (e.g., threshold the `c1` image to create a binary mask for gray matter).
- Ensure all segmentations are aligned with the original MRI image using MATLAB functions such as `spm_imcalc`.

### FSL (FMRIB Software Library)
FSL is a widely used software package for analysis of brain imaging data. To generate segmentation labels with FSL:
1. Use the `FAST` tool to perform segmentation:
   ```bash
   fast -t 1 -o output_prefix input_image.nii.gz
   ```
   - `-t 1` specifies that the input is a T1-weighted image.
   - `-o output_prefix` defines the prefix for output files.
2. The output will include several images, such as partial volume estimates and segmented tissue types (e.g., `output_prefix_pve_0` for CSF, `output_prefix_pve_1` for gray matter).

**Post-processing Steps:**
- Convert partial volume estimates to binary masks using FSL tools like `fslmaths`.
- Verify the alignment and registration of the segmented images to the original input.

### FreeSurfer
FreeSurfer is a software suite for the analysis and visualization of structural and functional neuroimaging data. To generate segmentation labels with FreeSurfer:
1. Use `recon-all` to run the full processing pipeline:
   ```bash
   recon-all -i input_image.nii.gz -s subject_id -all
   ```
   - `-i` specifies the input image.
   - `-s` specifies the subject ID.
   - `-all` runs the full pipeline, including skull stripping, segmentation, and surface reconstruction.
2. The segmentation labels are typically found in the `mri/aseg.mgz` file within the subject directory.

**Post-processing Steps:**
- Convert the `aseg.mgz` file to NIfTI format using:
  ```bash
  mri_convert $SUBJECTS_DIR/subject_id/mri/aseg.mgz output_segmentation.nii.gz
  ```
- Extract specific regions of interest using FreeSurfer commands like `mri_binarize` to create binary masks.

## Acknowledgment
We would like to acknowledge the developers of various open-source code repositories that have greatly inspired and aided our work. Specifically, we thank the authors of:
- [MGUIT](https://github.com/Jongchan/MGUIT) for their contributions to instance-level image-to-image translation.
- [DRIT++](https://github.com/HsinYingLee/DRIT) for the disentangled representation learning framework.
- [CycleGAN](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) for their pioneering work in image-to-image translation.

Their contributions have been instrumental in the development of HiFi-Syn.

## Contact Us
If you have any questions or suggestions, feel free to contact us.

