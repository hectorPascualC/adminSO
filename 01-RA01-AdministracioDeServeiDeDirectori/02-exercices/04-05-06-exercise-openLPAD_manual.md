# 📝 **PRÀCTICA 4,5,6 - OpenLDAP (GNU/Linux)**

## 🔹**Objectius**

1. Instal·lar un servidor OpenLDAP funcional.
2. Crear estructura DIT i objectes bàsics (OU, usuaris, grups).
3. Gestionar LDAP amb phpLDAPadmin.
4. Configurar TLS per connexions segures.

## 🔹**Infraestructura necessària**

* **1 MV Ubuntu Server**
* **1 MV Ubuntu Desktop (opcional)**
* PhpLDAPadmin
* OpenSSL

## 🔸**1. Instal·lació d’OpenLDAP**

1. `sudo apt install slapd ldap-utils`
2. Configurar el domini:

   * Domini LDAP: `dc=ieslocal,dc=local`
   * Admin: `cn=admin`

Comprovar:

    ```
    ldapsearch -x -LLL -H ldap:/// -b dc=ieslocal,dc=local dn
    ```

## 🔸**2. Crear OU, usuaris i grups amb LDIF**

### 2.1 Crear OU

`ou-estructura.ldif`:

    ```ldif
    dn: ou=Profesores,dc=ieslocal,dc=local
    objectClass: organizationalUnit
    ou: Profesores

    dn: ou=Alumnos,dc=ieslocal,dc=local
    objectClass: organizationalUnit
    ou: Alumnos
    ```

Aplicar:

    ```
    ldapadd -x -D cn=admin,dc=ieslocal,dc=local -W -f ou-estructura.ldif
    ```

### 2.2 Crear usuaris

`usuario.ldif`:

    ```ldif
    dn: uid=alopez,ou=Profesores,dc=ieslocal,dc=local
    objectClass: inetOrgPerson
    cn: Ana López
    sn: López
    uid: alopez
    userPassword: {SSHA}...
    ```

### 2.3 Crear grups

    ```ldif
    dn: cn=Profesores,ou=Grupos,dc=ieslocal,dc=local
    objectClass: groupOfNames
    cn: Profesores
    member: uid=alopez,ou=Profesores,dc=ieslocal,dc=local
    ```

## 🔸**3. Instal·lació phpLDAPadmin**

1. `sudo apt install phpldapadmin`
2. Editar configuració:
   `/etc/phpldapadmin/config.php`
3. Accedir via navegador:
   `http://IPServidor/phpldapadmin`

Validar que podem:

* Crear OU
* Crear usuaris
* Crear grups

# 🔸**4. Configurar TLS/SSL**

1. Crear CA pròpia i certificat:

   ```
   openssl genrsa -out ca.key 4096
   openssl req -new -x509 -key ca.key -out ca.crt
   ```
2. Configurar slapd perquè utilitzi TLS:

   ```
   olcTLSCertificateFile: /etc/ssl/certs/ldap.crt
   olcTLSCertificateKeyFile: /etc/ssl/private/ldap.key
   ```
3. Reiniciar:

    ```
    sudo systemctl restart slapd
    ```

4. Comprovar connexió segura:

    ```
    ldapsearch -H ldaps://IP -x -b dc=ieslocal,dc=local
    ```

# 🔸**Entrega**

* Captures d’instal·lació LDAP
* LDIF dels objectes creats
* Pantallazos phpLDAPadmin
* Comprovació de connexió TLS

