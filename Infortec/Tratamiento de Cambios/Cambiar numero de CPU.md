Se hacen todas las verificaciones como si fueramos a cambiar un disco a depent

Despues dentro de la maquina verificaos que se aplico el cambio

nproc ->>numero de procesadores



Conversacion con mario


echo "$(hostname)#MIN_UPTIME=0" > /usr/local/bin/script_uptime.conf

nohup bash -c "sleep 35m; rm -fv /usr/local/bin/script_uptime.conf" & 

rvtquery --guestcpu maquina

rvtquery --guestram maquina


lsmem && free -h

awk '/MemTotal/ {print $2 / 1024 / 1024 " GB"}' /proc/meminfo && date