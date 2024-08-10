创建docker镜像：

新建Dockerfile文件，其中对镜像描述，把Dockerfile和需要的资源放在同一个目录下

sudo docker build -t 镜像名 .

上传镜像到仓库：

sudo docker tag 本地镜像名 仓库/标签
sudo docker push 仓库/标签

ssh连接：

ssh user@35.236.5.46 -p 22



如果不使用持久化，对于环境的配置一定要在dockerfile配置好，不然每次容器重启都会丢失环境



k8s clilent构建需要的环境遍历

```
export KUBERNETES_SERVICE_HOST=10.96.0.1
export KUBERNETES_SERVICE_PORT=443
```
