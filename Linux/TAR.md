### **Convertir mayúsculas a minúsculas (o viceversa)**

```
echo "LINUX ES PODEROSO" | tr 'A-Z' 'a-z'
```

Y al revés:

```
echo "admin" | tr 'a-z' 'A-Z'
```

### **Eliminar caracteres específicos**

Eliminar los dos puntos de `/etc/passwd`:

```
cat /etc/passwd | tr -d ':'
```

### **Reemplazar espacios por guiones o cualquier otro carácter**

```
echo "backup del sistema" | tr ' ' '_'
```

### **Eliminar caracteres no imprimibles (limpiar texto/logs)**

```
cat archivo | tr -cd '\11\12\15\40-\176'
```

### **Convertir saltos de línea en espacios (una sola línea)**

```
cat lista.txt | tr '\n' ' '
```

### **Reemplazar múltiples caracteres por uno solo**

```
echo "111222333" | tr -s '123'

- `-s` = squeeze (comprime caracteres repetidos).
```

### **Quitar todos los números de una cadena**

```
echo "log123file456" | tr -d '0-9'

🟰 `logfile`
```

### Ejemplo real de uso en scripts:

Quitar comentarios y espacios en blanco de un archivo de configuración:

```
cat config.cfg | tr -d ' \t' | grep -v '^#'
```