```
subscription-manager register --username=atlasnx  --auto-attach --force

Contraseña: Te56flon-nueva;

En el fichero /etc/rhsm/rhsm.conf hay que dejar configurado proxy_hostname y proxy proxy_port

# Orange
proxy_hostname = 10.132.14.200
proxy_port = 8080

# Jazztel
proxy_hostname = 172.29.1.33
proxy_port = 8080


En este caso si le tiras "ip a show" ves las IPs en este caso 

Las IPs son del tipo 10.x.x.x por lo que sería una maquina de ORANGE

Si fueran de JAZZTEL serían 172.x.x.x

Nos registramos
subscription-manager register --username=atlasnx  --auto-attach --password="Te56flon-nueva;" --force

Revisamos un paquete
yum info python3.x86_64 | grep -i version
```

Registrar un repositorio 
```
subscription-manager repos -- enable rhel-server-rhscl-7-rpms
```