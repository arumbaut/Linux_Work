# 🔎 Plan de Diagnóstico de Red en Linux

## 1. Verificar la interfaz de red

Primero confirmo si la interfaz está **arriba y con IP**.
```
ip addr show 
ip link show
```

👉 Si veo que la interfaz está `DOWN`, la levanto:

```
ip link set dev eth0 up
```

---

## 2. Comprobar conectividad local

- Revisar la configuración IP y gateway:
    

```
ip route show
```
- Ver si llego a mi **gateway**:
    

```
ping -c 4 <gateway>
```

---

## 3. Verificar resolución DNS

Muchas veces el problema parece de red, pero es DNS.

```
dig www.google.com cat /etc/resolv.conf
```

👉 Si por IP funciona (`ping 8.8.8.8`) pero por nombre no, el problema es DNS.

---

## 4. Revisar conectividad externa

- Ping a una IP pública:
    

`ping -c 4 8.8.8.8`

- Test de traceroute para ver dónde se corta:
    

`traceroute 8.8.8.8`

---

## 5. Medir velocidad / latencia

- Ver latencias y pérdida de paquetes:
    

```
mtr -rw 8.8.8.8
```

- Comprobar ancho de banda (si está disponible):
    

`iperf3 -c <servidor_iperf>`

---

## 6. Revisar el estado de la interfaz

- Velocidad y duplex:
    

```
ethtool eth0
```

- Estadísticas de errores/drops:
    

```
ethtool -S eth0 | egrep "err|drop"
```

- Paquetes en la interfaz:
    

```
netstat -i
```

---

## 7. Revisar uso de red en el servidor

- Procesos que consumen red:
    

```
ss -tulpn
```

- Flujo de tráfico:
    

`iftop -i eth0 nload eth0`

---

## 8. Logs del sistema

Errores en logs del kernel o de red:

`dmesg | egrep -i 'eth|enp|network' journalctl -u NetworkManager --since -10m`

---

## 9. Acciones posibles según hallazgo

- Si la interfaz está caída → levantarla o reiniciar servicio:
    
    `systemctl restart NetworkManager`
    
- Si DNS falla → revisar `/etc/resolv.conf` o configuración de systemd-resolved.
    
- Si la velocidad está limitada (ej: 100Mb en vez de 1Gb) → ajustar con `ethtool`:
    
    `ethtool -s eth0 speed 1000 duplex full autoneg on`
    
- Si hay saturación → identificar procesos con `iftop` y limitar o detenerlos.
    

---

✅ **Resumen**:

1. Confirmo que la interfaz esté activa (`ip addr`, `ethtool`).
    
2. Verifico gateway y DNS (`ping`, `dig`).
    
3. Hago pruebas de latencia (`mtr`, `traceroute`).
    
4. Reviso logs y consumo de red (`ss`, `iftop`).
    
5. Corrijo según hallazgo (levantar interfaz, reiniciar servicio, ajustar velocidad, arreglar DNS, cortar proceso que satura).