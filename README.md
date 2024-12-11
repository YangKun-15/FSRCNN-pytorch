# FSRCNN

该存储库是后续论文的实现 ["Accelerating the Super-Resolution Convolutional Neural Network"](https://arxiv.org/abs/1608.00367)。

<center><img src="./thumbnails/fig1.png"></center>

## Differences from the original

- Added the zero-padding
- Used the Adam instead of the SGD

## 需要环境

- PyTorch 1.0.0
- Numpy 1.15.4
- Pillow 5.4.1
- h5py 2.8.0
- tqdm 4.30.0

## 训练

下面的91张图片组成的Set5数据集，转换成HDF5格式后，可以通过以下链接下载。

| Dataset | Scale | Type | Link |
|---------|-------|------|------|
| 91-image | 2 | Train | [Download](https://www.dropbox.com/s/01z95js39kgw1qv/91-image_x2.h5?dl=0) |
| 91-image | 3 | Train | [Download](https://www.dropbox.com/s/qx4swlt2j7u4twr/91-image_x3.h5?dl=0) |
| 91-image | 4 | Train | [Download](https://www.dropbox.com/s/vobvi2nlymtvezb/91-image_x4.h5?dl=0) |
| Set5 | 2 | Eval | [Download](https://www.dropbox.com/s/4kzqmtqzzo29l1x/Set5_x2.h5?dl=0) |
| Set5 | 3 | Eval | [Download](https://www.dropbox.com/s/kyhbhyc5a0qcgnp/Set5_x3.h5?dl=0) |
| Set5 | 4 | Eval | [Download](https://www.dropbox.com/s/ihtv1acd48cof14/Set5_x4.h5?dl=0) |

否则，您可以使用 `prepare.py` 来创建自定义数据集。

```bash
python train.py --train-file "BLAH_BLAH/91-image_x3.h5" \
                --eval-file "BLAH_BLAH/Set5_x3.h5" \
                --outputs-dir "BLAH_BLAH/outputs" \
                --scale 3 \
                --lr 1e-3 \
                --batch-size 16 \
                --num-epochs 20 \
                --num-workers 8 \
                --seed 123                
```

## Test

预训练的权重可以从下面的链接下载。

| Model | Scale | Link |
|-------|-------|------|
| FSRCNN(56,12,4) | 2 | [Download](https://www.dropbox.com/s/1k3dker6g7hz76s/fsrcnn_x2.pth?dl=0) |
| FSRCNN(56,12,4) | 3 | [Download](https://www.dropbox.com/s/pm1ed2nyboulz5z/fsrcnn_x3.pth?dl=0) |
| FSRCNN(56,12,4) | 4 | [Download](https://www.dropbox.com/s/vsvumpopupdpmmu/fsrcnn_x4.pth?dl=0) |

结果存储在与查询图像相同的路径中。

```bash
python test.py --weights-file "BLAH_BLAH/fsrcnn_x3.pth" \
               --image-file "data/butterfly_GT.bmp" \
               --scale 3
```

## 结果

PSNR 在 Y 通道上计算。

### Set5

| Eval. Mat | Scale | Paper | Ours (91-image) |
|-----------|-------|-------|-----------------|
| PSNR | 2 | 36.94 | 37.12 |
| PSNR | 3 | 33.06 | 33.22 |
| PSNR | 4 | 30.55 | 30.50 |

<table>
    <tr>
        <td><center>Original</center></td>
        <td><center>BICUBIC x3</center></td>
        <td><center>FSRCNN x3 (34.66 dB)</center></td>
    </tr>
    <tr>
    	<td>
    		<center><img src="./data/lenna.bmp""></center>
    	</td>
    	<td>
    		<center><img src="./data/lenna_bicubic_x3.bmp"></center>
    	</td>
    	<td>
    		<center><img src="./data/lenna_fsrcnn_x3.bmp"></center>
    	</td>
    </tr>
    <tr>
        <td><center>Original</center></td>
        <td><center>BICUBIC x3</center></td>
        <td><center>FSRCNN x3 (28.55 dB)</center></td>
    </tr>
    <tr>
    	<td>
    		<center><img src="./data/butterfly_GT.bmp""></center>
    	</td>
    	<td>
    		<center><img src="./data/butterfly_GT_bicubic_x3.bmp"></center>
    	</td>
    	<td>
    		<center><img src="./data/butterfly_GT_fsrcnn_x3.bmp"></center>
    	</td>
    </tr>
</table>
