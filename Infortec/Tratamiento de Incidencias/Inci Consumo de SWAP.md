Entramos a la maquina
```
#Para ver los procesos
top

#Para abrir opciones de top y agregar columna de Swa
f

#Para ordenar los procesos por el uso de CPU
Shift+P 
```

```
Ver el comando completo que arrancó el proceso , en este caso el proceso que esta consumiendo de mas

ps -fp 117179

o para todos los java:

ps -ef | grep java

**Si quieres ver el árbol de procesos (qué lo inició)**

pstree -ap | grep 117179
```
