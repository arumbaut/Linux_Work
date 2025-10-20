No se puede actualizar un Suset porque esta pasando por los repos de redhat

Error
```
[root@AOTLXPRCMV00001 1 ~]S yum update
Linux 8 for x86_64 - Baseos (RPMS)
Red Hat Enterprise Linux 8 for x8
's during downloading metadata for repository 'rhel-8-for-x86_64-baseos-rpms':
- Curl error (77): Problem with the SSL CA cert (path? access rights?) for https://cdn. redhat.com/content/dist/rhe18/8/x86_64/baseos/os/repodata/repomd.xml [error setting certificate verify locations:
etc/rhsm/ca/redhat-uep.pem
e: etc/rns
CApath: none]
Error: Failed to download metadata for repo 'rhel-8-for-x86_64-baseos-rpms': Cannot download repomd. xml: Curl error (77): Problem with the SSL CA cert (path? access rights?) for https://cdn. redhat.com/content/dist/rhe18/8/x86_64/baseos/os/repodata/repomd.xml [error setting certifi
cate verify locations:
CAfile: /etc/rhsm/ca/redhat-uep.pem
CApath: none1
[root@AOTLXPRCMV00001 1 ~]$
```


```
root@AOTLXPRCMV00001 -]# mv /etc/yum. repos. d/redhat. repo /etc/yum. repos. d/redhat. repo. disabled
[root@AOTLXPRCMV00001 -]# yum repolist
```