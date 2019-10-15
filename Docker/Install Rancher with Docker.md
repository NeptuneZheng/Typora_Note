# 										Install Rancher with Docker

- [Rancher Doc][https://www.rancher.cn/docs/rancher/v2.x/cn/installation/single-node-install/]

- [Step by Step！Rancher 2.2+K3s集成部署实践教程 ][https://yq.aliyun.com/articles/704089]
- [Running K3S][https://rancher.com/docs/k3s/latest/en/running/]

## 1. Get latest stable Rancher from DockerHub

- [DockerHub][https://hub.docker.com/search?q=rancher&type=image]

```linux
docker pull rancher/rancher:latest    //get rancher2.x
```

![1571052842969](D:\Typora\MKDFiles\media\1571052842969.png)

## 2. Deploy image

- mapping new port: 9080:80, 9443:43

```linux
docker run -d --restart=unless-stopped -p 9080:80 -p 9443:443 \
-v /docker_volume/rancher_home/rancher:/var/lib/rancher \
-v /docker_volume/rancher_home/auditlog:/var/log/auditlog \
--name rancher rancher/rancher:latest
```

## 3. Check Network port

```linux
sudo netstat -lntp
```

![1571053603365](D:\Typora\MKDFiles\media\1571053603365.png)

## 4. finish

- https://192.168.117.128:9443/g/clusters

![1571053650581](D:\Typora\MKDFiles\media\1571053650581.png)

## 5. Install K3S

- [Manual Download][https://rancher.com/docs/k3s/latest/en/quick-start/]

### A. download latest `k3s`,`k3s-arm64`,`k3s-armhf` from [git](https://github.com/rancher/k3s/releases/tag/v0.9.1), upload to linux server

![1571142839641](D:\Typora\MKDFiles\media\1571142839641.png)

### B. pull  `rancher/pause-amd64` from DockerHub and rename

> if not pull or rename, in k3s install step, it will pull `k8s.gcr.io/pause:3.1` from k8s.gcr.io repository, since China can't connect, may cause connection issue

```linux
docker pull rancher/pause-amd64:3.1
docker tag rancher/pause-amd64:3.1 k8s.gcr.io/pause:3.1
```

![1571143299580](D:\Typora\MKDFiles\media\1571143299580.png)

double check id rename works.

```linux
docker images | grep "k8s.gcr.io/pause"
```

![1571143374677](D:\Typora\MKDFiles\media\1571143374677.png)

### C. start k3s server

#### a. cd k3s shell path and exe below cmd, then it will waitting for register:

```linux
sudo ./k3s server --write-kubeconfig-mode default --docker --no-deploy traefik
```

>./k3s server: k3s server start cmd
>
>--write-kubeconfig-mode default: since rancher no CA, when run rancher `curl cmd` need to rewrite the 															 k3s.yaml file 
>
>--docker --no-deploy traefik: switch k3s default Container engine from  `Containerd`  to `Docker`

![1571143917321](D:\Typora\MKDFiles\media\1571143917321.png)

make sure k3s server is start

```linux
sudo ./k3s kubectl get node
```

![1571144309650](D:\Typora\MKDFiles\media\1571144309650.png)

#### b. get node-token and start the agent

```linux
 sudo cat /var/lib/rancher/k3s/server/node-token
```

then update the server and NODE_TOKEN:

```linux
sudo ./k3s agent --server https://myserver:6443 --token ${NODE_TOKEN}
```

> server is k3s server not rancher server as well as NODE_TOKEN

for me is:

```linux
sudo ./k3s agent --server https://127.0.0.1:6443 --token K10adc5ac2a649586db6aa0907b8ec68e31cd05627b1357cc4118d6c67650f711fd::node:344c91765000e7f9cdb1e395411da9a3
```

![1571144504960](D:\Typora\MKDFiles\media\1571144504960.png)

#### c. import k3s to Rancher

copy from rancher server, for kubectl cmd add ./k3s before it

![1571144602961](D:\Typora\MKDFiles\media\1571144602961.png)

```linux
sudo curl --insecure -sfL https://192.168.117.128:9443/v3/import/5nswpwhzdkhpfwvxmq66687zh77wgw59g47r5m56nzf4mwgfbjfrgd.yaml | ./k3s kubectl apply -f -
```

![1571144629539](D:\Typora\MKDFiles\media\1571144629539.png)

get above result proves import sucess, wait a moment will find K3S in rancher

![1571144750520](D:\Typora\MKDFiles\media\1571144750520.png)





