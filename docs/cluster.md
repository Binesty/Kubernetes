## kubeconfig copy
````bash
mkdir -p $HOME/.kube && cp -i /etc/kubernetes/admin.conf $HOME/.kube/config && chown $(id -u):$(id -g) $HOME/.kube/config
````

## Taint
- remove taint of master
````bash
kubectl taint nodes binesty-master node-role.kubernetes.io/control-plane:NoSchedule-
````

## Label nodes

- include label node role node and node-group
````bash
kubectl label --overwrite nodes binesty-worker-01 kubernetes.io/role=worker-01
kubectl label --overwrite nodes binesty-worker-02 kubernetes.io/role=worker-02
kubectl label --overwrite nodes binesty-worker-03 kubernetes.io/role=worker-03
kubectl label --overwrite nodes binesty-worker-04 kubernetes.io/role=worker-04
kubectl label --overwrite nodes binesty-worker-05 kubernetes.io/role=worker-05
kubectl label --overwrite nodes binesty-worker-06 kubernetes.io/role=worker-06
kubectl label --overwrite nodes binesty-worker-07 kubernetes.io/role=worker-07

kubectl label nodes binesty-master node-group=management
kubectl label nodes binesty-worker-01 node-group=application
kubectl label nodes binesty-worker-02 node-group=application
kubectl label nodes binesty-worker-03 node-group=application
kubectl label nodes binesty-worker-04 node-group=application
kubectl label nodes binesty-worker-05 node-group=application
kubectl label nodes binesty-worker-06 node-group=application
kubectl label nodes binesty-worker-07 node-group=application
````


kubespray create dns: [service].[namespace].svc.binesty