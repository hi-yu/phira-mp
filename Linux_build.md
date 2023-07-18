# Linux搭建 Phira 联机服务器（phira-mp）

## 一、CentOS7

1.更新源

```
yum update -y
```

2.安装开发工具组

```
yum groupinstall -y "Development Tools"
```

3.安装openssl

```
yum install -y openssl-devel
```

4.安装git

```
yum install -y git
```

## 二、Ubuntu20.02

1.更新源

```
sudo apt update
```

2.安装开发工具组

```
sudo apt install build-essential
```

3.安装openssl

```
sudo apt install libssl-dev
```

4.安装git

```
sudo apt install git
```

## 后续步骤相同



5.安装rust，运行后按1即可

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

6.添加环境变量

```
source "$HOME/.cargo/env"
```

5.克隆仓库

```
git clone https://github.com/TeamFlos/phira-mp.git
```

6.进入仓库目录，编译

```
cd phira-mp

cargo build --release -p phira-mp-server
```

7.编译完成后进入target/releases运行

```
cd target/releases

./phira-mp-server
```

8.日志文件在同目录log文件夹下

```
cd log

cat phira-mp.2023-mm-dd-mm
```

9.进程守护

```
vim /usr/lib/systemd/system/phiramp.service
```

```
[Unit]
Description=Phira-mp
Documentation=https://github.com/TeamFlos/phira-mp/blob/main/README.md
After=network.target

[Service]
WorkingDirectory=/PATH_TO_PHIRA-MP
ExecStart=/PATH_TO_PHIRA-MP/phira-mp-server

Restart=on-abnormal
RestartSec=5s
KillMode=mixed

StandardOutput=null
StandardError=syslog

[Install]
WantedBy=multi-user.target
```

如果上面的所有操作都是在root目录下完成的，可以直接将

```
WorkingDirectory=/root/phira-mp/target/release
ExecStart=/root/phira-mp/target/release/phira-mp-server
```

替换为

```
WorkingDirectory=/root/phira-mp/target/release
ExecStart=/root/phira-mp/target/release/phira-mp-server
```

10.重载配置

```
systemctl daemon-reload
```

11.使用systemctl管理phira-mp

```
#启动
systemctl start phiramp
#重启
systemctl restart phiramp
#停止
systemctl stop phiramp
#查看状态
systemctl status phiramp
```

