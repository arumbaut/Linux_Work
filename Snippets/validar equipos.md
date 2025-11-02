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