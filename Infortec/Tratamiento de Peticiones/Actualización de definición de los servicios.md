```
#Para editar un servicio 
systemctl edit laspalmas.service 

#Para hacer un reload de los servicios
systemctl daemon-reload

```

```
[root@AOTLXPRCYC00011 ~]# systemctl cat coslada.service
# /etc/systemd/system/coslada.service
[Unit]
Description = Sistema de Seguridad Cyclops coslada
[Service]
Type=forking
User=odekia
Restart=yes
ExecStart = /centinela/centinelaXXI/coslada/config/centinela start
ExecStop  = /centinela/centinelaXXI/coslada/config/centinela stop
[Install]
WantedBy=multi-user.target graphical.target
[root@AOTLXPRCYC00011 ~]#
```