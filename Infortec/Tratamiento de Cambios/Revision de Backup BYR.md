1-Detectar localizacion de las maquinas si en Torrejon o En yecora con rvtquery --location, ver los discos rvtquery --vmdisck

2-Detectar el disco del sistema operativo e identificarlo en el esx
```
lsblk -f
df -h /
```

3-Crear en uno de los almacenes al que corresponda o el de Yecora o el de torrejon una carpeta con NoCambio_NomMaquina
4-Copiar salva del disco a la carpeta creada.



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