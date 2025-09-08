### Instalacion  
  
1.   
```bash  
# Paquetes  
dnf install postfix  
dnf install mailx  
  
# Servicio  
systemctl enable postfix ; systemctl start postfix   
```  
  
2. Configuracion (relay, limite ficheros adjuntos)  
3. `echo "Prueba" | mail -s Prueba Martin.Alen.Freire@kyndryl.com`  
  
```  
echo "Prueba" | mail -s Prueba Martin.Alen.Freire@kyndryl.com  
```  
### Casos  


###### Modificar Return Path  
  
**Return-Path**: La dirección a la que se devolverán los mensajes no entregables (bounces). Esta dirección puede ser diferente de la dirección "From" y se especifica en el encabezado "Return-Path". El "Return-Path" se configura automáticamente cuando el MTA (Mail Transfer Agent) maneja los rebotes de correo.  
  
- Mailx  
  
La opción `-r address` en `mailx` se utiliza para establecer la dirección de correo electrónico que aparecerá en el campo "From" de los correos electrónicos enviados. Esta opción sobrescribe cualquier dirección "From" especificada en las variables de entorno o en los archivos de inicio.  
  
```bash  
# Con mailx usar el parametro -r  
echo "prueba10" |mailx -r "no-reply@orange.es" -s prueba10  [jcherranz@sigifi.com](mailto:jcherranz@sigifi.com "mailto:jcherranz@sigifi.com")  
```  
  
- Sendmail  
  
Para configurar el "Return-Path" en `sendmail`, es común utilizar la opción `-f` para establecer tanto la dirección "From" como la dirección "Return-Path". Sin embargo, para asegurarse de que el "Return-Path" se establece correctamente, puedes usar encabezados adicionales en el correo.  
  
```bash  
# Con sendmail usar el parametro -f  
echo prueba | sendmail -f noreply@orange.es [jcherranz@sigifi.com](mailto:jcherranz@sigifi.com "mailto:jcherranz@sigifi.com")  
```  
  
***  
###### Limite Ficheros Adjuntos  
El valor por defecto es 10 MB (en realidad, 10000 KB)  
  
```bash  
postconf message_size_limit    
message_size_limit = 10240000  
```  
  
1. Añadir la línea a `/etc/postfix/main.cf`   
2. `systemctl reload postfix.service`  
  
***  
###### Un usuario ordinario al lanzar el sednmail -bp le sale este error  
```bash  
/usr/sbin/sendmail -bp  
can not chdir(/var/spool/mqueue/): Permission denied  
Program mode requires special privileges, e.g., root or TrustedUser.  
```  
  
Se debe a que el sendmail requiere de permisos de adminstrador pero en cambio el postfix no.  
  
Para saber que sendmail estamos usando podemos verlo con un ls al sendmail  
  
```bash  
ls -ld /usr/sbin/sendmail  
lrwxrwxrwx 1 root root 21 Jan 31 12:17 /usr/sbin/sendmail -> /etc/alternatives/mta  
  
ls -ld /etc/alternatives/mta  
lrwxrwxrwx 1 root root 26 Apr 18 16:24 /etc/alternatives/mta -> /usr/sbin/sendmail.sendmail  
```  
En este caso esta usando el sendmail si usara el postfix saldria esto otro:  
  
```bash  
ls -ld /etc/alternatives/mta  
lrwxrwxrwx 1 root root 26 Apr 18 16:24 /etc/alternatives/mta -> /usr/sbin/sendmail.postfix  
```  
  
Asi que si queremos dejar postfix como gestor de mail por defecto haremos lo siguiente:  
  
- Parar el sendmail  
```bash  
systemctl stop sendmail  
systemctl disable sendmail  
```  
- Poner el postfix como por defecto  
```bash  
ls -ld /etc/alternatives/mta  
alternatives --install /usr/sbin/mta mta /usr/sbin/sendmail.postfix 10  
```  
*Esto crea una nueva alternativa llamada mta que apunta a /usr/sbin/sendmail.postfix con una prioridad de 10. Debes asegurarte de que /usr/sbin/sendmail.postfix es la ruta correcta del binario de Postfix en tu sistema.  
  
```  
alternatives --set mta /usr/sbin/sendmail.postfix  
```  
- Arrancar el postfix y verificar que apunta el sendmail al de postfix  
```bash  
systemctl enable postfix  
systemctl start postfix  
ls -ld /etc/alternatives/mta  
```  
  
***  
  
###### Llenado /var/spool/clientmqueue  
  
Llenado del /var por correos encolados  
  
*Casos*  
  
- Ver si está parado el sendmail y arrancarlo para que mande los correos encolados y libere espacio del fs(queue). Si llevan mucho tiempo esos ficheros borrar los mas antiguos (30-90 dias) y arrancar servicio sendmail  
  ```bash  
  find /var/spool/clientmqueue/ -type f -mtime +120  
  find /var/spool/clientmqueue/ -type f -mtime +120 -print0 | xargs -0 ls -l | awk '{print $NF}' | xargs rm  
  find /var/spool/clientmqueue/ -type f -mtime +120 -print0 | xargs -0 grep -l 'Subject:' | xargs rm  
  ```  
  
- Eliminar todos los ficheros que tengan como contenido  
> Subject: Cron <root@AOTLXPRBPM00001> /usr/local/bin/LNX-agent/LNX-agent.sh refresh  
  
Redirigir todos los encolados antiguos que tengan esa coincidencia a un fichero `grep -lR "LNX-agent.sh" . > /tmp/ficheroprueba2.txt` y ejecutar su borrado con el `vi (:%s/^/rm -f /)` y luego ejecutar `/tmp/ficheroprueba2.txt` directamente con la instrucción guardada del rm en el vi, también libera espacio.  
  
***  
  
##### Un usuario no puede logar, le da un error "broken pipe"  
  
Mirando el sistema se ven errores de este tipo en el messags  
```bash  
Apr 10 14:12:51 AOTLXPRSBI00002 postfix/local[126232]: A82C6544: to=<root@AOTLXPRSBI00002.localdomain>, relay=local, delay=0.01, delays=0/0/0/0, dsn=5.2.2, status=bounced (cannot update mailbox /var/mail/root for user root. error writing message: File too large)  
```  
Y procesos defunct el usuario siro_p y otro encolados del postfix  
```bash  
siro_p    55966 111514  0 09:52 ?        00:00:00 /usr/sbin/sendmail -FCronDaemon -i -odi -oem -oi -t -f root  
siro_p    56025  55944  0 09:52 ?        00:00:00 /usr/sbin/postdrop -r  
```  
  
Estos errores se deben a que se ha llegado al limite del tamaño del fichero mailbox de dicho usuario.  
  
Si verificamos el limite en el postfix vemos que son 49mb aprox  
```bash  
[root@AOTLXPRSBI00002 siro_p]# postconf -h mailbox_size_limit |awk '{print $1/1024/1024}'  
48.8281  
[root@AOTLXPRSBI00002 siro_p]# du -sm /var/mail/siro_p  
49      /var/mail/siro_p  
```  
  
Asique o podemos ampliar ese limite o vaciar los mailbox, en nuestro caso optamos por esto segundo, ademas se paro el postfix, se mataron todos los procesos de siro_p que estaban pillados y se arranco de nuevo el postfix revisando que todo arranco correctamente  
  
***  
  
```bash  
curl -v telnet://fsp.si.orange.es:587  
curl -v telnet://fsp.si.orange.es:25  
```  
  
***  
**Correos Encolados  
  
```bash  
mailq  
postqueue -p  
```  
  
❌ **Borrar toda la cola de correo**  
```bash  
postsuper -d ALL  
```  
❌ ***Borrar mensajes "deferred"**  
```bash  
postsuper -d ALL deferred  
```  
🔁 **Reintentar enviar todos los correos en cola**  
```bash  
postqueue -f  
```  
  
**Identificar origen correo**  
  
- Listar los IDs de mensajes:  
```bash  
mailq  
```  
- Luego inspeccioná uno con:  
```bash  
postcat -vq <ID>  
```  
Buscar lineas de este tipo  
```text  
Copiar  
Editar  
From: root@tuservidor.local  
Sender: apache@tuservidor.local  
```  
***  
**Error TLS**

```text  
cat /etc/mail/access  
Check the /usr/share/doc/sendmail/README.cf file for a description  
of the format of this file. (search for access_db in that file)  
The /usr/share/doc/sendmail/README.cf is part of the sendmail-doc  
package.

by default we allow relaying from localhost...  
Connect:localhost.localdomain           RELAY  
Connect:localhost                       RELAY  
Connect:127.0.0.1                       RELAY

fsp.si.orange.es             RELAY  
Try_TLS:fsp.si.orange.es     NO   
```