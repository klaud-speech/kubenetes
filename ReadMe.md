
source from  https://blog.naver.com/adamdoha/222245086772



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
yum install conntrack
```

```
minikube start --driver=none
```
<details><summary>결과화면</summary>
* minikube v1.18.1 on Centos 7.3.1611 (kvm/amd64)  <br>
* Using the none driver based on user configuration  <br>
* Starting control plane node minikube in cluster minikube  <br>
* Running on localhost (CPUs=4, Memory=3790MB, Disk=102388MB) ...  <br>
* OS release is CentOS Linux 7 (Core)  <br>
    > kubelet: 108.73 MiB / 108.73 MiB [-------------] 100.00% 7.17 MiB p/s 16s  <br>
  - Generating certificates and keys ...  <br>
  - Booting up control plane ...  <br>
  - Configuring RBAC rules ...  <br>
* Configuring local host environment ...  <br>
*  <br>
! The 'none' driver is designed for experts who need to integrate with an existing VM  <br>
* Most users should use the newer 'docker' driver instead, which does not require root!  <br>
* For more information, see: https://minikube.sigs.k8s.io/docs/reference/drivers/none/  <br>
*  <br>
! kubectl and minikube configuration will be stored in /root  <br>
! To use kubectl or minikube commands as your own user, you may need to relocate them. For example, to overwrite your own settings, run:  <br>
*  <br>
  - sudo mv /root/.kube /root/.minikube $HOME  <br>
  - sudo chown -R $USER $HOME/.kube $HOME/.minikube  <br>  
*  <br>
* This can also be done automatically by setting the env var CHANGE_MINIKUBE_NONE_USER=true  <br>
* Verifying Kubernetes components...  <br>
  - Using image gcr.io/k8s-minikube/storage-provisioner:v4  <br>
* Enabled addons: default-storageclass, storage-provisioner  <br>
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default  <br>
</details>



shiny new cluster
```
kubectl get po -A
```
<details><summary> 결과화면 </summary>
NAMESPACE     NAME                                READY   STATUS    RESTARTS   AGE             <br>
kube-system   coredns-74ff55c5b-xst4m             1/1     Running   0          8m20s           <br>  
kube-system   etcd-localhost                      1/1     Running   0          8m30s           <br>
kube-system   kube-apiserver-localhost            1/1     Running   0          8m30s           <br>
kube-system   kube-controller-manager-localhost   1/1     Running   0          8m30s           <br>
kube-system   kube-proxy-fwmtn                    1/1     Running   0          8m20s           <br>
kube-system   kube-scheduler-localhost            1/1     Running   0          8m30s           <br>
kube-system   storage-provisioner                 1/1     Running   0          8m36s           <br>
</details>



```
minikube dashboard
```
<details><summary> 결과화면 </summary>
http://211.236.230.232:38012/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#1/pod?namespace=kube-system    
</details>

#### Deploy Applications
```
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080
minikube service hello-minikube
```
<details><summary> 결과화면 </summary>
|-----------|----------------|-------------|-----------------------|  <br>
| NAMESPACE |      NAME      | TARGET PORT |          URL          |  <br>
|-----------|----------------|-------------|-----------------------|  <br>
| default   | hello-minikube |        8080 | http://10.0.0.2:30768 |  <br>
|-----------|----------------|-------------|-----------------------|  <br>
* Opening service default/hello-minikube in default browser...        <br>
  - http://10.0.0.2:30768                                             <br>
 ==> CloudIT에서는  외부 IP 즉 http://211.236.230.232:30768            <br>
</details>

#### Application 실행하기
```
kubectl run test --image=adamdoha/test --port=8080 --generator=run/v1
kubectl get pods
kubectl describe pods
```

### Deployment 만들기
```
kubectl create deployment test-node --image=adamdoha/test
kubectl get deployments
kubectl describe pods
kubectl expose deployment test-node --type=LoadBalancer --port=8080
kubectl get services
minikube service test-node
curl http://211.236.230.232:30768/
```

#### 각종 조회
```
kubectl cluster-info
kubectl get nodes
kubectl get pods
kubectl get svc
```

#### Delete a local cluster
```
minikube stop
minikube delete
```


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

