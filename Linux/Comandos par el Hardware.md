### **Comandos básicos para ver el hardware físico:**

---

#### 🔹 1. **CPU:**

`lscpu`

---

#### 🔹 2. **Memoria RAM:**

`free -h`

o más detallado:

`sudo dmidecode --type memory`

---

#### 🔹 3. **Disco(s):**

`lsblk`

o con detalles:

`sudo fdisk -l`

---

#### 🔹 4. **Tarjeta gráfica:**

`lspci | grep VGA`

---

#### 🔹 5. **Información general del hardware:**

`sudo lshw -short`

> Si `lshw` no está instalado:

`sudo apt install lshw`

---

#### 🔹 6. **Información del sistema completo:**

`inxi -Fxz`

> Si no lo tenés:


`sudo apt install inxi`