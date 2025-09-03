Rvisar el servicio del sevidor local 

```
 service postfix status
```

Ver las configuraciones del servidor de con respecto a quien le entrega
```
grep -i "relayhost" /etc/postfix/main.cf

#Mas especifico para ver cual es al que esta activo no comentado
grep -i "relayhost" /etc/postfix/main.cf | grep -iv "#"
```


Ver los logs que esta lanzando el servidor
```
tail -f /var/log/maillog
```



Apuntes de Cortes a Revisar 

Enviar con sendmail  
```  
echo -e "Subject: Test Email\nThis is a test email." | sendmail -f jose.antonio.cortes.fraga@kyndryl.com [jose.antonio.cortes.fraga@kyndryl.com](mailto:jose.antonio.cortes.fraga@kyndryl.com)  
```  
al añadir el -f cambiamos el return path e indicamos el que queramos

Forma de configurar con sender y return path:  
```  
  (  
echo "From: no-reply.nombredeaplicacion@es.orange.com"  
echo "Sender: no-reply.nombredeaplicacion@es.orange.com"  
echo "To: jose.antonio.cortes.fraga@kyndryl.com"  
echo "Subject: Prueba66666"  
echo "holii"  
echo "Prueba"  
) | sendmail -t -f no-reply.nombredeaplicacion@es.orange.com

```

Enviar con mail

cambiar el sender con mail:    -r  
```  
echo "Prueba" | mail -r no-reply.nombredeaplicacion@es.orange.com -s Prueba987 jose.antonio.cortes.fraga@kyndryl.com

```  
  
  

# Mail

que sea sendmail o postfix da igual, se envian igual, puedes usar mail -s o sendmail aunque este postfix configurado.

[root@AOTLXPRTHA00002 ~]# systemctl list-unit-files | grep enabled | grep sendmail

[root@AOTLXPRTHA00002 ~]# systemctl list-unit-files | grep enabled | grep postfix

postfix.service                               enabled 

    

Prueba de envio sencilla:

echo prueba >/home/esy9ftu3/prueba.txt

mailx -s "prueba correo" -a /home/esy9ftu3/prueba.txt Jose.antonio.cortes.fraga@kyndryl.com < /home/esy9ftu3/prueba.txt

—------------------------------------------------------------------------------------------------------------

Mas completa:

mailx -s "prueba correo" -a /home/esy9ftu3/prueba.txt Jose.antonio.cortes.fraga@kyndryl.com < /home/esy9ftu3/cuerpo_correo.txt

y para finalizar un .

Otra forma:

[root@AOTLXPRSCO10001 ~]# mail -s "Esto es una prueba" jose.antonio.cortes.fraga@kyndryl.com

PRUEBA

.

el asunto con el correo

luego escribes el cuerpo del correo

le das a intro

escribes un "."

y le das a intro y ya se envia

Para comprobar si se queda encolado usamos mailq

Si no funcionara comando mail instalamos:

mirar var log mailog.log para ver si salen correos

[root@AOTLXPRPMV00004 ~]# tail /var/log/maillog

Aug 26 09:44:33 AOTLXPRPMV00004 postfix/qmgr[995657]: 6150B20E69: removed

Aug 26 09:44:33 AOTLXPRPMV00004 postfix/local[995661]: 61E3720E87: to=<root@AOTLXPRPMV00004.localdomain>, orig_to=<root>, relay=local, delay=23012, delays=23012/0.01/0/0, dsn=2.0.0, status=sent (delivered to mailbox)

Aug 26 09:44:33 AOTLXPRPMV00004 postfix/qmgr[995657]: 61E3720E87: removed

Aug 26 09:44:34 AOTLXPRPMV00004 postfix/local[995660]: 6199220E47: to=<cdevdisc@AOTLXPRPMV00004.localdomain>, orig_to=<cdevdisc>, relay=local, delay=8073, delays=8072/0.01/0/1.1, dsn=2.0.0, status=sent (delivered to mailbox)

Aug 26 09:44:34 AOTLXPRPMV00004 postfix/qmgr[995657]: 6199220E47: removed

Aug 26 09:53:11 AOTLXPRPMV00004 postfix/pickup[995656]: C900B20E47: uid=0 from=<root>

Aug 26 09:53:11 AOTLXPRPMV00004 postfix/cleanup[1008536]: C900B20E47: message-id=<20240826075311.C900B20E47@AOTLXPRPMV00004.localdomain>

Aug 26 09:53:11 AOTLXPRPMV00004 postfix/qmgr[995657]: C900B20E47: from=<root@AOTLXPRPMV00004.localdomain>, size=1047, nrcpt=1 (queue active)

Aug 26 09:53:11 AOTLXPRPMV00004 postfix/smtp[1008538]: C900B20E47: to=<Jose.antonio.cortes.fraga@kyndryl.com>, relay=fsp.si.orange.es[10.113.68.52]:25, delay=0.17, delays=0.02/0.03/0.02/0.1, dsn=2.6.0, status=sent (250 2.6.0 <20240826075311.C900B20E47@AOTLXPRPMV00004.localdomain> [InternalId=188729452923887, Hostname=MYEC85FSP104P.cosmos.es.ftgroup] 1961 bytes in 0.102, 18,744 KB/sec Queued mail for delivery)

Aug 26 09:53:11 AOTLXPRPMV00004 postfix/qmgr[995657]: C900B20E47: removed

[root@AOTLXPRPMV00004 ~]# 

    

relay

[root@AOTLXPRMAR10001 ~]# grep -iR ftgroup /etc/mail/

/etc/mail/sendmail.cf:DSrelay.cosmos.es.ftgroup

yum install mailx

Enviar correos con sendmail:

/usr/sbin/sendmail [jcortes@sigifi.com](mailto:jcortes@sigifi.com)

server]$ /usr/sbin/sendmail youremail@example.com

Subject: Test Send Mail

Hello World

control d (this key combination of control key and d will finish the email.)

# clientmqueue

reiniciar sendamil

**