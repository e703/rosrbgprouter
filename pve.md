Proxmox VE 8.0 日常维护
移除未使用的Linux内核
若不存在pvekclean，请先安装
git clone https://github.com/jordanhillis/pvekclean.git
cd pvekclean
chmod +x pvekclean.sh
安装完成后执行pvekclean即可

./pvekclean.sh
日常软件更新命令：
apt update -y && apt dist-upgrade -y
更新PVE，并安装常用软件
apt-get update && apt-get install vim lrzsz unzip net-tools curl screen uuid-runtime git -y && apt dist-upgrade -y
Proxmox VE 6.3 / 6.4 / 7.0 / 7.1 / 7.2 / 7.3 / 7.4 / 8.0 去掉未订阅的提示
sed -i_orig "s/data.status === 'Active'/true/g" /usr/share/pve-manager/js/pvemanagerlib.js
sed -i_orig "s/if (res === null || res === undefined || \!res || res/if(/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
sed -i_orig "s/.data.status.toLowerCase() !== 'active'/false/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
systemctl restart pveproxy
确认无误后，重新启动服务器
reboot
PVE免密登录
cd ~
mkdir .ssh
echo ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAyq1pB5aF0w6ps4OzwQl1C8uP41Iq7J+gqylLMXkoESrTUVhH1+irHuImxi2At886sO7x9s+b4jhRZoJZpZURPU4UmzUEBHKoXlqOf9eO//GtUita2AaPFw5tc0YgLPrgnO+z5MKfjo20aoJtVBvleRA/0YJcWy1a6ufXa8944D8a1Dirc9uVNR5QjKVFRbQt/twLkLdFB6t16HCwISKCVI56DcJOoY2g7mXI8clKaESeB+ANIhSKJclPwjoC6P0pHFfgqNauxC+0xugx3W2ZSIkVhdZu1L7iKvzXXPiETjPQA6qMjp/1dY2WU49Lf+wDOQplCy4HLq7QqNNVSzIBGw== Administrator@PCOS-1407251925 >> ~/.ssh/authorized_keys
配置DNS，解决无法上网的问题
新增阿里云的公共DNS
vi /etc/resolv.conf
:d9999
nameserver 223.5.5.5
nameserver 223.6.6.6
重启网络服务
service networking restart
如何设置国内源 - For PVE 6.x
设置 debian 阿里云源 - For PVE 6.x
cat > /etc/apt/sources.list <<EOF
deb http://mirrors.huaweicloud.com/debian/ buster main non-free contrib
deb http://mirrors.huaweicloud.com/debian/ buster-updates main non-free contrib
deb http://mirrors.huaweicloud.com/debian/ buster-backports main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian/ buster main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian/ buster-updates main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian/ buster-backports main non-free contrib
deb http://mirrors.huaweicloud.com/debian-security/ buster/updates main non-free contrib
deb-src http://mirrors.huaweicloud.com/debian-security/ buster/updates main non-free contrib
EOF
删除企业源 - For PVE 6.x
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
下载秘钥 - For PVE 6.x
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg
添加国内源 - For PVE 6.x
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve buster pve-no-subscription" >/etc/apt/sources.list.d/pve-install-repo.list
apt update -y && apt dist-upgrade -y
如何设置国内源 - For PVE 7.x
设置 debian 阿里云源 - For PVE 7.x
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
EOF
删除企业源 - For PVE 7.x
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
下载秘钥 - For PVE 7.x
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg
添加国内源 - For PVE 7.x
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve bullseye pve-no-subscription" >/etc/apt/sources.list.d/pve-install-repo.list
apt update -y && apt dist-upgrade -y
如何设置国内源 - For PVE 8.x
设置 debian 阿里云源 - For PVE 8.x
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.huaweicloud.com/debian/ bookworm main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm main non-free contrib
deb https://mirrors.huaweicloud.com/debian-security/ bookworm-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ bookworm-security main
deb https://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free contrib
deb https://mirrors.huaweicloud.com/debian/ bookworm-backports main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bookworm-backports main non-free contrib
EOF
删除企业源 - For PVE 8.x
rm -rf /etc/apt/sources.list.d/pve-enterprise.list
下载秘钥 - For PVE 8.x
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg
添加国内源 - For PVE 8.x
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve bookworm pve-no-subscription" >/etc/apt/sources.list.d/pve-install-repo.list
apt update -y && apt dist-upgrade -y
配置DNS，解决无法上网的问题
新增阿里云的公共DNS
vi /etc/resolv.conf
:d9999
nameserver 223.5.5.5
nameserver 223.6.6.6
重启网络服务
service networking restart
安装并设置NTP服务 - For PVE 6.x
参考https://pve.proxmox.com/wiki/Time_Synchronization

新增阿里云的公共NTP地址
mv /etc/systemd/timesyncd.conf /etc/systemd/timesyncd.conf_bak
echo [Time] >> /etc/systemd/timesyncd.conf
echo NTP=ntp1.aliyun.com ntp2.aliyun.com ntp3.aliyun.com ntp4.aliyun.com ntp5.aliyun.com ntp6.aliyun.com ntp7.aliyun.com >>    /etc/systemd/timesyncd.conf
cat /etc/systemd/timesyncd.conf
timedatectl set-ntp true 
timedatectl status
安装并设置NTP服务 - For PVE 7.x / 8.x
参考https://help.aliyun.com/document_detail/187016.html?utm_content=g_1000230851&spm=5176.20966629.toubu.3.f2991ddcpxxvD1#title-ik2-31x-dso

新增阿里云的公共NTP地址
vim /etc/chrony/chrony.conf
新增下面的server
# Aliyun NTP
server ntp1.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp2.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp3.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp4.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp5.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp6.aliyun.com minpoll 4 maxpoll 10 iburst
server ntp7.aliyun.com minpoll 4 maxpoll 10 iburst
使用命令行修改PVE默认的IP地址
vim /etc/network/interfaces
会出现类似下面的配置文件

auto lo
iface lo inet loopback

iface enp2s0 inet manual

auto vmbr0
iface vmbr0 inet static
	address 192.168.1.2
	netmask 255.255.255.0
	gateway 192.168.1.1
	bridge_ports enp2s0
	bridge_stp off
	bridge_fd 0
建议只修改address，netmask和gateway这3个配置值即可，含义分别是IP地址，子网掩码和网关地址。

如果更新时出现错误 E: Sub-process /usr/bin/dpkg returned an error code
https://blog.csdn.net/yusiguyuan/article/details/24269129

apt-get update --fix-missing
apt-get autoremove && sudo apt-get clean && sudo apt-get install -f
如果更新时出现错误 You are attempting to remove the meta-package 'proxmox-ve'
参考https://forum.proxmox.com/threads/apt-get-dist-upgrade-wants-to-remove-proxmox-ve-pve-firmware.39360/

#Yes, I've tested it. I can remove any kernels listed with this command:
#列出当前系统的Linux镜像
dpkg --list | egrep -i --color 'linux-image|linux-headers'
#Then:
#删除旧的Linux镜像
apt-get --purge remove linux-image-4.9.0-4-amd64 linux-image-4.9.0-5-amd64
#更新grub
update-grub
解决locale: Cannot set LC_CTYPE to default locale: No such file or directory报错
参考文章

# localedef -i en_US -f UTF-8 en_US.UTF-8
