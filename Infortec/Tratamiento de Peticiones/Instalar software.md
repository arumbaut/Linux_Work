Es importante revisar que entornos tinene los clientes y al momento de instalar paquetes es recomendable que las pruebas se realicen en entornos de test. Conel siguiente comando podemos verificar que tengan el entorno disponible para test.

```
rvtquery --machine AOTLX%SAP%
```

En las maquinas de RedHat primero debemos hacer el proceso de registro , en las de susets no es necesario 

```
###Comando a utilizar.###
subscription-manager clean; subscription-manager register --username=atlaslnx --password="Te56flon-nueva;" --auto-attach --force --release=8.10

Para desregistrar
subscription-manager remove --all 

subscription-manager unregister


```

Despues de registrar revisamos los si estan los paquetes en el repo y que contienen
```
dnf info python3  

dnf info python3-12*

```

```
dnf install python3.12
```