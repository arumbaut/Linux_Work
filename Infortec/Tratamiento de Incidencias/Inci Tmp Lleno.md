Nos conectamos a la maquina

```
df -hTP

df -hTP /tmp


#Listar los ficheros que mas espacio consumen
du -mx --max-depth=1 /tmp| sort -n

#Archivos que detectamos eran los mas grandes y empiezan por tibco_ADM*

ls -ld /tmp/tibco_ADM*
drwxr-xr-x 120 tibae5t tibco 20480 Sep  3 12:30 /tmp/tibco_ADMINT59_Administrator_AOTLXTETIB00009_tibae5t
drwxr-xr-x 106 tibae5t tibco 12288 Sep  3 12:43 /tmp/tibco_ADMINT59_TSM_AOTLXTETIB00009_tibae5t
drwxr-xr-x 147 tibae5u tibco 16384 Aug 29 18:09 /tmp/tibco_ADMUAT59_TSM_AOTLXTETIB00009.cosmos.es.ftgroup_tibae5u


#Para que nos de el tamaño de los archivos.
du -sh /tmp/tibco_ADM*
127M    /tmp/tibco_ADMINT59_Administrator_AOTLXTETIB00009_tibae5t
112M    /tmp/tibco_ADMINT59_TSM_AOTLXTETIB00009_tibae5t
137M    /tmp/tibco_ADMUAT59_TSM_AOTLXTETIB00009.cosmos.es.ftgroup_tibae5u
```