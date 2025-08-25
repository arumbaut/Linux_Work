### Fundamentos: imprimir columnas y filtrar

1. Imprimir primera columna de un archivo:
    
`awk '{print $1}' archivo.txt`

2. Imprimir primera y tercera columna:
    
`awk '{print $1, $3}' archivo.txt`

3. Imprimir líneas con más de 5 palabras:
    
`awk 'NF > 5' archivo.txt`

4. Imprimir líneas con exactamente 3 columnas:
    
`awk 'NF == 3' archivo.txt`

5. Imprimir líneas donde la segunda columna es mayor a 1000:
    
`awk '$2 > 1000' archivo.txt`

---

### 🔍 Análisis de archivos del sistema

6. Ver usuarios reales del sistema (UID >= 1000):
    
`awk -F: '$3 >= 1000 {print $1}' /etc/passwd`

7. Mostrar usuarios y su shell:
    
`awk -F: '{print $1, $7}' /etc/passwd`

8. Mostrar grupos del sistema (GID):
    
`awk -F: '{print $1, $3}' /etc/group`

9. Ver procesos que usan más de 50% CPU:
    
`ps aux | awk '$3 > 50'`

10. Ver procesos con más de 500 MB de memoria:
    
`ps aux | awk '$6 > 500000'`

---

### 🧮 Sumar, contar y calcular

11. Sumar los valores de la segunda columna:
    
`awk '{sum += $2} END {print sum}' archivo.txt`

12. Calcular promedio de la tercera columna:
    
`awk '{sum += $3} END {print sum/NR}' archivo.txt`

13. Contar cuántas líneas hay:
    
`awk 'END {print NR}' archivo.txt`

14. Contar cuántas líneas cumplen una condición:
    
`awk '$3 > 1000 {count++} END {print count}' archivo.txt`

15. Encontrar el valor máximo de una columna:
    
`awk 'max < $2 {max = $2} END {print max}' archivo.txt`

---
### 🧹 Procesamiento de logs

16. Ver direcciones IP de logs web:
    
`awk '{print $1}' /var/log/apache2/access.log`

17. Contar cuántas veces aparece un código 404:
    
`awk '$9 == 404 {count++} END {print count}' /var/log/apache2/access.log`

18. Filtrar errores del kernel:
    
`dmesg | awk '/error|fail|critical/ {print}'`

19. Ver intentos de login fallidos:
    
`awk '/Failed password/ {print $0}' /var/log/auth.log`

20. Contar usuarios únicos conectados vía SSH:
    
`awk '/Accepted/ {print $9}' /var/log/auth.log | sort | uniq | wc -l`

---

### 🛠️ Modificando separadores y formato

21. Usar separador personalizado (ej: `:` en `/etc/passwd`):
    
`awk -F: '{print $1, $3, $7}' /etc/passwd`

22. Separador por coma (CSV):
    
`awk -F, '{print $1, $3}' archivo.csv`

23. Formatear salida alineada:
    
`awk '{printf "%-10s %-10s\n", $1, $2}' archivo.txt`

24. Reemplazar valores durante la lectura:
    
`awk '{gsub(/ERROR/, "ERR"); print}' archivo.log`

25. Añadir texto al final de cada línea:
    
`awk '{print $0, " <- línea analizada"}' archivo.txt`

---

### 🧩 Combos útiles con otros comandos

26. Ver uso de disco (de `df`) si uso > 80%:
    
`df -h | awk '$5+0 > 80'`

27. Listar solo nombres de usuarios conectados:
    
`who | awk '{print $1}' | sort | uniq`

28. Ver tamaño total de directorios:
    
`du -sh /home/* | awk '{print $2, $1}' | sort -k2 -hr`

29. Mostrar procesos con su PID y memoria usada:
    
`ps aux | awk '{print $2, $4}' | sort -k2 -nr | head`

30. Ver conexiones activas por puerto (con `ss`):
    
`ss -tunlp | awk '{print $5}' | cut -d':' -f2 | sort | uniq -c`