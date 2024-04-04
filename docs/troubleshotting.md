## Kubernetes

prune images containerd
crictl rmi --prune


downgrade version kubernetes
````bash
sudo apt-get purge kubeadm kubectl kubelet
sudo apt-get update
sudo apt-get install kubeadm=1.25.0-00 kubectl=1.25.0-00 kubelet=1.25.0-00
sudo apt-mark hold kubeadm kubectl kubelet
````

## Longhorn


If necessary because problems multipath mount pvs  
Create file in de node with problems
````bash
touch /etc/multipath.conf  
````

Configure file
````bash
nano /etc/multipath.conf  
````

Content
````bash
blacklist {
    devnode "^sd[a-z0-9]+"
}
````

Restart Service
````bash
systemctl restart multipathd.service
````

Verify configuration
````bash
multipath -t
````

Referente [longhorn](https://longhorn.io/kb/troubleshooting-volume-with-multipath/)


