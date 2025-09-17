Super importante , no solo se salva el vmdk (Disco) sino tambien el fichero de configuracion de la maquina vmx (config)
Dependiendo de si es de Torrejon o Yecora se salva en uno de estos datastorage dedicado para esto 
Yecora -> LINUX_REPO_YEC
Torrejon ->  LINUX_REPO_TOR

1-Detectar localizacion de las maquinas si en Torrejon o en yecora con rvtquery --location, ver los discos rvtquery --vmdk


```
[root@AOTLXTEADM00001 ~]# rvtquery --location AOTLXPRKEY00001

VM              
AOTLXPRKEY00001

POWERSTATE  
poweredOn   
     
DATACENTER
Datacenter_Yecora   #Datacenter dende se encuentra la maquina 

CLUSTER   #Nombre del cluster 
CLVC-ORANGE30_BBDD_PRO_YEC                 

HOST       # Servidor dentro del cluster donde se encuentra
aotlxprvin10289.cosmos.es.ftgroup

VI_SDK_SERVER      #  Servidor esx donde se encuentra 
AOTLXPRVC700001
```

```
[root@AOTLXTEADM00001 ~]# rvtquery --vmdk AOTLXPRKEY00001

VM           
AOTLXPRKEY00001

DISK         
Hard_disk_1

CAPACITY_MIB  
40960

RAW    
False

DISK_MODE  
dependent

SHARING_MODE  
None

THIN  
False
 
EAGERLY  
False

PATH
[CLVC-ORANGE30_BBDD_PRO_YEC_FS9K_N1_SISTEMA_2] AOTLXPRKEY00001_1/AOTLXPRKEY00001.vmdk

Importante el path se compone 
Nombre del cluster + File donde se encuentra mas el vmdk del disco es decir es toda la ruta para llegar al vmdk 
```


2-Detectar el disco del sistema operativo e identificarlo en el esx
Comandos utiles , por norma general el que contiene / es el de sistemas

```
[root@AOTLXPRKEY00001 ~]# lsblk
NAME                             MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                                8:0    0   40G  0 disk
├─sda1                             8:1    0    1G  0 part /boot
└─sda2                             8:2    0   39G  0 part
  ├─root_vg-lv_root              253:0    0    2G  0 lvm  /
  ├─root_vg-lv_usr               253:1    0  3.6G  0 lvm  /usr
  ├─root_vg-lv_var_opt_ansible   253:6    0    2G  0 lvm  /var/opt/ansible
  ├─root_vg-lv_gestion           253:8    0    4G  0 lvm  /gestion
  ├─root_vg-lv_home              253:10   0    2G  0 lvm  /home
  ├─root_vg-lv_tmp               253:11   0  500M  0 lvm  /tmp
  ├─root_vg-lv_itm               253:12   0    2G  0 lvm  /opt/IBM/ITM
  ├─root_vg-lv_tivoli            253:13   0    1G  0 lvm  /opt/Tivoli
  ├─root_vg-lv_opt               253:14   0    2G  0 lvm  /opt
  ├─root_vg-lv_var               253:15   0    4G  0 lvm  /var
  └─root_vg-lv_BinariosCommvault 253:25   0   10G  0 lvm  /BinariosCommvault
sdb                                8:16   0   20G  0 disk
└─swap_vg-lv_swap                253:2    0   20G  0 lvm  [SWAP]
sdc                                8:32   0   60G  0 disk
├─datos_vg-lv_homeuser_sigaux    253:3    0  100M  0 lvm  /homeuser/sigaux
├─datos_vg-lv_atoslec            253:4    0  500M  0 lvm  /atoslec
├─datos_vg-lv_opt_keycloak       253:5    0   50G  0 lvm  /opt/keycloak
├─datos_vg-lv_homeuser_keycloak  253:7    0    1G  0 lvm  /homeuser/keycloak
├─datos_vg-lv_proactiva          253:9    0  300M  0 lvm  /proactivanet
├─datos_vg-lv_homeuser           253:22   0  120M  0 lvm  /homeuser
└─datos_vg-lv_homeuser_sistemgc  253:24   0   64M  0 lvm  /homeuser/sistemgc
sdd                                8:48   0    2G  0 disk
└─ister_vg-lv_monauto            253:23   0    1G  0 lvm  /monauto
sde                                8:64   0   40G  0 disk
└─oracle_vg-lv_oracle            253:16   0   40G  0 lvm  /oracle
sdf                                8:80   0   20G  0 disk
└─oracle_temp_vg-lv_oracle_temp  253:17   0   20G  0 lvm  /oracle_temp
[root@AOTLXPRKEY00001 ~]#
```

Revisamod los pvs para ver cual tiene root que nos puede servir de informacion
```
[root@AOTLXPRKEY00001 ~]# pvs
  PV         VG             Fmt  Attr PSize   PFree
  /dev/sda2  root_vg        lvm2 a--  <39.00g   <5.90g
  /dev/sdb   swap_vg        lvm2 a--  <20.00g       0
  /dev/sdc   datos_vg       lvm2 a--  <60.00g   <7.94g
  /dev/sdd   ister_vg       lvm2 a--   <2.00g 1020.00m
  /dev/sde   oracle_vg      lvm2 a--  <40.00g       0
  /dev/sdf   oracle_temp_vg lvm2 a--  <20.00g       0
[root@AOTLXPRKEY00001 ~]#	
```

Notar que \[0:0:0:0]   Primer 0 es la controladora y 3er 0 es la posicion del disc en la controladora informacion util para identificar el dico ademas del tamaño y el device que coincide con el que tiene los archivos de sistemas -> /
```
[root@AOTLXPRKEY00001 ~]# lsscsi -s
[0:0:0:0]    disk    VMware   Virtual disk     2.0   /dev/sda   42.9GB
[0:0:1:0]    disk    VMware   Virtual disk     2.0   /dev/sdb   21.4GB
[0:0:2:0]    disk    VMware   Virtual disk     2.0   /dev/sdc   64.4GB
[0:0:3:0]    disk    VMware   Virtual disk     2.0   /dev/sdd   2.14GB
[0:0:4:0]    disk    VMware   Virtual disk     2.0   /dev/sde   42.9GB
[0:0:5:0]    disk    VMware   Virtual disk     2.0   /dev/sdf   21.4GB
```

Una vez identificado el disco o los discos que debemos guardar porque puede darse el caso excepcional que se expandiera el disco por un tema de capacidad e involucre a otro disco pues pasamos a revisa en cual datacenter lo guardaremos si en el de Torrejon o el de Yecora. para este caso de ejemplo es  Yecora 

Verificamos si tenemos espacio en el datacenter. Detectamos que tiene capacidad

![[Pasted image 20250911110453.png]]
Pues buscamos  LINUX_REPO_YEC y dentro de los files nos creamos un carpeta con el nombre del Cambio_NombreMaquina en este ejemplo seria CRQ000000165305_AOTLXPRKEY00001
![[Pasted image 20250911110310.png]]

![[Pasted image 20250911110613.png]]

4-El dia de la intervencion pues nos vamos al fichero vmx y el vmdk del sistema operativo y le damos Copiar salva del disco a la carpeta creada.
![[Pasted image 20250911110946.png]]
![[Pasted image 20250911111210.png]]

![[Pasted image 20250911111305.png]]

Mas notas a revisar
### 3. Revisar el archivo `.vmx` de la VM

Si entras al datastore (por NFS o datastore browser):

- Abre el archivo `nombre_vm.vmx`.
    
- Busca las entradas `scsi0:0.fileName = "disk1.vmdk"`.
    
    - Eso te dice qué **VMDK está asignado al bus principal (SCSI 0:0)** → suele ser el del SO.
        
- Otros discos (`scsi0:1`, `scsi1:0`, etc.) son secundarios.

## . Listar los discos que ve el SO

En Linux (dentro de la VM):

`lsblk -o NAME,SIZE,TYPE,MOUNTPOINT`

o

`sudo fdisk -l`

- Verás los discos como `/dev/sda`, `/dev/sdb`, etc.
    
- El que contiene `/` (root) es el **disco de sistema operativo**.
    
- Los demás probablemente son discos de datos.
    

👉 Esto **no te dice el número SCSI (0:0, 0:1, etc.)**, pero sí qué disco tiene el SO.

---

## 🔹 2. Ver el identificador de cada disco en Linux

Puedes mapear los discos internos de la VM con los VMDK desde ESXi usando sus identificadores únicos (UUID o WWN).

Dentro de la VM:

`sudo lshw -class disk -short`

o

`sudo ls -l /dev/disk/by-id/`

Eso te mostrará detalles de cada disco, incluyendo el **UUID** que luego puedes comparar en vSphere/ESXi.

---

## 🔹 3. Verlo desde **ESXi / vSphere**

Como no puedes ver el **bus SCSI** desde la VM, la forma más clara es:

- **vSphere Client / Host UI** → Edit Settings → Virtual Hardware → Disks.
    
    - Ahí sí verás:
        
        - Hard disk 1 → SCSI(0:0) → VMDK (SO).
            
        - Hard disk 2 → SCSI(0:1).
            
        - Etc.
            
- **SSH en ESXi**:
    
    `vim-cmd vmsvc/getallvms vim-cmd vmsvc/device.getdevices <VMID>`
    
    Y ahí listará cada disco con su `fileName` (`.vmdk`) y el bus (`scsi0:0`, etc.).
    

---

## 🔹 4. Truco práctico

👉 Si solo necesitas **detectar cuál es el disco del SO para hacer backup**:

- Desde la VM: usa `lsblk` → anota el tamaño del disco donde está `/`.
    
- Desde ESXi: revisa en la config de la VM cuál VMDK corresponde a ese tamaño.
    
- Ese es el disco que contiene el **sistema operativo**.
    

---

✅ En resumen:

- Desde la VM → puedes ver **qué disco contiene el sistema operativo** (por tamaño y punto de montaje).
    
- Para saber si es **SCSI(0:0)**, necesitas mirar la config en **ESXi** (UI o comandos).

Ejecuta:

`lscsi`

Ejemplo de salida en una VM con discos virtuales:

`[0:0:0:0]    disk    VMware   Virtual disk    2.0   /dev/sda [1:0:0:0]    disk    VMware   Virtual disk    2.0   /dev/sdb`

- `[0:0:0:0]` → corresponde al bus SCSI, target y LUN.
    
- `/dev/sda` → el dispositivo que ve el sistema operativo.
    

👉 Así puedes relacionar:

- `SCSI(0:0)` de VMware → `/dev/sda` en Linux.
    
- `SCSI(0:1)` → `/dev/sdb`.
    
- Y así sucesivamente.
    

---

## 🔹 Para ver qué disco contiene el SO

Luego combinas con:

`lsblk -o NAME,SIZE,MOUNTPOINT`

Ejemplo:

`sda   40G  / sdb  200G  /mnt/datos`

Aquí sabes que **`/dev/sda` (SCSI 0:0)** tiene el sistema operativo.