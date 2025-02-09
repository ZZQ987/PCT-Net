# PCT-Net

## 介绍  

这是我的数学建模大作业的代码仓库  
我fork了原代码仓库，并添加了以下内容：  
1. 一份代码内容介绍文件（代码文件介绍.md），来说明代码仓库整体架构  
2. 运行测试代码并添加注释，注释内容主要集中在pct_net.py和pct_functions.py  
3. 自行测试的结果（附录图，与论文附录C相同；examples文件夹内容）

**勘误** ：原代码实现中，/iharm/model/losses.py中的Gram() 函数实现有问题。已修正

## 测试方法

1. 配置环境  
2. 按照下面Testing的要求，准备图片，放入自己创建的一个文件夹  
3. 运行scripts/evaluate_folder.py，需要指定待处理图片文件夹位置，以及模型配置等  
4. 得到结果   
 
---

This is the official repository of PCT-Net (CVPR2023) by [Rakuten Institute of Technology, Rakuten Group, Inc.](https://rit.rakuten.com/)
- [Paper](https://openaccess.thecvf.com/content/CVPR2023/papers/Guerreiro_PCT-Net_Full_Resolution_Image_Harmonization_Using_Pixel-Wise_Color_Transformations_CVPR_2023_paper.pdf)
- [Supp](https://openaccess.thecvf.com/content/CVPR2023/supplemental/Guerreiro_PCT-Net_Full_Resolution_CVPR_2023_supplemental.pdf)


## Requirements

In order to train our model, a GPU and CUDA is required. For inference (Testing), a CPU is sufficient. However, you need to remove +cu116 from the file `requirements.txt` and add `--gpu cpu` to the commands to run the code through the CPU     

## Environment

We built the code using Python 3.9 on Linux with NVIDIA GPUs and CUDA 11.6. We provide a `Dockerfile` to run our code. Alternatively, the required packages can be installed using the `requirements.txt` file.

```
pip install -r requirements.txt
```

## Dataset

We use the [iHarmony4](https://github.com/bcmi/Image-Harmonization-Dataset-iHarmony4) dataset for training and testing. The dataset directory needs to be updated in the files `config.yaml` and `config_test_FR.yml`. 
Since the dataset contains some images with a very high resolution, we resize the HAdobe5k to not be greater than 1024 pixels on the biggest side using `./notebooks/resize_dataset`.

## Training

To train the model from scratch, we provide two different models, a CNN-based and a ViT based model. The training setting are defined in the files `models/PCTNet_CNN.py` and `models/PCTNet_ViT.py`. 
The different architecture options are listed in the config file `iharm/mconfigs/base.py` and should be changed in the model files.
To start training, simply run the shell file. 

For PCTNet_CNN:

```
runs/train_PCTNet_CNN.sh
```

For PCTNet_ViT:

```
runs/train_PCTNet_ViT.sh
```

## Testing

We provide a script to reproduce the results reported in our paper. 
Our pretrained models can be found in `pretrained_models`.
To evaluate our models simply specify `pretrain_path` in `runs/test_PCTNet.sh` and then just run:

```
runs/test_PCTNet.sh
```

To apply our method to any composite image, you can use the script `scripts/evaluate_folder.py`, which evaluates all jpg files `[filename].jpg` in a specified folder that have a corresponding mask `[filename]_mask.png` or `[filename]_mask.jpg` file. For example, to evaluate the `PCTNet_ViT.pth` model, you can run:

```
python3 scripts/evaluate_folder.py source_directory ViT_pct --weights pretrained_model/PCTNet_ViT.pth
```

## Evaluation

For further evaluation, we created a notebook `evaluation/evaluation.ipynb` which processes a csv file that contains the calculated errors for each individual file. We also provide the csv files for both our methods as well as [DCCF](https://github.com/rockeyben/DCCF) and [Harmonizer](https://github.com/ZHKKKe/Harmonizer).

## Citation
If this work is helpful for your research, please consider citing the following BibTeX entry.
```
@InProceedings{Guerreiro_2023_CVPR,
    author    = {Guerreiro, Julian Jorge Andrade and Nakazawa, Mitsuru and Stenger, Bj\"orn},
    title     = {PCT-Net: Full Resolution Image Harmonization Using Pixel-Wise Color Transformations},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    month     = {June},
    year      = {2023},
    pages     = {5917-5926}
}
```

## Acknowledgements

Our code is based on Konstantin Sofiiuk's [iDIH](https://github.com/saic-vul/image_harmonization) code as well as the modifications made by Ben Xue's [DCCF](https://github.com/rockeyben/DCCF). The transformer model is based on Zonghui Guo's [HT+ model](https://github.com/zhenglab/HarmonyTransformer).  
