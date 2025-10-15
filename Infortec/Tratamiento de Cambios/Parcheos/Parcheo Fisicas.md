	(TAS de validacion del parcheo)
Revisar a traves de la RSA: 

- System status sea correcto
- Que no haya Active events
- Que en el event log no haya nada de al menos 6 meses de antiguedad
- Que haya acceso por consola

Acceder por ssh a la maquina, lanzar y revisar que no de error:

crqcheck

Acceder por ssh a la maquina, lanzar y guardar los datos en un TXT:

```
{
crqcheck --verbose
cat /etc/selinux/config | grep SELINUX= | grep -v "# SELINUX"
df -hPT /boot
df -hPT /usr
df -hPT /var
ip a
route -n
cat /etc/redhat-release
uname -a
cat /etc/shadow | grep root | cut -d ':' -f1-2 
}
```

(Tras de Revision del backup)

Entrar por ssh a la maquina y realizar:

Nos creamos un fichero en nuestro home que se llame mkrear con el siguiente contenido de 

```
mkrear() {
[[ $UID -ne 0 ]] && { echo "Run as root."; return 1; }
message() { echo -e "\e[1;37m$*\e[0m"; }
local arch="$(uname -i|sed 's:i6:i?:')"
local bck_date
local config="/etc/rear/local.conf"
local exclude_bck exclude_cfg exclude_set
local hostname="$(uname -n | cut -d. -f1 | tr '[A-Z]' '[a-z}')"
local lv_name="lv_rear"
local lv_size="2500"
local mount_dir="/rear"
local nfs_dir="/run/rear"
local uri_server uri_type uri_volume
local os_version="$(grep -Eo "release [0-9]" /etc/{oracle,redhat}-release 2>/dev/null | awk '{print $NF}' | sort | tail -n1)"
local rear_installed="$(rpm -qa rear | sed "s/.${arch}//")"
local route_yec="$(ip route show | grep -Fo 192.168.0.0/22)"
local route_tor="$(ip route show | grep -Fo 10.152.248.0/22)"
local route_gea="$(ip route show | grep -Fo 192.168.254.0/23)"
local route_jzz="$(ip route show | awk '/default via 172./ {print $3}')"
local rpm_deps rpm_file rpm_main
local vg_name="$(vgs | grep -o -e root_vg -e rootvg -e vg00 -e VolGroup00 -e rear_vg | head -n1)"
local vg_gblimit="10"
local vg_gbused vg_gbexcl vg_gbcomp
[[ -n ${route_gea} ]] && { uri_type="nfs"; uri_server="aotlxteadm00003"; uri_volume="192.168.255.32:/rearbackup"; }
[[ -n ${route_jzz} ]] && { uri_type="nfs"; uri_server="aotlxteadm00002"; uri_volume="172.29.9.89:/rearbackup"; }
[[ -n ${route_tor} ]] && { uri_type="nfs"; uri_server="aotlxteadm00001"; uri_volume="10.152.251.138:/rearbackup"; }
[[ -n ${route_yec} ]] && { uri_type="nfs"; uri_server="aotlxteadm00003"; uri_volume="192.168.0.134:/rearbackup"; }
[[ -z ${uri_volume} ]] && { uri_type="nfs"; uri_server="aotlxterep00001"; uri_volume="10.113.56.216:/temporal_vmdk_1/rear"; }
[[ $1 =~ ^--output= && $1 =~ =/.*$ ]] && { uri_type="file"; uri_server=""; uri_volume="${1##*=}"; uri_volume="${uri_volume%/}"; shift; }
echo "[#] Checking OS version:        '${os_version:--}'"
case ${os_version} in
  (8) rpm_main="rear-2.6-4.el8.${arch}.rpm" ;;
  (7) rpm_main="rear-2.6-1.el7.${arch}.rpm" ;;
  (6) rpm_main="rear-2.6-1.el6.${arch}.rpm" ;;
  (5) rpm_main="rear-2.5-1.el5.${arch}.rpm" rpm_deps="mkisofs-2.01-10.7.el5.${arch}.rpm syslinux-4.02-7.2.el5.${arch}.rpm" ;;
  (4) rpm_main="rear-1.13.0-1.el4.rf.noarch.rpm" rpm_deps="mkisofs-2.01.1-5.2.el4.${arch}.rpm syslinux-2.11-2.el4.${arch}.rpm" ;;
esac
exclude_bck="$(awk -F"[()]" '/^BACKUP_PROG_EXCLUDE/ {print $2}' ${config} 2>/dev/null)"
exclude_cfg="\"\${BACKUP_PROG_EXCLUDE[@]}\" '/media' '/var/tmp' '/var/crash' '/var/cache/yum' '${mount_dir}'"
exclude_set="${exclude_bck:-${exclude_cfg}}"
echo "[#] Checking local VG name:     '${vg_name}'"
[[ -z ${vg_name} ]] && { message " error: no system VG detected"; return 1; }
echo "[#] Checking local LV name:     '/dev/mapper/${vg_name}-${lv_name}'"
if lvs | grep -qw ${lv_name}; then
  true
else
  [[ ${os_version} -lt 7 ]] && lv_size="1300"
  [[ ${os_version} -eq 7 ]] && lv_size="2000"
  lvcreate -n ${lv_name} -L ${lv_size}m ${vg_name} || return 1
  if grep -qw ext4 /proc/self/mounts; then
   mkfs.ext4 -q /dev/mapper/${vg_name}-${lv_name} || return 1
  else
   mkfs.ext3 -q /dev/mapper/${vg_name}-${lv_name} || return 1
  fi
fi
tune2fs -m0 /dev/mapper/${vg_name}-${lv_name} &>/dev/null || return 1
grep -qw "${mount_dir}" /etc/fstab || echo "/dev/mapper/${vg_name}-${lv_name} ${mount_dir} $(file -Ls /dev/mapper/${vg_name}-${lv_name} | grep -ow "ext[0-9]") defaults 1 2" >> /etc/fstab
[[ -d ${mount_dir} ]] || { mkdir ${mount_dir} || return 1; }
grep -qw "${mount_dir}" /proc/self/mounts || { printf " "; mount -v ${mount_dir} || return 1; }
df -P /rear | awk 'NR>1 && /%/ {sub(/%/,"",$5); if(0+$5 > 50) print "w"}' | grep -q "w" && message " warning: low space available on '${mount_dir}'"
echo "[#] Checking output location:   '${uri_server}'"
[[ ${uri_server} == "aotlxterep00001" ]] && message " warning: fallback NFS server configured"
#[[ ${uri_server} =~ ^aotlx ]] && { showmount -e ${uri_volume%%:*} &>/dev/null || { message " error: connection '${uri_volume%%:*}' failed "; return 1; }; }
echo "[#] Checking output directory:  '${uri_volume}/${hostname}'"
if [[ ${uri_server} =~ ^aotlx || ${uri_server} =~ ^[0-9] ]]; then
  grep -qw "${nfs_dir}" /proc/self/mounts && { message " error: testing NFS mountponint '${nfs_dir}' in use"; return 1; }
  [[ -d ${nfs_dir} ]] || mkdir -p ${nfs_dir} || return 1
  mount -t nfs ${uri_volume} ${nfs_dir} || return 1
  [[ -d ${nfs_dir}/${hostname} ]] || { printf " "; mkdir -pv ${nfs_dir}/${hostname} || return 1; }
  bck_date="$(find ${nfs_dir}/${hostname}/${hostname} ${nfs_dir}/${hostname}/${hostname^^} -name backup.tar.gz -exec stat -c%y {} \; 2>/dev/null | cut -c1-16 | sed 's: :_:')"
  bck_size="$(let $(find ${nfs_dir}/${hostname}/${hostname} ${nfs_dir}/${hostname}/${hostname^^} -name backup.tar.gz -exec stat -c%s {} \; 2>/dev/null) / 1024 / 1024 2>/dev/null)"
  bck_size="$([[ -n "${bck_size}" ]] && echo "${bck_size}MB")"
  if [[ ${rpm_main%.*.*} != ${rear_installed} && ${os_version} =~ ^[4567]$ ]]; then
   for rpm_file in ${rpm_main} ${rpm_deps}; do
    [[ -r "${nfs_dir}/software/${rpm_file}" ]] || { umount ${nfs_dir}; message " error: local RPM update '${rpm_file}' not found"; return 1; }
    \cp -f "${nfs_dir}/software/${rpm_file}" "${HOME}/${rpm_file}" 2>/dev/null || { umount ${nfs_dir}; return 1; }
   done
  fi
  umount ${nfs_dir} || return 1
elif [[ -z "${uri_server}" ]]; then
  mkdir -pv "${uri_volume}/${hostname}" || return 1
fi
echo "[#] Checking ReaR install:      '${rear_installed:--}'"
if [[ ${os_version} -eq 8 ]]; then
  [[ ${rpm_main} =~ ${rear_installed##*.} ]] || message " info: execute this command: yum install rear genisoimage syslinux"
elif [[ ${os_version} =~ ^[67]$ && ${rear_installed} != ${rpm_main%.*.*} ]]; then
  message " info: execute this command: yum install rear genisoimage syslinux"
  message " info: execute this command: rpm --force -U $(for rpm_file in ${rpm_main} ${rpm_deps}; do printf "%s " "${HOME}/${rpm_file}"; done)"
  message " info: execute this command: rm -fv $(for rpm_file in ${rpm_main} ${rpm_deps}; do printf "%s " "${HOME}/${rpm_file}"; done)"
elif [[ ${os_version} =~ ^[45]$ && ${rear_installed} != ${rpm_main%.*.*} ]]; then
  message " info: execute this command: rpm --force --nodeps -U $(for rpm_file in ${rpm_main} ${rpm_deps}; do printf "%s " "${HOME}/${rpm_file}"; done)"
  message " info: execute this command: rm -fv $(for rpm_file in ${rpm_main} ${rpm_deps}; do printf "%s " "${HOME}/${rpm_file}"; done)"
elif [[ ${rear_installed} != ${rpm_main%.*.*} ]]; then
  message " info: execute this command: rpm --force -U ${HOME}/${rpm_main}"
  message " info: execute this command: rm -fv ${HOME}/${rpm_main}"
elif [[ ${rear_installed} == ${rpm_main%.*.*} ]]; then
  true
else
  message " info: os version '${os_version}' not supported"
fi
echo "[#] Checking ReaR config file:  '${config}'"
[[ -d ${config%/*} ]] || { printf " "; mkdir -pv ${config%/*}; } || return 1
[[ -r ${config} && ! -r ${config}.$(date +%Y%m%d) ]] && cp -fp ${config} ${config}.$(date +%Y%m%d)
[[ -e ${config}.rpmnew ]] && rm -f ${config}.rpmnew
[[ -e ${config}.lck ]] 
cat << EOF > ${config}
export TMPDIR=${mount_dir}
OUTPUT=ISO
OUTPUT_URL=${uri_type}://${uri_volume//:/}/${hostname}/
BACKUP=NETFS
BACKUP_URL=${uri_type}://${uri_volume//:/}/${hostname}/
NETFS_KEEP_OLD_BACKUP_COPY=y
ONLY_INCLUDE_VG=( "${vg_name}" )
BACKUP_PROG_EXCLUDE=(${exclude_set})
EXCLUDE_MOUNTPOINTS=(\$(df -Pl | egrep -v '${vg_name}|boot|tmpfs|Filesystem|fichero|Disp' | awk '{print \$6}'))
AUTOEXCLUDE_MULTIPATH=n
EOF
if [[ -s ${config//.conf/.custom} ]]; then
  \cp -fp ${config//.conf/.custom} ${config}
  message " warning: config file '${config}' overwrited with '${config//.conf/.custom}'"
else
  message " info: use '${config//.conf/.custom}' for custom configuration"
fi
[[ $(grep -cE "(${hostname}|${mount_dir}|${uri_volume%%:*}|${vg_name})" ${config}) -ne 6 ]] && message " warning: review config file '${config}'"
grep -Fq "(${exclude_cfg})" ${config} \
   || message " warning: custom exclude option 'BACKUP_PROG_EXCLUDE' detected\n warning: current exclusion: $(awk '/BACKUP_PROG_EXCLUDE/ {gsub(/^.*EXCLUDE...." |)$/,""); print}' ${config})"
echo "[#] Checking last backup date:  '${bck_date:--}'"
echo "[#] Checking last backup size:  '${bck_size:--}'"
vg_gbused="$(df -P | awk -v m="${vg_name}" 'BEGIN{printf "%s","("} $1 ~ m {printf "%s+",$3} END{print "0)/1024/1024"}')"
vg_gbused="$(( ${vg_gbused} ))"
vg_gbexcl="$(eval du -csx -BG ${exclude_set} 2>/dev/null | awk '$2 == "total" {gsub(/[gGbB]/,"",$1); print $1}')"
vg_gbcomp="$((vg_gbused - vg_gbexcl))"
[[ ${vg_gbcomp} -gt ${vg_gblimit} ]] \
  && message " warning: disk usage '${vg_gbcomp}GB' is higher than usual '${vg_gblimit}GB'" \
  && message " warning: configure 'BACKUP_PROG_EXCLUDE' option in '${config}'" \
  && eval "df -hPT $([[ ${os_version} -lt 6 ]] && echo '| column -t')" | grep --color -e Used -e ${vg_name}
message " info: execute this command: rear -d -v mkbackup"
}
 
```



Lanzamos luego:

source mkrear

y luego:

mkrear

Comprobamos que no haya ningun warning y si los hay los solucionamos hasta que no salga ninguno.

Despues lanzamos:
```
rear -d -v mkbackup
```
(Si da probleamas de espacio en /rear revisar y eliminar backups anteriores)
```
rm -Rf /rear/rear.*
```
(Si el agente de trendmicro esta levantado puede fallar y si el seleniun esta en enforcing tambien )

```
[root@AOTLXPRCMV10002 ~]# setenforce 0
[root@AOTLXPRCMV10002 ~]# getenforce
Permissive
[root@AOTLXPRCMV10002 es48389e]# setenforce 1
[root@AOTLXPRCMV10002 es48389e]# getenforce
Enforcing

```


echo $?

que guardara un backup en la maquina AOTLXTEADM00003 en la ruta /rearbackup

Para recoger las evidencias hacemos:

ll
file backup.tar.gz rear-AOTLXPRPBD10055.iso

copiamos la salida y la adjuntamos al tratado de la tarea




