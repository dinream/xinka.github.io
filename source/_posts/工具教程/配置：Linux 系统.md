# Linux 安装盘安装
## 准备工作
1. 准备一个空 U 盘，作为后续安装盘。
2. 下载 rufus  U 盘 格式化软件，分区类型：选择GPT，目标系统类型UEFI（非SM） [下载地址](https://rufus.ie/zh/#google_vignette) 
3. 下载 Ubuntu iso 镜像 [Ubuntu22.04.4](https://ubuntu.com/download/server/thank-you?version=22.04.4&architecture=amd64)
4. 使用 rufus 对安装盘进行制作，配置如下
	![[Pasted image 20240406101639.png]]
	磁盘分区方式：MBR和GPT，MBR的主分区只能划分最多4个，GPT的分区数量没有限制，这里选择 GPT。
## 开始安装
1. 开启服务器主机，进入系统 BIOS（Del键、F2键、F10键或者ESC键，依据主板型号，或者在开机界面一般会显示具体按键）
2. 选择 UEFI  U 盘作为启动盘。保存并重启系统。
	- Secure Boot设置：Security —> Secure Boot —> Disabled
	- Boot Option设置：Boot —> Boot Option #1 —> 将 UEFI USB Key: **** 选择至第一位
	- 保存BIOS设置：Exit—>Save Changes and Reset
3. 大部分默认设置。
	1. 网络 IP 使用默认？
	2. 安装源：http://mirrors.aliyun.com/ubuntu
	3. 磁盘分区：[参考](https://blog.csdn.net/2301_79810514/article/details/137122218)
		1. SWAP 分区：Linux 的虚拟内存，建议将交换分区的大小设置为物理内存的1.5倍到2倍。
		2. /boot 分区：引导分区，建议 300-500M ；
			【关于是否要分/boot分区的问题：如果你的主板bios里设置的是UEFI+GPT分区表模式，那么给ubuntu分区的时候不用设置这个/boot分区，设置下面第3步的efi系统分区即可； 但如果你用的是legacy+MBR分区表那就正常设置/boot分区，这个很重要，特别是装双系统或多系统时，避免破坏到其他系统的引导文件】
		3. EFI引导分区，类型为逻辑分区，，默认ext4。 推荐分 512 ~ 1024M，注意：放在空间起始位置。
		4. /var 分区（可选）：log 的日志文件存放，如果不分则默认在 / 下。如果 linux 用于服务器和经常做日志分析，建议划分。最少 300-500M，一般 2-3 G。
		5. 根分区（/）：存储操作系统文件、应用程序和用户数据。建议较大，15G+；
		6. home 分区：存放个人的文件，建议最大；（/ 和 /home 之间类似 C 和 D 盘的关系）
# Linux 系统配置
#### root 账户设置密码
当前普通用户界面下输入命令，然后按提示两次输入密码即可。

```sh
sudo passwd root
```

#### 新增用户并且增加用户权限
1. `useradd`：用于创建新用户账号。
    ```
    sudo useradd username
    ```
    其中，`username` 是要创建的新用户的用户名。该命令会创建一个新的用户账号，并分配一个默认的用户ID和组ID。
    
2. `passwd`：用于设置用户的密码。
    ```
    sudo passwd username
    ```
    其中，`username` 是要设置密码的用户的用户名。该命令会提示你输入新的密码，并要求确认密码。
    
3. `usermod`：用于修改用户的属性，包括用户权限。
    ```
    sudo usermod -aG groupname username
    ```
    其中，`groupname` 是要将用户添加到的用户组的组名，`username` 是要修改权限的用户的用户名。该命令会将指定用户添加到指定用户组中，从而增加用户的权限。
    注意：在执行 `usermod` 命令时，使用 `-aG` 选项是为了将用户添加到用户组而不覆盖用户原有的用户组。
    
4. 重新登录：在完成上述步骤后，建议重新登录用户账号，以使用户权限的更改生效。


#### 源配置
###### 1. 首先备份官方自带的软件源

`cp /etc/apt/sources.list /etc/apt/sources.list.bak`

###### 2.然后编辑修改为清华源

`sudo gedit sources.list`
#### ssh配置
```sh
# 查看是否安装了SSH服务
ps -ef | grep ssh 

# 没有安装的话，执行下面语句
sudo apt-get update                   #先更新下资源列表
sudo apt-get install openssh-server   #安装openssh-server
sudo ps -ef | grep ssh                #查看是否安装成功
sudo systemctl restart sshd           #重新启动SSH服务 

# 进入ssh配置文件
sudo vim  /etc/ssh/sshd_config    
```

`PermitRootLogin` 是一个用于配置 SSH 服务器的选项。这个选项决定了是否允许 root 用户通过 SSH 直接登录到服务器。通常情况下，为了提高安全性，最好禁止 root 用户通过 SSH 直接登录，而是使用一个普通用户登录后再通过 su 或者 sudo 切换到 root 用户来执行需要特权的操作。这样可以降低系统受到攻击的风险。  
常见的 PermitRootLogin 选项取值包括：

- `yes`：允许 root 用户通过 SSH 直接登录。
- `no`：禁止 root 用户通过 SSH 直接登录。
- `without-password`：允许 root 用户通过 SSH 密钥登录，但不允许使用密码登录。

按i进入编辑模式，找到`#PermitRootLogin prohibit-password`，默认是注释掉的。  
把 `PermitRootLogin without-password` 改为 `PermitRootLogin yes`，注意`PermitRootLogin without-password`被注释掉了，要去掉注释。如果没有找到`PermitRootLogin without-password`，直接文件末尾添加`PermitRootLogin yes`即可。然后按esc，输入:wq保存并退出。  
重启sshd服务

```sh
sudo systemctl restart sshd
```

#### 驱动更新

```sh
# 1. 更新软件包列表：
sudo apt update

# 2. 升级已安装的软件包：
sudo apt upgrade -y

# 3. 更新驱动程序：
sudo apt install ubuntu-drivers-common
sudo ubuntu-drivers autoinstall
```


#### 换源
```text
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
 
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
 
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
 
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse 
```
#### 网络配置

1. 打开终端，使用root权限登录或者使用sudo命令获取root权限。
    
2. 使用文本编辑器打开网络配置文件`/etc/netplan/00-installer-config.yaml`。
    
    ```bash
    sudo vim /etc/netplan/00-installer-config.yaml
    ```
    
3. 在文件中，找到`network`部分，然后根据你的网络设置进行编辑。以下是一个示例配置：
    
    ```yaml
    network:
      ethernets:
        enp0s3:
          addresses: [192.168.1.10/24]
          gateway4: 192.168.1.1
          nameservers:
            addresses: [8.8.8.8, 8.8.4.4]
      version: 2
    ```
    
    - `enp0s3`是网络接口的名称，你需要根据你的实际网络接口名称进行替换。
    - `addresses`是你的服务器的静态IP地址和子网掩码。
    - `gateway4`是你的网关IP地址。
    - `nameservers`是DNS服务器的IP地址。
4. 保存文件并关闭文本编辑器。
    
5. 在终端中执行以下命令以应用配置更改：
    
    ```bash
    sudo netplan apply
    ```
    
6. 重新启动网络服务以使更改生效：
    
    ```bash
    sudo systemctl restart networking
    ```
    

现在，你的 Ubuntu Server 已经配置了静态网络。你可以通过 ping 命令或打开浏览器测试网络连接。

#### 禁止自动休眠

1. 执行如下命令查看休眠模式的情况，如果 sleep 状态是loaded，也就是处于自动休眠开启状态

```sh
systemctl status sleep.target
```

2. 接下来，执行如下命令关闭系统的自动休眠开关：

```sh
# 这些命令做了以下几点：
# 通过mask命令，你禁用了所有休眠相关的systemd目标。
# 通过set-default命令，你设置默认的运行级别为多用户文本模式（即命令行界面）
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
sudo systemctl set-default multi-user.target

####################################################
# 如果你想要恢复自动休眠功能，可以使用以下命令：
sudo systemctl unmask sleep.target suspend.target hibernate.target hybrid-sleep.target
sudo systemctl set-default graphical.target

```

3. 再次执行查看命令，可以看到 sleep 的状态已经变成了 masked，也就是关闭了。
4. 编辑`/etc/systemd/logind.conf`文件：
    
    ```bash
    sudo vim /etc/systemd/logind.conf
    ```
    
5. 在文件中找到以下行：
    
    ```bash
    #IdleAction=
    #IdleActionSec=
    ```
    
6. 将这两行的注释符号`#`移除，并设置`IdleAction`为`ignore`，`IdleActionSec`为`0`，如下所示：
    ```text
	    IdleAction=ignore
	    IdleActionSec=0
    ```
	
7. 保存文件并关闭编辑器。
8. 使用以下命令重新加载systemd配置：
    
    ```undefined
    sudo systemctl restart systemd-logind.service
    ```
    
9. 完成！现在 Ubuntu Server 将不会因为无操作而自动休眠。

#### 设置关盖/合盖不挂起/不睡眠

1. 通过更改登录配置文件`logind.conf`设置

```sh
sudo vim /etc/systemd/logind.conf
```

2. 里面影响关盖操作的变量包括`HandleLidSwitch`、`HandleLidSwitchExternalPower`和`HandleLidSwitchDocked`。  
    `logind.conf` 里面各项的含义如下：

```ini
1. NAutoVTs=6：指定系统启动时自动分配的虚拟终端数量。
2. ReserveVT=6：指定系统保留的虚拟终端数量。
3. KillUserProcesses=no：指定当用户注销时是否终止用户进程。
4. KillOnlyUsers=：指定需要终止进程的用户列表。
5. KillExcludeUsers=root：指定不需要终止进程的用户列表。
6. InhibitDelayMaxSec=5：指定阻止操作的最大延迟时间（以秒为单位）。
7. HandlePowerKey=poweroff：指定按下电源键时的操作，此处设置为关机。
8. HandleSuspendKey=suspend：指定按下休眠键时的操作，此处设置为休眠。
9. HandleHibernateKey=hibernate：指定按下休眠键时的操作，此处设置为休眠。
10. HandleLidSwitch=suspend：指定关闭笔记本电脑盖子时的操作，此处设置为休眠。
11. HandleLidSwitchExternalPower=suspend：指定在外部电源连接时关闭笔记本电脑盖子的操作，此处设置为休眠。
12. HandleLidSwitchDocked=ignore：指定连接到底座时关闭笔记本电脑盖子的操作，此处设置为忽略。
13. PowerKeyIgnoreInhibited=no：指定是否忽略阻止操作的电源键按下。
14. SuspendKeyIgnoreInhibited=no：指定是否忽略阻止操作的休眠键按下。
15. HibernateKeyIgnoreInhibited=no：指定是否忽略阻止操作的休眠键按下。
16. LidSwitchIgnoreInhibited=yes：指定是否忽略阻止操作的盖子关闭。
17. HoldoffTimeoutSec=30s：指定在执行操作前等待的时间（以秒为单位）。
18. IdleAction=ignore：指定系统处于空闲状态时的操作，此处设置为忽略。
19. IdleActionSec=30min：指定系统处于空闲状态多长时间后执行操作（以分钟为单位）。
20. RuntimeDirectorySize=10%：指定运行时目录的最大大小，此处设置为总磁盘空间的10%。
21. RemoveIPC=yes：指定是否在用户注销时删除IPC对象。
22. InhibitorsMax=8192：指定系统允许的阻止操作的最大数量。
```

`HandleLidSwitch`: 定义笔记本电脑关闭盖子时的行为。可以设置的值有：

- `ignore`：忽略关闭盖子的事件。
- `poweroff`：关闭电源。
- `reboot`：重新启动。
- `halt`：停止系统。
- `kexec`：通过 kexec 进行快速启动。
- `suspend`：挂起系统。
- `hibernate`：休眠系统。

`HandleLidSwitchExternalPower`: 定义当笔记本电脑连接外部电源时关闭盖子的行为。可以设置的值与 `HandleLidSwitch` 相同。

`HandleLidSwitchDocked`: 定义当笔记本电脑连接到底座时关闭盖子的行为。可以设置的值与 `HandleLidSwitch` 相同。

`HandlePowerKey`: 定义电源按钮的行为。可以设置的值与 `HandleLidSwitch` 相同。

`IdleAction`: 定义系统空闲时的行为。可以设置的值有：

- `ignore`：忽略空闲事件。
- `poweroff`：关闭电源。
- `reboot`：重新启动。
- `halt`：停止系统。
- `kexec`：通过 kexec 进行快速启动。
- `suspend`：挂起系统。
- `hibernate`：休眠系统。

`IdleActionSec`: 定义系统空闲多少秒后触发 `IdleAction`。

`UserTasksMax`: 定义每个用户可以同时运行的任务数的最大值。

`KillUserProcesses`: 定义当用户注销时是否终止用户的进程。可以设置的值有：

- `yes`：终止用户的进程。
- `no`：不终止用户的进程。

#### 定时自动关闭显示器

1. 想要定时关闭显示器，可以编辑`/etc/default/grub`这个文件
2. 通过修改GRUB_CMDLINE_LINUX_DEFAULT变量，你可以添加或修改传递给内核的参数，以便在启动时对系统进行自定义配置。

```sh
# ipv6.disable=1：禁用 IPv6 协议。
# consoleblank=300：设置控制台空闲时间为 300 秒后自动关闭。
GRUB_CMDLINE_LINUX_DEFAULT="ipv6.disable=1 consoleblank=300"
```

3. 修改完了运行`sudo update-grub`，然后重启，即可设置系统5分钟没有活动自动关闭显示器。

#### 网络自动认证
https://github.com/kewuaa/uestc_wifi_helper/releases/download/v0.4.1/uestc_wifi_helper-x86_64-linux

1. 将软件放在 /root/path/uestc-network-login/uestc_wifi_helper_cli-x86_64-linux
2. 将脚本放在 /root/boot/auto-login-uestc-network.sh
3. 部署服务在 /etc/systemd/system/auto-network-login.service
4. 启动服务。
#### 外接 U 盘挂载

1. 确认 U 盘被正确地检测到：在终端中输入以下命令来查看系统识别的存储设备列表：
   ```
   lsblk
   ``` 
   这将显示所有已连接的存储设备，包括 U 盘。通常，U 盘的设备名称类似于 `/dev/sdb` 或 `/dev/sdc`。

2. 挂载 U 盘：在 Linux 中，需要将 U 盘挂载到文件系统才能访问其中的文件。首先，创建一个用于挂载的目录。在终端中输入以下命令：
   ```
   sudo mkdir /mnt/usb
   ```
   这将创建一个名为 `/mnt/usb` 的目录作为挂载点。

3. 挂载 U 盘：使用 `mount` 命令将 U 盘挂载到刚刚创建的目录。假设 U 盘的设备名称为 `/dev/sdb1`，在终端中输入以下命令：
   ```
   sudo mount /dev/sdb1 /mnt/usb
   ```
   如果 U 盘有多个分区，将 `/dev/sdb1` 替换为相应的分区设备名称。

4. 访问 U 盘中的文件：现在，可以通过访问挂载点 `/mnt/usb` 来查看和操作 U 盘中的文件。你可以使用文件管理器（如 Nautilus、Dolphin 或 Thunar）浏览 U 盘，也可以在终端中使用命令行来执行相关操作。

5. 卸载 U 盘：在完成对 U 盘的操作后，应该将其卸载以安全地移除。在终端中输入以下命令来卸载 U 盘：
   ```
   sudo umount /mnt/usb
   ```
   确保在操作完成后再拔出 U 盘。

请注意，U 盘的设备名称和挂载点可能会根据系统和具体情况有所不同。因此，在使用上述命令时，请根据你的实际情况进行相应的调整。

#### 域名配置
在 Ubuntu 22.04 TLS 上进行动态 DNS 部署的步骤：
0. 注册 DNS 服务商账号
	[ZoneEdit - Control Panel: Sign Up](https://cp.zoneedit.com/signup/)
1. 安装 `ddclient`：
   打开终端并执行以下命令来安装 `ddclient`：
   ```
   sudo apt update
   sudo apt install ddclient
   ```

2. 配置 `ddclient`：
   `ddclient` 的配置文件位于 `/etc/ddclient.conf`。使用你喜欢的文本编辑器（如 nano）打开该文件：
   ```
   sudo nano /etc/ddclient.conf
   ```

   在配置文件中，你需要提供动态 DNS 服务提供商的相关信息，例如域名、用户名、密码等。具体配置取决于你选择的动态 DNS 服务提供商。以下是一个示例配置文件的部分内容：

   ```
   # 使用动态 DNS 服务提供商的示例配置
   protocol=dyndns2
   server=members.dyndns.org
   login=username
   password='password'
   your.domain.com
   
   # DIY
   protocol=zoneedit1
   use=if, if=eno1
   server=dns1.zoneedit.com 
   ssl=yes 
   login=dinream 
   password='dreamin8888' 
   lm.lab302.com
   
   ```

   替换示例配置中的 `protocol`、`server`、`login`、`password` 和 `your.domain.com` 为你的实际设置。请参考你选择的动态 DNS 服务提供商的文档，获取正确的配置参数。

3. 保存并关闭配置文件。

4. 启用并启动 `ddclient` 服务：
   执行以下命令启用 `ddclient` 服务：
   ```
   sudo systemctl enable ddclient
   ```

   然后，执行以下命令启动服务：
   ```
   sudo systemctl start ddclient
   ```

   `ddclient` 将会读取配置文件并在启动时或 IP 地址变化时更新动态 DNS 记录。

5. 验证配置：
   执行以下命令来检查 `ddclient` 服务的状态：
   ```
   sudo systemctl status ddclient
   ```

   如果状态显示为 "active"，则表示 `ddclient` 服务已成功启动并正在运行。
6. 查看详细信息
	```sh
	ddclient -foreground  -verbose -force

	ddclient -daemon=0 -debug -verbose -noquiet

	```
7. 注意域名的更新在域名服务器之间可能需要一个小时左右，需要等待。
https://blog.cre0809.com/archives/231/
内网穿透：
https://juejin.cn/post/7346072037674418187
多种方式解决：
[如何映射我的私人IP动态变化到我的VPS IP？ - adsl - 码客 (oomake.com)](https://www.oomake.com/question/9612393)


# 网络高级配置
## 双网卡跨网卡 iptables 完成数据转发
目标：将主机的一个 ip 的发送包，转换为主机另一个 ip 的发送包。

1. 配置网卡IP地址， 配置静态IP地址， 
	/etc/sysconfig/network-stripts/ifcfg-eth0 配置网卡一， 外网卡， 可以联网
	/etc/sysconfig/network-stripts/ifcfg-eth1 配置网卡二， 内网卡， 连接内网

	配置网卡配置文件
	DEVICE=eth1
	IPADDR=192.168.145.122// IP地址  
	NETMASK=255.255.255.0// IP掩码  
	gateway=192.168.142.2// 外网网关地址  
	ONBOOT=yes
	
	或者配置/etc/sysconfig/network 添加网关地址
	gateway=192.168.142.2  // 外网网关地址

  

2. 打开包转发功能：

	echo "1" > /proc/sys/net/ipv4/ip_forward

3. 修改/etc/sysctl.conf文件， 让包转发功能在系统启动时自动生效：

	net.ipv4.ip_forward = 1

4. 添加iptables nat路由规则： // 清楚其他不需要的iptables规则

	/sbin/iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE// 其中网卡eth0 为外网卡。  

5. 保存规则到/etc/sysconfig/iptables 配置文件中

	service iptables save

6. 重新启动iptables

	service iptables restart



1、首先确保转发开关打开

	vim /etc/sysctl.conf

		net.ipv4.ip_forward = 1

	sysctl -p

2、核对双网卡配置

	cat /etc/sysconfig/network-scripts/ifcfg-eth0
		DEVICE=eth0  
		HWADDR=  
		TYPE=Ethernet  
		UUID=61e9bdb5-b1ff-4c8f-8c19-938c834de345  
		ONBOOT=yes  
		NM_CONTROLLED=yes  
		BOOTPROTO=static  
		IPADDR=192.8.8.57  
		NETMASK=255.255.255.224  
		GATEWAY=192.8.8.62

	cat /etc/sysconfig/network-scripts/ifcfg-eth2
		DEVICE=eth2
		HWADDR=
		TYPE=Ethernet
		ONBOOT=yes
		NM_CONTROLLED=no
		BOOTPROTO=static
		IPADDR=11.10.10.2
		NETMASK=255.255.255.0


3、执行 iptables 并保存 

	iptables -t nat -A POSTROUTING -s 11.10.10.0/24 -j SNAT  --to 192.8.8.57  
	service iptables save


## 局域网自动认证 TODO
对于学校或者公司，每次主机重启可能需要进行账号的认证，才可上网，这里实现一个脚本进行自动登录
```text
一个请求
[10.253.0.237/cgi-bin/rad_user_info?callback=jQuery]
(http://10.253.0.237/cgi-bin/rad_user_info?callback=jQuery)
可以获取，断开网线没有响应。

jQuery({"client_ip":"113.54.145.42","ecode":0,"error":"not_online_error","error_msg":"","online_ip":"113.54.145.42","res":"not_online_error","srun_ver":"SRunCGIAuthIntfSvr V1.18 B20181212","st":1712478100})


登录页面的请求。
请求 URL:
http://10.253.0.237/srun_portal_pc?ac_id=1&theme=dx
请求方法:
GET
Status Code:
200 OK
远程地址:
10.253.0.237:80
Referrer Policy:
strict-origin-when-cross-origin


请求指令
http://10.253.0.237/cgi-bin/get_challenge?callback=jQuery112405824069609933546_1712476428639&username=202321081216%40dx-uestc&ip=8.8.8.8%2C+113.54.145.42

Request URL:

http://10.253.0.237/cgi-bin/get_challenge?callback=jQuery1124028377090795615145_1712478534516&username=202321081216%40dx-uestc&ip=113.54.145.42&_=1712478534518

Request Method:

GET

Status Code:

502 Bad Gateway

Remote Address:

127.0.0.1:7890

Referrer Policy:

strict-origin-when-cross-origin


如果连接成功，http://10.253.0.237/cgi-bin/rad_user_info 也可也正常获取数据，但是是字符串形式。而不是json

http://10.253.0.237/cgi-bin/srun_portal?callback=jQuery112405960456199614257_1712479784865&action=login&username=202321081216%40dx-uestc&password=%7BMD5%7Dfb5b782af15febf4ebd53a45952273b8&ac_id=1&ip=8.8.8.8%2C+113.54.145.42&chksum=567d3b8c5bd5ef9c7f14c76b487e7ba67a3d245a&info=%7BSRBX1%7D3OHj0r4O4ZBBY1c%2FRJUyhmdftyTtmQ%2BTHXWm%2BGqNqPp2dL59T78gfgRJXnmUW55cnpEZ3LfJ4NvTAu0bX76wfxQdKZrCon42SRXUdymVpUCnBoBvn1cq7pCYcgK%2BGQ7GA%2BClDLGqvIsHwc5ue8FYqip54p4C2MYgtlHTfv%3D%3D&n=200&type=1&os=Windows+10&name=Windows&double_stack=0&_=1712479784868

```
curl -d "username=202321081216&password=feng0408" http://10.253.0.237/cgi-bin/srun_portal?callback=jQueryaction=login/
```
{
    "Code": 0,
    "Message": "ok",
    "Data": [
        {
            "Id": 3,
            "Title": "清水河校区办公教学区通知",
            "Content": "\u003cp style=\"margin-top:auto;margin-bottom: auto;text-align:left;text-indent:36px;line-height: 24px;background:white\"\u003e\u003cspan style=\"font-size: 18px;font-family:宋体\"\u003e清水河校区办公教学区通过墙面预制网络接口连接到有线校园网。连接成功后，打开浏览器输入aaa.uestc.edu.cn弹出认证页面，可选择校园网登录或电信登录。具体使用方法如下：\u003c/span\u003e\u003c/p\u003e\u003cp style=\"margin-top:auto;margin-bottom: auto;text-align:left;line-height:24px;background: white\"\u003e\u003cspan style=\"font-size:18px;font-family:宋体\"\u003e1\u003c/span\u003e\u003cspan style=\"font-size:18px;font-family: 宋体\"\u003e、校园网登录：在右侧登录框输入统一身份认证信息（工资号或学号），点击校园网登录。\u003c/span\u003e\u003c/p\u003e\u003cp style=\"margin-top:auto;margin-bottom: auto;text-align:left;line-height:24px;background: white\"\u003e\u003cspan style=\"font-size:18px;font-family:宋体\"\u003e2\u003c/span\u003e\u003cspan style=\"font-size:18px;font-family: 宋体\"\u003e、电信登录：先点击右侧自服务按钮，进行电信账号绑定；绑定完成重新打开aaa.uestc.edu.cn，输入学校统一身份认证信息，点击电信登录。电信账号绑定流程详见《清水河有线校园网使用手册》。\u003c/span\u003e\u003c/p\u003e\u003cp style=\"margin-top:auto;margin-bottom: auto;text-align:left;line-height:24px;background: white\"\u003e\u003cstrong\u003e\u003cspan style=\"font-size:18px;font-family:宋体\"\u003e用户服务电话：61831184\u003c/span\u003e\u003c/strong\u003e\u003c/p\u003e\u003cp\u003e\u003cbr/\u003e\u003c/p\u003e",
            "Created_by": "srun",
            "Created_at": 1622792930,
            "Updated_at": 1622792930
        },
        {
            "Id": 2,
            "Title": "清水河行政教学区有线网络割接通知",
            "Content": "\u003cp style=\"margin-top:5px;margin-right:0;margin-bottom:5px;margin-left: 0;text-indent:28px\"\u003e\u003cspan style=\"font-size: 18px;\"\u003e\u003c/span\u003e\u003c/p\u003e\u003cp style=\"line-height: 1.5em;\"\u003e清水河校区有线网络设备已运行10年以上，故障频发，不支持IPv6，802.1X认证系统对于终端兼容性较差。为解决以上问题，信息中心协调中国电信高新西区分公司对清水河校区有线网进行改造。改造后网络使用web认证，支持IPv6协议，用户可选择校园网登录或电信登录。具体使用方法如下：\u003c/p\u003e\u003cp style=\"line-height: 1.5em;\"\u003e1、校园网登录：在右侧登录框输入统一身份认证信息，点击校园网登录。\u003c/p\u003e\u003cp style=\"line-height: 1.5em;\"\u003e2、电信登录：先点击右侧自服务按钮，进行电信账号绑定；绑定完成重新打开aaa.uestc.edu.cn，输入学校统一身份认证信息，点击电信登录。电信账号绑定流程详见《清水河有线校园网使用手册》。\u003c/p\u003e\u003cp style=\"line-height: 1.5em;\"\u003e我们对给您带来的不便表示歉意！如有任何疑问，请联系信息中心61831184。\u003c/p\u003e\u003cp style=\"margin-top:5px;margin-right:0;margin-bottom:5px;margin-left: 0;text-indent:28px;font-variant-ligatures: normal;font-variant-caps: normal;orphans: 2;text-align:start;widows: 2;-webkit-text-stroke-width: 0px;word-spacing: 0px\"\u003e\u003cbr/\u003e\u003c/p\u003e\u003cp\u003e\u003cbr/\u003e\u003c/p\u003e",
            "Created_by": "yqli",
            "Created_at": 1544507894,
            "Updated_at": 1622792876
        }
    ]
}
```

# 程序自启动
在 Ubuntu 中，你可以使用 Systemd 来配置在系统启动时自动后台执行可执行程序。

下面是一些基本的步骤：

1. 创建一个用于 Systemd 配置的服务文件。在终端中使用以下命令创建一个新的服务文件：
   ```
   sudo nano /etc/systemd/system/test.service
   ```

2. 在打开的编辑器中，输入以下内容：
   ```
   [Unit]
   Description=Test Service
   After=network.target

   [Service]
   ExecStart=/path/to/test
   Type=simple
   Restart=always
   User=your_username

   [Install]
   WantedBy=multi-user.target
   ```

   请确保将 `/path/to/test` 替换为你的可执行程序的实际路径，并将 `your_username` 替换为你的用户名。

3. 保存并关闭文件。使用 `Ctrl + X`，然后输入 `Y` 保存文件并退出编辑器。

4. 重新加载 Systemd 配置，使其识别新的服务文件。在终端中使用以下命令：
   ```
   sudo systemctl daemon-reload
   ```

5. 启用服务，使其在系统启动时自动运行。在终端中使用以下命令：
   ```
   sudo systemctl enable test.service
   ```

6. 最后，启动服务。在终端中使用以下命令：
   ```
   sudo systemctl start test.service
   ```

现在，你的可执行程序 `test` 将在系统启动时自动以后台进程的形式运行。

你可以使用以下命令来检查服务的状态：
```
sudo systemctl status test.service
```

如果需要停止服务，可以使用以下命令：
```
sudo systemctl stop test.service
```


# 后台运行程序


1. 在执行命令时，可以使用 `&` 符号将其放在命令的末尾。这样会将命令放入后台运行，并且不会阻塞当前终端。例如：
```
	./test & 
```
2.  如果你希望程序在后台运行的同时不产生任何输出，你可以将其输出重定向到 `/dev/null`，这样输出将被丢弃。例如：
```
	nohup ./clash-linux-amd64 > /dev/null 2>&1 &
	nohup ./aaa > output.log 2>&1 &
	disown

```
这样，可执行程序 `./test` 将在后台运行，并与当前终端解耦。它不会阻塞当前终端，并且输出将被重定向到 `/dev/null`，不会显示在终端上。

请注意，使用这种方法运行程序后，你将无法看到程序的输出。如果你需要查看程序的输出或进行其他交互操作，可能需要使用其他工具或技术来实现。



# 离线安装包的使用
## Centos
1. 打包
	```bash
	yum install [package]--downloadonly --downloaddir=dir/
	```
2. 使用
	```bash
	yum -y localinstall dir/*
```


# 查询指定域名的 IP

```sh
nsloopup
```


# 日志查看
```sh
/var/log/syslog
/var/log/kern.log
```

# 查看显存运行情况


```shell
watch -n 0.1 nvidia-smi
```
