
```
top -d1 -c
```

#### `-d 1`

Indica el **intervalo de actualización** en **segundos**.

#### `-c`

Muestra el **comando completo** con todos los argumentos de cada proceso.

 - Show only processes owned by given user:
```
   top [-u|--filter-only-euser] username
```

- Show the individual threads of a given process:
```
   top [-Hp|--threads-show --pid] process_id
```