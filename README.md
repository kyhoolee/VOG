## Estimating Example Difficulty using Variance of Gradients

This repository contains source code necessary to reproduce some of the main results in [the paper](https://arxiv.org/abs/2008.11600):

**If you use this software, please consider citing:**
    
    @article{agarwal2020estimating, 
    title={Estimating Example Difficulty using Variance of Gradients},
    author={Agarwal, Chirag and Hooker, Sara},
    journal={arXiv preprint arXiv:2008.11600},
    year={2020}
    }
    
## 1. Setup

### Installing software
This repository is built using a combination of TensorFlow and PyTorch. You can install the necessary libraries by pip installing the requirements text file `pip install -r ./requirements_pytorch.txt` & `pip install -r ./requirements_tf.txt`

## 2. Usage
### Toy experiment
[toy_script.py](toy_script.py) is the script for running toy dataset experiment. You can analyze the training/testing data at diffferent stages of the training, viz. Early, Middle, and Late, using the flags `split` and `mode`. The `vog_cal` flag enables visualizing different versions of VOG scores such as the raw score, class normalized, or the absolute class normalized scores. 

#### Examples
Running `python3 toy_script.py --split test --mode early --vog_cal normalize` generates the toy dataset decision boundary figure along with the relation between the perpendicular distance of individual points from the decision boundary and the VOG scores. The respective figures are:

<p align="center">
    <img src="figures/toy_dataset_decision_boundary.jpg" width=250px>
    <img src="figures/test_early_normalize.jpg" width=250px>
</p>
<p align="left"><i>Left: The visualization of the toy dataset decision boundary with the testing data points. The Multiple Layer Perceptron model achieves 100% training accuracy. Right: The scatter plot between the Variance of Gradients (VoGs) for each testing data point and their perpendicular distance shows that higher scores pertain to the most
challenging examples (closest to the decision boundary)</i></p>

### ImageNet
The main scripts for the ImageNet experiments are in the `./imagenet/` folder. 

1. Before calculating the VOG scores you would need to store the gradients of the respective images in the `./scripts/train.txt/` file using model snapshots. For demonstration purpose, we have shared the model weights at snapshot `32000`. To store the gradients for the imagenet dataset (stored as <path>/imagenet_dir/train), we run the shell script [train_get_gradients.sh](train_get_gradients.sh) like:
`source train_get_gradients.sh 32000 ./imagenet/train_results/ 9 ./scripts/train.txt/`

2. For this repo, we have generated the gradients for 10 images at 9 different snapshots and are stored in `./imagenet/train_results/`. To generate the error rate performance at different VOG deciles run [train_visualize_grad.py](train_visualize_grad.py) using the following command.
`python train_visualize_grad.py`

<p align="center">
    <img src="imagenet/imagenet_error_plot_late.jpg" width=250px>
</p>

In this work, we hypothesize that examples that a model has difficulty learning will exhibit higher variance in gradient updates over the course of training. On the
other hand, the gradient updates for the relatively easier examples are expected to stabilize early in training and converge to a narrow range of values.
<p align="center">
    <img src="imagenet/train_vog_sample.jpg" width=700px>
</p>
<p align="left"><i>Left: The image sample is not learned by the model in the early stage resulting in a higher VOG score. Middle: At the end of the middle training stage, the model is able to classify the image correctly and hence the VOG score starts to decrease. Right: The example is easily learned by the model before the last training stage and therefore individual gradients stabilize resulting in a low VOG score.</i></p>

## 4. Licenses
Note that the code in this repository is licensed under MIT License, but, the pre-trained condition models used by the code have their own licenses. Please carefully check them before use. 

## 5. Questions?
If you have questions/suggestions, please feel free to [email](mailto:chiragagarwall12@gmail.com) or create github issues.     
