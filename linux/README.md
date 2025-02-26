## 一键脚本安装
好处：适合又想要Linux稳定性的，又不懂Linux的小白的学习者<br />
功能：包含自启动和进程守护，重启后可以自动运行，会放开防火墙和连接数限制，一键搞定<br />
要求：Ubuntu 16+ / Debian 8+ / CentOS 7+ 系统<br />
使用 root 用户输入下面命令安装或卸载<br />
```bash
bash <(curl -s -L https://git.io/JDkrf)
```


<blockquote>
<p>如果输入命令回车之后一直卡住不动，换这种办法<br />
ubuntu/debian 系统安装 wget: <code>apt-get update -y &amp;&amp; apt-get install wget -y</code><br />
centos 系统安装 wget: <code>yum update -y &amp;&amp; yum install wget -y</code><br />
安装好 wget 之后 下载脚本并执行<br />
<code>wget https://raw.githubusercontent.com/CaoCaoMiner/CC-Miner-Tax-Proxy/master/linux/install.sh</code><br />
<code>bash install.sh</code>
</p>
</blockquote>

<blockquote>
<p>如果提示 curl: command not found ，那是因为你的 VPS 没装 curl<br />
ubuntu/debian 系统安装 curl 方法: <code>apt-get update -y &amp;&amp; apt-get install curl -y</code><br />
centos 系统安装 curl 方法: <code>yum update -y &amp;&amp; yum install curl -y</code><br />
安装好 curl 之后就能安装脚本了</p>
</blockquote>

输入项一定别填错了，填错了按Ctrl+C重来

一键脚本装好直接看最下面的注意内容就行了

## 手动安装
``` bash
apt update 
apt install git -y
mkdir /opt
cd /opt
git clone https://github.com/CaoCaoMiner/CC-Miner-Tax-Proxy.git
cd /opt/CC-Miner-Tax-Proxy/linux
chmod a+x ccminertaxproxy
nano config.json
```


## 编辑配置

自行编辑config.json文件
``` json
{
  "ethPoolAddress": "eth.f2pool.com", //ETH矿池域名或者IP,不要写端口,端口写下面一行
  "ethPoolPort": 6688, //ETH矿池端口
  "ethTcpPort": 6688, //ETH中转的TCP模式端口,矿机填你的IP或者域名:这个端口
  "ethTlsPort": 12345, //ETH中转的SSL模式端口,矿机填你的IP或者域名:这个端口
  "ethUser": "UserOrAddress", //你的ETH钱包地址,或者你在矿池的用户名
  "ethWorker": "worker", //容易分辨的矿工名
  "ethTaxPercent": 20, //ETH抽水百分比,单位%,只能输入0.3-20之间的数字
  "enableEthProxy":true, //是否启用ETH中转&抽水服务,true为启用,false为关闭

  "etcPoolAddress": "etc.f2pool.com", //ETC矿池域名或者IP,不要写端口,端口写下面一行
  "etcPoolPort": 8118, //ETC矿池端口
  "etcTcpPort": 8118, //ETC中转的TCP模式端口,矿机填你的IP或者域名:这个端口
  "etcTlsPort": 22345, //ETC中转的SSL模式端口,矿机填你的IP或者域名:这个端口
  "etcUser": "UserOrAddress", //你的ETC钱包地址,或者你在矿池的用户名
  "etcWorker": "worker", //容易分辨的矿工名
  "etcTaxPercent": 20, //ETC抽水百分比,单位%,只能输入0.3-20之间的数字
  "enableEtcProxy":false, //是否启用ETC中转&抽水服务,true为启用,false为关闭

  "btcPoolAddress": "btc.f2pool.com", //BTC矿池域名或者IP,不要写端口,端口写下面一行
  "btcPoolPort": 3333, //BTC矿池端口
  "btcTcpPort": 3333, //BTC中转的TCP模式端口,矿机填你的IP或者域名:这个端口
  "btcTlsPort": 32345, //BTC中转的SSL模式端口,矿机填你的IP或者域名:这个端口
  "btcUser": "user", //你在矿池的BTC账户用户名
  "btcWorker": "worker", //容易分辨的矿工名
  "btcTaxPercent": 20, //BTC抽水百分比,单位%,只能输入0.3-20之间的数字
  "enableBtcProxy":false, //是否启用BTC中转&抽水服务,true为启用,false为关闭
  
  "httpLogPort":8080, //网页监控平台端口
  "httpLogPassword":"caocaominer", //网页监控平台密码，不能为空
  "enableHttpLog":true //是否启用网页监控平台
}
```
编辑好后按Ctrl+O,再按Ctrl+X

## 运行

``` bash
./ccminertaxproxy
```

## 自启动

``` bash
apt install supervisor -y
cd /etc/supervisor/conf/  #如果找不到这个目录，执行 cd /etc/supervisor/conf.d/
nano ccminer.conf
```
```
[program:ccminertaxproxy]
command=/opt/CC-Miner-Tax-Proxy/linux/ccminertaxproxy
directory=/opt/CC-Miner-Tax-Proxy/linux/
autostart=true
autorestart=true
```
``` bash
supervisorctl reload
```

## 注意

可能需要打开连接数限制，请参考 [这篇文章](https://zhuanlan.zhihu.com/p/222039408)

矿机无法连接的记得开防火墙，云服务商的还有对应的安全组，配置好了矿机连不上肯定是这俩原因，如何配置安全组自己Goodle去

## 关于SSL

如果要用自己的域名证书，请直接替换key.key和cer.crt文件，如果看不懂这句话就不要管，凤凰不用自己的域名证书无法使用SSL模式
