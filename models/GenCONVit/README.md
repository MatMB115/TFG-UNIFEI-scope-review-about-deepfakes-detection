# Deepfake Video Detection Using Generative Convolutional Vision Transformer
Deressa Wodajo, Solomon Atnafu, Zahid Akhtar

This repository contains the implementation code for **Deepfake Video Detection Using Generative Convolutional Vision Transformer (GenConViT)** paper. Find the full paper on arXiv [here](https://arxiv.org/abs/2307.07036).

<br/><br/>
![The Proposed GenConViT Deepfake Detection Framework](img/genconvit.png)
<p align="center">The Proposed GenConViT Deepfake Detection Framework</p>

<p style="text-align: justify;">
Deepfakes have raised significant concerns due to their potential to spread false information and compromise digital media integrity. In this work, we propose a Generative Convolutional Vision Transformer (GenConViT) for deepfake video detection. Our model combines ConvNeXt and Swin Transformer models for feature extraction, and it utilizes Autoencoder and Variational Autoencoder to learn from the latent data distribution. By learning from the visual artifacts and latent data distribution, GenConViT achieves improved performance in detecting a wide range of deepfake videos. The model is trained and evaluated on DFDC, FF++, DeepfakeTIMIT, and Celeb-DF v2 datasets, achieving high classification accuracy, F1 scores, and AUC values. The proposed GenConViT model demonstrates robust performance in deepfake video detection, with an average accuracy of 95.8% and an AUC value of 99.3% across the tested datasets. Our proposed model addresses the challenge of generalizability in deepfake detection by leveraging visual and latent features and providing an effective solution for identifying a wide range of fake videos while preserving media integrity.
</p>

## GenConViT Model Architecture

The GenConViT model consists of two independent networks and incorporates the following modules:
<pre>
    Autoencoder (ed),
    Variational Autoencoder (vae), and
    ConvNeXt-Swin Hybrid layer
</pre>

The code in this repository enables training and testing of the GenConViT model for deepfake detection.

## Table of Contents

- [Requirements](#requirements)
- [Usage](#usage)
  - [Model Training](#model-training)
  - [Model Testing](#model-testing)
- [Results](#results)

## Requirements
<pre>
    * Python 3.x
    * PyTorch
    * numpy
    * torch
    * torchvision
    * tqdm
    * decord
    * dlib
    * opencv
    * face_recognition
    * timm
</pre>

## Usage

1. Clone this repository:

```bash
git clone https://github.com/erprogs/GenConViT
```

2. Install the required dependencies:

```bash
pip install -r requirements.txt
```

## Model Training

To train the GenConViT model, follow these steps:

1. Prepare the training data, or use the sample training data provided:
    * Ensure that the training data is located in the specified directory path.
    * The training data should be organized in the required format. The `fake` directory contains images that are fake, while the `real` directory contains images that are real.
<pre>
    train:
        - fake
        - real
    valid:
        - fake
        - real
    test:
        - fake
        - real
</pre>
 

2. Run the training script:

```bash
python train.py
    -d <training-data-path>
    -m <model-variant>
    -e <num-epochs>
    -p <pretrained-model-file>
    -b <batch-size>
    -t
```

`<training-data-path>`: Path to the training data.<br/>
`<model-variant>`: Specify the model variant (`ed` for Autoencoder or `vae` for Variational Autoencoder).<br/>
`<num-epochs>`: Number of epochs for training.<br/>
`<pretrained-model-file>` (optional): Specify the filename of a pretrained model to continue training.<br/>
`-b` (optional): Batch size for training. Default is 32.<br/>
`-t` (optional): Run the test on the test dataset after training.

The model weights and metrics are saved in the `weight` folder.

**Example usage:** 
```bash
python train.py --d sample_train_data --m vae -e 5 -t y
```
```bash
python train.py --d sample_train_data --m ed --e 5 -t y
```

## Model Testing
**Deepfake Detection using GenConViT**

To make prediction using the trained GenConViT model, follow these steps:

1. Download the pretrained model from [Huggingface](https://huggingface.co/Deressa/GenConViT) and save it in the `weight` folder.

Network A (ed) 
```bash
wget https://huggingface.co/Deressa/GenConViT/resolve/main/genconvit_ed_inference.pth
```
Network B (vae)
```bash
wget https://huggingface.co/Deressa/GenConViT/resolve/main/genconvit_vae_inference.pth
```

2. Run the prediction script:

To run the code, use the following command:

```bash
python prediction.py \
    --p <path-to-video-data> \
    --f <number-of-frames> \
    --d <dataset> \
    --n <model-variant>
    --fp16 <half-precision>
```
  `<path-to-video-data>`: Path to the video data or `[ dfdc, faceforensics, timit, celeb ]`.<br/>
  `<number-of-frames>`: Specify the number of frames to be extracted for the video prediction. The default is 15 frames.<br/>
  `<model-variant>`: Specify the model variant (`ed` or `vae` or both:genconvit).<br/>
  `<dataset>`: the dataset type. `[ dfdc, faceforensics, timit, celeb ]` or yours.<br/>
  `<half-precision>`: Enable half-precision (float16).

**Example usage:** 
```bash
python prediction.py --p DeepfakeTIMIT --d timit --f 10 
```
To use ed, or vae variant:

``` 
python prediction.py --p sample_prediction_data --n vae --f 10
```
```
python prediction.py --p sample_prediction_data --n ed --f 10
```
```
python prediction.py --p DeepfakeTIMIT --n vae --d timit --f 10
```

## Results

The results of the model prediction documented in the paper can be found in the `result` directory. 
```bash
python result_all.py
```

## Bibtex
```bash
@misc{wodajo2023deepfake,
      title={Deepfake Video Detection Using Generative Convolutional Vision Transformer}, 
      author={Deressa Wodajo and Solomon Atnafu and Zahid Akhtar},
      year={2023},
      eprint={2307.07036},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

## Acknowledgement

This research was funded by Addis Ababa University Research Grant for the Adaptive Problem-Solving Research. Reference number RD/PY-183/2021. Grant number AR/048/2021.