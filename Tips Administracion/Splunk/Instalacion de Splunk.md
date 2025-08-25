## 1. Instalar Splunk en una Máquina Virtual (VM)

- Puedes usar cualquier hypervisor como **VirtualBox**, **VMware** o **Hyper-V**.
    
- Instala una distribución Linux compatible (por ejemplo Ubuntu, Debian o incluso Kali).
    
- Sigue el proceso de instalación normal (descargar paquete `.deb` o `.rpm`, instalar, arrancar Splunk).
    
- Ventajas:
    
    - Entorno aislado.
        
    - Puedes dedicar recursos específicos.
        
    - Simula un entorno real.
        

---

## 2. Instalar Splunk usando Docker

### Ventajas

- Muy rápido para levantar y bajar.
    
- No requiere instalación ni configuración compleja en tu SO.
    
- Ideal para pruebas y desarrollo.
    

### Cómo hacerlo

Si no tienes Docker, instálalo primero:

bash

CopiarEditar

`sudo apt update sudo apt install docker.io sudo systemctl start docker sudo systemctl enable docker`

### Comando para ejecutar Splunk en Docker

bash

CopiarEditar

`docker run -d --name splunk -p 8000:8000 -p 8088:8088 -p 9997:9997 \   -e SPLUNK_START_ARGS="--accept-license" \   -e SPLUNK_PASSWORD=TuPasswordSegura \   splunk/splunk:latest`

- `-p 8000:8000` mapea la interfaz web.
    
- `-p 8088:8088` para HTTP Event Collector.
    
- `-p 9997:9997` para recibir datos de forwarders.
    
- Cambia `TuPasswordSegura` por una contraseña fuerte.
    

### Verifica que el contenedor esté corriendo

bash

CopiarEditar

`docker ps`

### Accede a Splunk desde tu navegador

arduino

CopiarEditar

`http://localhost:8000`