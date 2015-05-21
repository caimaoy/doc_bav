# 小工具升级测试方法

## 小工具升级测试服务器部署


### 如果小工具链接是ToolsServer.zip下载链接，如：
- ftp://newns:a1b2c3.@ftp.jenkins.baidu.com:8557/bav_advtools_release_cluster/127/output/ToolsServer.zip，
- 部署方法如下：
- http://mars.baidu.com/Monitor/Channel.html#create
- 选择小工具部署，填写小工具下载链接，进行创建。
![](http://7xif3g.com1.z0.glb.clouddn.com/小工具部署.png)


### 如果是BAV的打包链接，如：
- http://172.17.194.10:8088/Build/bav_new_trunk/5.7.0.133878_504，部署方法如下：
- 将打包链接转换为jenkins地址，加上需要测试的小工具名称，如：
http://ftp.jenkins.baidu.com/newns/bav_copy_products/bav_new_trunk/504/output/8A73A249-C3BE-401A-8335-A832080931D3_1.0.3.2_BavPluginRemove.zip

- 打开部署链接：http://bls.baidu.com/pack/
-选择小工具，填写小工具地址，选择测试的BAV版本，选择自动部署到测试服务器。
![](http://7xif3g.com1.z0.glb.clouddn.com/BAV打包工具.png)
![](http://7xif3g.com1.z0.glb.clouddn.com/自动部署到测试服务器.png)

### 如果没有链接，只有ToolsServer.zip包，可以手动部署测试服务器。方法如下：
```
	ssh work@ 10.242.108.23	
	pw：bavmm@baidu
```

#### 大陆测试服务器部署
1.	把小工具安装包上传到目录：  

  - /home/work/apache-noroot/cgi-bin-secure-update-sd/tool_update/data，
  - 使用cd命令进入这个目录后，先查看目录内是不是已经有旧的ToolsServer.zip文件，有的话使用rm ToolsServer.zip删除
  - 输入rz -be，出现UI可控弹窗，选择本地的目标文件ToolsServer.zip

2.	执行部署脚本：  
直接敲打命令：
```
/home/work/apache-noroot/cgi-bin-secure-update-sd/tool_update/script/create_tool_config.sh cn 
```

#### 国外机器部署：
1.	把小工具安装包上传到目录：/home/work/apache-noroot/cgi-bin-bav-update/tool_update/data
2.	执行部署脚本：
```
/home/work/apache-noroot/cgi-bin-bav-update/tool_update/script/create_tool_config.sh hk
```

### 测试服务器部署验证方法

#### 查看小工具是否部署成功：
- 双击打开需要部署的ToolsServer.zip\ToolsServer\ToolsUpdateRule.zip\ToolsUpdateRule\ToolsUpdateRule.ini文件
- 查看到里面的升级规则如client\_version = 5.2.*对应的tools\_list = ToolsList\_2014_10_10_17_10_26_0.xml
- 访问网址：http://update.sd.baidu.com/cgi-bin/get_bavtools_update_info.cgi?guid=TF645&ProgramVersion=5.5.2.114912&os=6&ChannelName=web%7Cth%7Cofficial%7Cdirect&IsPro=yes&Ismanual=yes
- 将网址中的ProgramVersion修改为升级规则里的client_version后再访问，页面返回如下：

```
<?xml version="1.0" encoding="utf-8" ?> 
<ServerRespond XmlVersion="1.0">
    <UpdateTools Md5="0xf644072993c0e84ecde6b598158b3111" NeedUpdate="Yes" Size="1268" 
    Url="http://download.sd.baidu.com/baidu_tool/ToolsList_2014_10_10_17_10_26_0_******.xml" /> 
</ServerRespond>
```

- 从返回内容中可以找到ToolsList_2014_10_10_17_10_26_0，与规则文件中的tools_list匹配，证明部署成功
- 在ini文件中再找一个client_version和其对应的tools_list，重复前面的方法进行验证，匹配就证明部署成功



## 小工具升级验证

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
3. 关闭小红伞引擎，Kill 升级进程。
4. 修改config.ini：
```
	ToolsLastUpdateTime=0
	BackgroundUpdating=0
```
5.	退出托盘。
6.	打开主界面，对各个小工具进行下载、使用，每个小工具都能正常使用。



