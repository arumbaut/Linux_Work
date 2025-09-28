
Nos paramos en el directorio y vemos el tamaño que ocupan sus subdirectorio para determinar cuales son los mas grande los ordenamos de menos a mayor 
```
Mas recomendado

du -sh * | sort -h | column -t

Orden Inverso
du -sh * | sort -hr | column -t

du -sh * | sort -h

Mostrar solo los más grandes (top 10)
du -sh * | sort -h | tail -10

Resaltar con column
du -sh * | sort -h | column -t

Ver ocupación total + detalle
echo "=== ESPACIO TOTAL ==="
df -h .
echo
echo "=== DETALLE DE CARPETAS ==="
du -sh * | sort -h | column -t
```

Compimir log
```
Comprime lso que tengan ese patron
gzip dnf.librepo.log.[0-9]*
```


Limpiar cache en caso de estar muy repleta
```
yum clean all
```