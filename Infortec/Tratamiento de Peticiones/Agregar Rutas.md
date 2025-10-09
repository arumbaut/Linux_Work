

Comando para saber el camino que coje una ip por la interface que sale y tal 
```
ip route get ip
[root@AOTLXPPPBD10006 network-scripts]# ip route get 10.132.6.186
10.132.6.186 via 10.132.56.1 dev eth5.362 src 10.132.56.144
    cache
```

Para rutas temporales
```
ip route add <RED>/<MÁSCARA> via <GATEWAY> dev <INTERFAZ>

Ejemplo

ip route add 192.168.50.0/24 via 192.168.1.1 dev eth0

```

### Formas de hacerlo persistente

#### 🔸 Opción 1: Usando `nmcli` (si se usa NetworkManager)

```
nmcli connection modify "System eth0" +ipv4.routes "192.168.50.0/24 192.168.1.1"

nmcli connection modify eth5.362 +ipv4.routes "10.132.6.186/23 10.132.56.1"
 nmcli connection up "System eth0"
```

Esto añade la ruta al perfil de red gestionado por NetworkManager.

Nota Importante
Hay **dos formas** de modificar rutas en una conexión de NetworkManager:

| Sintaxis       | Qué hace                                                  |
| -------------- | --------------------------------------------------------- |
| `ipv4.routes`  | **Reemplaza** completamente la lista de rutas actuales. ❌ |
| `+ipv4.routes` | **Agrega** una nueva ruta sin borrar las existentes. ✅    |
| `-ipv4.routes` | **Elimina** una ruta específica de la lista. 🧹           |

#### 🔸 Opción 2: Archivo de rutas de interfaz

1. Ir al directorio de configuración de red:
    
    `/etc/sysconfig/network-scripts/`
    
2. Crear o editar el archivo:
    
    `route-<nombre_interfaz>`
    
    (por ejemplo `route-eth0`)
    
3. Agregar líneas con el formato:
    
    `192.168.50.0/24 via 192.168.1.1 dev eth0`
    
    o usando el formato clásico de Red Hat:
    
    `ADDRESS0=192.168.50.0 NETMASK0=255.255.255.0 GATEWAY0=192.168.1.1`
    
4. Reiniciar el servicio de red:
    
    `systemctl restart network`

Esa ruta se cargará **automáticamente en cada arranque**.




