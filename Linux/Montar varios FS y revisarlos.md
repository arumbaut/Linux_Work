```
for i in /homeuser/sigaux /atoslec /l_tibco /opt/portal /homeuser/pinm_ftp /homeuser/pinm_st /tiba5mt /var/portal/7.5/pinm_st /Oracle_client /homeuser2; do mount $i;done
```

Revisarlos
```
for i in /homeuser/sigaux /atoslec /l_tibco /opt/portal /homeuser/pinm_ftp /homeuser/pinm_st /tiba5mt /var/portal/7.5/pinm_st /Oracle_client /homeuser2; do df -hPT $i;done
```
