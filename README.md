# **FBNet**

This repository reproduces the results of the following paper:

[**FBNet: Hardware-Aware Efficient ConvNet Design via Differentiable Neural Architecture Search**](https://arxiv.org/pdf/1812.03443.pdf)  
Bichen Wu1, Xiaoliang Dai, Peizhao Zhang, Yanghan Wang, Fei Sun, Yiming Wu, Yuandong Tian, Peter Vajda, Yangqing Jia, Kurt Keutzer (Fasebook Research)

Layers to Search are from a [FacebookResearch repository](https://github.com/facebookresearch/maskrcnn-benchmark/tree/master/maskrcnn_benchmark/modeling/backbone)
Utils stuff is taken from [DARTS repository](https://github.com/quark0/darts/blob/master/cnn/utils.py)

# Advantages

* Building blocks (searched layers) was taken from the FacebookResearch repository
(Quick Note: their repo consists files with names fbnet*, but doesn't include any fbnet architecture from their paper)
* All hyperparameters exactly as in the original paper
* Latency Calculation Code
* Successfully Tested on Cifar10
* Logging. You can find all my logs and tensorboards into *SAVED_LOGS/supernet_training* (for supernet training) and *SAVED_LOGS/architectures_training* (for architectures training)

#### TODO List
* Multi-GPU Support
* ImageNet Training. It will take 27 hours x 8 GPU 

# Results, Cifar10

> The architectures are not SOTAs: we **search only for filters' sizes** (these numbers are good for the simple architecture) and the goal is to **Reduce Inference Time for Your Device**

> FacebookResearch didn't share latencies for their test machines, so, I couldn't prove their latencies results, but I have builded and trained theier proposed architectures:

| FBNet Architecture | **top1** validation accuracy | **top3** validation accuracy | 
| ------ | ------ | ------ |
| FBNet-A | 78.8% | 95.4% |
| FBNet-B | 82% | 96% |
| FBNet-C | 79.9% | 95.6% |
| FBNet-s8 (for Samsung Galaxy S8) | 79.6% | 95.7% |
| FBNet-iPhoneX | 76.2% | 94.3% |
| ------ | ------ | ------ |
| fbnet_cpu_sample1 | 82.8% | 98.9% |
| fbnet_cpu_sample2 | 80.6% | 95.7% |

**Note: be cautious!** these numbers are just validation's the bests (without confidence intervals, measured in a single run). Do not use these numbers to make decisions. They are here to compliment the tensorboards in the `SAVED_LOGS` directory. The reason why I don't split data into validation and test is *in the next note*. *I plan to test on Imagenet someday and calculate the final numbers*

**Note**: as it was stated in the paper and according to my results, if we train with small images (as cifar's 32x32), we can see a lot of 'skip' layers into the resulting architecture. I feel, for cifar10 we should search for less number of layers

# Code Structure and Training Pipeline, Cifar10

The repository consists of 2 Neural Net Models:

**(1)** FBNet Searched Architectures. All tools in the *architecture_functions* folder

**(2)** Stochastic SuperNet to search for new architectures. All tools in the *supernet_functions* folder

> They use different functions and architectures specification. Functions used by both Nets are in the folders: *general_functions* (utilities) and *fbnet_building_blocks* (modified code of facebookresearch team)

I encourage you to visit **TRAINIG_DETAILS.md in this folder** for details and instructions.

# Dependencies

I have tested the code with the following dockerfile: [DOCKERFILE](https://github.com/facebookresearch/maskrcnn-benchmark/blob/master/docker/Dockerfile) (Pytorch 0.4.1 Nightly)

btw I think it should work well with Pytorch 0.4.0+

# License

MIT
