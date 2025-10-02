1. **Verificar expiración de contraseña o cuenta**

```
chage -l <usuario>
```

- **Password expires** → fecha en que caduca la contraseña.
    
- **Account expires** → fecha en que la cuenta se desactiva.  
    ✅ Si todo aparece como _never_ o dentro de plazo → no es un problema de expiración.


2. **Verificar si la cuenta está bloqueada administrativamente**
```
passwd -S <usuario>

```

PS → contraseña activa (Password Set).

L → cuenta bloqueada (Locked).
🔧 Desbloquear en caso necesario:

```
passwd -u <usuario>
```

### 3. **Verificar bloqueos por intentos fallidos (faillock o pam_tally2)**

#### En sistemas con `faillock` (RHEL8+):

```
faillock --user <usuario>
```

Muestra intentos fallidos.  
🔧 Resetear contador:
```
faillock --user <usuario> --reset
```

En sistemas con `pam_tally2` (RHEL7):
```
pam_tally2 --user <usuario>
```

Si muestra _Failures > 0_, el usuario está bloqueado.

🔧 Resetear contador:
```
pam_tally2 --user <usuario> --reset
```

Si no se limpia, especificar el servicio:

```
pam_tally2 --user <usuario> --reset --service=su 
pam_tally2 --user <usuario> --reset --service=sshd 
pam_tally2 --user <usuario> --reset --service=login
```

### 4. **Verificar en /etc/shadow si la cuenta está bloqueada**

```
grep <usuario> /etc/shadow
```

- Si la entrada comienza con `!` o `!!` → cuenta bloqueada.  
    🔧 Desbloquear con:
    

```
passwd -u <usuario>
```

### 5. **Probar login después del desbloqueo**

```
su - <usuario>
```