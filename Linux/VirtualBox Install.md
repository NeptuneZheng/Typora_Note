# VirtualBox Install Centos

- [win10虚拟机Oracle VM VirtualBox安装和使用教程](https://zhuanlan.zhihu.com/p/111567471)

### 1. Donload VirtualBox and other useful tools

- VirtualBox: https://www.virtualbox.org/wiki/Downloads

![1593504489763](../media/1593504489763.png)+



- VBoxGuestAdditions: https://www.virtualbox.org/wiki/Downloads

![1593504537246](../media/1593504537246.png)



-  Oracle_VM_VirtualBox_Extension_Pack: http://download.virtualbox.org/virtualbox/6.1.10/

![1593504716880](../media/1593504716880.png)+

### 2. Install VirtualBox 

### 3.  Install VirtualBox_Extension_Pack

![1593506037645](../media/1593506037645.png)+

![1593506194003](../media/1593506194003.png)+

![1593506254074](../media/1593506254074.png)+



### 4. Install Centos

- [使用VirtualBox安装CentOS7](https://www.cnblogs.com/gaomanito/p/11460381.html)
- [用virtualbox安装LINUX系统后下次打开时为什么又要重新安装？已经曾经登陆过一次。求帮助！](https://zhidao.baidu.com/question/1800554892249387467.html)

![1593506382548](../media/1593506382548.png)+

![1593506903072](../media/1593506903072.png)+



用virtualbox安装LINUX系统后下次打开时为什么又要重新安装？

![1593675159447](../media/1593675159447.png)

### 5. Install VBoxGuestAdditions

![1593678911483](../media/1593678911483.png)+



如果安装报错：

![1593740039761](../media/1593740039761.png)+

可通过如下安装方式：

- [VirtualBox虚拟机CentOS安装增强功能Guest Additions](https://www.cnblogs.com/aliuwoai/p/10417453.html)
- [Virtualbox主机和虚拟机之间文件夹共享及双向拷贝](https://blog.csdn.net/wcx1293296315/article/details/82955219)

```linux
cd /run/media/
ls
```

![1593741077574](../media/1593741077574.png)

```linux
sudo sh VBoxLinuxAdditions.run
```

安装成功后，如果主机和虚拟机之间还是无法互相拷贝或文件共享，可设置如下：

![1593767821530](../media/1593767821530.png)+



### 6. Port mapping with host 

