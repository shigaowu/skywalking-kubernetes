# skywalking-kubernetes
该项目可以迅速将skywalking 6.1.0部署进kubernetes(k8s）

## 描述
我弄这个主要是某项目的测试环境，因为项目部署在k8s上，需要在k8s中部署一套skywalking来监控项目的一些状态

-------------
# 安装使用

###### 先自己创建一个namespace或者使用现有项目的namespace:

在6.1.0目录下创建00-init.yaml内容如下:
```
---
apiVersion: v1
kind: Namespace
metadata:
  name: {{your_namespace}}
```

然后下载yaml，所有的文件里{{your_namespace}}都改成你自己所需的namespace。mysql的pvc也改成自己所创建的pvc名称，用于数据的持久化存储！

**git地址**
[shigaowu/skywalking-kubernetes](https://github.com/shigaowu/skywalking-kubernetes)
                  
		  
		  clone下来后按需修改,并增加00-init.yaml
                  
		  cd 6.1.0
                  
                  kubectl apply -f 00-init.yml
                  kubectl apply -f mysql
                  kubectl apply -f oap
                  kubectl apply -f ui
                  

**注意clone下来改好里面提示需要改的再执行，00-init.yaml是建名称空间的(当然你可以整合其他初始化环境的比如pv，pvc等一起初始化环境)，参考上文**

这样你就装好了skywalking的server端了！可以打开页面了！


##### 联系方式
shi.gaowu@163.com

