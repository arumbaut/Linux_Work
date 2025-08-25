## ✅ 1. **Crear el usuario normalmente**

Primero, crea el usuario:

`sudo adduser nombre_usuario`

Esto crea el usuario, su carpeta `/home/nombre_usuario` y solicita contraseña.

---

## ✅ 2. **Dar permisos elevados (limitados) con sudo**

### Opción A – Agregar al grupo `sudo` (acceso completo con sudo)


`sudo usermod -aG sudo nombre_usuario`

Pero **esto le da acceso completo como root** con `sudo`, lo que quizás no quieres.

---

### Opción B – Crear una política de `sudo` personalizada (lo recomendado en tu caso)

Edita el archivo de sudoers de forma segura con:

`sudo visudo`

Y añade al final una línea como esta:

`nombre_usuario ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl`

Esto le da permiso para ejecutar **solo los comandos `apt` y `systemctl`** con `sudo`.

📌 Puedes agregar los comandos que quieras permitir, por ejemplo:

bash

CopyEdit

`nombre_usuario ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart apache2`

Esto le permite reiniciar Apache sin pedir contraseña.

---

## ✅ 3. (Opcional) Denegar acceso a comandos peligrosos

Puedes ir más allá y usar herramientas como:

- **`rbac`** (control de acceso basado en roles)
    
- **`sudoers.d/`** para múltiples reglas organizadas
    
- **crear alias o shells limitados**
    

Pero eso se usa más en entornos empresariales.

---
## ✅ 4. Verificar acceso

Cambia al usuario y prueba:

`su - nombre_usuario sudo apt update         # ✅ Debería funcionar si se permitió apt sudo reboot             # ❌ Debería fallar si no está en sudoers`



La línea `ALL=(ALL:ALL) ALL` es una regla típica en el archivo `sudoers`, y se lee como una **regla de permisos sudo**. Vamos a desglosarla paso a paso:

---
## 📘 Formato general de una línea en `sudoers`:

`usuario  host = (usuario_objetivo:grupo_objetivo) comando`

---

## 🧠 Entonces, ¿cómo se interpreta esto?

bash

CopyEdit

`ALL=(ALL:ALL) ALL`

Supongamos que está asociada a un usuario como este ejemplo completo:

bash

CopyEdit

`alexandross ALL=(ALL:ALL) ALL`

### 🔍 Significado de cada parte:

|Parte|Significado|
|---|---|
|`alexandross`|Usuario al que se aplica la regla|
|`ALL`|Puede usar sudo **en cualquier host** (usado en entornos distribuidos o LDAP)|
|`(ALL:ALL)`|Puede actuar **como cualquier usuario** y **cualquier grupo**|
|`ALL`|Puede ejecutar **cualquier comando** con `sudo`|