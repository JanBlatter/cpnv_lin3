

# Centos 9 Client 

## Script d'installation

```
#!/bin/bash
 
#Obligatoire
systemctl disable --now systemd-timesyncd.service
 
dnf install -y chrony
 
systemctl enable --now chronyd.service
 
cat << EOF > 

# Use Debian vendor zone.
peer 10.0.5.10
peer 10.0.5.12
 
allow 127.0.0.0/8
# allow 10.0.5.0/24

 

EOF
 
systemctl restart chronyd.service

```


# Commands 

## enabled service
```
systemctl enabled chronyd.service
```

##  restart service
```
systemctl restart chronyd.service
```

## status
```
systemctl status chronyd.service
```


## Sources 
https://chrony-project.org/doc/4.4/chrony.conf.html
https://www.howtoforge.com/crony-ntp-server-centos-8/
