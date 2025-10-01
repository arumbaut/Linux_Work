Es importante revisar que entornos tinene los clientes y al momento de instalar paquetes es recomendable que las pruebas se realicen en entornos de test. Conel siguiente comando podemos verificar que tengan el entorno disponible para test.
```
rvtquery --machine AOTLX%SAP%
```

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