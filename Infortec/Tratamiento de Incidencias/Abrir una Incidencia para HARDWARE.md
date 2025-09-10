[Resumen: Llamada con MARIO SANCHEZ CANCELA y 1 persona másmartes, 9 de septiembre | Reunión | Microsoft Teams](https://teams.microsoft.com/l/meetingrecap?driveId=b%21Xc5Qqwa8mEe52XQogNr9vJ7oUh7DF9tCoIFkhQxVd8vUYcPhh6xmTq0bueWk8lFJ&driveItemId=014ZKHOFV2F36HM5R7D5GZLT4TH2P6RWJU&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEbou_HZ2Px9Nlc-TPp_o2TQB2cgTWAfriSWrIHa6ovdtrw&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEbou_HZ2Px9Nlc-TPp_o2TQB2cgTWAfriSWrIHa6ovdtrw&threadId=19%3A0cc5180b239f43029459a4efd7d6bfca%40thread.v2&organizerId=9b2a6a84-471d-470e-bf5e-6b50b6c83149&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=c57b0f3b-b87c-4d9b-b471-7103b9597945&threadType=GroupChat&meetingType=Unknown&subType=RecapSharingLink_RecapChiclet "https://teams.microsoft.com/l/meetingrecap?driveId=b%21Xc5Qqwa8mEe52XQogNr9vJ7oUh7DF9tCoIFkhQxVd8vUYcPhh6xmTq0bueWk8lFJ&driveItemId=014ZKHOFV2F36HM5R7D5GZLT4TH2P6RWJU&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEbou_HZ2Px9Nlc-TPp_o2TQB2cgTWAfriSWrIHa6ovdtrw&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEbou_HZ2Px9Nlc-TPp_o2TQB2cgTWAfriSWrIHa6ovdtrw&threadId=19%3A0cc5180b239f43029459a4efd7d6bfca%40thread.v2&organizerId=9b2a6a84-471d-470e-bf5e-6b50b6c83149&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=c57b0f3b-b87c-4d9b-b471-7103b9597945&threadType=GroupChat&meetingType=Unknown&subType=RecapSharingLink_RecapChiclet")


Texto para la incidencia 

```
Desde el depto LINUX-VMWARE se solicita revision Hardware de la maquina AOTLXPRPBD10087  
  
(info de la web de victor)  
Serial - S4AJS957  
Hw - LENOVO ThinkSystem SR650  
Hw IBM - 7X06CTO1WW  
Rack - 7X06CTO1WW  
  
(Resultado del comando >dmidecode -t1/ o si es ESX: >esxcli hardware platform get )  
Manufacturer: Lenovo  
Product Name: ThinkSystem SR650 -[7X06CTO1WW]-  
Version: 07  
UUID: 115216a8-924a-11e8-8bc9-7ed30a5e01c7  
Wake-up Type: Power Switch  
SKU Number: 7X06CTO1WW  
Family: ThinkSystem  
  
  
Evento RSA:  
  Memory  FQXSFMA0026I  DIMM 6 Self-healing, attempt post package repair (PPR) at Rank 1 Sub Rank 0 Bank 12 Row 181FA on Device 3. 2CB039A7-VC10-FRU 01DE974  January 7, 2025 12:46:16 PM  
  
Quedamos a la espera del link para subir el service data
```

Relleno de la Inci

```
Entorno Productivo
Urgencia  Baja
Disponibilidad Degradado 

Categorizacion Operacional
Nivel 1  Infraestructura

Nivel 2  HWSAN

Nivel 3   ERROR HW-SERVER

Categorizacion de Producto
Nivel 1    Infraestructura Tecnologica

Nivel 2    COMPONETE HW

Nivel 3    COMPONETE HW
```

Para cargar el fichero de service dato cuando nos lo requieran
![[Pasted image 20250910110923.png]]