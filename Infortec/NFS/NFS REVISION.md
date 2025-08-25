## ✅ **1. Ver NFS montados actualmente**

`mount -t nfs`

Esto te muestra solo los sistemas de archivos **montados como NFS**.

También puedes usar:

`df -hT | grep nfs`

- `-hT`: muestra el tipo de sistema de archivos (como NFS) en formato legible.
    
- `grep nfs`: filtra para mostrar solo las entradas NFS.
    

---

## ✅ **2. Ver los puntos NFS en `/etc/fstab` (auto-montaje)**

`cat /etc/fstab | grep nfs`

Esto muestra las configuraciones NFS que se montan automáticamente al iniciar el sistema.

---

## ✅ **3. Ver servidores NFS remotos disponibles (si tienes permisos y NFSv3)**

`showmount -e <ip_o_nombre_del_servidor>`

Ejemplo:

`showmount -e 192.168.1.100`

Esto muestra las exportaciones NFS disponibles desde ese servidor.