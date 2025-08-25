find . -type f \( -name "*.ova" -o -name "*.vmdk" \)

## 🔍 **1. Buscar archivos por nombre**

`find /ruta -name "archivo.txt"`

> Búsqueda sensible a mayúsculas/minúsculas.

---

## 🔍 **2. Buscar archivos ignorando mayúsculas**

bash

CopiarEditar

`find /ruta -iname "archivo.txt"`

---

## 🗑️ **3. Buscar y eliminar archivos**

`find /ruta -name "*.log" -type f -delete`

> Elimina todos los `.log`.

---

## 🕑 **4. Buscar archivos modificados en los últimos 7 días**


`find /ruta -type f -mtime -7`

---

## 🕑 **5. Buscar archivos no modificados en 30 días**

`find /ruta -type f -mtime +30`

---

## 📁 **6. Buscar directorios vacíos**


`find /ruta -type d -empty`

---

## 🗃️ **7. Buscar archivos vacíos**


`find /ruta -type f -empty`

---

## 🔥 **8. Buscar y eliminar archivos vacíos**

`find /ruta -type f -empty -delete`

---

## 🧹 **9. Buscar archivos temporales y borrarlos**


`find /tmp -type f -name "*.tmp" -delete`

---

## 📦 **10. Buscar archivos mayores a 500 MB**

`find /ruta -type f -size +500M`

---

## 🔑 **11. Buscar archivos con permisos peligrosos (777)**


`find /ruta -type f -perm 0777`

---

## 🛡️ **12. Buscar archivos sin dueño**

`find /ruta -type f -nouser`

---

## 🛡️ **13. Buscar archivos sin grupo**


`find /ruta -type f -nogroup`

---

## 🧨 **14. Buscar archivos ejecutables**


`find /ruta -type f -executable`

---

## 🧰 **15. Buscar por propietario**


`find /ruta -type f -user nombre_usuario`

---

## 🧰 **16. Buscar por grupo**


`find /ruta -type f -group nombre_grupo`

---

## 🧱 **17. Buscar enlaces simbólicos rotos**


`find /ruta -xtype l`

---

## 🔧 **18. Buscar archivos modificados en las últimas X horas**


`find /ruta -type f -mmin -60`

> Archivos modificados en la última hora.

---

## 🔄 **19. Buscar y ejecutar un comando (por ejemplo: cambiar permisos)**


`find /ruta -type f -name "*.sh" -exec chmod +x {} \;`

---

## 📦 **20. Buscar los 10 archivos más grandes**


`find /ruta -type f -exec du -h {} + | sort -hr | head -n 10`