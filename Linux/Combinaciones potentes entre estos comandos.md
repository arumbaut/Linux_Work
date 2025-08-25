
### Usuarios únicos ordenados de logs:

```
cut -d' ' -f1 /var/log/auth.log | sort | uniq
```

### Archivos más grandes en `/var/log`:

```
du -sh /var/log/* | sort -hr | head -10
```

### Palabras más repetidas en un archivo:

```
tr -cs '[:alpha:]' '\n' < archivo.txt | tr '[:upper:]' '[:lower:]' | sort | uniq -c | sort -nr | head
```