### 1.linux主机tftp目录：`/home/lipei/data/tftp`

开发板下载文件：

```shell
ifconfig eth0 192.168.10.50
tftp -g -r 文件名 192.168.10.100
```



### 2.nfs目录：`/home/lipei/data/nfs`

开发板下载文件：

```shell
mount -t nfs -o nolock,nfsvers=3 192.168.10.100:/home/lipei/data/nfs get/

#卸载
umount get
```





ifconfig eth0 192.168.10.50  



网络图标消失：

```shell
sudo nmcli network off
sudo nmcli network on
```

