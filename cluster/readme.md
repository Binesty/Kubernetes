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
