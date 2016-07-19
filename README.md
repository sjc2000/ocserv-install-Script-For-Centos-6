Ocserv install script for CentOS 6
=======================================
这是 ocserv 在 CentOS 6的一键安装脚本，在最小化安装环境的 CentOS 6 下已测试通过<br />
由于CentOS 6的软件仓库中的大部分软件包版本过低，所以安装过程会有较多的编译安装，配置较差的主机所需时间可能会很长。</br>
此脚本基于 https://github.com/travislee8964/Ocserv-install-script-for-CentOS-RHEL-7 修改

* 与CentOS 7安装脚本不同的是防火墙仅支持iptables；<br />
* 默认采用用户名密码验证，本安装脚本编译的 ocserv 也支持 pam 验证，只需要修改配置文件即可；<br />
* 默认配置文件在 /usr/local/etc/ocserv/ 目录，可自行更改脚本里的参数；<br />
* 安装时会提示你输入端口、用户名、密码等信息，也可直接回车采用默认值，密码是随机生成的；<br />
* 安装脚本会关闭 SELINUX；<br />
* 默认安装完成后的路由表将不会影响公网访问，请自定义路由表。如果只需要全局VPN将ocserv.conf中的所有route全部注释掉即可；<br />
* 如果你有证书机构颁发的证书，可以把证书放到脚本的同目录下，确保文件名和脚本里的匹配，安装脚本会使用你的证书，客户端连接时不会提示证书错误；<br />
* 配置文件修改为每个账号允许 10 个连接，全局 1024 个连接，可修改脚本前面的变量。1024 个连接大约需要 2048 个 IP，所以虚拟接口的 IP 配置了 8 个 C 段。<br />

安装脚本分为以下几大块，如果中间有错误，可以注释掉部分然后重新执行脚本，ConfigEnvironmentVariable 为必须，后面的脚本会使用这里的变量<br />

* ConfigEnvironmentVariable // 配置环境变量<br />
* PrintEnvironmentVariable // 打印环境变量<br />
* CompileOcserv $@ // 下载并编译 ocserv（包括其前置依赖）<br />
* ConfigOcserv // 配置 ocserv，包括修改 ocserv.conf，配置 ocserv.service<br />
* ConfigFirewall // 配置防火墙，会自动判断防火墙为 iptables 或 firewalld<br />
* ConfigSystem  // 配置系统<br />
* PrintResult // 打印最后的安装结果和 VPN 账号等<br />
