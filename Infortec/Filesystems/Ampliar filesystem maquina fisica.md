Solucion
Nos da los detalles del FS
```
 df -hPT /repocopytoran01
 
 Filesystem                                   Type  Size  Used Avail Use% Mounted on
/dev/mapper/veeamcptoran_vg-lv_repocptoran01 xfs    10T  9.9T  139G  99% /repocopytoran01
```

Importante al momento de expandir el tipo de filesystms xfs se utiliza este comando

⚠️ ¡No uses `resize2fs`! Eso es para ext3/ext4. Para XFS, es **xfs_growfs**, y debe apuntar al **punto de montaje**, no al dispositivo.
```
sudo xfs_growfs /repocopytoran01
```

Para ver los discos que tenemos mapeados
```
 multipath -ll
```

Para ver el los pv que conforman el vg y sus caracteristicas
```
[root@aotlxprver10006 ~]# pvs | grep -i veeamcptoran_vg
  /dev/mapper/mpathay veeamcptoran_vg lvm2 a--    <5.00t       0
  /dev/mapper/mpathm  veeamcptoran_vg lvm2 a--    <5.00t       0
```

Obtener el numero de serie de uno de los discos del pv
```
[root@aotlxprver10006 ~]# multipath -ll mpathm
mpathm (360050763808106f49800000000000157) dm-25 IBM,2145
size=5.0T features='1 queue_if_no_path' hwhandler='1 alua' wp=rw
|-+- policy='service-time 0' prio=50 status=active
| |- 7:0:2:11  sds  65:32   active ready running
| `- 17:0:2:11 sdbz 68:208  active ready running
`-+- policy='service-time 0' prio=10 status=enabled
  |- 7:0:6:11  sdam 66:96   active ready running
  `- 17:0:6:11 sdav 66:240  active ready running
```

Retorna el identificador de cada  Fibre Channel (host FC), con su correspondiente **WWPN (World Wide Port Name) **.
```
[root@aotlxprver10006 ~]# cat /sys/class/fc_host/host*/port_name
0x2100f4e9d456d7df
0x2100f4e9d456d7e2
0x2100f4e9d456d7e3
0x2100f4e9d456d7de
```

Obtener los  Fibre Channel  que estan activos.
```
[root@aotlxprver10006 ~]# cat /sys/class/fc_host/host*/port_state
Linkdown
Online
Linkdown
Online
```


Con esta informacion le harmos una solicitud al Grupo SPAIN ALMACENAMIENTO pidiendole que nos agreguen el espacio requerido.

Reescanear el multipath
```
for s in /sys/class/scsi_host/host*/scan; do echo "- - -" > "$s"; done
```

cabina es1v7k10fte (Pool_Nearline_TSM)

Lun 52 (dec) 5TiB 60050763808106F49800000000000228
Lun 53 (dec) 5TiB 60050763808106F49800000000000229


Multipath verificacion
```
[root@aotlxprver10006 ~]# multipath -ll | grep -i -e 60050763808106F49800000000000229
mpathba (360050763808106f49800000000000229) dm-29 IBM,2145
[root@aotlxprver10006 ~]# multipath -ll | grep -i -e 60050763808106F49800000000000228
mpathaz (360050763808106f49800000000000228) dm-28 IBM,2145
[root@aotlxprver10006 ~]#
```
