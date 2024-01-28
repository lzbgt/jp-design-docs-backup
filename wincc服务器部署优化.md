1.OPC.Automation注册表需安装，点击注册表右键全并即可。

2.OPCDAAuto.dl的注册表类，需要将相关三个dll注册表放入新建的system64文件夹，同时也需放入C:\\Windows\\SysWOW64和systemc32文件夹内，三个文件夹放入后。使用此条命令regsvr32 三个文件夹OPCDA路径即可注册成功。

3.服务机配置完成开启远程服务还有连接问题，如下解决关闭**Security Center**安全中心服务：

1）计算机右击管理，进入服务进行关闭，或出现如下无法关闭情况

<img class="op-uc-image op-uc-image_inline" src="">

2）直接按键盘上的：[**win键**](https://www.itmemo.cn/html/26.html) + R 弹出运行，输入：regedit，再敲回车键，[**打开注册表编辑器**](https://www.itmemo.cn/html/477.html)**。。**

3）复制下面的注册表代码，到注册表编辑器地址栏，敲回车键，直接定位到：Security Center服务项的启动类型修改处。

计算机\\HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\wscsvc

4）双击：Start 项

<img class="op-uc-image op-uc-image_inline" src="https://www.itmemo.cn/d/file/2022/04-16/4516d12784fb3a086e2c09304325f5af.jpg" alt="3、双击：Start 项">

5）修改：Start项的数值，**4**：**禁用**，**2**：**自动启动**，大家根据自己的情况修改完确定即可，最后需要重启下电脑才会生效！

<img class="op-uc-image op-uc-image_inline" src="https://www.itmemo.cn/d/file/2022/04-16/1e388d976ac2299e0a3b49e9251743f7.jpg" alt="4、修改：Start项的数值，4代表：禁用，2代表：自动启动，大家根据自己的情况修改完确定即可，最后需要重启下电脑才会生效！">