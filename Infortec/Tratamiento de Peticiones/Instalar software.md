En las maquinas de RedHat primero debemos hacer el proceso de registro , en las de susets no es necesario 

```
subscription-manager register 

subscription-manager remove --all 

subscription-manager unregister


#Para registrar nos pedira un user y pass

user atlaslnx
pass Te56flon-nueva;
```

Despues de registrar instalamos los paquetes con 
```
yum install pack
```