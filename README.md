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



ldapadd -x -W -D "cn=ldapadm,dc=hadoop,dc=com" -f base.ldif
ldapsearch -D cn="ldapadm,dc=hadoop,dc=com" -W -b "dc=hadoop,dc=com" objectClass=*

```
