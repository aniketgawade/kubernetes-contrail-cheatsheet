# kubernetes-contrail-cheatsheet

Welcome to the kubernetes-contrail-cheatsheet wiki!

To get all contrail pod status:

```shell
for pod_name in `kubectl get pods -n kube-system -o wide | grep contrail | awk '{print $1}'`; do kubectl exec -it  $pod_name -n kube-system -- contrail-status ; done
```

Save kube docker id:

```shell
kubeid=$(docker ps | egrep kube-manager | egrep -v pause | awk '{print $1}')
```

Restart all contrail services in one go:

```shell
for pod_name in `kubectl get pods -n kube-system -o wide | grep contrail | awk '{print $1}'`; do for serv in `kubectl exec -it  $pod_name -n kube-system -- contrail-status | awk '{print $1}' | egrep -v == `; do servic=$(echo -n $serv | head -c -1); echo $pod_name $servic restarting ; kubectl exec -it  $pod_name -n kube-system -- service $servic restart; done  ; done
```

Manually run contrail-kube-manager:

```shell
/usr/bin/python /usr/bin/contrail-kube-manager -c /etc/contrail/contrail-kubernetes.conf
```

Manually restart all service in contrail container:

```shell
for a in `contrail-status  | awk '{print $1}' | egrep -v ==`; do servic=$(echo -n $a | head -c -1); service  $servic restart; done
```

If you get libcrypt error install faollowing 

```shell
wget https://launchpad.net/~ubuntu-security-proposed/+archive/ubuntu/ppa/+build/7110687/+files/libgcrypt11_1.5.4-2ubuntu1.1_amd64.deb

sudo dpkg -i libgcrypt11_1.5.4-2ubuntu1.1_amd64.deb
```