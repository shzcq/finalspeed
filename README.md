finalspeed

服务器TCP双边加速软件可达到90％，高效提升物理的带宽利用率

安装方法：

1 #yum update

2 安装SS，此处省略

3 开放端口命令

service iptables start

iptables -A INPUT -p tcp --dport SSH端口号 -j ACCEPT

iptables -A OUTPUT -p tcp --sport SSH端口号 -j ACCEPT

service iptables save

4 开始安装

rm -f install_fs.sh

wget https://raw.githubusercontent.com/shzcq/finalspeed/master/install_fs.sh

chmod +x install_fs.sh

./install_fs.sh 2>&1 | tee install.log

5 其他常用命令： 更 新 执行一键安装会自动完成更新. 卸 载 sh /fs/stop.sh ; rm -rf /fs 启 动 sh /fs/start.sh 停 止 sh /fs/stop.sh 重新启动 sh /fs/restart.sh 日 志 tail -f /fs/server.log

6 设置服务端口 默认udp 150和tcp 150 ,由于finalspeed的工作原理,请不要在本机防火墙开放 finalspeed所使用的tcp端口.

mkdir -p /fs/cnf/ ; echo 端口号 > /fs/cnf/listen_port ; sh /fs/restart.sh

例：

mkdir -p /fs/cnf/ ; echo 150最好改个新端口不要用150这个端口 > /fs/cnf/listen_port ; sh /fs/restart.sh

7 设置开机自启动（下面命令新手这个建议配合使用Winscp操作）

chmod +x /etc/rc.local

vi /etc/rc.local

加入 sh /fs/start.sh 命令说明 i进入编辑可写模式，esc退出可写模式，然后选择 :wq存盘并保存，:q!不存盘强行退出

每天晚上3点自动重启 crontab -e 加入 0 3 * * * sh /fs/restart.sh 服务端完成

windows客户端设置

1 管理员模式 2 服务器地址，搬瓦工传输协议只能UDP 3 添加加速列表，名称任意，加速端口为服务器SS真实端口，本地端口是SS设置的本地端口 4 SS设置服务器地址127.0.0.1 端口任意，但要注意和FS的本地端口一样，密码为真实SS密码，加密方式为服务器加密方式。 完成
