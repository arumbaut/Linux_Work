[Resumen: LINUX - EPSILONmartes, 26 de agosto | Reunión | Microsoft Teams](https://teams.microsoft.com/l/meetingrecap?driveId=b%21Xc5Qqwa8mEe52XQogNr9vJ7oUh7DF9tCoIFkhQxVd8vUYcPhh6xmTq0bueWk8lFJ&driveItemId=014ZKHOFSVLTAOOBEGIBHKJBGHCONER7S7&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEVVcwOcEhkBOpITHE5pI_l8B3CAhNGqovjOUegj36BmiNQ&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEVVcwOcEhkBOpITHE5pI_l8B3CAhNGqovjOUegj36BmiNQ&threadId=19%3Af9a5f99fca6d4d9f99525c322ddf9904%40thread.v2&organizerId=b72f1f5b-629b-4edc-8de1-641eac7af42b&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=6423ba6e-d12a-47b5-86a6-fb8af226a2b7&threadType=GroupChat&meetingType=Adhoc&subType=RecapSharingLink_RecapChiclet "https://teams.microsoft.com/l/meetingrecap?driveId=b%21Xc5Qqwa8mEe52XQogNr9vJ7oUh7DF9tCoIFkhQxVd8vUYcPhh6xmTq0bueWk8lFJ&driveItemId=014ZKHOFSVLTAOOBEGIBHKJBGHCONER7S7&sitePath=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEVVcwOcEhkBOpITHE5pI_l8B3CAhNGqovjOUegj36BmiNQ&fileUrl=https%3A%2F%2Fkyndrylde-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fadrian_alonsorumbaut_kyndryl_com%2FEVVcwOcEhkBOpITHE5pI_l8B3CAhNGqovjOUegj36BmiNQ&threadId=19%3Af9a5f99fca6d4d9f99525c322ddf9904%40thread.v2&organizerId=b72f1f5b-629b-4edc-8de1-641eac7af42b&tenantId=f260df36-bc43-424c-8f44-c85226657b01&callId=6423ba6e-d12a-47b5-86a6-fb8af226a2b7&threadType=GroupChat&meetingType=Adhoc&subType=RecapSharingLink_RecapChiclet")


- **Instalación de certificados en Linux (Red Hat 7)**
    
    - Los certificados de **autoridades certificadoras (CA)** se instalan en el almacén del sistema operativo (directorio `/etc/pki/...`) mediante el paquete `ca-certificates`.
        
    - El sistema gestiona únicamente certificados raíz (auto-firmados por la entidad).
        
    - Los certificados emitidos para usuarios/servicios específicos no corresponden al sistema operativo, sino a la **aplicación o usuario solicitante**.
        
- **Límites de responsabilidad**
    
    - El equipo de sistemas administra solo la **configuración del sistema operativo**, no aplicaciones ni procesos de usuario.
        
    - Peticiones de configuración que afecten a aplicaciones o directorios fuera del ámbito del sistema deben ser resueltas por los responsables de la aplicación.
        
- **Gestión de solicitudes del cliente**
    
    - Antes de aceptar cambios a nivel de sistema, se debe verificar:
        
        - **Tipo de certificado** (raíz de CA o de usuario).
            
        - **Alcance del cambio** (afecta a todos los usuarios o solo a uno).
            
    - Modificaciones globales pueden impactar procesos de administración, monitorización y otros servicios, por lo que deben evitarse salvo que estén plenamente justificadas.
        
- **Alternativas para el cliente**
    
    - Herramientas como `wget` y `curl` permiten **parametrizar certificados por línea de comando** o en archivos de configuración dentro del **home del usuario** (ej. `.wgetrc`), evitando cambios en todo el sistema.
        
    - Esto hace que el **usuario sea autónomo** y no dependa de sistemas para realizar la configuración.
        
- **Comunicación con el cliente**
    
    - Evitar respuestas negativas absolutas como “no se puede” o “imposible”.
        
    - Usar un lenguaje positivo: _“vuestro usuario es autónomo para realizar la configuración”_.
        
    - Ofrecer apoyo en caso de errores **relacionados con el sistema operativo**, pero aclarar que la gestión de certificados de aplicación es responsabilidad del usuario.
        
    - Si el cliente insiste en que debe hacerse a nivel de sistema, solicitar documentación oficial de la aplicación que lo exija.

Respuesta formal a este tipo de peticiones despues del previo analisis.
Hemos revisado vuestra solicitud sobre la instalación del certificado. Os confirmamos que las utilidades como **wget** y **curl** permiten parametrizar certificados directamente desde línea de comandos o a través de un fichero de configuración en vuestro **home de usuario** (ejemplo: `.wgetrc`). Esto os da autonomía para realizar la configuración sin necesidad de aplicar cambios a nivel de sistema operativo, evitando así posibles impactos sobre otros procesos y usuarios de la plataforma.

En caso de que, tras configurar la opción en vuestros scripts o directorios de usuario, detectéis algún error **relacionado con el sistema operativo**, podéis reasignarnos la petición para que lo revisemos.

Si vuestra aplicación requiere de forma explícita la instalación del certificado en el sistema operativo, os agradeceríamos que nos facilitéis la documentación oficial donde se indique este requisito, de manera que podamos analizarlo conjuntamente y proceder de la forma más adecuada.

Quedamos atentos a vuestros comentarios.