# Out-of-distribution generalization benchmarks for image recognition models
This repository contains code for evaluating the out-of-distribution generalization performance of various image recognition models. Currently, performance on the following benchmarks are evaluated:

* [ImageNet-C](https://github.com/hendrycks/robustness)
* [ImageNet-P](https://github.com/hendrycks/robustness)
* [ImageNet-A](https://github.com/hendrycks/natural-adv-examples)
* [ImageNet-Sketch](https://github.com/HaohanWang/ImageNet-Sketch)
* [Stylized ImageNet](https://github.com/rgeirhos/texture-vs-shape/tree/master/stimuli/style-transfer-preprocessed-512)
* Adversarial robustness

The following models are currently evaluated:

* `resnext101_32x48d_wsl`: the largest ResNeXt-WSL model released by Facebook AI Research.
* `tf_efficientnet_l2_ns`: the largest noisy student model released by Google AI.
* `tf_efficientnet_l2_ns_475`: lower resolution version of the above.

All simulation results reported on this page can be found in the [`results`](https://github.com/eminorhan/ood-benchmarks/tree/master/results) folder. 

## Requirements
The code was written and tested with:

* torch == 1.3.0
* torchvision == 0.4.0
* foolbox == 1.8.0

Other versions may or may not work. In addition, you will need to download the datasets listed above in order to replicate the results.

## Results
| Model | IN | IN-A | IN-C | IN-P | Stylized IN | IN-Sketch | Adv. acc. |
| ----- |:--:|:----:|:----:|:----:|:-----------:|:---------:|:---------:|
| `resnext101_32x48d_wsl`     | TBD | TBD | TBD | TBD | 42.8 | 59.1 | TBD |
| `tf_efficientnet_l2_ns`     | TBD | TBD | TBD | TBD | 39.0 | 52.7 | TBD |
| `tf_efficientnet_l2_ns_475` | TBD | TBD | TBD | TBD | 61.8 | 53.6 | TBD |

## Replication
The results reported on this page can be reproduced as follows:

1. To evaluate the ImageNet validation accuracy of the models, run [`evaluate_validation.py`](), e.g.:
```
python evaluate_validation.py /IMAGENET/DIR/ --model-name 'resnext101_32x16d_wsl'
```
Here and below, `model-name` should be one of `'resnext101_32x8d'`, `'resnext101_32x8d_wsl'`, `'resnext101_32x16d_wsl'`, `'resnext101_32x32d_wsl'`, `'resnext101_32x48d_wsl'`. `/IMAGENET/DIR/` is the top-level ImageNet directory (it should contain a `val` directory containing the validation images).


2. To evaluate the models on ImageNet-A, run [`evaluate_imageneta.py`](), e.g.:
```
python3 evaluate_imageneta.py /IMAGENETA/DIR/ --model-name 'resnext101_32x16d_wsl'
```
where `/IMAGENETA/DIR/` is the top-level ImageNet-A directory.


3. To evaluate the models on ImageNet-C, run [`evaluate_imagenetc.py`](), e.g.:
```
python3 evaluate_imagenetc.py /IMAGENETC/DIR/ --model-name 'resnext101_32x16d_wsl'
```
where `/IMAGENETC/DIR/` is the top-level ImageNet-C directory.


4. To evaluate the models on ImageNet-P, run [`evaluate_imagenetp.py`](), e.g.:
```
python3 evaluate_imagenetc.py /IMAGENETP/DIR/ --model-name 'resnext101_32x16d_wsl' --distortion-name 'gaussian_noise'
```
where `/IMAGENETP/DIR/` is the top-level ImageNet-P directory, and `distortion-name` should be one of `'gaussian_noise'`, `'shot_noise'`, `'motion_blur'`, `'zoom_blur'`, `'brightness'`, `'translate'`, `'rotate'`, `'tilt'`, `'scale'`, `'snow'`.


5. To run black-box adversarial attacks on the models, run [`evaluate_blackbox.py`](), e.g.:
```
python3 evaluate_blackbox.py /IMAGENET/DIR/ --model-name 'resnext101_32x16d_wsl' --epsilon 0.06
```
where `epsilon` is the normalized perturbation size.


6. To run white-box adversarial attacks on the models, run [`evaluate_whitebox.py`](), e.g.:
```
python3 evaluate_whitebox.py /IMAGENET/DIR/ --model-name 'resnext101_32x16d_wsl' --epsilon 0.06 --pgd-steps 10
```
where `epsilon` is the normalized perturbation size and `pgd-steps` is the number of PGD steps.


7. To evaluate the shape biases of the models, run [`evaluate_shapebias.py`](), e.g.:
```
python3 evaluate_shapebias.py /CUECONFLICT/DIR/ --model-name 'resnext101_32x16d_wsl'
```
where `/CUECONFLICT/DIR/` is the directory containing the shape-texture cue-conflict images. We provide these images in [this location](https://github.com/eminorhan/resnext-wsl/tree/master/cueconflict_images). They are copied from Robert Geirhos's [texture-vs-shape](https://github.com/rgeirhos/texture-vs-shape) repository (see [here](https://github.com/rgeirhos/texture-vs-shape/tree/master/stimuli/style-transfer-preprocessed-512)), but with the non-conflicting images (images with the same shape and texture category) removed.


8. To visualize the learned features of the models, run [`visualize_features.py`](), e.g.:
```
python3 visualize_features.py /IMAGENET/DIR/ --model-name 'resnext101_32x16d_wsl'
```

## Acknowledgments
The code here utilizes code and stimuli from the [texture-vs-shape](https://github.com/rgeirhos/texture-vs-shape) repository by Robert Geirhos, the [robustness](https://github.com/hendrycks/robustness) and [natural adversarial examples](https://github.com/hendrycks/natural-adv-examples) repositories by Dan Hendrycks, and the [ImageNet example](https://github.com/pytorch/examples/tree/master/imagenet) from PyTorch. We are also grateful to the authors of [Mahajan et al. (2018)](https://arxiv.org/abs/1805.00932) for making their pre-trained ResNeXt WSL models publicly available.
