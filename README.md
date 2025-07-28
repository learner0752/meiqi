# 部署Bifromq

使用阿里云4核8G，40SSD的服务器搭建MQTT-broker，能支持10万tls连接并支持数据持久化(qos1/2)

## 部署步骤

1.__安装docker__
#更新现有的软件包
```bash
sudo apt update
sudo apt upgrade -y
```
#安装依赖项
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
#添加Docker官方GPG密钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
#添加docker仓库到APT源
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
#更新APT缓存
sudo apt update
#安装docker引擎
sudo apt install -y docker-ce docker-ce-cli containerd.io
#启动并检查docker服务
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
#验证docker安装
sudo docker run hello-world
```
2.__配置镜像加速源-轩辕镜像__
#创建和编辑配置文件/etc/docker/daemon.json
```

## 示例代码

```c
#include <stdio.h>
int main() {
    printf("Hello GitHub Markdown!\n");
    return 0;
}
