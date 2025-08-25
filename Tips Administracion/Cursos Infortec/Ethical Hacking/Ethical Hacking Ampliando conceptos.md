Herramientas para la enumeracion de subdominios

Sublist3r
sublist3r -d ejemplo.com

Amass
amass enum -d ejemplo.com

Otros puertos a tener en cuenta en la audtoria web
80, 442, 8080, 8000,8001, 8443, 8888, 81/82 

Herramienta
HTTPX para escaneo de servicios web

cat host.txt | httpx -ports 80,443,8080,8000,8443,8888,81,82 -path / -title -status-code -web-server -o resultados.txt


