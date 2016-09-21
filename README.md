FinalSpeed服务端安装及教程


说明

1.FinalSpeed必须服务端和客户端同时配合使用,否则没有任何加速效果.

2.服务器64M-128M内存即可稳定运行,搬瓦工由于存在超售问题至少要256M.

3.openvz架构只支持udp协议.

4.服务端可以和锐速共存,互不影响.
 FinalSpeed服务端Linux版 (支持CentOS,Ubuntu,Debian)

注意问题

1.服务端会启动iptables,如果服务器修改过ssh端口,请先开放ssh端口,否则可能导致ssh连接失败.

开放端口命令
service iptables start
iptables -I INPUT -p tcp –dport 端口号 -j ACCEPT
iptables -I OUTPUT -p tcp –sport 端口号 -j ACCEPT
service iptables save

2.不熟悉不要乱改配置,如果无法连接,请卸载后一键安装,不要做任何修改,按照教程操作.

国外服务器一键安装：
rm -f install_fs.sh
wget https://raw.githubusercontent.com/shzcq/finalspeed/master/install_fs.sh
chmod +x install_fs.sh
./install_fs.sh 2>&1 | tee install.log

国内服务器一键安装：
rm -f install_fs.sh
wget https://coding.net/u/siyanglee/p/finalspeed/git/raw/master/install_fs.sh
chmod +x install_fs.sh
./install_fs.sh 2>&1 | tee install.log

debian,ubuntu下如果执行脚本出错,请切换到dash,

切换方法: sudo dpkg-reconfigure dash 选no

安装完后查看日志

tail -f /fs/server.log

如果服务端正常运行会有类似以下提示
如果出现java运行失败的提示,说明脚本安装java失败,需要手动安装java.

更新

执行一键安装会自动完成更新.

卸载
sh /fs/stop.sh ; rm -rf /fs

启动
sh /fs/start.sh; tail -f /fs/server.log

重复运行启动会出现以下端口绑定错误,请先停止或直接重启服务.

停止
sh /fs/stop.sh

重新启动
sh /fs/restart.sh; tail -f /fs/server.log

查看日志
tail -f /fs/server.log

设置服务端口

默认udp 150和tcp 150 ,修改端口后服务端会自动修改防火墙.

linux版: mkdir -p /fs/cnf/ ; echo 端口号 > /fs/cnf/listen_port ; sh /fs/restart.sh

windows 版: 在cnf目录下新建文件listen_port,文件内容为端口号.

注意:由于finalspeed的工作原理,不要开放finalspeed所使用的tcp端口.

设置开机启动

chmod +x /etc/rc.local

vi /etc/rc.local

加入

sh /fs/start.sh

每天晚上3点自动重启

crontab -e

加入

0 3 * * * sh /fs/restart.sh


FinalSpeed服务端Windows版


注意问题

Windows版服务端运行需要java环境和winpcap.


下载地址

https://raw.githubusercontent.com/shzcq/finalspeed/master/finalspeed_server_windows.zip

国内镜像：

https://coding.net/u/siyanglee/p/finalspeed/git/raw/master/finalspeed_server_windows.zip
 

FinalSpeed客户端下载及教程



FinalSpeed客户端Windows版


下载地址:

https://raw.githubusercontent.com/shzcq/finalspeed/master/finalspeedclient_1.0.rar

系统需安装java运行环境,Linux还需安装libpcap.

Ubuntu,Debian安装libpcap: apt-get -y install libpcap-dev

Centos安装libpcap: yum -y install libpcap



安装:

下载解压.

运行:

打开终端,假设finalspeed_client.jar所在路径为/fsclient ,先切换到该路径cd /fsclient ,

然后执行 sudo java -jar finalspeed_client.jar ,前面加sudo,因为必须以root权限运行,如果没有root权限,会无法启用tcp协议.

FinalSpeed客户端命令行版,支持Linux

由于事情繁忙,暂时没有提供官方命令行版本

有用户修改编译了第三方的命令行版,可按其说明使用.

获取 https://github.com/zqhong/finalspeed

———————————————————————————-

注意问题,必读!

1.服务器必须同时部署FinalSpeed服务端才能进行加速.

2.客户端必须准确设置物理带宽,最终加速的速度不会超过所设置的带宽值,如果设置值高于实际带宽会造成丢包,导致速度变慢.

3.客户端首选tcp协议,如果udp不稳定,请切换到tcp.

4.若服务器为openvz架构,客户端只能选dp协议,其他架构同时支持tcp和udp协议.

5.windows客户端使用tcp协议时不兼容锐速,停止锐速后可以正常运行.

6.FinalSpeed不提供加密功能,如有安全需求,不要直接加速明文协议.

FinalSpeed加速失败原因和解决

FinalSpeed不稳定原因汇总及解决

———————————————————————————-

加速ss教程

假设服务器IP为10.10.10.10,finalspeed端口为默认150,ss端口为8989.

加速前提ss服务端运行正常,ss客户端也能正常登录.

1.运行FinalSpeed客户端,填写服务器地址 10.10.10.10 .

2.点击添加,增加加速端口,加速端口为ss端口8989,如果为其他端口,请相应修改,本地端口任意,这里是2000 .

3.打开ss客户端,添加服务器,服务器IP为127.0.0.1,服务器端口为加速端口对应的本地端口,这里是2000,然后设置你的ss密码,加密方式.

5.确定保存,选择使用刚添加的服务器,并设置浏览器代理,成功连接后,FinalSpeed状态栏会出现”连接服务器成功”提示.

———————————————————————————-


加速ssh教程

假设服务器IP为10.10.10.10,finalspeed端口为默认150,ssh端口为22.

1.运行FinalSpeed客户端,填写服务器地址 10.10.10.10 .

2.点击添加,增加加速端口,加速端口为ssh的监听端口22,本地端口任意,这里是1000 .

3.运行ssh客户端,增加连接,主机为127.0.0.1,端口号为加速端口对应的本地端口,这里是1000,设置如下.

4.设置完成后,输入ssh账号,密码,成功连接,FinalSpeed状态栏会出现”连接服务器成功”提示.

