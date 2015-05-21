# Checklist升级测试方法

## 验证Checklist前，需要部署版本升级测试服务器。部署方法如下：
- 打开http://mars.baidu.com/Monitor/Channel.html#create  

- 选择版本升级部署，输入地址如下：

http://ftp.jenkins.baidu.com/newns/bav_copy_products/bav_5.4/80/output/server5.4.3.133394.zip
- PS： 请输入jenkins地址或者ftp地址，不支持172的地址，即使只有server.zip，也要加上版本号server5.3.2.102694.zip

# 静默增量升级验证
__需要验证2点__。以发布5.5.3.131382为例。
## 第一个验证点，验证目的：保证低版本升级到目标版本，BAV能正常使用，没有异常出现。

1.	配置hosts。
```
10.242.108.23 download.antivirus.baidu.com
10.242.108.23 dl2.bav.baidu.com
10.242.108.23 dl-vip.bav.baidu.com
10.242.108.23 updown.bav.baidu.com
10.242.108.23 update.bav.baidu.com
10.242.108.23 download.bav.baidu.com
10.242.108.23 update.sd.baidu.com
10.242.108.23 download.sd.baidu.com
```
2. 安装低版本，保证版本号前3位相同，如5.5.3.125486。
3. 关闭小红伞引擎，Kill 升级进程。
4. 修改config.ini [update] connect_time=2
5. 关闭BAV主界面。
6. 重启BAVSVC服务。
7. 等待1min，进行升级，观察安装目录下的update文件夹，升级结束后，升级进程退出，update文件夹下的文件会自动替换，BAV的版本号更新，基本功能正常。

## 第二个验证点，验证目的：保证目标版本升级功能正常。

1.	配置hosts。
```
10.242.108.23 download.antivirus.baidu.com
10.242.108.23 dl2.bav.baidu.com
10.242.108.23 dl-vip.bav.baidu.com
10.242.108.23 updown.bav.baidu.com
10.242.108.23 update.bav.baidu.com
10.242.108.23 download.bav.baidu.com 
10.242.108.23 update.sd.baidu.com
10.242.108.23 download.sd.baidu.com
```
2. 安装__待发布版本__5.5.3.131382.
3. 关闭小红伞引擎，Kill 升级进程。
4. 修改version.xml中的版本号，将最后一位改为1，如5.5.3.1。
<ProgramVersion Time="2015-05-06 19:40:50" >5.5.3.1</ProgramVersion>
5. 修改ProgramFileList.xml中IEProtect.exe和version.xml的 Size、DcSize为0x00000001.
<File Name="IEProtect.exe.7z" Size="0x00000001" Time="0x01d087f44ec7384c" Md5="0x5df1fb365aab084ca7630b2251e890bb" DcSize="0x00000001" DcTime="0x01d087f3e4f2d1dd" DcMd5="0xf836301dd3e68460ced8923172fc9c58" />
<File Name="version.xml.7z" Size="0x00000001" Time="0x01d087f443b5fd66" Md5="0x757c17d1ba3943c95ebbb46234ac3e27" DcSize="0x00000001" DcTime="0x01d087f1b1690e11" DcMd5="0x428cb73b5a245ea60062950d520003ea" />
4. 修改config.ini [update] connect_time=2
5. 关闭BAV主界面。
6. 重启BAVSVC服务。
7. 等待1min，进行升级，观察安装目录下的update文件夹，升级结束后，升级进程退出，update文件夹下的文件会自动替换，BAV的版本号更新，基本功能正常。

# 手动全量升级验证
以发布5.5.3.131382为例。  

__验证目的：保证低版本升级到目标版本，BAV能正常使用，没有异常出现。__
1.	配置hosts。
```
10.242.108.23 download.antivirus.baidu.com
10.242.108.23 dl2.bav.baidu.com
10.242.108.23 dl-vip.bav.baidu.com
10.242.108.23 updown.bav.baidu.com
10.242.108.23 update.bav.baidu.com
10.242.108.23 download.bav.baidu.com 
10.242.108.23 update.sd.baidu.com
10.242.108.23 download.sd.baidu.com
```
2. 安装低版本，保证版本号前3位不同，如5.2.3.125486。
3. 关闭小红伞引擎，Kill 升级进程。
4. 重启BAVSVC服务。
5. 重启BAV主界面。
6. 点击界面的Update按钮进行升级，中途有提示弹窗选择“升级”。观察安装目录下的update文件夹，升级结束后，升级进程退出，update文件夹下的安装文件会安装，BAV的版本号更新，基本功能正常。

# 增量包安装之后，线上部署的工具下载
以发布5.5.3.131382为例。  

__验证目的：保证低版本增量升级到目标版本后，BAV小工具能正常下载使用，不会下载失败或者使用异常。__
1. __清空hosts，保证拉取待发布BAV的小工具已经上线。__
2. 保证已经按照第一步增量升级到目标版本5.5.3.131382。
3. 关闭小红伞引擎，Kill 升级进程。
4. 修改config.ini：
```
	ToolsLastUpdateTime=0
	BackgroundUpdating=0
```
5.	退出托盘。
6.	打开主界面，对各个小工具进行下载、使用，每个小工具都能正常使用。






