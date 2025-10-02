
```
🔹 Campos de crontab

# ┌──────── minuto (0-59)
# │ ┌────── hora (0-23)
# │ │ ┌──── día del mes (1-31)
# │ │ │ ┌── mes (1-12)
# │ │ │ │ ┌ día de la semana (0-7, 0=domingo)
# │ │ │ │ │
# * * * * * comando

|Símbolo|Significado|
|---|---|
|`*`|Todos los valores posibles|
|`*/n`|Cada n unidades (por ejemplo `*/5` cada 5 min)|
|`-`|Rango (por ejemplo `1-5` lunes a viernes)|
|`,`|Separar valores (por ejemplo `1,15,30`)|
|`@reboot`|Ejecutar al iniciar el sistema|
|`@daily`|Todos los días a medianoche (equivale a `0 0 * * *`)|
|`@hourly`|Cada hora (`0 * * * *`)|
|`@weekly`|Cada semana (`0 0 * * 0`)|
|`@monthly`|Cada mes (`0 0 1 * *`)|
```

Si lo quieres hacer para **otro usuario** (como root), puedes usar:
```
sudo crontab -u root -l   #Lista los crons
```


Para root. Logueado como root 
```
crontab -l    #Lista los crons
```

Editar crontab del usuario actual
```
crontab -e
0 3 * * * /home/usuario/backup.sh
```


Editar el crontab de otro usuario

```
sudo crontab -u user -e
0 3 * * * /home/usuario/backup.sh
```