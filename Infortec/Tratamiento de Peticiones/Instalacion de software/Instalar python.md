```
rpm -qa | grep -i python

alternatives --config python

yum info python

cat /etc/redhat-release

export http_proxy=http://127.0.0.1:8800 ; 
export https_proxy=https://127.0.0.1:8800

subscription-manager clean; subscription-manager register --username=atlaslnx --password="Te56flon-nueva;" --auto-attach --force --release=8.10

subscription-manager list

yum info python

yum info python*

python --version

rpm -qa | grep -i pip

alternatives --config python

dnf info python

dnf info python*

dnf info python3.12*

dnf install python3.12

rpm -qa | grep -i python3.12
```

Explicacion de los comandos

### 🔍 1. `rpm -qa | grep -i python`

- **Objetivo:** Listar todos los paquetes instalados relacionados con Python.
    
- **Explicación:**
    
    - `rpm -qa`: lista todos los paquetes instalados.
        
    - `grep -i python`: filtra los que contienen "python", sin importar mayúsculas.
        

---

### 🔧 2. `alternatives --config python`

- **Objetivo:** Cambiar la versión de Python predeterminada del sistema.
    
- **Explicación:**
    
    - Usa el sistema de "alternatives" de Linux para gestionar múltiples versiones del mismo software.
        
    - Este comando muestra las versiones instaladas de Python registradas en `alternatives` y permite elegir cuál usar por defecto.
        
    - ⚠️ Requiere que `alternatives` esté configurado previamente para `python`.
        

---

### 📦 3. `yum info python`

- **Objetivo:** Ver información detallada del paquete `python` desde los repositorios.
    
- **Explicación:** Te dice si está disponible, su versión, descripción, tamaño, etc.
    

---

### 🏷️ 4. `cat /etc/redhat-release`

- **Objetivo:** Mostrar la versión del sistema operativo (RHEL, CentOS, Rocky, AlmaLinux, etc.).
    
- **Explicación:** Lee el archivo donde se guarda el nombre y versión de la distribución basada en Red Hat.
    

---

### 🌐 5. `export http_proxy=http://127.0.0.1:8800 ; export https_proxy=https://127.0.0.1:8800`

- **Objetivo:** Establecer un proxy para que las herramientas del sistema (como `yum`, `dnf`, `curl`, etc.) usen esa dirección para conectarse a internet.
    
- **Explicación:**
    
    - Estás redirigiendo todo el tráfico HTTP y HTTPS a un proxy local en el puerto 8800.
        
    - Útil si estás detrás de un proxy corporativo o haciendo debugging.
        

---

### 📛 6. `subscription-manager clean; subscription-manager register --username=atlaslnx --password="Te56flon-nueva;" --auto-attach --force --release=8.10`

- **Objetivo:** Registrar el sistema RHEL en Red Hat Subscription Manager.
    
- **Explicación:**
    
    - `clean`: limpia cualquier configuración o estado anterior de suscripción.
        
    - `register`: registra el sistema con tu cuenta de Red Hat (`atlaslnx`) y adjunta automáticamente los repos.
        
    - `--release=8.10`: asegura que se usen repositorios compatibles con RHEL 8.10.
        

---

### 📋 7. `subscription-manager list`

- **Objetivo:** Ver el estado actual de las suscripciones registradas.
    
- **Explicación:** Muestra qué suscripciones están activas y asociadas al sistema.
    

---

### 📦 8. `yum info python`

(ya explicado en el paso 3, pero repetido aquí)

---

### 📦 9. `yum info python*`

- **Objetivo:** Ver información de todos los paquetes que comienzan con `python`.
    
- **Explicación:** El asterisco actúa como comodín y muestra todos los paquetes relacionados, como `python3`, `python-libs`, `python-pip`, etc.
    

---

### 🐍 10. `python --version`

- **Objetivo:** Mostrar la versión actual del binario `python`.
    
- **Nota:** En muchas distros modernas, este comando puede fallar si `python` no está vinculado (symlink) a ninguna versión (como `python3` o `python2`).
    

---

### 📦 11. `rpm -qa | grep -i pip`

- **Objetivo:** Ver si `pip` está instalado y en qué paquete RPM viene.
    
- **Explicación:** Similar al comando del paso 1, pero filtrando `pip`.
    

---

### ⚙️ 12. `dnf install python 3.12`

- **Objetivo:** (Probablemente mal escrito)
    
- **Error:** Este comando no es válido así, porque DNF interpretará `3.12` como un segundo paquete, no una versión.
    
- Si lo que querías era instalar `python3.12`, el comando correcto sería:
    
    `dnf install python3.12`
    

---

### 🔧 13. `alternatives --config python`

(Repetido; ya explicado en el paso 2.)

---

### 📦 14. `dnf info python`

- **Objetivo:** Ver detalles del paquete `python` en los repositorios.
    
- Funciona igual que `yum info python`.
    

---

### 📦 15. `dnf info python*`

- **Objetivo:** Ver info de todos los paquetes relacionados con Python.
    
- `dnf` es el reemplazo moderno de `yum` en RHEL 8+.
    

---

### 📦 16. `dnf info python3.12*`

- **Objetivo:** Ver todos los paquetes relacionados con Python 3.12 disponibles en los repositorios.
    
- Útil para ver si están disponibles `python3.12`, `python3.12-libs`, `python3.12-pip`, etc.
    

---

### 📦 17. `dnf install python3.12`

- **Objetivo:** Instalar Python 3.12 desde los repositorios si está disponible.
    
- Si no está en los repos oficiales, puedes agregar un repo adicional o compilarlo.
    

---

### 🔍 18. `rpm -qa | grep -i python3.12`

- **Objetivo:** Ver si tienes instalado Python 3.12 y qué paquetes relacionados hay en el sistema.`