
- Установил кластер из 1 мастера и 3 воркеров с помощью kubeadm
- По очереди обновил все ноды до версии 1.18.0
~~~
root@master:~# kubectl get nodes -o wide
NAME      STATUS                     ROLES    AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION   CONTAINER-RUNTIME
master    Ready                      master   22m   v1.18.0   10.166.0.18   <none>        Ubuntu 18.04.5 LTS   5.4.0-1029-gcp   docker://19.3.8
worker1   Ready                      <none>   16m   v1.18.0   10.166.0.19   <none>        Ubuntu 18.04.5 LTS   5.4.0-1029-gcp   docker://19.3.8
worker2   Ready,SchedulingDisabled   <none>   16m   v1.18.0   10.166.0.22   <none>        Ubuntu 18.04.5 LTS   5.4.0-1029-gcp   docker://19.3.8
worker3   Ready                      <none>   16m   v1.17.4   10.166.0.21   <none>        Ubuntu 18.04.5 LTS   5.4.0-1029-gcp   docker://19.3.8
~~~

- Попробовал kubespray, не уверен, что понравилось :)
~~~
[all]
 master ansible_host=35.228.3.44 etcd_member_name=etcd1
 worker1 ansible_host=35.228.128.60
 worker2 ansible_host=35.228.222.89
 worker3 ansible_host=35.228.255.221

[kube-master]
 master

[etcd]
 master

[kube-node]
 worker1
 worker2
 worker3

[k8s-cluster:children]
kube-master
kube-node
~~~