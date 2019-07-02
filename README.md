# dBeker / Faster-RCNN-TensorFlow-Python3源码运行调试经验
## 源码地址
[Faster-RCNN-TensorFlow-Python3](https://github.com/dBeker/Faster-RCNN-TensorFlow-Python3)
## 使用方法
1. 安装必要packages,requirements.txt在文件夹中  
`pip install -r requirements.txt`
2. 文件编译
* 进入./data/coco//PythonAPI  
运行`python setup.py build_ext --inplace`   
运行`python setup.py build_ext install`   
* 进入./lib/utils  
运行`python setup.py build_ext --inplace`  
3. 训练前 [官方教程没讲怎么运行demo.py，但经过实验发现一定要先train才能运行demo]  
下载VOC2007 [database](https://github.com/rbgirshick/py-faster-rcnn#beyond-the-demo-installation-for-training-and-testing-models)  
存放到如下路径`data\VOCDevkit2007\VOC2007\`
4. 下载安装预训练的网络  
此处用的是[VGG16](http://download.tensorflow.org/models/vgg_16_2016_08_28.tar.gz)  
其他的网络[nets](https://github.com/tensorflow/models/tree/master/research/slim#pre-trained-models)
5. 运行train.py
6. 把训练后的checkpoint文件放到`output\vgg16\voc_2007_trainval+voc_2012_trainval\default`文件中: .ckpt 和 .ckpt.meta
7. 运行demo.py
## config配置文件
* `lib\config\config.py` 
1. batch_size : 显存不足时可以下调 [注意是 8 的倍数，可以充分利用显卡性能]
2. snapshot_iterations : 进行 Saver.restore 的迭代数
## Bug List
* ckpt.meta not found  
![tu](https://github.com/hikaruzzz/FasterRCNN-TF-Py3-experience/blob/master/png_pack/1.png)  
这是没找到meta文件，官方code中没有，只能自己先训练，然后把得到的 .ckpt.meta和.ckpt.data.... 文件放到这个目录的文件中。
* vgg_16/bbox_pred/biases not found in checkpoint files  
![tu](https://github.com/hikaruzzz/FasterRCNN-TF-Py3-experience/blob/master/png_pack/2.png)  
一定要把train.py训练后的 .ckpt文件放到该目录，而不是直接用官方下载的 vgg16.ckpt（官方 vgg16.ckpt 中没有 bbox_pred 模块对应的 weight 和 bias 。会有此错误）
* Dst tensor is not initialized. 显存不足  
可以再train.py中加入命令，用CPU跑  
  import os  
  os.environ['CUDA VISIBLE DEVICES']='-1'

* 命名错误 vgg_16.ckpt 改成vgg16.ckpt
![tu](https://github.com/hikaruzzz/FasterRCNN-TF-Py3-experience/blob/master/png_pack/3.png)
