# fracbnn [![DOI](https://zenodo.org/badge/327196438.svg)](https://zenodo.org/badge/latestdoi/327196438)

Code in this repo is for reproducing the results in the paper [FracBNN: Accurate and FPGA-Efficient Binary Neural Networks with Fractional Activations](https://arxiv.org/abs/2012.12206).


## Structure
```
|   cifar10.py (training script)
|
└── models/
|       |   binput_resnet20_pg.py
|
└───utils/
|   |   quantization.py
|   |   utils.py
|
└───pretrained-models/
|   |   binput-resnet20-pg.pt (binary input layer, W/A=1/1.4)
```

## Dependency
```
Python 3.6.8
torch 1.6.0
torchvision 0.7.0
numpy 1.16.4
```

## Usage
First, make sure to import the right model. This can be done by choosing the ```candidates``` in **line 27** in the training script ```cifar10.py```.

#### Test Only:
- Run ```python cifar10.py -gpu 0 -t -r ./pretrained-models/binput-resnet20-pg.pt```

#### Train: Two-Step Training:
- Step 1: Binary activations, floating-point weights.
    - In ```utils/quantization.py```, use ```self.binarize = nn.Sequential()``` in ```BinaryConv2d()```, or modify ```self.binarize(self.weight)``` to ```self.weight``` in ```PGBinaryConv2d()```.
    - Run ```python cifar10.py -gpu 0 -s```
- Step 2: Binary activations, binary weights.
    - In ```utils/quantization.py```, use ```self.binarize = FastSign()``` in ```BinaryConv2d()```, or ```self.binarize(self.weight)``` in ```PGBinaryConv2d()```.
    - Run ```python cifar10.py -gpu 0 -f -r /path/to/model_checkpoint.pt -s```

## Results
Dataset: CIFAR-10; Model: ResNet-20
| Model         | Top-1 %   |
| ------------- | --------- |
| binput; W/A=1/1.4 (PG)  | 89.1      |

