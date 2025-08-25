## 30 comandos esenciales con `grep` (búsqueda y análisis de texto)

### 🔍 Búsqueda básica

1. Buscar una palabra exacta en un archivo:

`grep "root" /etc/passwd`

2. Buscar ignorando mayúsculas:
    
`grep -i "error" /var/log/syslog`

3. Buscar palabra completa:
    
`grep -w "Listen" /etc/httpd/conf/httpd.conf`

4. Buscar varias palabras (OR):
    
`grep -E "sshd|nginx" /var/log/auth.log`

5. Buscar líneas que **no** contienen una palabra:
    
`grep -v "nologin" /etc/passwd`

---

### 🧠 Expresiones regulares (regex)

6. IPs en un log:
    
`grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' /var/log/syslog`

7. Líneas que empiezan con `#`:
    
`grep "^#" /etc/fstab`

8. Líneas que terminan en `;`:
    
`grep ";$" archivo.conf`

9. Números en archivos de texto:
    
`grep -E "[0-9]+" archivo.txt`

10. Palabras con más de 6 letras:
    
`grep -E '\b\w{7,}\b' archivo.txt`

---

### 🔄 Recursión y contexto

11. Buscar en todos los archivos de un directorio:
    
`grep -r "PermitRootLogin" /etc/ssh`

12. Incluir nombre de archivo en la salida:
    
`grep -H "fail" /var/log/*`

13. Mostrar líneas anteriores y posteriores:
    
`grep -C 2 "Failed password" /var/log/auth.log`

14. Mostrar 3 líneas antes del match:
    
`grep -B 3 "segfault" /var/log/syslog`

15. Mostrar 3 líneas después del match:
    
`grep -A 3 "nginx" /var/log/nginx/error.log`

---

### 🧮 Conteo y resumen

16. Contar ocurrencias de un patrón:
    
`grep -o "error" /var/log/syslog | wc -l`

17. Mostrar solo nombres de archivos con coincidencias:
    
`grep -l "bash" /etc/*`

18. Mostrar solo los archivos **sin** coincidencias:
    
`grep -L "bash" /etc/*`

---

### 🛠️ Integrado con otros comandos

19. Buscar procesos en ejecución:
    
`ps aux | grep "[s]shd"`

20. Buscar conexiones de red:
    
`netstat -tunapl | grep ":80"`

21. Buscar usuarios conectados por SSH:
    
`who | grep "pts"`

---

### 🔒 Seguridad y monitoreo

22. Buscar accesos fallidos:
    
`grep "Failed password" /var/log/auth.log`

23. Buscar cambios de grupo en sudoers:
    
`grep "^%admin" /etc/sudoers`

24. Buscar archivos con contraseñas o claves:
    
`grep -r "password" /etc/`

---

### 🧩 Combinaciones útiles

25. Filtrar logs por fecha:
    
`grep "2025-08-08" /var/log/syslog`

26. Buscar errores en logs del sistema:
    
`grep -i "error" /var/log/syslog`

27. Ver cambios recientes en cron:
    
`grep -i "cron" /var/log/syslog`

28. Buscar conexiones SSH:
    
`grep "sshd" /var/log/auth.log`

29. Buscar configuraciones comentadas en ficheros:
    
`grep "^#" /etc/ssh/sshd_config`

30. Buscar líneas vacías (solo saltos de línea):
    
`grep "^$" archivo.txt`