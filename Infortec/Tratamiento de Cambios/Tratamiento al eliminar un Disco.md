Primero debemos tener en cuenta que el disco no tenga ningun home user o asociado a un file sistem

Al momente de eliminar el disco no eliminaremos los datos del almacen en caso de que se requiera volver a restaurar dicho disco.  
Se deben comentar las linesa del fstab asocadas a este para que la maquina arranque sin nigun fallo.


Al momento de restaurar devemos revisar las configuraciones que tenia caundo estaba activo 

```
Para ver el almacen al que pertenecia para buscarlo y restaurarlo
 rvtquery --date=-1 --vmdk AOTLXPPBRM00001  
 
 Para ver la controladora que tenia
 rvtquery --date=-1 --scsi AOTLXPPBRM00001
```

Nos vamos a la mquina y seleccionamos Agregar Nuevo dispositivo/Disco duro existente 
Y lo buscamos en el almacen que se encontraba y el disco que queremos remontar
![[Pasted image 20251006115831.png]]

![[Pasted image 20251006115846.png]]

Ponemos los parametros como antes y pasamos a la maquina para acoplarlo y montar las rutas

Primero 
```
Para ver si detecta el disco
[root@AOTLXPPBRM00001 ~]# lsblk

Si no lo ve reescaneamos los discos
for s in /sys/class/scsi_host/host*/scan; do echo "- - -" > "$s"; done

Vemos si lo agrego al vg 
vgs

Modificamos el fstab para que vuelva a montar los FS en sus puntos de montaje

Hacemos un montaje de cada uno de los que pertenecen a este
 for i in /homeuser/sigaux /atoslec /l_tibco /opt/portal /homeuser/pinm_ftp /homeuser/pinm_st /tiba5mt /var/portal/7.5/pinm_st /Oracle_client /homeuser2; do mount $i;done
 
 Revisamos 
 for i in /homeuser/sigaux /atoslec /l_tibco /opt/portal /homeuser/pinm_ftp /homeuser/pinm_st /tiba5mt /var/portal/7.5/pinm_st /Oracle_client /homeuser2; do df -hPT $i;done
 
 Evidencia
 
[root@AOTLXPPBRM00001 ~]# pvs | grep 120
  /dev/sdf   datos_vg     lvm2 a--  <120.00g      0
[root@AOTLXPPBRM00001 ~]# date
Mon Oct  6 11:17:36 CEST 2025
[root@AOTLXPPBRM00001 ~]#
```
Para ver evidencias de una mv:

```
[root@AOTLXTEADM00001 251006]# ls -lrt | grep AOTLXPPBRM00001  
-r--r--r-- 1 ftp  ftp    11748 Oct  6 06:37 inf_linux_**AOTLXPPBRM00001**.txt  
[root@AOTLXTEADM00001 251006]# pwd  
/evidencias/INFOSERVER/251006  
[root@AOTLXTEADM00001 251006]#
```
