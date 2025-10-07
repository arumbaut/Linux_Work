Habría que dar de alta las siguientes IPs de la vlan de backup (vlan 1010) en las maquinas:

Primero en la maquina virtual agregamos una interface de red y en el adaptador le ponemos la vlan a la que hace referencia. Los primeros numeros son la vlan a la que pertenece el adaptador
Ejemplo el adamtador 4 pertenece a la VLAN 1010

![[Pasted image 20251007100223.png]]

Luego nos conectamos a la maquina y con nmcli

```
Revisamos que detecto la interface
nmcli c show

ip a

Muestra las rutas
pi a route

```

Copiamos la configuaracion para ejecutar 
```
{
 name="ens161"  ;   # Nombre del adaptador
 device="ens161"  ; # Nombre del adaptador
 ipaddr="192.168.6.227/23"; #Ip que nos pasan por parametro
 nmcli connection add con-name ${name} ifname ${device} type ethernet  ; 
 nmcli connection modify ${name} ipv4.addresses ${ipaddr}   ; 
 nmcli connection modify ${name} ipv4.method manual; 
 }
 
 para ver cuando se aplica la configuracion 
 
 watch ip a show ens161
```

Verificamos intentando hacerle ping a la maquina desde otro destinos en su vlan, buscamos en el inventario de maquinas en ADM01 alguna maquina de esa red y le hacemos ping
```
Adm01#infoserver
grep 192.168.7 * | grep RED

Buscamos una maquina de su red y nos conectamos a ella y le hacemos un ping 

Y luego revisamos con arp si coincide la mac
arp -n | grep 192.168.7.138
```

Existe un procedimiento por si existen dudas
[Configuración de interfaces de red (NMCLI) [SANTIAGO] - Atlas Wiki @ Kyndryl](https://10.113.57.21:1521/bin/view/Main/Grupos%20T%C3%A9cnicos/Linux-VMWARE/PROCEDIMIENTOS/LINUX/Configuraci%C3%B3n%20de%20interfaces%20de%20red%20%28NMCLI%29%20%5BSANTIAGO%5D/)

AOTLXPRCER00009 192.168.6.225
AOTLXPRCER00010 192.168.6.226
mascara: 255.255.254.0

```
Mostrar las interfaces fisicas

ip link show` o `ifconfig -a

ip a

Configuracion con nmcli
name="eth4"

device="eth4"

ipaddr="192.168.6.225/23"

nmcli connection add con-name ${name} ifname ${device} type ethernet

nmcli connection modify ${name} ipv4.addresses ${ipaddr}

nmcli connection modify ${name} ipv4.method manual

nmcli con show eth0

ip a


Sin nmcli
cd /etc/sysconfig/network-scripts/


ls
cp -p ifcfg-eth2 ifcfg-eth4

Agregamos los datos de ip que nos envian en la peticion 
vi ifcfg-eth4

cat ifcfg-eth4

nmcli connection up eth4

ifup eth4

ip a

ping 192.168.6.223

ip a | grep -i 192.168.6.225
```

¿Qué pasa si hay VLANs?

En Linux, cuando una interfaz física (`eth4`) está en una VLAN, normalmente no ves las VLANs directamente en el archivo base (`ifcfg-eth4`).  
En lugar de eso, se crean **interfaces lógicas VLAN** encima de la interfaz física.
Si el puerto físico `eth4` está conectado a un switch que **trunkea VLAN 10 y VLAN 20**, en Linux se crean interfaces:

```
Se crean los archivos para las vlans que crearemos

/etc/sysconfig/network-scripts/ifcfg-eth4.10
/etc/sysconfig/network-scripts/ifcfg-eth4.20

¿Cómo se configuran?

Ejemplo de archivo para `VLAN 10` sobre `eth4`:

/etc/sysconfig/network-scripts/ifcfg-eth4.10

DEVICE=eth4.10
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.10.100
NETMASK=255.255.255.0
VLAN=yes

para VLAN 20:
	
DEVICE=eth4.20
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.20.100
NETMASK=255.255.255.0
VLAN=yes

```