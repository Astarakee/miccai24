# Description

This repository provides instructions on utilizing my submitted models for several MICCAI 2024 segmentation challenges.

## Models

- BraTS Adult Glioma Post-treatment segmentation (GLI)
- BraTS Pediatric Glioma Pre-operative segmentation (PEDs)
- BraTS Meningioma segmentation (MenRT)(BraTS/README.md)
- AutoPET Multicenter Multitracer Generalization (AutoPET)
- Head&Neck Tumor Segmentation for MR-guided RT (HNTSMRG)

## Installation

The trained models are available for use in the prediction phase, packaged as docker images. This eliminates the need for Python package installations.

> Running the models requires a Docker installation. <br>
> To install **Docker**, follow the official [instructions](https://docs.docker.com/get-docker/) <br>
> To install the **NVIDIA Container Toolkit** follow the official [instrations](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) <br>

To run a sample CUDA container as sanity check:
```bash
  sudo docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
```

## Available Models

### Adult Glioma Post-treatment Segmentation
Pull the docker image:
```bash
  docker pull astarakee/brats24_glipost:latest
```
The input preprocessed multi-parametric MRIs should be structured as standard BraTS dataset; i.e,.
```
├── BraTS-GLI-02073-100
│   ├── BraTS-GLI-02073-100-t1c.nii.gz
│   ├── BraTS-GLI-02073-100-t1n.nii.gz
│   ├── BraTS-GLI-02073-100-t2f.nii.gz
│   └── BraTS-GLI-02073-100-t2w.nii.gz
├── BraTS-GLI-02127-102
│   ├── BraTS-GLI-02127-102-t1c.nii.gz
│   ├── BraTS-GLI-02127-102-t1n.nii.gz
│   ├── BraTS-GLI-02127-102-t2f.nii.gz
│   └── BraTS-GLI-02127-102-t2w.nii.gz
├── BraTS-GLI-02127-103
│   ├── BraTS-GLI-02127-103-t1c.nii.gz
│   ├── BraTS-GLI-02127-103-t1n.nii.gz
│   ├── BraTS-GLI-02127-103-t2f.nii.gz
│   └── BraTS-GLI-02127-103-t2w.nii.gz
...
```
Make sure that `"brats" in foldername.lower() == True` <br>
To run the model, then:
```bash
  docker run --rm --gpus all --network none --memory="32g" -v <AbsPathInput>:/input/images -v <AbsPathOutput>:/output/pred --shm-size 2g astarakee/brats24_glipost:latest
```
The resulting segmentation masks in `.nii.gz` file format will be saved in `AbsPathOutput`.

### Pediatric Glioma Preoperative Segmentation
```bash
  docker pull astarakee/brats24_peds:latest
```
The input preprocessed multi-parametric MRIs should be structured as standard BraTS dataset; i.e,.
```
├── BraTS-PED-00281-000
│   ├── BraTS-PED-00281-000-t1c.nii.gz
│   ├── BraTS-PED-00281-000-t1n.nii.gz
│   ├── BraTS-PED-00281-000-t2f.nii.gz
│   └── BraTS-PED-00281-000-t2w.nii.gz
├── BraTS-PED-00282-000
│   ├── BraTS-PED-00282-000-t1c.nii.gz
│   ├── BraTS-PED-00282-000-t1n.nii.gz
│   ├── BraTS-PED-00282-000-t2f.nii.gz
│   └── BraTS-PED-00282-000-t2w.nii.gz
...
```
Make sure that `"brats" in foldername.lower() == True` <br>
```bash
  docker run --rm --gpus all --network none --memory="32g" -v <AbsPathInput>:/input/images -v <AbsPathOutput>:/output/pred --shm-size 2g astarakee/brats24_peds:latest
```
The resulting segmentation masks in `.nii.gz` file format will be saved in `AbsPathOutput`.

### Meningioma Segmentation
```bash
  docker pull astarakee/brats24_menrt:latest
```
The input T1c MRIs should be structured as standard BraTS dataset; i.e,.
```
├── BraTS-MEN-RT-0292-1
│   └── BraTS-MEN-RT-0292-1_t1c.nii.gz
├── BraTS-MEN-RT-0293-1
│   └── BraTS-MEN-RT-0293-1_t1c.nii.gz
├── BraTS-MEN-RT-0308-1
│   └── BraTS-MEN-RT-0308-1_t1c.nii.gz
└── BraTS-MEN-RT-0328-1
    └── BraTS-MEN-RT-0328-1_t1c.nii.gz
...
```
```bash
  docker run --rm --gpus all --network none --memory="32g" -v <AbsPathInput>:/input/images -v <AbsPathInput>:/output/pred --shm-size 2g astarakee/brats24_menrt:latest
```
The resulting segmentation masks in `.nii.gz` file format will be saved in `AbsPathOutput`.

### AutoPET - Whole Body PET-CT Lesion Segmentation
```bash
  docker pull astarakee/autopet24:latest
```
The input data directory should contain two subdirs for `ct` and `pet` volumes. <br>
Note that the CT and PT filesnames for each subject should be identical; i.e,
```
├── ct
│   ├── PETCT_0b57b247b6_0.nii.gz
│   ├── PETCT_0beb67c923_0.nii.gz
│   ├── case3.nii.gz
│   ├── subjectXYZ.nii.gz
│   └── RandomName_rand.nii.gz
└── pet
    ├── PETCT_0b57b247b6_0.nii.gz
    ├── PETCT_0beb67c923_0.nii.gz
    ├── case3.nii.gz
    ├── subjectXYZ.nii.gz
    └── RandomName_rand.nii.gz

```
```bash
  docker run --rm --gpus all --network none --memory="32g" -v <AbsPathInput>:/input -v <AbsPathInput>:/output --shm-size 2g astarakee/autopet24:latest
```
The predicted segmentation masks will be saved as `.mha` format at `AbsPathInput/images/automated-petct-lesion-segmentation`

### Head&Neck Tumor Segmentation for MR-guided RT
Pull the dockers for task 1 (pre-treatment MRIs) and task 2 (mid-treatment MRIs):
```bash
  docker pull astarakee/hntsmrg_pre:latest
```
```bash
  docker pull astarakee/hntsmrg_mid:latest
```
Task 1 requires only pre-treatment T2w MRIs in the following structure:
### TODO
