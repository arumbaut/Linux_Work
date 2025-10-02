1- Entramos en la maquina del ftp que son diferentes de las otras.

Se crea el grupo segun la planilla del cliente

[root@~] # groupadd -g XXX grupo

3- Creamos el usuario con su UID y el home que nos indiquen en la planilla

[root@~] # useradd -d /dir/$NOMBRE -s /sbin/nologin -c "ES/C/FTE/FTE/Usuario SFTP XXXXXXXXXX. Responsable, Nombre. WO000000000XXXXX" usuario

```
useradd -m -u 1180 -g ps.jazztel -d /sftp/PS/ps.jazztel/Cargas/JAZZPLAT/Guadalajara/Auditoria/jlopgall -s /sbin/nologin -c "ES/C/FTE/FTE/Ignacio_Lopez_Bandres WO0000001176912" jlopgall
```

7- Creamos contraseña y le ponemos que no caduque nunca:

[root@aotlxprftp00001~] # passwd usuario

[root@aotlxprftp00001~] # chage -M -1 usuario

[root@aotlxprftp00001~] # chage -l usuario

Para enjaular los usuarios en el fichero de configuracion de ssh agregamos lo siguiente.
```
vi /etc/ssh/sshd_config


#Subsystem      sftp    /usr/libexec/openssh/sftp-server

Subsystem sftp internal-sftp

Match User jazztel_f #Nombre del user 
    ChrootDirectory /datos/ceres/in/jazztel #Directorio donde lo enjaulamos
    ForceCommand internal-sftp -u 0002
    X11Forwarding no
    AllowTcpForwarding no
    


Despues de modificar necesitamos recargar la configuracion
systemctl reload sshd
```

10- Pasamos la contraseña por correo y enviarle la peticion a RTV-TECSE RED DATOS

Nota es importante poner los permisos adecuados porque si no el servidor no permitira conectarnos asi que 

En el home del usuario crearemos una carpeta que es la que los retringira y donde podra hacer lo necesario le llamaremos upload Ejempo de estructura
```
Home del usuario /datos/ceres/in/jazztel
dentro upload    /datos/ceres/in/jazztel/upload

Los permisos tambien seran importantes 
chown root:root /datos/ceres/in/jazztel
chmod 755 /datos/ceres/in/jazztel

chown sftpAutor:sftpAutor /datos/ceres/in/jazztel/upload
chmod 755 /datos/ceres/in/jazztel/upload
```