Habría que dar de alta las siguientes IPs de la vlan de backup (vlan 1010) en las maquinas:

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