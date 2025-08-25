Nos conectamos a la maquina vemos el consumo con 

```
free -h || free -hl
```

Revisamos que realmente el consumo es alto y vamos a ver los procesos que estan consumiendo la swap 

```
top
#Presionamos la tecla f para agregar la columna de la swap esto agrega la swap al menu
Luego ordenamos presionando

Shift+>

Nos permite organizar la info de mayor a menor y ver quienes estan consumiendo mas swap


```

Para dar respuesta a la inci nos vamos al portal de inventario
donde buscamos la maquina a la que estamos analizando y nos fjamos en los aplicativos que tines para revisar a quien le pasamos las incidencias

Portal
[App de Busqueda de Inventario - Victor Salazar - SRE](https://10.132.76.45/app/index.php)

En el signo de i nos da a que grupo remedi pertenece para el grupo para reenviarle la peticion.
	