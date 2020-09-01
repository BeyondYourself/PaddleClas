## Overview

The ResNeSt series was proposed in 2020. The original resnet network structure has been improved by introducing K groups and adding an attention module similar to SEBlock in different groups, the accuracy is greater than that of the basic model ResNet, but the parameter amount and flops are almost the same as the basic ResNet.

RegNet was proposed in 2020 by Facebook to deepen the concept of design space. Based on AnyNetX, the model performance is gradually improved by shared bottleneck ratio, shared group width, adjusting network depth or width and other strategies. What's more, the design space structure is simplified, whose interpretability is also be improved. The quality of design space is improved while its diversity is maintained. Under similar conditions, the performance of the designed RegNet model performs better than EfficientNet and 5 times faster than EfficientNet.

## Accuracy, FLOPs and Parameters

| Models           | Top1 | Top5 | Reference<br>top1 | Reference<br>top5 | FLOPS<br>(G) | Parameters<br>(M) |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| ResNeSt50_fast_1s1x64d        | 0.8035 | 0.9528|  0.8035 |            -| 8.68     | 26.3   |
| ResNeSt50        | 0.8102 | 0.9542|  0.8113 |            -| 10.78     | 27.5   |
| RegNetX_4GF        | 0.7850 | 0.9416|  0.7860 |            -| 8.0     | 22.1   |
