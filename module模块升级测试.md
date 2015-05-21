
# 模块升级测试服务器部署

以http://ftp.jenkins.baidu.com/newns/module_update_common/46/output/ module2015.05.18.46.zip为例。

1. 模块升级文件上传到10.242.108.23上。
```
ssh work@10.242.108.23
bavmm@baidu
cd  /home/work/luoman/tools
rz -be
上传module2015.05.18.46.zip文件
```
2.	操作测试服务器部署WEB，地址如下： 
http://hkg02-sys-web51.hkg02.baidu.com:8080/autoUpdate/auto_deploy_rebuild/index.php?m=Op&a=onlineRules&
3.	部署升级包
![](http://7xif3g.com1.z0.glb.clouddn.com/caimaoy_模块升级_部署升级包.png)
ftp地址:ftp://10.242.108.23:/home/work/luoman/tools
4.	创建升级规整 BAV 模块设计 测试国内 普通升级
![](http://7xif3g.com1.z0.glb.clouddn.com/caimaoy_模块升级_创建升级规则.png)
5.	对规则进行审核，需要审核2次：上线单审核、待上线单。

# 模块升级验证
1.	若是线下模块升级验证，需要配置hosts。
```
10.242.108.23 download.antivirus.baidu.com
10.242.108.23 dl2.bav.baidu.com
10.242.108.23 dl-vip.bav.baidu.com
10.242.108.23 updown.bav.baidu.com
10.242.108.23 update.bav.baidu.com
10.242.108.23 download.bav.baidu.com 
10.242.108.23 update.sd.baidu.com
10.242.108.23	ownload.sd.baidu.com
```
2.	安装BAV。
3.	关闭小红伞引擎，Kill 升级进程。
4.	修改config.ini [update] connect_time=2，删除ModuleUpdateVersions=***。
5.	关闭BAV主界面。
6.	重启BAVSVC服务。
7.	等待1min，进行升级，观察安装目录下的update文件夹，升级结束后，升级进程退出，update文件夹下的文件会自动替换。验证下载的模块文件是否正确，功能是否正常。
8.	注意：如果BAV存在其他升级，如自修复、增量、全量，会先完成其他升级，然后再进行模块升级。



