## Centos8 VM Usage Note

### 1. Install Centos8 in VMware

- [VMware12安装centOS8](https://blog.csdn.net/BryantJamesHua/article/details/101480034)
- [如何解决虚拟机安装CentOS无法全屏显示问题][https://blog.csdn.net/qq_36276335/article/details/60132184]
- [VMware centos屏幕和显示屏不匹配？改变分辨率][https://jingyan.baidu.com/article/0eb457e50d64ce03f1a90524.html]

**1. network issue**

- [Centos7 NAT无法上网的问题][https://blog.csdn.net/u011323949/article/details/79019475]

update  ONBOOT=no to yes

 systemctl restart NetworkManager 

### 2. Install Java

- [Linux上Java的安装与配置](https://www.cnblogs.com/lamp01/p/8932740.html)

### 3. Install Docker

- [How to install Docker CE on RHEL 8 / CentOS 8][https://linuxconfig.org/how-to-install-docker-in-rhel-8]

- [How to install Docker ce on Centos 8 or RHEL 8 Linux][https://www.how2shout.com/how-to/how-to-install-docker-ce-on-centos-8-or-rhel-8-linux.html]

### 4. Deploy Portainer

- [Docker实用工具推荐：Portainer][https://blog.csdn.net/qq_36792209/article/details/82695169]

```linux
docker pull portainer/portainer
```

```yml
version: '2'
services:
    portainer:
      image: portainer/portainer
      restart: always
      ports:
        - "9000:9000"
      volumes:
        - /var/run/docker.sock:/var/run/docker.sock
        - /data/docker/portainer/data:/data
```

### 5. Switch to root

```linux
sudo su - root
```



