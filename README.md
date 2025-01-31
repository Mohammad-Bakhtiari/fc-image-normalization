# Image Normalization FeatureCloud App

## Description
Using the Image Normalization app, users can normalize and/or standardize their local image datasets using different FeatureCloud platform techniques.
This app supports image datasets in NumPy files.

## Input
- `.npz`: NpmPy compressed files are supposed to have `data` and `targets` keys for samples and target values.
- train.npy containing the local training data  
- test.npy containing the local test data
`train.npy` and `test.npy` should include the same number of samples, and the structure should be the same as [Data distributor](https://github.com/FeatureCloud/fc-data-distributor) app.
## Output
- train.npy: Normalized training data 
- test.npy: Normalized test data
Both files have the same structure as the input files.
  
## Methods
- variance:     x<sub>j</sub> = (x<sub>j</sub> - &mu;<sub>j</sub>) / &sigma;<sub>j</sub>

Where
- x: inputs image
- j: channel index
- &mu;<sub>j</sub>: global mean for channel j
- &sigma;<sub>j</sub>: global standard deviation for channel j
  
    

## Workflows
Image Normalization app can be used alongside the following apps in the same workflow:
- Pre: Cross-Validation for Image datasets
- Post: Various Analysis apps that support `.npy` and `.npz` format for their input files (e.g., Deep Learning)

![Workflow](data/images/ImageNormalization.png)
## Config
Following config information should be included in the `config.yml` to run the Image Normalization app in a workflow:
```
fc_image_normalization:
  local_dataset:
    train: train.npy
    test: test.npy
    target_value: same-sep
  method: variance
  logic:
    mode: file
    dir: .
  use_smpc: false
  result:
    train: train.npy
    test: test.npy
```
### Run Image Normalization

#### Prerequisite

To run the Image Normalization application, you should install Docker and FeatureCloud pip package:

```shell
pip install featurecloud
```

Then either download the Image Normalization image from featurecloud docker repository:

```shell
featurecloud app download featurecloud.ai/fc_image_normalization
```

Or build the app locally:

```shell
featurecloud app build featurecloud.ai/fc_image_normalization
```

You can use provided example data or your own data with the desired settings in the `config.yml` file.
For `.npy` format you should set the `target_value` option to `same_sep`.

#### Running app

You can run fc_image_normalization as a standalone app in the [FeatureCloud test-bed](https://featurecloud.ai/development/test) or [FeatureCloud Workflow](https://featurecloud.ai/projects). You can also run the app using CLI:

```shell
featurecloud test start --app-image featurecloud.ai/fc_image_normalization --client-dirs './sample/c1,./sample/c2' --generic-dir './sample/generic'
```
