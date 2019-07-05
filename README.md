# skywalking-kubernetes
该项目可以迅速将skywalking 6.1.0部署进kubernetes(k8s）

## 描述
网上找不到在k8s上部署skywalking利用mysql存储的实例.所以自己研究！

-------------
## 安装server端

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
                  
                  kubectl apply -f 00-init.yaml
                  kubectl apply -f mysql
                  kubectl apply -f oap
                  kubectl apply -f ui
                  

**注意clone下来改好里面提示需要改的再执行，00-init.yaml是建名称空间的(当然你可以整合其他初始化环境的比如pv，pvc等一起初始化环境)，参考上文**

这样你就装好了skywalking的server端了,可以打开页面了!接下来要安装agent端.

## 安装agent端

###### 首先你要创建一个包含了apm-agent的基础镜像!

```
FROM openjdk:8

ADD apm_agent/skywalking /skywalking-agent/

# 构建镜像
# docker build -t 'apm-agent:1.0' -f .
```

###### 然后创建你的微服务时候基于这个镜像来创建：

```
FROM apm-agent:1.0

MAINTAINER shi.gaowu@163.com

RUN ...

ADD ...

WORKDIR ...

ENV SW_AGENT_NAMESPACE={{your_namespace}} SW_AGENT_NAME={{server_name}} SW_AGENT_COLLECTOR_BACKEND_SERVICES=oap:11800

CMD ["bash", "-c", "java -javaagent:/skywalking-agent/skywalking-agent.jar -jar *.jar --CONFIG_URL=..."]
```

**这里特别要注意设置ENV的三个值，还有启动jar时候需要带的参数-javaagent:/skywalking-agent/skywalking-agent.jar**


利用这种方式使你的微服务启动时候包含了apm-agent,这样就可以和server端通信了！


##### 欢迎学习交流 联系方式
**shi.gaowu@163.com**

