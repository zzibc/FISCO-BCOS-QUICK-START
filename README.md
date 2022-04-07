# FISCO-BCOS-QUICK-START
基于FISCO BCOS 2.x技术文档，建立快速入门手册

作者：ZZIBC

FISCO BCOS目前最新版本为3.x，2.x为稳定版本

**[FISCO BCOS仓库](https://github.com/FISCO-BCOS)**

**[FISCO BCOS 3.x技术文档（latest）](https://fisco-bcos-doc.readthedocs.io/zh_CN/latest/)**

**[FISCO BCOS 2.x 技术文档（stable）](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/)**

## 前言
FISCO BCOS是由金链盟开源工作组协作打造的国产联盟链底层平台，并于2017年正式对外开源。它是一个稳定、高效、安全的区块链底层平台，
经过多家机构、多个应用，长时间在生产环境运行的实际检验。

如果你还不知道什么是FISCO BCOS，请移步[FISCO BCOS区块链](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/introduction.html) ，
看看就可以。这些其实都不影响你入门；阻碍你前进步子的永远是你对未知东西的恐惧，做为一个IT开发人员，探索未知领域永远是我们的必修课；学习区块链，
需要找一个应用场景，带着这个应用场景去一步一步的深入理解其中的概念及思路会起到事半功倍的效果，闲话不多说，我们进入主题。

谨记谨记：如果文档描述不清楚或错误的地方，[所有内容！！！！] 都可在[FISCO BCOS仓库](https://github.com/FISCO-BCOS) 找到答案！
本手册只供新手入门
##准备工作

完全离线安装FISCO BCOS，一步一步的安装容易排查环境错误，有助于走进FISCO BCOS。

* 一、一台虚拟机软件（VMware、VirtualBox、Hyper-V）

      选择自己顺手的，保证可以创建虚拟机、修改IP、配通网络等基础操作；

* 二、下载CentOS的完整镜像（CentOS-7-x86_64-DVD-2009.iso）
  * [腾讯仓库](https://mirrors.cloud.tencent.com/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-DVD-2009.iso) 
  * [阿里仓库](https://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-DVD-2009.iso)

* 三、下载JDK1.8及以上的linux安装包（jdk-8u321-linux-x64.tar.gz）
  * [ORACLE官网下载](https://www.oracle.com/java/technologies/downloads/) | 在下载页面Ctrl+F查找jdk-8u321-linux-x64.tar.gz
  * [其他自行搜索下载]()

* 四、下载FISCO BCOS底层链安装脚本（build_chain.sh）
  * [github releases](https://github.com/FISCO-BCOS/FISCO-BCOS/releases/tag/v2.8.0)
  * [github下载地址](https://github.com/FISCO-BCOS/FISCO-BCOS/releases/download/v2.8.0/build_chain.sh)
  * [gitee releases](https://gitee.com/FISCO-BCOS/FISCO-BCOS/releases/v2.8.0)
  * [gitee下载地址](https://gitee.com/FISCO-BCOS/FISCO-BCOS/attach_files/816587/download/build_chain.sh)

* 五、下载FISCO BCOS底层链安装包（fisco-bcos.tar.gz v2.8.0）
  * [github releases](https://github.com/FISCO-BCOS/FISCO-BCOS/releases/tag/v2.8.0)
  * [github下载地址](https://github.com/FISCO-BCOS/FISCO-BCOS/releases/download/v2.8.0/fisco-bcos.tar.gz)
  * [gitee releases](https://gitee.com/FISCO-BCOS/FISCO-BCOS/releases/v2.8.0)
  * [gitee下载地址](https://gitee.com/FISCO-BCOS/FISCO-BCOS/attach_files/816588/download/fisco-bcos.tar.gz)

* 六、下载FISCO BCOS控制台安装包（console.tar.gz v2.8.0）
  * [github releases](https://github.com/FISCO-BCOS/console/releases/tag/v2.8.0)
  * [github下载地址](https://github.com/FISCO-BCOS/console/releases/download/v2.8.0/console.tar.gz)
  * [gitee releases](https://gitee.com/FISCO-BCOS/console/releases/v2.8.0)
  * [gitee下载地址](https://gitee.com/FISCO-BCOS/console/attach_files/812963/download/console.tar.gz)

##安装FISCO BCOS

* 一、基础环境安装
  * 以root用户登录，创建两个目录 fisco 、 jdk 和 soft目录
    * mkdir ~/fisco
    * mkdir ~/jdk
    * mkdir ~/soft
  * 1、虚拟一台CentOS7的虚拟机或找一台实体机，安装方法自行搜索
  * 2、挂载ISO镜像文件（CentOS-7-x86_64-DVD-2009.iso）
    * 通过光驱挂载镜像
      * 将CentOS-7-x86_64-DVD-2009.iso文件载入光驱
      * mkdir /media/cdrom  #创建目录
      * mount /dev/cdrom  /media/cdrom/ #挂载镜像
      * df -HT #命令-查看镜像是否挂载成功
    * 通过文件挂载镜像
      * 将CentOS-7-x86_64-DVD-2009.iso文件上传到/home（目录可以根据自己情况修改）
      * mkdir /media/cdrom  #创建目录
      * mount -o loop /home/CentOS-7-x86_64-DVD-2009.iso  /media/cdrom/ #挂载镜像
      * df -HT #查看镜像是否挂载成功
  * 3、配置本地yum源
    * cd /etc/yum.repos.d/
    * mkdir ./bak
    * mv ./*.repo ./bak/
    * cp ./bak/CentOS-Media.repo .  #注意最后的圆点
    * vi CentOS-Media.repo   
    * 修改配置文件 输入 i 进入编辑模式
    * 修改文件中的baseurl=file:///media/cdrom/
    * 修改文件中的enabled=1
    * 修改完以后按ESC，之后输入 :wq
    * yum clean all
    * yum repolist
  * 4、配置IP及网络环境
    * 如果你对虚拟机的使用非常熟悉，可以忽略如下建议
    * 建议：如果使用的是虚拟机，测试阶段建议将“网络适配器”改为“桥接模式”
    * vi /etc/sysconfig/network-scripts/ifcfg-ens33  #ens33不同机器可能不同
    * 开始修改 输入 i 进入编辑模式
    * 修改如下内容，其他未列出的配置保持原有值不变
 
          TYPE=Ethernet
          PROXY_METHOD=none
          BROWSER_ONLY=no
          BOOTPROTO=static           #注意修改
          DEFROUTE=yes
          IPV4_FAILURE_FATAL=no
          IPV6INIT=no
          IPV6_AUTOCONF=no
          IPV6_DEFROUTE=no
          IPV6_FAILURE_FATAL=no
          IPV6_ADDR_GEN_MODE=stable-privacy
          ONBOOT=yes                 #默认启动网卡
          IPADDR=192.168.0.97        #IP地址根据本地网络情况修改
          NETMASK=255.255.255.0      #子网掩码， 和本地网络环境一致
          GATEWAY=192.168.0.1        #默认网关， 和本地网络环境一致
          DNS1=192.168.1.1           #DNS服务器，和本地网络环境一致
          DNS2=192.168.0.1           #DNS服务器，和本地网络环境一致
    * 结束修改，按ESC，之后输入 :wq
    * service network restart    #重启网卡
  * 5、关闭防火墙
    * systemctl status firewalld.service
    * systemctl stop firewalld.service
    * systemctl status firewalld.service
    * systemctl disable firewalld.service
  * 6、安装JDK1.8（jdk-8u321-linux-x64.tar.gz）
    * mkdir ~/jdk #创建jdk目录
    * 将本地jdk-8u321-linux-x64.tar.gz文件上传到 ~/jdk目录
    * cd ~/jdk
    * tar -zxvf jdk-8u321-linux-x64.tar.gz
    * 配置环境变量
    * vi /etc/profile
    * 开始修改 输入 i 进入编辑模式
    * 在profile中最下面加入以下内容
    * export JAVA_HOME=~/jdk/jdk1.8.0_321
    * export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    * export PATH=$PATH:$JAVA_HOME/bin
    * 结束修改，按ESC，之后输入 :wq
    * source /etc/profile   #即时生效
* 二、安装配置底层链
  * 1、安装centos依赖
    * sudo yum install -y openssl openssl-devel
    * sudo yum install zip unzip
  * 2、上传安装文件
    * mkdir ~/fisco #创建fisco目录
    * 将本地build_chain.sh和fisco-bcos.tar.gz文件上传到 ~/fisco目录
    * 可以通过xshell等第三方工具上传文件，自行搜索
  * 3、安装单群组4节点联盟链
    * cd ~/fisco
    * tar -zxvf fisco-bcos.tar.gz
    * ./build_chain.sh -l 127.0.0.1:4 -p 30300,20200,8545 -e ./fisco-bcos -v 2.8.0
    * 注：127.0.0.1 为IP地址，根据自己机器情况修改
    * 注：30300,20200,8545 相关端口，如果防火墙没有关闭会造成外部无法访问
    * 注：-e ./fisco-bcos -v 2.8.0，离线安装的相关参数 指定本地安装文件位置及版本号
  * 4、启动FISCO BCOS
    * cd ~/fisco/nodes/127.0.0.1  #127.0.0.1根据实际情况修改
    * ./start_all.sh
    * ps -ef | grep -v grep | grep fisco-bcos #检查FISCO BCOS启动进程
    * 正常情况会有4个进程输出信息
    * 如果进程数不为4，则进程没有启动（一般是端口被占用导致的）
* 三、安装配置控制台
  * 1、上传安装文件
    * 将本地console.tar.gz文件上传到 ~/fisco目录
  * 2、安装控制台程序
    * cd ~/fisco
    * tar -zxvf console.tar.gz
    * cd config
    * cp -r ~/fisco/nodes/127.0.0.1/sdk/* . #127.0.0.1根据实际情况修改，注意最后的圆点
    * cp config-example.toml config.toml
    * vi config.toml
    * 修改 peers=["127.0.0.1:20200","127.0.0.1:20201"] #根据实际情况修改
    * cd ~/fisco/console
    * bash start.sh
    * 启动成功后输入 getNodeVersion，如下：
    * [group:1]> getNodeVersion
    * 输入 getPeers，如下：
    * [group:1]> getPeers
    * 正常输出，表示安装成功
    * 输入 quit 退出控制台
    * [group:1]> quit  # 退出控制台

##部署智能合约
* 一、编写智能合约
  * 以HellWorld合约为例
  * HellWorld.sol文件位于：~/fisco/console/contracts/solidity/目录下
  * HellWorld.sol源码
  
        contract HelloWorld {
           string name;
           function HelloWorld() {
              name = "Hello, World!";
           }
           function get()constant returns(string) {
               return name;
           }
           function set(string n) {
              name = n;
           }
        }
  
  * 如果是自己新编写的智能合约，需要把智能合约上传到~/fisco/console/contracts/solidity/目录下
* 二、部署智能合约
  * 启动控制台
  * cd ~/fisco/console
  * bash start.sh
  * 控制台输入：deploy HelloWorld
  * [group:1]> deploy HelloWorld
  * 回车后输出类似下面内容
  * transaction hash: 0xd0305411e36d2ca9c1a4df93e761c820f0a464367b8feb9e3fa40b0f68eb23fa
  * contract address:0xb3c223fc0bf6646959f254ac4e4a7e355b50a344
  * 正常输出，表示安装成功，请记下上面输出内容
  * 输入 quit 退出控制台
  * [group:1]> quit  # 退出控制台

##调用智能合约
* 一、控制台调用HelloWorld合约
  * 启动控制台
  * cd ~/fisco/console
  * bash start.sh
  * 控制台输入： getBlockNumber
  * [group:1]> getBlockNumber  # 查看当前块高，控制台输出当前块高
  * 调用HelloWorld合约的get接口获取name变量 此处的合约地址是deploy指令返回的地址（contract address）
  * [group:1]> call HelloWorld 0xb3c223fc0bf6646959f254ac4e4a7e355b50a344 get
  * 控制台输出开始
    
        ---------------------------------------------------------------------------------------------
        Return code: 0
        description: transaction executed successfully
        Return message: Success
        ---------------------------------------------------------------------------------------------
        Return values:
        [
        "Hello,World!"
        ]
        ---------------------------------------------------------------------------------------------
  * 控制台输出结束
  * 重新查看当前块高，块高不变，因为get接口不更改账本状态
  * [group:1]> getBlockNumber
  * 调用HelloWorld合约的set设置name
  * [group:1]> call HelloWorld 0xb3c223fc0bf6646959f254ac4e4a7e355b50a344 set "Hello, FISCO BCOS!"
  * 控制台输出开始
  
        transaction hash: 0x7e742c44091e0d6e4e1df666d957d123116622ab90b718699ce50f54ed791f6e
        ---------------------------------------------------------------------------------------------
        transaction status: 0x0
        description: transaction executed successfully
        ---------------------------------------------------------------------------------------------
        Output
        Receipt message: Success
        Return message: Success
        ---------------------------------------------------------------------------------------------
        Event logs
        Event: {}
  * 控制台输出结束
  * 再次查看当前块高，块高增加表示已出块，账本状态已更改
  * [group:1]> getBlockNumber
  * 调用HelloWorld合约的get接口获取name变量 此处的合约地址是deploy指令返回的地址（contract address）
  * [group:1]> call HelloWorld 0xb3c223fc0bf6646959f254ac4e4a7e355b50a344 get
  * 控制台输出开始

        ---------------------------------------------------------------------------------------------
        Return code: 0
        description: transaction executed successfully
        Return message: Success
        ---------------------------------------------------------------------------------------------
        Return values:
        [
        "Hello,FISCO BCOS!"
        ]
        ---------------------------------------------------------------------------------------------
  * 控制台输出结束
  * 正常输出，表示调用成功
  * 输入 quit 退出控制台
  * [group:1]> quit  # 退出控制台
* 二、JAVA SDK 调用智能合约
* 1、创建maven项目
* 2、应用JAVA SDK
