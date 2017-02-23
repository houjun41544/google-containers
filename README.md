# kube
由于不能从google container上直接pull镜像，所以这里通过docker hub的Automated Builds功能从项目的dockerfile中Build到docker的官方服务器上，然后再从它们上面拉取.

##	kube 1.5.1需要的镜像:
```
REPOSITORY                                               TAG
gcr.io/google_containers/kube-apiserver-amd64            v1.5.1
gcr.io/google_containers/kube-controller-manager-amd64   v1.5.1
gcr.io/google_containers/kube-proxy-amd64                v1.5.1
gcr.io/google_containers/kube-scheduler-amd64            v1.5.1
gcr.io/google_containers/kubernetes-dashboard-amd64      v1.5.0
gcr.io/google_containers/etcd-amd64                      3.0.14-kubeadm
gcr.io/google_containers/kube-discovery-amd64            1.0
gcr.io/google_containers/pause-amd64                     3.0
gcr.io/google_containers/kubedns-amd64                   1.9
gcr.io/google_containers/dnsmasq-metrics-amd64           1.0
gcr.io/google_containers/kube-dnsmasq-amd64              1.4
gcr.io/google_containers/exechealthz-amd64               1.2
gcr.io/google_containers/node-problem-detector           0.1
```

## docker hub上设置
由于docker hub不能后期更改一个image的tag，所以每次更新kubernetes时，都在build settings中，手动增加一个版本对应文件

## 更改tag
```
images=(kube-proxy-amd64:v1.5.1 kube-discovery-amd64:1.0 kubedns-amd64:1.9 kube-scheduler-amd64:v1.5.1 kube-controller-manager-amd64:v1.5.1 kube-apiserver-amd64:v1.5.1 etcd-amd64:3.0.14-kubeadm kube-dnsmasq-amd64:1.4 exechealthz-amd64:1.2 pause-amd64:3.0 kubernetes-dashboard-amd64:v1.5.0 dnsmasq-metrics-amd64:1.0)
for imageName in ${images[@]} ; do
  docker pull ist0ne/$imageName
  docker tag ist0ne/$imageName gcr.io/google_containers/$imageName
  docker rmi ist0ne/$imageName
done

images=(heapster:canary heapster_grafana:v3.1.1 heapster_influxdb:v0.7)
for imageName in ${images[@]} ; do
  docker pull ist0ne/$imageName
  docker tag ist0ne/$imageName kubernetes/$imageName
done
```