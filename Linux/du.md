### Básico: ver uso de directorios

1. Ver tamaño total de un directorio:
    
`du -sh /home/usuario`

2. Ver tamaño de subdirectorios en el nivel actual:
    
`du -h --max-depth=1 /var`

3. Ver todos los directorios y subdirectorios:
    
`du -ah /etc`

4. Mostrar tamaño total en bloques legibles:
    
`du -sh *`

5. Ver tamaños sin seguir enlaces simbólicos:
    
`du -h --max-depth=1 -L /ruta`

---

### 📊 Ordenar y analizar

6. Directorios más grandes (ordenados):
    
`du -h /var | sort -hr | head -n 10`

7. Archivos más grandes de una ruta:
    
`find /var -type f -exec du -h {} + | sort -hr | head -n 10`

8. Ver tamaño total en GB:
    
`du -BG -s /var`

9. Comparar uso de diferentes carpetas:
    
`du -sh /home /var /etc`

10. Ver tamaño excluyendo ciertos directorios:
    
`du -h --exclude="cache" /var`

---

### 🔎 Filtrado con `find` y `du`

11. Tamaño total de archivos `.log`:
    
`find /var/log -name "*.log" -exec du -ch {} + | grep total`

12. Tamaño total de archivos mayores a 500 MB:
    
`find / -type f -size +500M -exec du -h {} + | sort -hr`

13. Uso de archivos modificados hace más de 30 días:
    
`find /var/log -type f -mtime +30 -exec du -h {} +`

14. Archivos más pesados en un sistema:
    
`find / -type f -exec du -b {} + | sort -nr | head -n 10`

15. Tamaño total de archivos en `/home` excluyendo ocultos:
    
`du -ch /home/* | grep total`

---

### 🔒 Seguridad y limpieza

16. Encontrar archivos grandes de usuarios:
    
`du -sh /home/* | sort -hr`

17. Ver cuánto ocupa el contenido de `/tmp`:
    
`du -sh /tmp/*`

18. Archivos grandes de root:
    
`sudo du -ah /root | sort -rh | head -n 10`

19. Listar carpetas de backups y cuánto ocupan:
    
`du -sh /backup/*`

20. Tamaño de logs del sistema:
    
`du -sh /var/log/*`

---

### 🧠 Opciones avanzadas

21. Usar bloques de 1K en lugar de KB/MB:
    
`du -k /home`

22. Mostrar bloques reales (espacio físico):
    
`du --apparent-size -h /var`

23. Mostrar progresivamente mientras calcula:
    
`du -h --max-depth=1 -c /var`

24. Excluir archivos temporales:
    
`du -h --exclude="*.tmp" /ruta`

25. Mostrar tamaño sin contar subdirectorios:
    
`du -sh --max-depth=0 /var/log/`

---

### 🧩 Combinaciones útiles

26. Exportar informe a archivo:
    
`du -ah /var > uso_disco.txt`

27. Mostrar uso y fecha de archivos antiguos:
    
`find /var/log -type f -mtime +90 -exec du -h {} +`

28. Sumar el espacio de logs rotados:
    
`du -ch /var/log/*.gz | grep total`

29. Directorios de usuario que ocupan más de 1 GB:
    
`du -sh /home/* | awk '$1 ~ /[0-9\.]+G/'`

30. Mostrar el tamaño de subdirectorios de `/` sin cruzar a otros sistemas de archivos:
    
`du -h --max-depth=1 -x /`