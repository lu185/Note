# `MySQL`安装与配置
### 安装 `mysql`
```bash
$ sudo apt-get install mysql-server-5.7
```
### 关于安装完成后不能登陆的解决方法：
1. 修改 `MySQL` 配置文件 `vim /etc/mysql/mysql.conf.d/mysqld.cnf`
2. 在 配置文件 `[mysqld]` 末尾追加 `skip-grant-tables` 跳过登陆验证 
3. 重启服务 `service mysql restart`
4. 进入数据库,使用 `mysql` 表
5. 执行语句更新用户密码 `update user set authentication_string=password("你的密码"),plugin='mysql_native_password' where user='root';`
6. 执行语句刷新权限 `flush privileges;`
7. 退出数据库，并且重新修改 `MySQL` 配置文件删除 `skip-grant-tables` 后重启服务

### 运行 `SQL` 文件
```
source sql文件路径
```

```
外部主机不允许连接Mysql设置的解决方法
Mysql:is not allowed to connect to this MySQL server
如果你想连接你的mysql的时候发生这个错误：

ERROR 1130: Host '192.168.1.3' is not allowed to connect to this MySQL server

解决方法：

改表法。可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"
mysql -u root -pvmwaremysql>use mysql;mysql>update user set host = '%' where user = 'root';mysql>select host, user from user;
授权法。例如，你想myuser使用mypassword从任何主机连接到mysql服务器的话。
GRANT ALL PRIVILEGES ON . TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
如果你想允许用户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用mypassword作为密码
GRANT ALL PRIVILEGES ON . TO 'root'@'192.168.1.3' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON . TO 'root'@'10.10.40.54' IDENTIFIED BY '123456' WITH GRANT OPTION
```

# 没有`ipconfig`命令
### 安装 `net-tools`
```
sudo apt-get install net-tools
```

# 时间同步
```
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```

# 音质提升
```
sudo apt install jackd pulseaudio-module-jack caps
```

# `vim`基本配置
```
# 编辑 ~/.vimrc文件

set ts=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
```

# sougoupingyin
```
# pacman -S base-devel

sudo pacman -S fcitx-sogoupinyin fcitx-configtool fcitx-im

sudo pacman -U https://arch-archive.tuna.tsinghua.edu.cn/2019/04-29/community/os/x86_64/fcitx-qt4-4.2.9.6-1-x86_64.pkg.tar.xz

vim  ~/.xprofile

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"

qtconfig-qt4
```


# ` pacman`命令

1、列出已经安装的软件包

pacman -Q
 

2、查看sqlmap包是否已经安装

pacman -Q sqlmap
 

3、查看已安装的包sqlmap的详细信息

pacman -Qi sqlmap
 

4、列出已安装包sqlmap的所有文件

pacman -Ql sqlmap
 

5、查找某个文件属于哪个包？

pacman -Qo /etc/passwd
 

6、查询包组

pacman -Sg
 

7、查询包组所包含的软件包

pacman -Sg blackarch
 

8、搜索sqlmap相关的包

pacman -Ss sqlmap
 

9、从数据库中搜索sqlmap的信息

pacman -Si sqlmap
 

10、仅同步源

sudo pacman -Sy
 

10、更新系统

sudo pacman -Su
 

11、同步源并更新系统

sudo pacman -Syu
 

12、同步源后安装sqlmap包

sudo pacman -Sy sqlmap
 

13、从本地数据库中获取sqlmap的信息，并下载安装

sudo pacman -S sqlmap
 

14、强制安装sqlmap包

sudo pacman -Sf sqlmap
 

15、删除sqlmap

sudo pacman -R sqlmap
 

16、强制删除被依赖的包（慎用）

sudo pacman -Rd sqlmap
 

17、删除sqlmap包及依赖其的包

sudo pacman -Rc sqlmap
 

18、删除sqlmap包及其依赖的包

sudo pacman -Rsc sqlmap
 

19、清理/var/cache/pacman/pkg目录下的旧包

sudo pacman -Sc
 

20、清除所有下载的包和数据库

sudo pacman -Scc
 

21、安装下载的virtualbox包（有时候需要降级包的时候就用这个）

cd /var/cache/pacman/pkgsudo
pacman -U virtualbox-5.2.12-1-x86_64.pkg.tar.xz
 

22、升级时不升级sqlmap包

sudo pacman -Su --ignore sqlmap