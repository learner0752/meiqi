# 部署Bifromq

使用阿里云4核8G，40SSD的服务器搭建MQTT-broker，能支持10万tls连接并支持数据持久化(qos1/2)

## 部署步骤

1.__安装docker__
### 更新现有的软件包
```bash
sudo apt update
sudo apt upgrade -y
```
### 安装依赖项
```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```
### 添加Docker官方GPG密钥
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
### 添加docker仓库到APT源
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
### 更新APT缓存
```bash
sudo apt update
```
### 安装docker引擎
```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io
```
### 启动并检查docker服务
```bash
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```
### 验证docker安装
```bash
sudo docker run hello-world
```
2.__配置镜像加速源-轩辕镜像__
### 创建和编辑配置文件/etc/docker/daemon.json
```bash
sudo mkdir -p /etc/docker
sudo nano /etc/docker/daemon.json
```
### 填入轩辕加速器地址，点击https://xuanyuan.cloud/登录获取
```json
{
  "registry-mirrors": [
    "https://你的加速器ID.mirror.aliyuncs.com"
  ]
}
```
我的是这个：https://3u4cujqbh06um7.xuanyuan.run
<img width="2176" height="414" alt="image" src="https://github.com/user-attachments/assets/e214232a-320b-417f-ad5f-e4d7795b2554" />
保存并退出（按Ctrl+O->回车->Ctrl+X）
### 重启Docker服务
```bash
sudo systemctl daemon-reexec
sudo systemctl restart docker
```
### 验证是否生效
```bash
docker info
```
输出中能看到
```nginx
Registry Mirrors：
  https://3u4cujqbh06um7.xuanyuan.run/
```
3.__部署bifromq__
### 从轩辕镜像源拉取bifromq镜像
```bash
docker pull docker.xuanyuan.run/bifromq/bifromq:latest
```
登录轩辕账号和密码（充值50G/5元），实测使用登录的要快一些，不要用免登录拉取。

### 查看有无拉取成功
```bash
docker images
```

### 启动和配置bifromq参数
```bash
docker run -d \
  --name bifromq \
  -p 1883:1883 \
  -p 1884:1884 \
  -m 10G \
  -e JVM_HEAP_OPTS='-Xms4G -Xmx4G -XX:MetaspaceSize=256m -XX:MaxMetaspaceSize=512m -XX:MaxDirectMemorySize=4G' \
  bifromq/bifromq:latest
```
1884是tls连接，1883是tcp连接。记得安全组开放入方向端口。
可以使用MQTT客户端连接服务器，验证是否开启。

3.__Bifromq配置__
### tls连接（MQTTS加密连接）
__先绑定域名__
https://www.duckdns.org/
绑定域名。

<img width="491" height="300" alt="image" src="https://github.com/user-attachments/assets/2e7878c6-14c5-4609-8f06-6a25bf089c19" />

__Let's Encrypt证书申请__
安装Certbot工具
```bash
sudo apt update
sudo apt install certbot
```
申请证书（DNS验证方式）
```bash
sudo certbot certonly --manual --preferred-challenges dns -d 申请的域名.duckdns.org
```
执行后会提示添加DNS TXT记录。
使用DuckDNS提供的Web API添加TXT，将Token替换成DuckDNS控制台页面上看到的token（就在页面顶部）。域名换成服务器的域名，txt换成命令行指示的txt值。
```
https://www.duckdns.org/update?domains=你的域名&token=你的Token&txt=命令行指示的txt值&verbose=true
```
执行完成后会生成证书。

<img width="678" height="103" alt="image" src="https://github.com/user-attachments/assets/d34d61e2-ec73-4b9f-a6f2-ebeb83be3ed2" />

__配置bifromq__












































