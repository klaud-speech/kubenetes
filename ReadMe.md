



## Install
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
rpm -ivh minikube-latest.x86_64.rpm
```

```
PATH=$PATH:/usr/bin
echo $PATH
```
#### docker 설치
```
yum install docker
systemctl enable docker
systemctl start docker
```

#### kubectl 설치
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```
```
yum install -y kubectl
```

#### minikube 시작
```
minikube start
```
X Exiting due to DRV_AS_ROOT: The "docker" driver should not be used with root privileges.

```
minikube start --driver=none
```
X Exiting due to GUEST_MISSING_CONNTRACK: Sorry, Kubernetes 1.20.2 requires conntrack to be installed in root's path


```
[root@localhost /]# kubectl get po -A
```
The connection to the server localhost:8080 was refused - did you specify the right host or port?







## Uninstall
```
minikube stop; minikube delete
docker stop (docker ps -aq)
rm -r ~/.kube ~/.minikube
sudo rm /usr/local/bin/localkube /usr/local/bin/minikube
systemctl stop '*kubelet*.mount'
sudo rm -rf /etc/kubernetes/
docker system prune -af --volumes
```

