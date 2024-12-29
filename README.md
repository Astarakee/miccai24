# Description

This repository provides instructions on utilizing my submitted models for several MICCAI 2024 segmentation challenges.

## Models

- BraTS Adult Glioma Post-treatment segmentation (GLI) [website](https://www.synapse.org/Synapse:syn53708249/wiki/627500)
- BraTS Pediatric Glioma Pre-operative segmentation (PEDs) [website](https://www.synapse.org/Synapse:syn53708249/wiki/627505)
- BraTS Meningioma segmentation (MenRT) [website](https://www.synapse.org/Synapse:syn53708249/wiki/627503)
- AutoPET Multicenter Multitracer Generalization (AutoPET) [website](https://autopet-iii.grand-challenge.org/)
- Head&Neck Tumor Segmentation for MR-guided RT (HNTSMRG) [website](https://hntsmrg24.grand-challenge.org/)

## Installation

The trained models are available for use in the prediction phase, packaged as docker images. This eliminates the need for Python package installations.

> **Docker is required!** <br>
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
── in
│   ├── BraTS-GLI-02711-100
│   │   ├── BraTS-GLI-02711-100-t1c.nii.gz
│   │   ├── BraTS-GLI-02711-100-t1n.nii.gz
│   │   ├── BraTS-GLI-02711-100-t2f.nii.gz
│   │   └── BraTS-GLI-02711-100-t2w.nii.gz
│   ├── case_unknown-100
│   │   ├── case_unknown-100-t1c.nii.gz
│   │   ├── case_unknown-100-t1n.nii.gz
│   │   ├── case_unknown-100-t2f.nii.gz
│   │   └── case_unknown-100-t2w.nii.gz
│   ├── random123
│   │   ├── random123-t1c.nii.gz
│   │   ├── random123-t1n.nii.gz
│   │   ├── random123-t2f.nii.gz
│   │   └── random123-t2w.nii.gz
│   └── subject99_abc
│       ├── subject99_abc-t1c.nii.gz
│       ├── subject99_abc-t1n.nii.gz
│       ├── subject99_abc-t2f.nii.gz
│       └── subject99_abc-t2w.nii.gz
...
```
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
├── in
│   ├── BraTS-PED-00284-000
│   │   ├── BraTS-PED-00284-000-t1c.nii.gz
│   │   ├── BraTS-PED-00284-000-t1n.nii.gz
│   │   ├── BraTS-PED-00284-000-t2f.nii.gz
│   │   └── BraTS-PED-00284-000-t2w.nii.gz
│   ├── sample_rand_23
│   │   ├── sample_rand_23-t1c.nii.gz
│   │   ├── sample_rand_23-t1n.nii.gz
│   │   ├── sample_rand_23-t2f.nii.gz
│   │   └── sample_rand_23-t2w.nii.gz
│   └── testcase
│       ├── testcase-t1c.nii.gz
│       ├── testcase-t1n.nii.gz
│       ├── testcase-t2f.nii.gz
│       └── testcase-t2w.nii.gz
...
```
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
├── input
│   ├── BraTS-MEN-RT-0328-1
│   │   └── BraTS-MEN-RT-0328-1-t1c.nii.gz
│   ├── subjectAZ76
│   │   └── subjectAZ76-t1c.nii.gz
│   └── unknow654
│       └── unknow654-t1c.nii.gz
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
The predicted segmentation masks will be saved as `.mha` format at `AbsPathOutput/images/automated-petct-lesion-segmentation`

### Head&Neck Tumor Segmentation for MR-guided RT
Pull the dockers for task 1 (pre-treatment MRIs) and task 2 (mid-treatment MRIs):
```bash
  docker pull astarakee/hntsmrg_pre:latest
```
```bash
  docker pull astarakee/hntsmrg_mid:latest
```
Task 1 requires only pre-treatment T2w MRIs in the following structure:
```
in_path
    ├── 101_preRT_T2.nii.gz
    ├── MRI_PreRT_X12.nii.gz
    └── random_name123.nii.gz
    ...
```
The predicted masks in `.mha` format will be saved at `AbsPathOutput/mri-head-neck-segmentation` <br>
Task 2 requires both mid-treatment T2w MRIs and registered segmentation masks of pretreatment in the following structure:
```
└── test_set
    ├── mid_mri
    │   ├── 13_midRT_T2.nii.gz
    │   ├── randomXYZ.nii.gz
    │   └── subject1.nii.gz
    └── pre_mask_register
        ├── 13_midRT_T2.nii.gz
        ├── randomXYZ.nii.gz
        └── subject1.nii.gz
```
The predicted masks in `.mha` format will be saved at `AbsPathOutput/mri-head-neck-segmentation` <br>

## Reference
The development of these models was done by using [nnUNet](https://github.com/MIC-DKFZ/nnUNet), [MedNeXt](https://github.com/MIC-DKFZ/MedNeXt), and [MONAI SegResNetDS](https://github.com/Project-MONAI/tutorials).

## Citation
If you use these models, please consider citing the peer-reviewed papers/preprints. <br>
BraTS
```
TODO: will be added after LNCS publishes.
```
Head&Neck
```
TODO: will be added after LNCS publishes.
```
AutoPET
```
@misc{astaraki2024lesionsegmentationwholebodymultitracer,
      title={Lesion Segmentation in Whole-Body Multi-Tracer PET-CT Images; a Contribution to AutoPET 2024 Challenge}, 
      author={Mehdi Astaraki and Simone Bendazzoli},
      year={2024},
      eprint={2409.14475},
      archivePrefix={arXiv},
      primaryClass={eess.IV},
      url={https://arxiv.org/abs/2409.14475}, 
}
```
