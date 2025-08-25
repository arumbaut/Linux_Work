Recortar campos o columnas

### **Extraer primera columna (separada por `:`)**

```
cut -d: -f1 /etc/passwd
```

### **Extraer varias columnas**

```
cut -d: -f1,3,6 /etc/passwd
```

### **Cortar por posición de caracteres**

```
echo "1234567890" | cut -c 3-6
```