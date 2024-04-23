# Backup and Restore cluster

kubespray create configurations default backup etcd  
__roles/etcd/defaults/main.yml__ 

folder backup default: /var/backups
change to path nfs  

## Check ETCD Members

- list member
```bash
    sudo etcdctl --key /etc/ssl/etcd/ssl/admin-binesty-master-key.pem --cert /etc/ssl/etcd/ssl/admin-binesty-master.pem member list
```
- display endpoints health
```bash
   sudo etcdctl --key /etc/ssl/etcd/ssl/admin-binesty-master-key.pem --cert /etc/ssl/etcd/ssl/admin-binesty-master.pem endpoint health
```
	
## Backup and Restore

- save snapshot backup    
```bash
   sudo etcdctl --key /etc/ssl/etcd/ssl/admin-binesty-master-key.pem --cert /etc/ssl/etcd/ssl/admin-binesty-master.pem snapshot save /var/nfs/etcd/snapshot-binesty-master-$(date +%d-%m-%Y).db
```
- restore snapshot    
```bash
   sudo etcdctl --key /etc/ssl/etcd/ssl/admin-binesty-master-key.pem --cert /etc/ssl/etcd/ssl/admin-binesty-master.pem snapshot restore {file}.db
```