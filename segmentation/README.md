# Applying ViT-Adapter to Semantic Segmentation

Our segmentation code is developed on top of [MMSegmentation v0.20.2](https://github.com/open-mmlab/mmsegmentation/tree/v0.20.2).

For details see [Vision Transformer Adapter for Dense Predictions](https://arxiv.org/abs/2205.08534).

If you use this code for a paper please cite:

```
@article{chen2022vitadapter,
  title={Vision Transformer Adapter for Dense Predictions},
  author={Chen, Zhe and Duan, Yuchen and Wang, Wenhai and He, Junjun and Lu, Tong and Dai, Jifeng and Qiao, Yu},
  journal={arXiv preprint arXiv:2205.08534},
  year={2022}
}
```

## Usage

Install [MMSegmentation v0.20.2](https://github.com/open-mmlab/mmdetection/tree/v2.23.0).

```
ln -s ../detection/ops ./
cd ops & sh make.sh # compile deformable attention
pip install timm==0.4.12
pip install mmdet==2.22.0 # for Mask2Former
pip install mmsegmentation==0.20.2
# recommended environment: torch1.9 + cuda11.1
pip install mmcv-full==1.4.2 -f https://download.openmmlab.com/mmcv/dist/cu111/torch1.9.0/index.html
pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html
```

## Data preparation

Preparing ADE20K/Cityscapes/COCO Stuff/Pascal Context according to the [guidelines](https://github.com/open-mmlab/mmsegmentation/blob/master/docs/en/dataset_prepare.md#prepare-datasets) in MMSegmentation.

## Results and models

> Note that due to the capacity limitation of *GitHub Release*, some files are provided as `.zip` packages. Please **unzip** them before load into model.

**ADE20K val**

| Method      | Backbone      | Pre-train                                                                                                                                  | Lr schd | Crop Size | mIoU (SS)                                                                                  | mIoU (MS)                                                                                  | #Param | Config                                                                          | Download                                                                                                                                                                                                                |
|:-----------:|:-------------:|:------------------------------------------------------------------------------------------------------------------------------------------:|:-------:|:---------:|:------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------:|:------:|:-------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| UperNet     | ViT-Adapter-L | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth)                      | 160k    | 640       | [58.0](https://drive.google.com/file/d/1KsV4QPfoRi5cj2hjCzy8VfWih8xCTrE3/view?usp=sharing) | [58.4](https://drive.google.com/file/d/1haeTUvQhKCM7hunVdK60yxULbRH7YYBK/view?usp=sharing) | 451M   | [config](./configs/ade20k/upernet_beit_adapter_large_640_160k_ade20k_ss.py)     | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.1/upernet_beit_adapter_large_640_160k_ade20k.pth.tar) \| [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.1/20220313_233147.log) |
| Mask2Former | ViT-Adapter-L | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth)                      | 160k    | 640       | [58.3](https://drive.google.com/file/d/1jj56lSbc2s4ZNc-Hi-w6o-OSS99oi-_g/view?usp=sharing) | [59.0](https://drive.google.com/file/d/1hgpZB5gsyd7LTS7Aay2CbHmlY10nafCw/view?usp=sharing) | 568M   | [config](./configs/ade20k/mask2former_beit_adapter_large_640_160k_ade20k_ss.py) | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.2/mask2former_beit_adapter_large_640_160k_ade20k.zip) \| [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.2/20220426_003454.log) |
| Mask2Former | ViT-Adapter-L | [COCO-Stuff-164K](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.6/mask2former_beit_adapter_large_896_80k_cocostuff164k.zip) | 80k     | 896       | [59.4](https://drive.google.com/file/d/1B_1XSwdnLhjJeUmn1g_nxfvGJpYmYWHa/view?usp=sharing) | [60.5](https://drive.google.com/file/d/1UtjmgcYKR-2h116oQXklUYOVcTw15woM/view?usp=sharing) | 571M   | [config](./configs/ade20k/mask2former_beit_adapter_large_896_80k_ade20k_ss.py)  | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.0/mask2former_beit_adapter_large_896_80k_ade20k.zip) \| [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.0/20220430_154104.log)  |

- Note that the [COCO-Stuff-164K](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.6/mask2former_beit_adapter_large_896_80k_cocostuff164k.zip) pre-trained weights should be loaded by using `--cfg-options load_from=<pretrained_path>`

**Cityscapes val**

| Method      | Backbone      | Pre-train                                                                                                                        | Lr schd | Crop Size | mIoU (SS)                                                                                  | mIoU (MS)                                                                                  | #Param | Config                                                                                 | Download                                                                                                                                                                                                       |
|:-----------:|:-------------:|:--------------------------------------------------------------------------------------------------------------------------------:|:-------:|:---------:|:------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------:|:------:|:--------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Mask2Former | ViT-Adapter-L | [Mapillary](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.3/mask2former_beit_adapter_large_896_80k_mapillary.zip) | 80k     | 896       | [84.9](https://drive.google.com/file/d/1LKy0zz-brCBbKGmUWquadILaBHdDLR6s/view?usp=sharing) | [85.8](https://drive.google.com/file/d/1LSJvK1BPSbzm9eWpKL8Xo7RmYBrd2xux/view?usp=sharing) | 571M   | [config](./configs/cityscapes/mask2former_beit_adapter_large_896_80k_cityscapes_ss.py) | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.3/mask2former_beit_adapter_large_896_80k_cityscapes.zip) \| [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.3/log.txt) |

- Note that the [Mapillary](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.3/mask2former_beit_adapter_large_896_80k_mapillary.zip) pre-trained weights should be loaded by using `--cfg-options load_from=<pretrained_path>`

**COCO-Stuff-10K**

| Method      | Backbone      | Pre-train                                                                                                             | Lr schd | Crop Size | mIoU (SS)                                                                                  | mIoU (MS)                                                                                  | #Param | Config                                                                                      | Download                                                                                                                                                                                                                        |
|:-----------:|:-------------:|:---------------------------------------------------------------------------------------------------------------------:|:-------:|:---------:|:------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------:|:------:|:-------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Mask2Former | ViT-Adapter-B | [BEiT-B](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_base_patch16_224_pt22k_ft22k.pth)  | 40k     | 512       | 49.2                                                                                       | 50.2                                                                                       | 120M   | [config](./configs/coco_stuff10k/mask2former_beit_adapter_base_512_40k_cocostuff10k_ss.py)  | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.9/mask2former_beit_adapter_base_512_40k_cocostuff10k.pth.tar) \| [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.9/20220605_014125.log) |
| UperNet     | ViT-Adapter-L | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth) | 80k     | 512       | [51.0](https://drive.google.com/file/d/1xZodiAvOLGaLtMGx_btYVZIMC2VKrDhI/view?usp=sharing) | [51.4](https://drive.google.com/file/d/1bmFG9GA4bRqOEJfqXcO7nWYPwG3wSk2J/view?usp=sharing) | 451M   | [config](./configs/coco_stuff10k/upernet_beit_adapter_large_512_80k_cocostuff10k_ss.py)     | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.4/upernet_beit_adapter_large_512_80k_cocostuff10k.pth.tar) \| [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.4/20220505_091358.log)    |
| Mask2Former | ViT-Adapter-L | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth) | 40k     | 512       | [53.2](https://drive.google.com/file/d/1Buewc1n7GBAcBDXeia-QarujrDZqc_Sx/view?usp=sharing) | [54.2](https://drive.google.com/file/d/1kQgJUHDeQoO3pPY6QoXRKwyF7heT7wCJ/view?usp=sharing) | 568M   | [config](./configs/coco_stuff10k/mask2former_beit_adapter_large_512_40k_cocostuff10k_ss.py) | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.5/mask2former_beit_adapter_large_512_40k_cocostuff10k.zip) \| [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.5/20220504_205737.log)    |

**COCO-Stuff-164K**

| Method       | Backbone       | Pre-train                                                                                                             | Lr schd | Crop Size | mIoU (SS)                                                                                  | mIoU (MS)                                                                                  | #Param | Config                                                                                        | Download                                                                                                                                                                                                                      |
|:------------:|:--------------:|:---------------------------------------------------------------------------------------------------------------------:|:-------:|:---------:|:------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------:|:------:|:---------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| UperNet      | ViT-Adapter-L  | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth) | 80k     | 640       | [50.5](https://drive.google.com/file/d/1CninnhxkN3VDhmeOhhcg_K72ZG1hyW3x/view?usp=sharing) | [50.7](https://drive.google.com/file/d/1RUTAoL95giuG0vy-0nvkLIoUB7RZlh9V/view?usp=sharing) | 451M   | [config](./configs/coco_stuff164k/upernet_beit_adapter_large_640_80k_cocostuff164k_ss.py)     | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.6/upernet_beit_adapter_large_640_80k_cocostuff164k.pth.tar) \| [log](https://drive.google.com/file/d/1KkSOyUO2uBJCwDUdQH2_xMTVNBqpPaEz/view?usp=sharing) |
| Mask2Former* | ViT-Adapter-L* | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth) | 80k     | 896       | [51.7](https://drive.google.com/file/d/1n6fekFr6Kr69g5kTBPwkPfa6HbaBG4TC/view?usp=sharing) | [52.0](https://drive.google.com/file/d/1ED4l-2n1P2K2SplZ1JKvwja_uzaEuU1l/view?usp=sharing) | 571M   | [config](./configs/coco_stuff164k/mask2former_beit_adapter_large_896_80k_cocostuff164k_ss.py) | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.6/mask2former_beit_adapter_large_896_80k_cocostuff164k.zip) \| [log](https://drive.google.com/file/d/13VrhMPCOA9scnGrEk21jwu8Yh0tbFr78/view?usp=sharing) |

- The model marked with \* is used for fine-tuning ADE20K dataset.

**Pascal Context**

| Method      | Backbone      | Pre-train                                                                                                             | Lr schd | Crop Size | mIoU (SS)                                                                                  | mIoU (MS)                                                                                  | #Param | Config                                                                                            | Download                                                                                                                                                                                                                              |
|:-----------:|:-------------:|:---------------------------------------------------------------------------------------------------------------------:|:-------:|:---------:|:------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------:|:------:|:-------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Mask2Former | ViT-Adapter-B | [BEiT-B](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_base_patch16_224_pt22k_ft22k.pth)  | 40k     | 480       | 64.0                                                                                       | 64.4                                                                                       | 120M   | [config](./configs/pascal_context/mask2former_beit_adapter_base_480_40k_pascal_context_59_ss.py)  | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.8/mask2former_beit_adapter_base_480_40k_pascal_context_59.pth.tar) \|  [log](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.8/20220605_014124.log) |
| UperNet     | ViT-Adapter-L | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth) | 80k     | 480       | [67.0](https://drive.google.com/file/d/1BxnwkxGZzncpd_G4cDmHPB6Pq681YogD/view?usp=sharing) | [67.5](https://drive.google.com/file/d/1Ff-6CGyNs5_ORrlLnyYkV9spE59UjwiT/view?usp=sharing) | 451M   | [config](./configs/pascal_context/upernet_beit_adapter_large_480_80k_pascal_context_59_ss.py)     | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.7/upernet_beit_adapter_large_480_80k_pascal_context_59.pth.tar) \|  [log](https://drive.google.com/file/d/1JQ8fyfQpp1qTcFrlQW9WcZC1A83UWsEB/view?usp=sharing)    |
| Mask2Former | ViT-Adapter-L | [BEiT-L](https://conversationhub.blob.core.windows.net/beit-share-public/beit/beit_large_patch16_224_pt22k_ft22k.pth) | 40k     | 480       | [67.8](https://drive.google.com/file/d/1AbC7DZeTjZVIqNTRWFCkc16FHEmxvDNK/view?usp=sharing) | [68.2](https://drive.google.com/file/d/1gl_gPF7pxjWKvUubK7g1CB5wtihuAWgA/view?usp=sharing) | 568M   | [config](./configs/pascal_context/mask2former_beit_adapter_large_480_40k_pascal_context_59_ss.py) | [model](https://github.com/czczup/ViT-Adapter/releases/download/v0.2.7/mask2former_beit_adapter_large_480_40k_pascal_context_59.zip) \|  [log](https://drive.google.com/file/d/16HyIJ8n8HYHVjx2KMpf1s4429Rmexgq3/view?usp=sharing)    |

## Evaluation

To evaluate ViT-Adapter-L + Mask2Former (896) on ADE20k val on a single node with 8 gpus run:

```shell
sh dist_test.sh configs/ade20k/mask2former_beit_adapter_large_896_80k_ade20k_ss.py /path/to/checkpoint_file 8 --eval mIoU
```

This should give

```
Summary:

+-------+-------+-------+
|  aAcc |  mIoU |  mAcc |
+-------+-------+-------+
| 86.61 | 59.43 | 73.55 |
+-------+-------+-------+
```

## Training

To train ViT-Adapter-L + UperNet on ADE20k on a single node with 8 gpus run:

```shell
sh dist_train.sh configs/ade20k/upernet_beit_adapter_large_640_160k_ade20k_ss.py 8
```
