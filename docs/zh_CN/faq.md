# FAQ

>>
* Q: 多卡评估时，为什么每张卡输出的精度指标不相同？
* A: 目前PaddleClas基于fleet api使用多卡，在多卡评估时，每张卡都是单独读取各自part的数据，不同卡中计算的图片是不同的，因此最终指标也会有微量差异，如果希望得到准确的评估指标，可以使用单卡评估。


>>
* Q: 在配置文件的`TRAIN`字段中配置了`mix`的参数，为什么`mixup`的数据增广预处理没有生效呢？
* A: 使用mixup时，数据预处理部分与模型输入部分均需要修改，因此还需要在配置文件中显式地配置`use_mix: True`，才能使得`mixup`生效。


>>
* Q: 评估和预测时，已经指定了预训练模型所在文件夹的地址，但是仍然无法导入参数，这么为什么呢？
* A: 加载预训练模型时，需要指定预训练模型的前缀，例如预训练模型参数所在的文件夹为`output/ResNet50_vd/19`，预训练模型参数的名称为`output/ResNet50_vd/19/ppcls.pdparams`，则`pretrained_model`参数需要指定为`output/ResNet50_vd/19/ppcls`，PaddleClas会自动补齐`.pdparams`的后缀。


>>
* Q: 在评测`EfficientNetB0_small`模型时，为什么最终的精度始终比官网的低0.3%左右？
* A: `EfficientNet`系列的网络在进行resize的时候，是使用`cubic插值方式`(resize参数的interpolation值设置为2)，而其他模型默认情况下为None，因此在训练和评估的时候需要显式地指定resize的interpolation值。具体地，可以参考以下配置中预处理过程中ResizeImage的参数。
```
VALID:
    batch_size: 16
    num_workers: 4
    file_list: "./dataset/ILSVRC2012/val_list.txt"
    data_dir: "./dataset/ILSVRC2012/"
    shuffle_seed: 0
    transforms:
        - DecodeImage:
            to_rgb: True
            to_np: False
            channel_first: False
        - ResizeImage:
            resize_short: 256
            interpolation: 2
        - CropImage:
            size: 224
        - NormalizeImage:
            scale: 1.0/255.0
            mean: [0.485, 0.456, 0.406]
            std: [0.229, 0.224, 0.225]
            order: ''
        - ToCHWImage:
```

>>
* Q: 如果想将保存的`pdparams`模型参数文件转换为早期版本(Paddle1.7.0之前)的零碎文件(每个文件均为一个单独的模型参数)，该怎么实现呢？
* A: 可以首先导入`pdparams`模型，之后使用`fluid.io.save_vars`函数将模型保存为零散的碎文件。示例代码如下，最终所有零散文件会被保存在`path_to_save_var`目录下。
```
fluid.load(
        program=infer_prog, model_path=args.pretrained_model, executor=exe)
state = fluid.io.load_program_state(args.pretrained_model)
def exists(var):
    return var.name in state
fluid.io.save_vars(exe, "./path_to_save_var", infer_prog, predicate=exists)
```


>>
* Q: python2下，使用visualdl的时候，报出以下错误，`TypeError: __init__() missing 1 required positional argument: 'sync_cycle'`，这是为什么呢？
* A: 目前visualdl仅支持在python3下运行，visualdl需要是2.0以上的版本，如果visualdl版本不对的话，可以通过以下方式进行安装：`pip3 install visualdl==2.0.0b8  -i https://mirror.baidu.com/pypi/simple`cd
