# 费用相关FAQ
1. “[ModelArts新手资源包](https://console.huaweicloud.com/modelarts/?region=cn-north-1#/userPackageWindow)” 包含以下三种资源：(1) 20小时CPU、(2) 10小时GPU（注：<b>只包含P100+P4，不包含V100</b>）以及 (3) 10小时自动学习；只要其中一个资源包用完，即使仍有其他种类资源包，若持续使用已用完的资源类型，仍会按照使用量扣费。当前《[ModelArts-Lab AI实战营](https://github.com/huaweicloud/ModelArts-Lab/issues?q=is%3Aissue+is%3Aopen+label%3A%22%E5%8D%8E%E4%B8%BA%E4%BA%91+ModelArts-Lab+AI%E5%AE%9E%E6%88%98%E8%90%A5%22)》活动为开发者设计的 “ModelArts满减资源包” 中的 GPU 资源<b>只包含 P100+P4，不包含 V100</b>，请参加《ModelArts-Lab AI实战营》的开发者<b>不要选择 V100 规格</b>，避免产生套餐包之外的费用。
   * 目前 “ModelArts新手资源包” 以及 “ModelArts满减资源包” 只可于 “北京一” 区域使用，请<b>务必选择 “北京一” </b>以免产生扣费！
2. 案例实践中所使用的存储资源（OBS 或 EVS）不包含在上述 “ModelArts新手资源包” 中，可能涉及到费用。每个案例涉及的存储资源所需费用小于1元。
   * 对象存储服务 OBS 标准存储价格：0.0990元/GB/月
   * 云硬盘 EVS 超高 IO 收费：0.0014元/GB/小时
      * Notebook 案例使用之 EVS 资源，磁盘规格默认为 5GB，当磁盘规格为 5GB 时不收费，超出 5GB 时，从 Notebook 实例创建成功起，直至删除成功，超出部分每 GB 按照规定费用收费
3. 实验完成后，请<b>手动停止</b>占用云资源的服务，如：停止 “开发环境” 中所创建的 Notebook 以及 “部署上线”-“在线服务” 中所部署的模型，以避免因持续占用云资源导致资源包无谓消耗和欠费发生。需使用时，再启动即可。
4. 公有云的按需使用，都是后付费模式，也就是说，先使用再付费，一个小时会出一次话单，比如：2019.6.24 04:00~05:00 这段时间使用的费用，在 05:00 时刻采集完成后，才会出话单，进行扣费。
5. 在 “总览” 页中，请确保各个服务皆为 “<b>0 计费中</b>”，如下图所示：
<img src="images/Overview-billing.png" width="1000px" />

<b> 此外注意 TensorBoard 的服务关闭，关闭方法： 进入 ModelArts控制台--训练作业--TensorBoard --停止 </b>
<img src="images/停止TensorBoard.PNG" width="1000px" />


# ModelArts使用FAQs
* [自动学习训练失败原因是什么？](#自动学习训练失败原因是什么)
* [ModelArts是否支持Keras?](#modelarts是否支持keras)
* [创建Notebook时存储配置选择EVS和OBS有什么区别?](#创建notebook时存储配置选择evs和obs有什么区别)
* [使用pip install时出现没有空间的错误](#使用pip-install时出现没有空间的错误)
* [Notebook中Upload之后文件上传到哪里了?](#notebook中upload之后文件上传到哪里了)
* [Terminal中如何进入跟当前实例Notebook kernel一样的python环境](#terminal中如何进入跟当前实例notebook-kernel一样的python环境)
* [运行训练代码出现内存不够问题并导致实例崩溃](#运行训练代码出现内存不够问题并导致实例崩溃)
* [Notebook出现保存文件失败](#notebook出现保存文件失败)
* [如何下载GitHub代码库里面的单个文件](#如何下载github代码库里面的单个文件)
* [Notebook运行生成的文件如何保存到OBS](#notebook运行生成的文件如何保存到obs)
* [如何在Notebook中安装Python依赖](#如何在notebook中安装python依赖)
* [Notebook中调测好的代码如何用于训练作业](#notebook中调测好的代码如何用于训练作业)
* [是否支持在本地安装MoXing](#是否支持在本地安装moxing)
* [在GitHub网站上打开ipynb文件很缓慢或打开失败](#在github网站上打开ipynb文件很缓慢或打开失败)
* [Notebook卡死_无法执行代码](#notebook卡死_无法执行代码)

## 自动学习训练失败原因是什么？
自动学习项目存储图片数据的OBS路径下，不允许存放文件夹，同时文件的名称中不允许存在特殊字符(特殊字符集：['~', '`', '@', '#', '$', '%', '^', '&', '*', '{', '}', '[', ']', ':', ';', '+', '=', '<', '>', '/'])。如果违反了以上两点规则之一，就会训练失败。

## ModelArts是否支持Keras?

Keras是一个用Python编写的高级神经网络API，它能够以TensorFlow、CNTK或Theano作为后端运行。ModelArts支持tf.keras，创建AI引擎为TensorFlow的Notebook后，可执行!pip list查看tf.keras的版本。
TensorFlow Keras指南请参考：https://www.tensorflow.org/guide/keras?hl=zh-cn

## 创建Notebook时“存储配置”选择EVS和OBS有什么区别？

  * 选择EVS的实例
    用户在Notebook实例中的所有文件读写操作都是针对容器中的内容，与OBS没有任何关系。重启该实例，内容不丢失。
    EVS磁盘规格默认为5GB，最小为5G，最大为500G。磁盘会挂载到`~\work`目录下。
    当磁盘规格为5GB时不收费，超出5GB时，从Notebook实例创建成功起，直至删除成功，超出部分每GB按照规定费用收费。计费详情https://www.huaweicloud.com/price_detail.html#/modelarts_detail。
  * 选择OBS的实例
    用户在Notebook实例中的所有文件读写操作都是针对所选择的OBS路径下的内容，即新增，修改，删除等都是对相应的OBS路径下的内容来进行的操作，跟当前实例空间没有关系。
    如果用户需要将内容同步到实例空间，需要选中内容，单击Sync OBS按钮来实现将选中内容同步到当前容器空间。

## 使用pip install时出现没有空间的错误

* 问题现象
在Notebook实例中，使用pip install时，出现“No Space left...”的错误。
* 解决办法
建议使用pip install  --no-cache **  命令安装，而不是使用pip install **。加上“--no-cache”参数，可以解决很多此类报错

## Notebook中Upload之后文件上传到哪里了？

* 针对这个问题，有两种情况：
如果您创建的Notebook使用OBS存储实例时单击“upload”后，数据将直接上传到该Notebook实例对应的OBS路径下，即创建Notebook时指定的OBS路径。
如果您创建的Notebook不使用OBS存储单击“upload”后，数据将直接上传至当前实例容器中，即在“terminal”中的“~/work”目录下。

## Terminal中如何进入跟当前实例Notebook kernel一样的python环境？

如果您习惯于使用Notebook terminal来运行代码，那么需要切换到和对应Notebook kernel一样的python环境。
* 如果使用单独的AI引擎创建的Notebook实例。
1. 运行conda info -e命令查看当前实例下的python虚拟环境。
2. 获取对应的虚拟环境之后，运行source activate **命令激活对应的python环境，然后您可以正常使用此python环境。
3. 使用结束后，您可以运行source deactivate **命令退出该环境。
* 如果您使用Multi-Engine创建的Notebook实例（即一个Notebook可创建基于多种AI引擎的kernel）。
  在teminal的用户目录下，有一个“README”文件，此文件详细说明了如何切换不同的python环境。

## 运行训练代码出现内存不够问题并导致实例崩溃

在Notebook实例中运行训练代码，如果数据量太大或者训练层数太多，亦或者或者其他原因，导致出现“内存不够”问题，最终导致该容器实例崩溃。
出现此问题后，如果您重新打开此Notebook，系统将自动重启Notebook，来修复实例崩溃的问题。此时只是解决了崩溃问题，如果重新运行训练代码仍将失败。如果您需要解决“内存不够”的问题，建议您创建一个新的Notebook，使用更高规格的资源池，比如GPU或专属资源池来运行此训练代码。

## Notebook出现保存文件失败
如果当前Notebook还可以运行代码，但是无法保存，保存时会提示“save error”错误。
大多数原因是华为云WAF安全拦截导致的，当前页面，即用户的输入或者代码运行的输出有一些字符被华为云拦截，认为有安全风险。出现此问题时，请提交工单，联系专业的工程师帮您核对并处理问题。

## 如何下载GitHub代码库里面的单个文件
在GitHub中，打开要下载的文件（源代码或者图片等），右击`Download`或者`Raw`按钮（这两个按钮的功能是一样的，并且不会同时存在），然后点击"链接另存为"，保存文件到本地，如下图所示：
<img src="images/github下载单个文件.PNG" width="1000px" />

## Notebook运行生成的文件如何保存到OBS
使用ModelArts SDK可以上传Notebook本地的文件和文件夹（如果文件夹中的文件较多，建议将文件夹打成压缩包后再上传）至OBS，使用方法见[ModelArts官方帮助文档](https://support.huaweicloud.com/sdkreference-modelarts/modelarts_04_0126.html)

## 如何在Notebook中安装Python依赖
可以使用`pip install`命令安装即可，但是要注意在对应的Python虚拟环境中安装，有两种方式。
* 第一种（推荐），在ipynb开发环境中安装
直接在ipynb开发环境中使用：`!pip install <package_name>`安装，如下图：
<img src="images/在ipynb中安装Python依赖.PNG" width="1000px" />

* 第二种，在terminal中安装
打开Notebook terminal，使用`conda env list`命令列举所有的Python虚拟环境，然后使用`source activate <env_name>`命令进入一个Python虚拟环境，最后使用`pip install <package_name>`命令安装。如下图所示：
<img src="images/在terminal中安装Python依赖.PNG" width="1000px" />

## Notebook中调测好的代码如何用于训练作业
在Notebook中调测好训练代码之后，需要将当前ipynb转化为Python文件，才能用于ModelArts训练作业。
单击当前ipynb页面上方的“Convert to Python File”，即可生成用于ModelArts训练作业的启动文件（.py文件）。如下图所示：
<img src="images/转换为Python文件.PNG" width="1000px" />

## 是否支持在本地安装MoXing
不支持，目前MoXing只支持在ModelArts里面使用

## 在GitHub网站上打开ipynb文件很缓慢或打开失败
这是由于网络问题和GitHub网站本身的问题导致的，有如下两种解决办法：
1. 刷新网页；
2. 如果刷新网页仍然无效，则可以打开 https://nbviewer.jupyter.org/ ，在如下搜索框中粘贴ipynb文件的地址，如：https://github.com/huaweicloud/ModelArts-Lab/blob/master/notebook/DL_image_recognition/image_recongition.ipynb ，回车，即可查看ipynb文件。
<img src="images/nbviewer打开ipynb文件.png" width="1000px" />

## Notebook卡死_无法执行代码
按如下步骤依次进行排查处理：
1. 如果只是单个cell的执行过程卡死，但整个Notebook页面还可以点击，则先点击保存按钮，再点“Kernel->Restart”；
2. 如果整个Notebook页面也已经卡死，则打开ModelArts的Notebook管理页面，点击Runing，再点击Shutdown将对应的Notebook关掉，稍后再重新打开；
<img src="images/shutdown_notebook.png" width="1000px" />

3. 如果按第2步执行后重新打开的Notebook仍然卡死，则打开ModelArts的Notebook列表页面，将对应的Notebook虚拟机停止，再启动。
<img src="images/停止notebook.png" width="1000px" />


