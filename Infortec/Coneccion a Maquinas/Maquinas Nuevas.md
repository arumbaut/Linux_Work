[Resumen: REUNION DE GRUPO LINUX/UNIXlunes, 15 de septiembre | Reunión | Microsoft Teams](https://teams.microsoft.com/l/meetingrecap?driveId=b%21Tt3zYi3UX0O3etiIW0zZiKlnJg7EyqdNlBgQjrvy5sMaECfGNIVfTK6BX1NxttpC&driveItemId=01XABK53FAXJMSS6BI3ZEYIVVXW6JJFE6L&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&iCalUid=040000008200E00074C5B7101A82E00807E9090F0B2AA14AEABAD901000000000000000010000000D0914DE75CF3E341B2AE2BE844380C3A&masterICalUid=040000008200e00074c5b7101a82e008000000000b2aa14aeabad901000000000000000010000000d0914de75cf3e341b2ae2be844380c3a&threadId=19%3Ameeting_OTFjZDY0NDItMDEwOC00NDk0LThhNjgtZTE0ZDYzOGYzYTJk%40thread.v2&organizerId=5eceda44-576f-401d-bfc5-0aa0bda2a10a&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=1006338f-86a8-4542-b285-5f9cd7013a20&threadType=Meeting&meetingType=Recurring&subType=RecapSharingLink_RecapChiclet "https://teams.microsoft.com/l/meetingrecap?driveId=b%21Tt3zYi3UX0O3etiIW0zZiKlnJg7EyqdNlBgQjrvy5sMaECfGNIVfTK6BX1NxttpC&driveItemId=01XABK53FAXJMSS6BI3ZEYIVVXW6JJFE6L&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fjuan_carlos_herranz_kyndryl_com%2FEaC6WSl4KN5JhFa3t5KSk8sB2_NhrSFZATa4aagpwSa9dg&iCalUid=040000008200E00074C5B7101A82E00807E9090F0B2AA14AEABAD901000000000000000010000000D0914DE75CF3E341B2AE2BE844380C3A&masterICalUid=040000008200e00074c5b7101a82e008000000000b2aa14aeabad901000000000000000010000000d0914de75cf3e341b2ae2be844380c3a&threadId=19%3Ameeting_OTFjZDY0NDItMDEwOC00NDk0LThhNjgtZTE0ZDYzOGYzYTJk%40thread.v2&organizerId=5eceda44-576f-401d-bfc5-0aa0bda2a10a&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=1006338f-86a8-4542-b285-5f9cd7013a20&threadType=Meeting&meetingType=Recurring&subType=RecapSharingLink_RecapChiclet")


du -mx --max-depth=1 /home |sort -n | tail

Conexion ssh a la de salto 

creo 
config.atlas

```
Host mi-servidor
    HostName 10.113.46.179
    User adrian
    Port 2222
```

```
ssh -F /dir/config.atlas -fNq -D 2080 -o ControlMaster=no 10.113.46.179 
```