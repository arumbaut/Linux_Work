Ejemplo: Ver solo el nombre de usuario y su UID de `/etc/passwd`

```
awk -F: '{print $1, $3}' /etc/passwd

```

Ver usuarios con UID mayor a 1000:

```
awk -F: '$3 > 1000 { print $1, $3 }' /etc/passwd
```

### **Contar líneas o entradas específicas**

Contar cuántos usuarios son del grupo `sudo`:

```
grep '^sudo:' /etc/group | awk -F: '{print NF > 3 ? NF-3 : 0}'
```

### **Procesar logs del sistema**

Ver IPs que aparecen en `/var/log/auth.log` (por ejemplo, intentos SSH):

```
awk '/sshd/ && /Accepted/ {print $NF}' /var/log/auth.log
```

### **Monitorear uso de CPU o RAM**

Ejemplo: uso total de memoria por proceso con `ps`

```
ps aux | awk '{sum += $4} END {print "Uso total de RAM: " sum "%"}'
```

### **Transformar salidas para scripts o reportes**

Convertir el archivo `/etc/passwd` a formato CSV:

```
awk -F: '{print $1","$3","$4","$6}' /etc/passwd > usuarios.csv
```

Ver porcentaje de uso del ram
```
free | awk '/Mem:/ {printf("RAM usada: %.2f%%\n", $3/$2 * 100)}'
```


Ver porcentaje de uso del swap 
```
free | awk '/Swap:/ {if ($2 != 0) printf("Swap usada: %.2f%%\n", $3/$2 * 100); else print "Swap no está habilitada."}'
```

### **Comando combinado (RAM + Swap)**

```
free | awk ' /Mem:/ {     printf("RAM usada: %.2f%%\n", $3/$2 * 100) } /Swap:/ {     if ($2 != 0)         printf("Swap usada: %.2f%%\n", $3/$2 * 100)     else         print "Swap no está habilitada." }'
```

