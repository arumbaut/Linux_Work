Se hacen todas las verificaciones como si fueramos a cambiar un disco a depent

Despues dentro de la maquina verificaos que se aplico el cambio

lsmem -> numero de ram


```
{
echo "=== Memoria detectada por Linux ==="
lsmem | grep -E "Total online memory|Total offline memory|Memory block size"
echo
free -h
}
```

