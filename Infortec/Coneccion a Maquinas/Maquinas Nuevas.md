[Resumen: REUNION DE GRUPO LINUX/UNIXlunes, 15 de septiembre | Reunión | Microsoft Teams](https://teams.microsoft.com/l/meetingrecap?driveId=b%21Tt3zYi3UX0O3etiIW0zZiKlnJg7EyqdNlBgQjrvy5sMaECfGNIVfTK6BX1NxttpC&driveItemId=01XABK53FAXJMSS6BI3ZEYIVVXW6JJFE6L&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&iCalUid=040000008200E00074C5B7101A82E00807E9090F0B2AA14AEABAD901000000000000000010000000D0914DE75CF3E341B2AE2BE844380C3A&masterICalUid=040000008200e00074c5b7101a82e008000000000b2aa14aeabad901000000000000000010000000d0914de75cf3e341b2ae2be844380c3a&threadId=19%3Ameeting_OTFjZDY0NDItMDEwOC00NDk0LThhNjgtZTE0ZDYzOGYzYTJk%40thread.v2&organizerId=5eceda44-576f-401d-bfc5-0aa0bda2a10a&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=1006338f-86a8-4542-b285-5f9cd7013a20&threadType=Meeting&meetingType=Recurring&subType=RecapSharingLink_RecapChiclet "https://teams.microsoft.com/l/meetingrecap?driveId=b%21Tt3zYi3UX0O3etiIW0zZiKlnJg7EyqdNlBgQjrvy5sMaECfGNIVfTK6BX1NxttpC&driveItemId=01XABK53FAXJMSS6BI3ZEYIVVXW6JJFE6L&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&iCalUid=040000008200E00074C5B7101A82E00807E9090F0B2AA14AEABAD901000000000000000010000000D0914DE75CF3E341B2AE2BE844380C3A&masterICalUid=040000008200e00074c5b7101a82e008000000000b2aa14aeabad901000000000000000010000000d0914de75cf3e341b2ae2be844380c3a&threadId=19%3Ameeting_OTFjZDY0NDItMDEwOC00NDk0LThhNjgtZTE0ZDYzOGYzYTJk%40thread.v2&organizerId=5eceda44-576f-401d-bfc5-0aa0bda2a10a&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=1006338f-86a8-4542-b285-5f9cd7013a20&threadType=Meeting&meetingType=Recurring&subType=RecapSharingLink_RecapChiclet")


du -mx --max-depth=1 /home |sort -n | tail

Conexion ssh a la de salto 

creo 
config.atlas

Configuracion de nuestro archivo de conexion con la rsa. Archivo config.atlas
```
Host atlas
    HostName 10.113.46.179
    User es48389e
    Port 22
    IdentityFile "C:\Users\Adrian Alonso\.ssh\id_rsa"
    UserKnownHostsFile NUL
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

Para verificar que se abrio la conexion

```
Get-Process ssh

netstat -ano | findstr 2080
```

Comando de la conexion desde PowerShell
```
ssh -F .\config.atlas -D 2080 atlas -Nfq
```

Para cerrar si lo ejecutamos sin la opcion -f es solo hacer CTRL+C en la ventana donde ejecutamos el comando de lo contrario debemos matar el proceso de  la siguiente manera

```
PS C:\Users\Adrian Alonso>  netstat -ano | findstr 2080
  TCP    127.0.0.1:2080         0.0.0.0:0              LISTENING       24720
  TCP    [::1]:2080             [::]:0                 LISTENING       24720

PS C:\Users\Adrian Alonso> Stop-Process -Id 24720
PS C:\Users\Adrian Alonso>
```


```
Host mi-servidor
    HostName 10.113.46.179
    User adrian
    Port 2222
```

```
ssh -F /dir/config.atlas -fNq -D 2080 -o ControlMaster=no 10.113.46.179 
```