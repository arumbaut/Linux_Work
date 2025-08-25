### Básico: uso del espacio en disco

1. Ver el uso de disco en formato legible:
    
`df -h`

2. Ver uso de disco en megabytes:
    
df -m`

3. Mostrar uso de disco en bloques de 1K:
    
`df -k`

4. Ver solo la información de `/`:
    
`df -h /`

5. Mostrar sistemas de archivos junto a sus tipos:
    





`df -Th`

---

### 🧮 Ver uso de inodos

6. Ver uso de inodos en todo el sistema:
    





`df -i`

7. Ver uso de inodos con tipos de sistema de archivos:
    





`df -iT`

8. Ver inodos del sistema raíz:
    





`df -i /`

9. Ver sistemas de archivos con inodos casi llenos:
    





`df -i | awk '$5+0 > 80'`

10. Ver en porcentaje los más usados:
    





`df -h | grep -v tmpfs | sort -k 5 -r`

---

### 🛠️ Filtrado y procesamiento con `awk`, `grep`, etc.

11. Ver solo los sistemas de archivos con uso > 90%:
    





`df -h | awk '$5+0 > 90'`

12. Mostrar solo los puntos de montaje:
    





`df -h | awk '{print $6}'`

13. Mostrar solo los que están montados sobre `/dev/`:
    





`df -h | grep "^/dev/"`

14. Ver solo los dispositivos `ext4`:
    





`df -Th | grep ext4`

15. Ver el sistema de archivos del directorio actual:
    





`df -h .`

---

### 🔍 Uso combinado con otros comandos

16. Ver uso de disco y listar directorios grandes:
    





`df -h; du -h --max-depth=1 /`

17. Mostrar espacio libre del sistema raíz:
    





`df -h / | awk 'NR==2 {print $4}'`

18. Ver si alguna partición está llena y notificar:
    





`df -h | awk '$5+0 >= 95 {print "¡Partición casi llena! ", $6}'`

19. Comprobar si hay algún punto de montaje con espacio crítico:
    





`df -h | awk '$5+0 > 85 {print $0}'`

20. Mostrar las 5 particiones con menos espacio libre:
    





`df -h | sort -k 4 | head -n 5`

---

### 🔐 Auditoría y monitoreo

21. Ver espacio usado en `/home`:
    





`df -h /home`

22. Ver espacio libre en `/var` (logs):
    





`df -h /var`

23. Comprobar espacio libre antes de instalar software:
    





`df -h /opt`

24. Ver si `/boot` está en riesgo (muy común en servidores):
    





`df -h /boot`

25. Chequear espacio libre en montajes NFS:
    





`df -hT | grep nfs`

---

### 📁 Información por tipo de FS o filtros especiales

26. Ver solo particiones locales (omitir tmpfs, devtmpfs):
    





`df -h | grep -Ev "tmpfs|devtmpfs"`

27. Ver uso de disco sin encabezados (para scripts):
    





`df -h | tail -n +2`

28. Exportar uso de disco a un archivo:
    





`df -h > uso_disco.txt`

29. Ver FS de más de 80% con alerta:
    





`df -h | awk '$5+0 > 80 {print $1, $5, $6}'`

30. Usar `watch` para monitoreo en tiempo real:
    





`watch -n 5 'df -h | grep "^/dev/"'`