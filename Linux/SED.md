### **Reemplazar texto en una línea**

```
sed 's/error/ERROR/' archivo.log
```
Reemplaza la **primera aparición** de `error` por `ERROR`.

### **Reemplazo global en todas las líneas**

```
sed 's/error/ERROR/g' archivo.log
```

### **Eliminar líneas vacías**

```
sed '/^$/d' archivo.txt
```

### **Eliminar comentarios y espacios**

```
sed '/^#/d; s/ *$//' archivo.conf
```

### **Editar archivos en línea (-i)**

```
sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
```