fuser -m /siebel23_sw

fuser

Sirve para mostrar qué procesos están usando un archivo, directorio o sistema de archivos.

Devuelve los PID (Process ID) de los procesos que tienen abierto el recurso.

-m

Indica que el argumento (/siebel23_sw) se debe interpretar como un punto de montaje (mount point) o un archivo dentro de un sistema de archivos.

Entonces fuser lista todos los procesos que están utilizando ese sistema de archivos completo, no solo un archivo en particular.

/siebel23_sw

Es el directorio/sistema de archivos que quieres comprobar.

📌 Ejemplo de salida:

/siebel23_sw:  1234c  5678  91011

Los números son PID de los procesos que están usando ese sistema de archivos.

El sufijo (como c) indica el tipo de acceso:

c = directorio actual

e = ejecutando archivo

f = archivo abierto

m = archivo mapeado en memoria

✅ En resumen:
El comando muestra qué procesos están accediendo al sistema de archivos montado en /siebel23_sw (o archivos dentro de él).