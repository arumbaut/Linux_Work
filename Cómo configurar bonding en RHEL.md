
## 1\. Instala herramientas necesarias

Por lo general, `ifenslave` viene incluido, pero puedes asegurarte con:

---

## 2\. Carga el módulo bonding

Para cargarlo automáticamente al iniciar el sistema:

---

## 3\. Crea y configura las interfaces de red

### Paso A: Crea archivo para la interfaz bond

Crea `/etc/sysconfig/network-scripts/ifcfg-bond0` con el contenido:

> - `mode=active-backup` es un modo común (failover). Cambia según tus necesidades.
> - `miimon=100` chequea el estado del enlace cada 100 ms.

---

### Paso B: Configura las interfaces esclavas

Edita o crea `/etc/sysconfig/network-scripts/ifcfg-eth0`:

Haz lo mismo para `/etc/sysconfig/network-scripts/ifcfg-eth1`:

---

## 4\. Reinicia el servicio de red

---

## 5\. Verifica el estado del bonding

Deberías ver las interfaces esclavas, el modo configurado, estado de enlaces, etc.

---

## 6\. Configuración adicional (opcional)

- Para cambiar el modo de bonding, modifica la opción `mode` en `BONDING_OPTS` en `ifcfg-bond0`.
- Otros modos comunes:
	- `balance-rr` (round-robin)
	- `balance-xor`
	- `broadcast`
	- `802.3ad` (LACP, requiere switch compatible)
	- `balance-tlb`
	- `balance-alb`

---

## Notas importantes

- Asegúrate que tu switch soporte el modo elegido, especialmente para `802.3ad` (LACP).
- La configuración puede variar ligeramente según la versión exacta de RHEL o CentOS.
- Siempre haz una copia de seguridad de las configuraciones de red antes de modificar.

---

¿Quieres que te prepare un script o archivo de configuración listo para tu versión específica? ¿O ayuda para configurar bonding con LACP?