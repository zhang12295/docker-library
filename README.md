[TOC]

# docker-library



## Kubernetes 镜像同步



目前支持版本：

* Kubernetes v1.10.3
* Kubernetes v1.11.0
* Kubernetes v1.13.0


## 查看gcr.io官方镜像

在科学上网的情况下，打开 https://console.cloud.google.com/gcr/images/google-containers/GLOBAL ，在右边的“过滤条件“中输入关键词来搜索。

然后再选择正确的镜像。

通常，gcr.io官方镜像的命名规则为： 
gcr.io/google_containers/IMAGE_NAME:IMAGE_TAG

比如： 
gcr.io/google_containers/kube-apiserver-amd64:v1.10.3

## 使用阿里云镜像仓库构建镜像

打开阿里云容器镜像服务：https://cr.console.aliyun.com 

**新建镜像仓库:**

1. 选择离自己比较近的区域
2. 选择命名空间
3. 输入仓库名称，一般为镜像名称，比如`kube-apiserver-amd64`
4. 选择仓库类型
5. 填写摘要
6. 选择”海外机器构建“
7. 按提示选择GitHub repo



**构建和拉取镜像:**

1. 选择某个镜像仓库，点击【管理】

2. 选择【构建】，添加构建规则，比如

   ```
   类型：branch
   Branch/Tag：master 
   Dockerfile目录：/kube-apiserver-amd64/v1.10.3 
   Dockerfile文件名：Dockerfile 
   镜像版本：v1.10.3
   ```

3. 点击【立即构建】

4. 构建成功后，在【基础信息】中查看用法



参考文档：

* https://blog.csdn.net/nklinsirui/article/details/80581286