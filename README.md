
# 利用Github快速编译OpenWrt，可选择编译Passwall插件（含最新的Hysteria和Sing-Box)

Github在线编译提供了高性能的虚拟服务器环境，基于它可以进行构建、测试、打包、部署项目。利用它可以省去本地搭建或者购买服务器的时间成本，你只需要使用本仓库的代码，按照下方的使用方法，修改一些参数，即可开始编译openwrt，等待几个小时后，你就可以下载固件了。

说明：源码来自各位大佬分享，为了方便编译，做了一些修改，可以支持不同分支的opewrt源码，同时集成了打包img镜像的功能。

- 官方源码：    https://github.com/openwrt/openwrt      
- lede源码：    https://github.com/coolsnowwolf/lede  
- lienol源码：  https://github.com/Lienol/openwrt 
- immortalwrt源码： https://github.com/immortalwrt/immortalwrt

##### 为路由器编译OpenWRT固件 ####

1，登录GitHub账号

2，设置权限

- 右上角点击自己的头像，下拉菜单中选择【Settings/设置】 > 【Developer settings/开发者设置】 > 【Personal access tokens/个人访问令牌 > 【Tokens（classic）/令牌（经典）】 > 【 Generate new token/生成新令牌 】 ( Name: GITHUB_TOKEN, Select: public_repo )，其他选项根据自己需要可以多选，提交保存，复制系统生成的加密 KEY 的值，先保存到自己电脑的记事本，下一步会用到这个值。

- 打开https://github.com/catmaple/OpenWrt，点击右上的 Fork 按钮，复制一份仓库代码到自己的账户下，稍等几秒钟，提示 Fork 完成后，到自己的账户下访问自己仓库里的 build-openwrt 。在右上角的 Settings > Secrets > Actions > New repostiory secret ( Name: GH_TOKEN, Value: 填写刚才GITHUB_TOKEN的值 )，保存。并在左侧导航栏的 Actions > General > Workflow permissions 下选择 Read and write permissions 并保存。图

3，设置config
- 进入config文件夹，需要用哪个分支的源码，用下面的方法生成config进行替换。

--- a.在本地VirtualBox建立Ubuntu客户机，在Ubuntu中安装编译环境

# Update package lists
sudo apt-get update

# Update installed packages
sudo apt-get upgrade

sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3.5 unzip zlib1g-dev libgcc1 libc6-dev-i386 subversion flex quilt uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev xsltproc libxml-parser-perl mercurial bzr ecj cvs texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf

--- b.然后克隆源码到Ubuntu客户机，如：

git clone https://github.com/coolsnowwolf/lede openwrt

--- c.再添加源(openwrt-passwall 这个必需配置，不然提示一些包不存在)

cd openwrt/

sed -i '$a src-git ing https://github.com/wjz304/openwrt-packages;lede' feeds.conf.default

./scripts/feeds clean

./scripts/feeds update -a

./scripts/feeds install -a


--- d.生成config内容

make menuconfig

选择路由器对应的架构，型号和所需的插件, 如Passwall, SSR+等 ，退出保存后会生成.config文件在openwrt目录里。

--- e.拷贝这个文件内容，替换GitHub对应Code > config > 对应源码 > config文件内容。
 
   
4，开始编译
 
 - 点击菜单栏的【Actions】，左边菜单栏选择编译流程：路由器固件编译，再点击右边的Run workflow开始编译。大约1小时左右可以完成编译。


 5，下载固件
 
   编译完成后点击 Code, 再点击页面右下角Release进入下载页面。
   默认IP地址：192.168.1.1(root, password)

 编译完成后点击 Code, 再点击页面右下角Release进入下载页面。

原始代码来源：https://github.com/xinlingduyu/build-openwrt
