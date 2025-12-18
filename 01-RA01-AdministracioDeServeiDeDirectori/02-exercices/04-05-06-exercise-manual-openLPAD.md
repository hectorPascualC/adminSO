# PRÀCTICA 04-05-06 — OpenLDAP (GNU/Linux)

## 1. Infraestructura necessària

Abans de començar, prepara l'entorn perquè el servidor LDAP sigui accessible i estable. Si aquesta part no està bé, després “no et funcionarà res” encara que facis bé els passos.

### 1.1 Màquines virtuals

Necessites:

* 1 VM **Ubuntu Server** (servidor OpenLDAP)
* Opcional: 1 VM **Ubuntu Desktop** (per accedir via navegador a phpLDAPadmin)

> Si no tens Ubuntu Desktop, pots obrir la web des del navegador de l'amfitrió (el teu PC), sempre que la xarxa sigui Host-Only o similar.

### 1.2 Xarxa VirtualBox: configuració mínima recomanada

**Objectiu:** que el teu PC (amfitrió) i/o la VM client puguin accedir al servidor.

Opció A (recomanada per pràctiques FP): **Host-Only**

1. VirtualBox => **File / Tools** => **Host Network Manager**
2. Crea una xarxa Host-Only (si no existeix).
3. A la VM Ubuntu Server:

   * Settings => Network
   * Adapter 1 => **Host-only Adapter**
   * Selecciona la xarxa Host-Only creada

Opció B: **Internal Network** (si tens també una VM client dins VirtualBox)

* Adapter 1 => Internal Network (mateix nom a totes dues VMs)

IMPORTANT:

* Si uses **Internal Network**, el teu PC potser NO podrà obrir la web del servidor, perquè queda “tancat” dins VirtualBox.
* Si uses **Host-Only**, el teu PC sí que podrà obrir `http://IP_DEL_SERVIDOR/...`.

### 1.3 Com saber la IP del servidor (OBLIGATORI)

A Ubuntu Server, executa:

```bash
ip a
```

#### Explicació de la comanda

* `ip` => eina moderna per gestionar xarxes a Linux
* `a` => abreviació de `address`, mostra les adreces IP

#### Auditoria d'errors

* Si no apareix cap IP `inet`, indica que la interfície no té DHCP o no està connectada.

#### Anàlisi intern del servei

* El sistema consulta el kernel networking stack i mostra l'estat de cada interfície virtual associada a VirtualBox.

Busca la línia `inet`.

Alternativa:

```bash
hostname -I
```

#### Explicació de la comanda

* `hostname` => mostra informació del sistema
* `-I` => mostra totes les IP assignades a l'host

#### Auditoria d'errors

* Si no mostra cap IP, el sistema no té connectivitat de xarxa activa.

#### Anàlisi intern del servei

* El kernel retorna les IP actives associades a les interfícies de xarxa.

---

## 2. Instal·lació del servei OpenLDAP

### 2.1 Instal·lació de paquets

```bash
sudo apt update
sudo apt install slapd ldap-utils
```

Verificació:

```bash
dpkg -l | grep slapd
```

#### Explicació

* `dpkg -l` => llista paquets instal·lats
* `grep slapd` => filtra per slapd

---

## 3. Configuració inicial d'OpenLDAP

```bash
sudo dpkg-reconfigure slapd
```

#### Explicació de la comanda

* `dpkg-reconfigure` => reexecuta l'assistent de configuració
* `slapd`            => servei a configurar

#### Auditoria d'errors

* Si no apareix l'assistent => paquet mal instal·lat

#### Anàlisi intern del servei

* Es generen entrades a `cn=config`
* Es crea la base de dades MDB
* Es defineix el Base DN

---

## 4. Verificació del servei LDAP

```bash
systemctl status slapd
```

#### Auditoria d'errors

* `inactive` => servei aturat
* `failed`   => error de configuració

#### Anàlisi intern del servei

* systemd consulta l'estat del procés `slapd`
* Es comprova el socket d'escolta (389/636)

```bash
ldapsearch -x -LLL -H ldap:/// -b dc=ieslocal,dc=local dn
```

#### Explicació detallada

* `ldapsearch`  => eina de consulta LDAP
* `-x`          => autenticació simple
* `-LLL`        => sortida neta
* `-H ldap:///` => connexió local LDAP
* `-b`          => base DN
* `dn`          => atribut a mostrar

#### Auditoria d'errors

* `No such object`            => base DN incorrecte
* `Can't contact LDAP server` => slapd no actiu

#### Anàlisi intern del servei

* slapd valida el bind anònim
* Consulta l'arrel del DIT
* Retorna els DN existents

---

## 5. Creació de l'estructura del directori (OU)

```bash
nano ou-estructura.ldif
```

### Bloc LDIF

```ldif
dn: ou=Profesores,dc=ieslocal,dc=local
objectClass: organizationalUnit
ou: Profesores
```

#### Explicació LDIF

* `dn`          => ubicació exacta al DIT
* `objectClass` => defineix el tipus d'objecte
* `ou`          => nom de la unitat

#### Auditoria d'errors

* `Invalid DN syntax` => DN mal format

#### Anàlisi intern del servei

* slapd valida l'esquema
* Escriu l'entrada a MDB

Aplicació:

```bash
ldapadd -x -D cn=admin,dc=ieslocal,dc=local -W -f ou-estructura.ldif
```

#### Explicació

* `ldapadd` => afegeix entrades
* `-D`      => DN d'autenticació
* `-W`      => demana contrasenya
* `-f`      => fitxer LDIF

#### Auditoria d'errors

* `Invalid credentials (49)` => password incorrecte

#### Anàlisi intern del servei

* slapd valida credencials
* Escriu entrades persistents

---

## 6. Creació d'usuaris LDAP

### 6.1 Generació de contrasenya xifrada

```bash
slappasswd
```

#### Explicació de la comanda

* `slappasswd` => eina d'OpenLDAP per generar hashes de contrasenya
* Per defecte genera **SSHA** (Salted SHA-1), adequat per a LDAP

#### Auditoria d'errors

* Si no retorna cap sortida        => problema d'instal·lació de `slapd`
* Si el terminal queda “en espera” => està demanant la contrasenya (no és un error)

#### Anàlisi intern del servei

* L'eina genera un **salt aleatori**
* Calcula el **hash** de la contrasenya
* Retorna **només** el hash (la contrasenya original no es desa enlloc)

---

### 6.2 Crear el fitxer LDIF de l'usuari

```bash
nano usuario-alopez.ldif
```

#### Anàlisi intern del servei

* Encara **no** s'interactua amb LDAP
* Es crea un fitxer de definició que posteriorment serà processat per `slapd`

---

### Bloc LDIF — Usuari

```ldif
dn: uid=alopez,ou=Profesores,dc=ieslocal,dc=local
objectClass: inetOrgPerson
cn: Ana López
sn: López
uid: alopez
userPassword: {SSHA}XXXX
```

#### Explicació LDIF línia per línia

* `dn:`                       => defineix la ubicació exacta de l'usuari dins del DIT
* `uid=alopez`                => identificador únic de l'usuari dins del directori
* `ou=Profesores`             => unitat organitzativa on es crea l'usuari
* `objectClass: inetOrgPerson`=> esquema estàndard per a usuaris personals
* `cn:`                       => nom comú de l'usuari
* `sn:`                       => cognom (atribut obligatori en `inetOrgPerson`)
* `uid:`                      => identificador lògic utilitzat per consultes i autenticació
* `userPassword:`             => hash de la contrasenya (mai en text pla)

#### Auditoria d'errors

* `Object class violation (65)` => falta algun atribut obligatori (`sn`, `cn`, etc.)
* `Invalid DN syntax`           => DN mal format
* `No such object (32)`         => la OU no existeix

#### Anàlisi intern del servei

* slapd valida el DN contra el DIT existent
* Comprova l'esquema `inetOrgPerson`
* Verifica que els atributs obligatoris són presents
* L'entrada queda llesta per persistir-se a la base de dades MDB

---

### 6.3 Afegir l'usuari al directori

```bash
ldapadd -x -D cn=admin,dc=ieslocal,dc=local -W -f usuario-alopez.ldif
```

#### Explicació de la comanda

* `ldapadd` => eina per afegir entrades LDAP
* `-x`      => autenticació simple (no SASL)
* `-D`      => DN amb què s'autentica l'operació
* `-W`      => demana la contrasenya per terminal
* `-f`      => fitxer LDIF a importar

#### Auditoria d'errors

* `Invalid credentials (49)` => DN o contrasenya incorrectes
* `Already exists (68)`      => l'usuari ja existeix
* `No such object (32)`      => la OU no existeix

#### Anàlisi intern del servei

* slapd autentica l'usuari administrador
* Processa el fitxer LDIF
* Escriu l'entrada de l'usuari a la base de dades MDB
* Actualitza índexs interns del directori

---

### 6.4 Verificació de l'usuari creat

```bash
ldapsearch -x -LLL -b dc=ieslocal,dc=local "(uid=alopez)" dn cn sn uid
```

#### Explicació de la comanda

* `ldapsearch`   => eina de consulta LDAP
* `-x`           => autenticació simple
* `-LLL`         => sortida neta (sense capçaleres)
* `-b`           => base DN de la cerca
* `(uid=alopez)` => filtre LDAP
* `dn cn sn uid` => atributs a mostrar

#### Auditoria d'errors

* Cap resultat                => l'usuari no existeix o filtre incorrecte
* `Can't contact LDAP server` => servei no disponible

#### Anàlisi intern del servei

* slapd avalua el filtre
* Recorre el DIT sota el Base DN
* Retorna únicament els atributs sol·licitats
* Confirma la persistència correcta de l'usuari

---

## 7. Creació de grups LDAP

### 7.1 Crear LDIF del grup

```bash
nano grupo-profesores.ldif
```

#### Auditoria d'errors

* Si el fitxer no es desa => problemes de permisos o sortida incorrecta de l'editor

#### Anàlisi intern del servei

* No s'executa cap acció LDAP encara
* Només es crea un fitxer al sistema de fitxers

### Bloc LDIF

```ldif
dn: cn=Profesores,ou=Grupos,dc=ieslocal,dc=local
objectClass: groupOfNames
cn: Profesores
member: uid=alopez,ou=Profesores,dc=ieslocal,dc=local
```

#### Explicació LDIF línia per línia

* `dn:`                       => defineix la ubicació exacta del grup dins del DIT
* `objectClass: groupOfNames` => indica que el grup contindrà una llista de membres (`member`)
* `cn:`                       => nom del grup
* `member:`                   => DN complet d'un objecte existent que forma part del grup

#### Auditoria d'errors

* `No such object (32)`         => el DN del membre no existeix
* `Object class violation (65)` => falta algun atribut obligatori

#### Anàlisi intern del servei

* slapd valida l'existència del DN referenciat
* Comprova l'esquema `groupOfNames`
* Escriu el grup a la base de dades MDB

---

### 7.2 Afegir el grup al directori

```bash
ldapadd -x -D cn=admin,dc=ieslocal,dc=local -W -f grupo-profesores.ldif
```

#### Explicació de la comanda

* `ldapadd` => afegeix una entrada LDAP
* `-x`      => autenticació simple
* `-D`      => DN d'autenticació
* `-W`      => sol·licita la contrasenya
* `-f`      => fitxer LDIF a importar

#### Auditoria d'errors

* `Invalid credentials (49)` => contrasenya incorrecta
* `Already exists (68)`      => el grup ja existeix

#### Anàlisi intern del servei

* slapd autentica l'admin
* Valida el DN del grup
* Escriu l'entrada persistentment

---

## 8. Administració amb phpLDAPadmin

### 8.1 Instal·lació

```bash
sudo apt install phpldapadmin
```
---

### 8.2 Comprovació del servidor web

```bash
systemctl status apache2
```

#### Explicació

* `systemctl` => gestor de serveis
* `status`    => consulta estat
* `apache2`   => servidor web

#### Auditoria d'errors

* `inactive` => Apache no està actiu

#### Anàlisi intern del servei

* systemd consulta el procés Apache
* Verifica el port 80 en escolta

---

### 8.3 Configuració bàsica

```bash
sudo nano /etc/phpldapadmin/config.php
```

#### Explicació

* Edició del fitxer de configuració principal

```php
$servers->setValue('server','base',array('dc=ieslocal','dc=local'));
```

#### Explicació

* Defineix el Base DN per defecte

#### Auditoria d'errors

* Base DN incorrecte => errors de navegació LDAP

#### Anàlisi intern del servei

* phpLDAPadmin construeix consultes LDAP a partir d'aquest DN

---

### 8.4 Accés web

```
http://IP_DEL_SERVIDOR/phpldapadmin
```

#### Anàlisi intern del servei

* El navegador inicia connexió HTTP
* Apache entrega la interfície PHP
* phpLDAPadmin obre sessió LDAP

---

## 9. Configuració de connexions segures (TLS / LDAPS)

### 9.1 Generació de claus i certificats

```bash
openssl genrsa -out ca.key 4096
```

#### Explicació

* `openssl` => eina criptogràfica
* `genrsa`  => genera clau privada RSA
* `4096`    => mida de la clau

#### Auditoria d'errors

* `Permission denied` => permisos insuficients

#### Anàlisi intern del servei

* Es genera una clau privada utilitzada com a CA

---

```bash
openssl req -new -x509 -key ca.key -out ca.crt
```

#### Explicació

* `req`   => crea sol·licitud/certificat
* `-x509` => certificat autosignat
* `-key`  => clau privada utilitzada

#### Anàlisi intern del servei

* Es crea un certificat arrel per confiança TLS

---

### 9.2 Activació TLS

```bash
sudo systemctl restart slapd
```

#### Explicació

* Reinicia el servei LDAP

#### Auditoria d'errors

* Servei no arrenca => error en certificats o permisos

#### Anàlisi intern del servei

* slapd carrega certificats a memòria
* Habilita negociació TLS

---

### 9.3 Verificació LDAPS

```bash
ldapsearch -H ldaps://IP_SERVIDOR -x -b dc=ieslocal,dc=local
```

#### Explicació

* `-H ldaps://` => connexió xifrada
* Port 636

#### Auditoria d'errors

* `TLS: peer cert untrusted` => problema de confiança

#### Anàlisi intern del servei

* Es negocia canal TLS
* slapd valida el certificat presentat

---

## 10. Comprovació final

```bash
systemctl status slapd
```

```bash
ldapsearch -x -LLL -b dc=ieslocal,dc=local "(ou=*)"
```

```bash
ldapsearch -x -LLL -b dc=ieslocal,dc=local "(uid=alopez)"
```

```bash
ldapsearch -x -LLL -b dc=ieslocal,dc=local "(cn=Profesores)"
```

#### Anàlisi intern del servei

* Es valida la coherència del DIT
* Es comprova persistència a MDB
* Es confirma funcionament LDAP i LDAPS

---

## 11. Entrega

### Evidències a lliurar

* Captures de totes les comandes executades
* Fitxers LDIF utilitzats
* Captures de phpLDAPadmin
* Proves LDAP i LDAPS
