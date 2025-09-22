Necesitamos que se hagan las siguientes modificaciones en el servidor de PRE AOTLXPPSPI00001:

1. Quitar del TLS el puerto 990, pero dejar escuchando por FTP estándar
2. Incluir en el TLS el puerto 21 solamente, para realizar conexiones FTPS


```
[root@AOTLXPPSPI00001 vsftpd]# systemctl status vsftpd-990  
● vsftpd-990.service - Vsftpd ftp daemon on port 990  
   Loaded: loaded (/usr/lib/systemd/system/vsftpd-990.service; enabled; vendor preset: disabled)  
   Active: active (running) since Thu 2025-08-21 12:53:41 CEST; 1 months 1 days ago  
Main PID: 2984056 (vsftpd)  
    Tasks: 1 (limit: 61717)  
   Memory: 1.5M  
   CGroup: /system.slice/vsftpd-990.service  
           └─2984056 /usr/sbin/vsftpd /etc/vsftpd/vsftpd-990.conf

Warning: Journal has been rotated since unit was started. Log output is incomplete or unavailable.  
[root@AOTLXPPSPI00001 vsftpd]# systemctl stop vsftpd-990  

[root@AOTLXPPSPI00001 vsftpd]# ss -tlnp | grep 990  

[root@AOTLXPPSPI00001 vsftpd]# systemctl disable vsftpd-990  
Removed /etc/systemd/system/multi-user.target.wants/vsftpd-990.service.  
[root@AOTLXPPSPI00001 vsftpd]#  

[root@AOTLXPPSPI00001 vsftpd]# ss -tlnp | grep 990  
LISTEN 0      32                  0.0.0.0:990        0.0.0.0:*    users:(("vsftpd",pid=2984056,fd=3))                                                  

[root@AOTLXPPSPI00001 vsftpd]# cat /usr/lib/systemd/system/vsftpd-990.service  

[Unit]  
Description=Vsftpd ftp daemon on port 990  
After=network-online.target

[Service]  
Type=forking  
ExecStart=/usr/sbin/vsftpd /etc/vsftpd/vsftpd-990.conf

[Install]  
WantedBy=multi-user.target  
[root@AOTLXPPSPI00001 vsftpd]#
```