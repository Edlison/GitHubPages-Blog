# CluasteringGANSiamese

## 代码复现

ST-SiameseNet

安装依赖`pip install -r requirement`  

pip指定版本安装`pip install xxx==version`

conda指定版本安装`conda install xx=version`

作为Keras后台的Tensorflow有一些版本约束-[版本匹配](https://docs.floydhub.com/guides/environments/)



## GAN

报错信息  
`Error while reading resource variable _AnonymousVar41 from Container: localhost. This could mean that the variable was uninitialized. Not found: Resource localhost/_AnonymousVar41/class tensorflow::Var does not exist.`

解决办法  
keras改为tensorflow.keras

