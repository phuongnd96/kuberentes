# Install NFS Provisioner for Kubernetes

OS: 20.04.1 LTS

### Install NFS Kernel Server

```
$ sudo apt update
$ sudo apt install nfs-kernel-server

```

### Create an NFS Export Directory

```
$ sudo mkdir -p /srv/nfs/kubedata
$ sudo chown -R nobody:nogroup /srv/nfs/kubedata
$ sudo chmod 777 /srv/nfs/kubedata
```

### Grant NFS Share Access to Client Systems

```
$ sudo vim /etc/exports
#allowed any host to have access to the NFS share.

/srv/nfs/kubedata $(rw,sync,no_subtree_check,no_root_squash,no_all_squash,insecure)
```

### Export Share Directory

```
$ sudo exportfs -rav
$ sudo systemctl restart nfs-kernel-server
```

### Allow NFS Access through the Firewall

```
sudo ufw allow from 0.0.0.0/0 to any port nfs
$ sudo ufw enable
$ sudo ufw status

#Port 2049, which is the default file share, should be opened.
```

### Install the NFS Client on the Client Systems

```
$ sudo apt update
$ sudo apt install nfs-common
```

### Test mount on Client

```
mount -t nfs <NFS_SERVER_IP_ADDR>:/srv/nfs/kubedata /mnt
mount | grep kubedata
umount /mnt
```

### Create ClusterRole, ClusterRoleBinding, StorageClass and NFS Client Provision Deployment

```
kubectl apply -f .
```
