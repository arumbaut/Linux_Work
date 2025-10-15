
Para saber en que DC estan las maquinas
```
for i in $(<maquinas.txt); do rvtquery --location $i;done | awk '{print $1, $3, $6}'
```

Para saber el estado de los discos en un listados 
```
 for i in $(<maquinas.txt); do rvtquery --vmdk $i;done | awk '{print $1, $2, $5}'
```