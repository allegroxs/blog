Visual Box 如何打开远程桌面

一般过程：
只要安装了增强功能就能通过设置-显示-远程桌面打开这个功能，主要问题是Vbox怎么安装增强功能
- Linux 虚拟机
  1. 打开虚拟机，登入一个root组用户
  2. 选择菜单：设备-安装增强功能...
  3. 等待光驱出现
  4. 需要apt install build-essential，且无问题。参考[这篇](https://blog.csdn.net/wuyy75/article/details/82595000)
  5. 在光驱目录运行sudo ./xxxx.run

- Windows 虚拟机
    - 等待光驱出现后运行

我的问题：  
Kali 2021及以后版本已经预先安装了Guest Additional，无需在内部运行，但是在打开远程桌面的时候还是提示了设置无效，需要安装增强功能。

解决办法：  
1. 点击菜单：管理-工具-扩展包管理，发现扩展包已经过期（我升级了VBox7但是扩展包还是6的版本）
2. 点击安装扩展包，选择下好的extpack文件（到官网下载[Extension pack](https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html#extpack)）
3. 提示消失。使用本机测试：
   1. 启动虚拟机后放置
   2. windows任务栏搜索项键入“远程桌面连接”，打开应用
   3. 连到本地127.0.0.1
   4. 出现虚拟机界面