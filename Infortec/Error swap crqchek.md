Cuando da fallo por el montaje del swap por la opcion none hay que corregir el crqcheck de la siguiente manera:


pues al buscar por la palabra swap entrecomillada  
grep '"swap"' /usr/local/bin/crqcheck  
la versión mala te aparece así  
![[Pasted image 20250910131819.png]]
y lo que hay que hacer es cambiar "swap" por "swap|none"