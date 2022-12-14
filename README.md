# ldap

```
sudo yum -y install openldap compat-openldap openldap-clients openldap-servers openldap-servers-sql openldap-devel
sudo systemctl start slapd.service
sudo systemctl enable slapd.service
sudo systemctl status slapd.service

slappasswd

ldapadd -Y EXTERNAL -H ldapi:/// -f db.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f monitor.ldif

cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
chown -R ldap:ldap /var/lib/ldap/DB_CONFIG
systemctl restart slapd

ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif



ldapadd -x -W -D "cn=ldapadm,dc=example,dc=com" -f base.ldif
ldapsearch -D cn="ldapadm,dc=example,dc=com" -W -b "dc=example,dc=com" objectClass=*


yum install epel-release -y
yum install -y phpldapadmin

nano /etc/phpldapadmin/config.php
$servers->setValue('server','host','127.0.0.1');
$servers->setValue('server','port',389);
$servers->setValue('login','bind_id','cn=ldapadm,dc=example,dc=com');
$servers->setValue('login','attr','dn');
 
 nano /etc/httpd/conf.d/phpldapadmin.conf
 Require all granted

 http://domain/phpldapadmin
```

# set replika 
```

in Master
backup config 
slapcat -b cn=config -l openldap-config.ldif

backup data
slapcat -n 1 -l openldap-data.ldif

scp openldap-data.ldif root@slave:/opt
scp openldap-config.ldif root@slave:/opt


in Slave
clean all data in Slave
sudo rm -rf /etc/openldap/slapd.d/*
sudo rm -rf /var/lib/openldap/*
restore config
cd /opt && sudo slapadd -b cn=config -l openldap-config.ldif -F /etc/openldap/slapd.d/
restore data
cd /opt && sudo slapadd -n 1 -l openldap-data.ldif -F /etc/openldap/slapd.d/



In Master
ldapadd -Y EXTERNAL -H ldapi:/// -f replikasi_master_mod_syncprov.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f  replikasi_master_syncprov.ldif

In Slave
ldapadd -Y EXTERNAL -H ldapi:/// -f replikasi_slave_synrepl.ldif


```


