	Primero realizamos un crqcheck para ver el estado de la maquina esto es en el host que le haremos el cambio.

```
  crqcheck --verbose
```

Revisamos el estado de los discos en ese host haciendo un rvtquery desde la maquina AOTLXTEADM00001

```
[root@AOTLXTEADM00001 ~]# rvtquery --vmdk spmg10yr

```

Hacemos una salva del hash del root por si surge algun problema al momento del reinicio.

```
getent shadow root > root-$(date +%F).shadow
root-2025-08-12.shadow
root:$1$KgF4jb1O$asdasadasdasqqdddq/63/:20301:1:90:7:::

```

para restaurar shadow de root en caso de problemas con el pass

```
usermod --password $(awk -F : '{ print $2 }' nombre_fichero.shadow) root

usermod -p '$1$CJJYHxtl$asdasd/hzcIva1' root
```

Lanzamos este comando para guardar el estado de las caracteristicas de la maquina

```
{
crqcheck --verbose
cat /etc/selinux/config | grep SELINUX= | grep -v "# SELINUX"
df -hPT /boot
df -hPT /usr
df -hPT /var
ip a
route -n
cat /etc/redhat-release
uname -a
cat /etc/shadow | grep root | cut -d ':' -f1-2 
}

```


Una ves obtenida toda esta info vamos a desmonitorizar la maquina para el momento del cambio en este portal.
https://es01pap00cs00xg:9444/Portal/facelets/home.xhtml

management 
	-> maintenance mode
		 -> new y te saldra esto:
		 ![[Pasted image 20250812122308.png]]
		 En browse añadir txt con las maquinas a desmonitorizar con este formato: Siempre poner aot_nombre de la maquina
		 
		 aot_wfdd01ma02;aot_wfdp01ma01;aot_wfdr30rr;
		 Luego, upload
	
Finalmente guardar desmonitorizado en START


Antes de apagar la VM en el cambio lanzar:

```
echo `hostname`"#MIN_UPTIME=0" > /usr/local/bin/script_uptime.conf
```

Cuando arranque

```
nohup bash -c "sleep 35m; rm -fv /usr/local/bin/script_uptime.conf" &
```


Pasos cuando vamos a ejecutar las tareas de cambio 
1-Nos conectamos por ssh 
2-Volvemos a hacer un crqcheck del estado de la maquina
3-Cambiamos la pass de root a te56flon
4-Abrimos la consola web de la maquina en VMware 
5-Apagamos la pc desde la terminal (no desde el vmware)
6-Cambiamos los discos de estado al deseado
7-Iniciamos la pc desde  la interface de vmware
8-Reestablecemos la pass del admin inyectandole el hash obtenido previamente
```
usermod -p '' root
```
9-Hacemos un crqcheck de la maquina y comparamos el estado actual con el estado despues de reiniciar
10-Hacemos un uptime de la maquina para ponerlo como evidencia en remedy
11-Cerramos las tareas en remedy y damos paso en Linux-CPD de ser necesario.
