
#glusterFS
[introduction-to-glusterfs-file-system-and-installation-on-rhelcentos-and-fedora](http://www.tecmint.com/introduction-to-glusterfs-file-system-and-installation-on-rhelcentos-and-fedora/)
##### if first run
```shell
sudo wget -P /etc/yum.repos.d http://download.gluster.org/pub/gluster/glusterfs/LATEST/EPEL.repo/glusterfs-epel.repo
sudo yum install glusterfs-server
sudo service glusterd start
sudo service glusterd status
sudo chkconfig glusterd on
#Open ‘/etc/sysconfig/selinux‘ and change SELinux to either “permissive” or “disabled” mode on both the servers. Save and close the file.
sudo iptables -F
sudo gluster peer probe hpc204.sys.zzbc2.qihoo.net ...(all other nodes exclude self) # on old server
sudo mkdir -m 777 -p /euler/glusterfs/gv0 /gfs
# if first run
sudo gluster volume create gv0 replica 2 hpc204.sys.zzbc2.qihoo.net:/euler/glusterfs/gv0 hpc201.sys.zzbc2.qihoo.net:/euler/glusterfs/gv0 ...(all nodes)
sudo gluster volume start gv0
sudo gluster volume info gv0
sudo mount -t glusterfs server1:/gv0 /gfs
```
##### add new server
```shell
# on new server:
sudo wget -P /etc/yum.repos.d http://download.gluster.org/pub/gluster/glusterfs/LATEST/EPEL.repo/glusterfs-epel.repo
sudo yum install glusterfs-server
sudo service glusterd start
sudo service glusterd status
sudo chkconfig glusterd on
sudo mkdir -m 777 -p /euler/glusterfs/gv0 /gfs
# on old serve:
sudo gluster peer probe hpc*.sys.zzbc2.qihoo.net
sudo gluster volume add-brick gv0 hpc190.sys.zzbc2.qihoo.net:/euler/glusterfs/gv0 hpc189.sys.zzbc2.qihoo.net:/euler/glusterfs/gv0
sudo gluster volume rebalance gv0 start (sudo gluster volume rebalance gv0  start force)
sudo gluster volume rebalance gv0 status
# on new server:
sudo mount -t glusterfs hpc*.sys.zzbc2.qihoo.net:/gv0 /gfs (local hostname)
```
##### delete volume 
```
gluster volume delete test-volume
```

##### manage
- restart:
`sudo service glusterd restart`
