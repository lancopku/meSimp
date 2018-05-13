# meSimp

The codes were used for experiments on MNIST with _Training Simplification and Model Simplification for Deep Learning: A Minimal Effort Back Propagation Method_ [[pdf]](https://arxiv.org/pdf/1711.06528) by Xu Sun, Xuancheng Ren, Shuming Ma, Bingzhen Wei, Wei Li, Houfeng Wang. Codes are writen in C#.


# Introduction

We propose a simple yet effective technique to simplify the training and the resulting model of neural networks. The technique is based on the top-k selection of the gradients in back propagation.

Based on the sparsified gradients from meProp, we further simplify the model by **eliminating the rows or columns that are seldom updated**, which will reduce the computational cost both in the training and decoding, and potentially accelerate decoding in real-world applications. We name this method **meSimp** (*m*inimal *e*ffort *simp*lification).

The model simplification results show that we could adaptively simplify the model which could often be **reduced by around 9x, without any loss on accuracy or even with improved accuracy**.

The following figure is an illustration of the idea of meSimp.

![An illustration of the idea of meSimp.](./docs/mesimp.svg)

**TL;DR**: Training with meSimp can substantially reduce the size of the neural networks, without loss on accuracy or even with improved accuracy. The method works with different neural models (MLP and LSTM). The trained reduced networks work better than normally-trained dimensional networks of the same size.

Results on test set (please refer to the paper for detailed results and experimental settings):

| Method (Adam, CPU)      | Dimension (Avg.)  | Test (%)          |
| ----------------------- | ----------------- | ----------------- |
| Parsing (MLP 500d)      | 500               | 89.80             |
| Parsing (meProp top-20) | **51 (10.2%)**    | **90.11 (+0.31)** |
| POS-Tag (LSTM 500d)     | 500               | 97.22             |
| POS-Tag (meProp top-20) | **60 (12.0%)**    | **97.25 (+0.03)** |
| MNIST (MLP 500d)        | 500               | 98.20             |
| MNIST (meProp top-160)  | **154 (30.8%)**   | **98.31 (+0.11)** |

See [[pdf]](https://arxiv.org/pdf/1711.06528) for more details, experimental results, and analysis.


# Usage

## Requirements

* Targeting Microsoft .NET Framework 4.6.1+
* Compatible versions of Mono should work fine (tested Mono 5.0.1)
* Developed with Microsoft Visual Studio 2017


## Dataset

MNIST: Download from [link](http://yann.lecun.com/exdb/mnist/). Extract the files, and place them at the same location with the executable.


## Run

Compile the code first, or use the executable provided in releases.

Then
```
nnmnist.exe <config.json>
```
or
```
mono nnmnist.exe <config.json>
```
where <config.json> is a configuration file. There is [an example configuration file](./src/csharp/nnmnist/default.json) in the source codes. The example configuration file runs meSimp. The output will be written to a file at the same location with the executable. 


# Citation

bibtex:
```
@article{sun17mesimp,
  title     = {Training Simplification and Model Simplification for Deep Learning: A Minimal Effort Back Propagation Method},
  author    = {Xu Sun and Xuancheng Ren and Shuming Ma and Bingzhen Wei and Wei Li and Houfeng Wang},
  journal   = {CoRR},
  volume    = {abs/1711.06528},
  year      = {2017}
}
```
