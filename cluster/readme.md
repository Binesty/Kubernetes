# Step by Step

## Requirements
- Docker
- Hosts to Kubernetes

## Customize
You can create customize playbooks  
Create files on the folder _blaybooks_ will be imported to local repository in nexts steps  
Edit files _environment.json_ and _cluster.yml_ in agreement with your environment  


## Initialize Container Semaphore Environment 
Create images with base from files in folder
```bash
docker compose build
```

Initialize containers from images builded
```bash
docker compose up -d
```

After initialize is recommended create a new user admin  
In the bash of container and inside folder semaphore, create the user a new user  
Below a sample to create new user, change XXXXXX to your password
```bash
semaphore user add --admin --login binesty --name binesty --email binesty@example.com --password XXXXXXX
```


## Configure semaphore UI
You can change configurations in page home, like languages and dark themes

## Access Hosts
- Create SSH keys container semaphore and copy public key in the hosts from kubernetes cluster
- In the menu semaphore set SSH Key access your hosts inventory
- Configure Key none to access repository github kubespray

Create ssh-key
```bash
ssh-keygen 
/home/ansible/.ssh/semaphore
```

Copy public SSH-key to root user ansible authorized_keys in .ssh folder on hosts kubernetes
```bash
cat <<EOF > ~/.ssh/authorized_keys
[file.pub]
EOF
```

## Access Git
The same ssh key generate before add to acess readonly on private repositories git

## Repositories
Configure repository to kubespray  
https://github.com/kubernetes-sigs/kubespray.git  
Inform branch master  

Configura Local Repository to local playbooks  
_/semaphore/blaybooks_  


## Iventory Hosts
select key access to hosts created before with ssh-key
select static file yaml and inform content data in file __cluster.yaml__


## Environment
Create a environment to kubespray configure kubernetes cluster  
A sample in file __environment.json__


## Playbook Create Cluster
- Create playbook to deploy cluster
- inform playbook filename: "cluster.yml" this is name file configuration default to kubespray  
- select inventory, environment and vault access (key) created before  
- check option allow CLI args in tasks


## Disks configurations Longhorn
after install longhorn if you need configure aditional disks falow steps below  

- execute lsblk on nodes
```bash
lsblk
```

view disks to configure longhorn __hdd aditional__  
- add node tag on longhorn set name in column node   
- add disk tag on longhorn set sdd and hdd disk  

|   node        |   disk   |   size    |   path   |  reserve |
|---------------|----------|-----------|----------|----------|
|   master      |    -     |     -     |    -     |    -     |
|   worker-01   |   sda    |   931 Gi  |  /data   |  50 Gi   |
|   worker-02   |   sda    |   465 Gi  |  /data   |  30 Gi   |
|   worker-03   |   sda    |   698 Gi  |  /data   |  40 Gi   |
|   worker-04   |   sda    |   465 Gi  |  /data   |  30 Gi   |
|   worker-05   |   sda    |   465 Gi  |  /data   |  30 Gi   |
|   worker-06   |   sda    |   465 Gi  |  /data   |  30 Gi   |
|   worker-07   |    -     |     -     |    -     |    -     |
|                                                            |



## Kubspray

### playbook remove node
remove node: parameter fix playbook: **["-e", "skip_confirmation=yes"]**  
advanced parameter run playbook : **["-e", "node=node-name"]**  


### playbook add node
advanced parameter limit node: **["--limit=node-name"]**


### install k9s
```bash
wget https://github.com/derailed/k9s/releases/download/v0.32.4/k9s_Linux_amd64.tar.gz -O /tmp/k9s.tar.gz 
tar -zxvf /tmp/k9s.tar.gz --directory /tmp/ 
chmod +x /tmp/k9s 
sudo mv /tmp/k9s /usr/local/bin/k9s 
```

### use vs code editor
nano .bashrc  
KUBE_EDITOR='code -w'