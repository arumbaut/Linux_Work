dvwa github maquina docker a explotar

 Burpsuit 
crtl+u codifica la url

```
sqlmap -u "url/?id=*" --level 1 --risk 1 --random-agent --proxy http://127.0.0.1:8080

```

Lo pasamos por el proxy de burpsuit y podemos ver las peticiones que esta ejecutando.

Integracion de BurpSuit con sqlmap


```
sqlmap -r req.txt --level 1 --risk 1 --random-agent --proxy http://127.0.0.1:8080

```

en la reques tendremos la request extraida desde bupsuit donde tendremos el campo id=*