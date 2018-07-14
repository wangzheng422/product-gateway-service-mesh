## updated by wangzheng422

* bug fix for pom.xml
* IDE setting, refer: https://github.com/trustin/os-maven-plugin#issues-with-eclipse-m2e-or-other-ides ,  "java.jdt.ls.vmargs": "-Dos.detected.name=osx -Dos.detected.arch=x86_64 -Dos.detected.classifier=osx-x86_64",

## Note

在 istio 目录下，istio版本0.7.1

下载 istio

curl -L https://git.io/getLatestIstio | sh -

自动会解压缩，设置好里面bin的PATH

git clone  https://github.com/wangzheng422/product-gateway-service-mesh

bash deploy.sh 会编译镜像，并上传，提前修改一下里面的参数。

cd istio

kubectl apply -f istio.yaml

kubectl apply -f prometheus.yaml

kubectl apply -f grafana.yaml

istioctl kube-inject -f product-grpc-istio.yaml > 1.yaml

修改1.yaml，加入alb的annotation。范例可参考 product-grpc-istio.changed.yaml

用1.yaml启动应用即可

## 效果

应用页面

http://10.11.0.6:9999/product/9310301.html#

!(https://github.com/wangzheng422/product-gateway-service-mesh/raw/master/docs/image2018-4-8%2021_6_37.png)

调用链跟踪

http://10.11.0.7:61080/ （由于使用nodeport暴露的服务，使用集群中任意一个slave地址）


istio dashboard

http://10.11.0.7:60300/dashboard/db/istio-dashboard  （由于使用nodeport暴露的服务，使用集群中任意一个slave地址 ）


调用关系图

http://10.11.0.8:60088/dotviz （由于使用nodeport暴露的服务，使用集群中任意一个slave地址 ）



## Overview

This project is used to demonstrate Istio and Linkerd service meshes with GRPC and Spring Boot services.

The documentation behind this blog is available at:
https://sleeplessinslc.blogspot.com/2017/09/service-mesh-examples-of-istio-and.html

## Points of Note

- Demonstration of same product-gateway across linkerd and istio
- Use of Spring Boot and Protocol Buffers
- Composition of protocol buffer schemas
- Creation of Docker images for the Microservices
- Product Gateway exposes a HTML and JSON representation
- The application only supports ONE Product at the resource /product/9310301
- The applications only support happy path!

## Structure of Project

- baseproduct: Location of Base product microservice
- inventory: Location of Inventory microservice
- price: Location of price microservice
- reviews: Location of Reviews microservice
- product-gateway: Location of Product Gateway microservice
- linkerd: Folder containing Kubernetes deployment script for linkerd
- istio: Folder containing Kubernetes deployment script for linkerd

# Getting Started

You can run this project by importing into Eclipse and starting the individual services. The below will create Docker images as well.
Building Maven project:
```bash
mvn clean install
```
# Running in Kubernetes

Follow the instructions at https://sleeplessinslc.blogspot.com/2017/09/service-mesh-examples-of-istio-and.html for more details
