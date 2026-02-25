# BIFROST: A method for registering diverse imaging datasets

[https://doi.org/10.5061/dryad.8pk0p2nx1](https://doi.org/10.5061/dryad.8pk0p2nx1)

This repository provides four things:

*   the Functional Drosophila Atlas (FDA)
*   bridging transformations between the FDA and JRC 2018
*   the code that was used to generate the FDA and recapitulated ANTs format transformations
*   a replication dataset

## Description of the data and file structure

### FDA

Three closely related but distinct FDA images are provided, a thresholded version, an unthresholded version and an (unthresholded) version that complies strictly with the NIfTI-1 specification. A bridging transformation is available only for the thresholded version. The NIfTI-1 compliant and unthresholded versions share the same raster data. The thresholded and unthresholded versions share the same metadata but have slightly different raster data. The NIfTI compliant image follows the NIfTI convention for the spatial coordinates of +x = Right, +y = Anterior, +z = Superior, which is not related by a scaling transform to the voxel coordinates. The spatial coordinates of the other versions are related to the voxel coordinates by the identity. If you do not intend to bridge your dataset to other atlases, it is recommended that you use the NIfTI compliant version.

### Bridging transformations

#### thresholded FDA to JRC 2018

##### BIFROST format

`thresholded_FDA_to_JRC2018_female.h5`

Thresholded FDA to JRC 2018 central brain female transform for use with `bifrost transform`. As this was obtained from the thresholded FDA, it can be applied to images registered to the unthresholded FDA but is "undefined" outside of the support of the thresholded image. It is strongly recommended that that you use this where possible as `bifrost` design not expose foot-guns present in ANTs.

Does not support coordinate transformation.

Usage:

```
mkdir thresholded_FDA_to_JRC2018_female
mv thresholded_FDA_to_JRC2018_female.h5 thresholded_FDA_to_JRC2018_female/transform.h5
bifrost transform thresholded_FDA_to_JRC2018_female IMAGE_PATH
```

##### ANTs format

`thresholded_FDA_to_JRC2018_female_recapitulated.tar.gz`

ANTs format transform derived from the `bifrost` transform. Supports point transforms. Follows the convention established in the `ants.apply_transforms` docstring. In particular, note the `transformlist` order and the `whichtoinvert` foot-gun.

Archive contents:

```
fwd
├── aff.mat                                                 # affine transform as accepted by ants.apply_transforms
└── forward_warp.nii                                        # warp field image as accepted by ants.apply_transforms
```

#### JRC 2018 to thresholded FDA

##### ANTs format

`JRC2018_female_to_thresholded_FDA_recapitulated.tar.gz`

ANTs format transform derived from the `bifrost` transform. Supports point transforms. Follows the convention established in the `ants.apply_transforms` docstring. In particular, note the `transformlist` order and the `whichtoinvert` foot-gun.

Archive contents:

```
inv
├── aff.mat                                                 # affine transform as accepted by ants.apply_transforms
└── inverse_warp.nii                                        # warp field image as accepted by ants.apply_transforms
```

Note that due to aforementioned `whictoinvert`foot-guns, `aff.mat` is byte-identical to its counterpart in `thresholded_FDA_to_JRC2018_female_recapitulated.tar.gz`

#### To other reference brains

Bridging transformations to other reference brains can be obtained by composition using [navis](https://github.com/navis-org/navis-flybrains) or the [Jefferis lab](https://github.com/jefferislab/BridgingRegistrations) bridging resources.

### Replication

`replication_dataset.tar.gz` can be used to replicate our LC11 registration
accuracy result using the `bifrost` pipeline. Refer to the `bifrost` README for
instructions and full data description. Cluster execution recommended.

Archive contents:

```
replication_dataset
├── config.yaml
├── data
│   ├── fly_0
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   ├── fly_1
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   ├── fly_2
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   ├── fly_3
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   ├── fly_4
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   ├── fly_5
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   ├── fly_6
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   ├── fly_7
│   │   ├── green
│   │   │   └── lc11.nii
│   │   └── structural_image.nii
│   └── fly_8
│       ├── green
│       │   └── lc11.nii
│       └── structural_image.nii
└── templates
    └── FDA.nii
```

## Code/Software

Our production software (`bifrost`) is available on [zenodo](https://zenodo.org/doi/10.5281/zenodo.11097259) and [github](https://github.com/ClandininLab/bifrost). It provides utilities and a pipeline for template building and multi-modal registration.

This repository additionally provides the scripts used to generate the FDA and the ANTs format recapitulated transforms.

It is not recommended that you use the FDA generation script. The production release provides a more user friendly and mature utility for template building.
