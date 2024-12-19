NFS , Network File System, a easiest way to get started for simple excercise. For production, use CEPH or GlusterFS like file system. We don't cover distributed file system in this training.

 


```
sudo apt-get update
sudo apt-get install nfs-kernel-server -y
```


```
sudo mkdir -p /nfs
sudo chown -R nobody:nogroup /nfs
sudo chmod -R 777 /nfs

mkdir -p /nfs/nifi-node1
mkdir -p /nfs/nifi-node2
mkdir -p /nfs/nifi-node3
mkdir -p /nfs/shared
```

```
sudo nano /etc/exports
```

```
/nfs/nifi-node1 *(rw,sync,no_subtree_check,no_root_squash)
/nfs/nifi-node2 *(rw,sync,no_subtree_check,no_root_squash)
/nfs/nifi-node3 *(rw,sync,no_subtree_check,no_root_squash)
/nfs/shared *(rw,sync,no_subtree_check,no_root_squash)
```

```
rw: Allows clients to read and write to the directory.
sync: Ensures changes are written to disk before confirming to the client (safer).
no_subtree_check: Improves performance by disabling checks on subdirectories.
no_root_squash: Allows the root user on the client to access the directory as root (useful for containers).
```

see if anything exported
```
showmount -e localhost
```

export the path in /etc/exports

```
sudo exportfs -ra
```



```
showmount -e localhost
```

now we are ready to use those paths in nifi cluster. We expect nifi to start flowfiles, content and provinence repo for syncing.

For your reference, nifi mount in below directory.. NIFI does not export those variable.. 



```
/opt/nifi/nifi-current/flowfile_repository/
/opt/nifi/nifi-current/database_repository/
/opt/nifi/nifi-current/provenance_repository/
/opt/nifi/nifi-current/content_repository/
```


```

docker exec -it nifi-node1 id

```

uid=1000(nifi) gid=1000(nifi) groups=1000(nifi)


Best is to add this to nifi-nodes yaml to start at 1000:1000


```
user: "1000:1000"
```


```
sudo chown -R 1000:1000 /nfs
sudo chmod -R 755 /nfs
```

REMEMBER to change IP ADDRESS in the docker compose for NFS server

