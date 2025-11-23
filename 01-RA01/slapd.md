# **Què és `slapd`?**

`slapd` és **el servidor LDAP oficial d’OpenLDAP**.
El nom ve de:

**S**tand **L**one **A**lone **P**rotocol **D**aemon
→ *slapd*

És a dir:

> **slapd és el servei (daemon) que implementa i executa un servidor LDAP en un sistema Linux.**

És l’equivalent en LDAP al que seria un **Domain Controller** en Active Directory.

---

## **1. Quin paper té `slapd`?**

`slapd` és:

* el **motor principal del directori**,
* el responsable de **guardar i gestionar el DIT**,
* el que **respon consultes LDAP**,
* el que **processa autenticacions** (bind),
* el que **aplica l’esquema**,
* el que **gestiona objectes, atributs i operacions** (search, add, modify, delete).

Sense `slapd`, no hi ha servidor LDAP.

---

##**2. On s’executa `slapd`?**

En sistemes Linux:
* Ubuntu Server
* Debian
* Red Hat / CentOS / RockyLinux
* SLES (SUSE)

També pot estar en:
* **contenidors Docker**
* **màquines virtuals**
* **servidors al núvol**

És molt lleuger i flexible.

---

## **3. Com funciona `slapd` per dins?**

### **3.1 Carrega l’esquema LDAP**

(atributs, objectClass, sintaxi…)

### **3.2 Llegeix la base de dades del directori**

Normalment a:

```
/var/lib/ldap
```

### **3.3 Obre els ports:**

* **389** → LDAP sense xifrar
* **636** → LDAPS (LDAP sobre SSL)

### **3.4 Espera peticions dels clients**

Com per exemple:

* `ldapsearch`
* `ldapadd`
* aplicacions que es connecten per LDAP
* serveis de login de Linux
* servidors web o aplicacions corporatives

### **3.5 Aplica lògica d’autenticació**

* Simple Bind (usuari + contrasenya)
* SASL
* GSSAPI (Kerberos) si està configurat

---

## **4. Exemple real d’un servidor amb `slapd`**

Aquesta és la sortida típica del servei en una màquina Ubuntu:

```
$ systemctl status slapd

slapd.service - LSB: OpenLDAP standalone server (Lightweight Directory Access Protocol)
   Loaded: loaded (/etc/init.d/slapd; generated)
   Active: active (running)
   Docs: man:slapd(8)
   Process: 1870 ExecStart=/etc/init.d/slapd start (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/slapd.service
           └─1885 /usr/sbin/slapd -h ldap:/// ldapi:/// -g openldap -u openldap -F /etc/ldap/slapd.d
```

Això indica:
* `slapd` està **executant-se**
* escoltant en ports LDAP
    * ldap:///
    * Servei LDAP
    * Protocol LDAP regular
    * SENSE SSL
    * Escoltant al port 389
* servint el directori des de `/etc/ldap` i `/var/lib/ldap`

---

## **5. Exemples de comunicació amb slapd**

A continuació es mostren les tres operacions LDAP més habituals: **cerca**, **creació d’objectes** i **modificació d’atributs**. Totes elles són processades pel servidor `slapd`.

---

### **5.1. Buscar usuaris (ldapsearch)**

```

ldapsearch -x -LLL -H ldap://localhost -b "dc=empresa,dc=local"

```

Aquesta comanda **llegeix informació del directori**. No modifica res.

#### **Què fa cada paràmetre?**

| Part                       | Explicació                                                         |
|---------------------------|--------------------------------------------------------------------|
| `ldapsearch`              | Eina de línia d'ordres per fer cerques al directori               |
| `-x`                      | Simple bind → autenticació simple (usuari/contrasenya o anònim)    |
| `-LLL`                    | Resultat “net”, sense informació addicional                       |
| `-H ldap://localhost`     | Indica **quin servidor LDAP** consultar                            |
| `-b "dc=empresa,dc=local"`| Base de cerca (punt de partida del DIT)                            |

#### **Què retorna?**

- Tots els objectes que hi ha dins del DIT
- Usuaris, grups, equips, OUs…

És l’equivalent a:

> “Mostra totes les fitxes del directori.”

#### **Exemple de sortida**

```

dn: cn=Hector Pascual,ou=Professors,dc=empresa,dc=local
cn: Hector Pascual
sn: Pascual
mail: [hector@empresa.local](mailto:hector@empresa.local)
objectClass: inetOrgPerson

```

---

### **5.2. Afegir un nou usuari (ldapadd)**

```

ldapadd -x -D "cn=admin,dc=empresa,dc=local" -W -f usuari.ldif

```

Aquesta comanda **crea un objecte nou** al directori.

#### **Què fa cada part?**

| Part                                | Significat                                       |
|------------------------------------|--------------------------------------------------|
| `ldapadd`                           | Ordre per **afegir objectes** al directori       |
| `-x`                                | Autenticació simple                               |
| `-D "cn=admin,dc=empresa,dc=local"` | DN de l’usuari administrador                      |
| `-W`                                | Demana contrasenya                                |
| `-f usuari.ldif`                    | Fitxer LDIF amb les dades de l’objecte           |

#### **Exemple de fitxer usuari.ldif**

```

dn: cn=pau.girona,ou=Informatica,dc=empresa,dc=local
objectClass: inetOrgPerson
cn: Pau Girona
sn: Girona
mail: [pau@empresa.local](mailto:pau@empresa.local)

```

#### **Què fa?**

- Llegeix el fitxer LDIF  
- Valida que segueix l’esquema LDAP  
- Crea un nou objecte dins del directori  

És com dir:

> “Afegeix aquesta nova fitxa d’usuari.”

---

### **5.3. Modificar atributs (ldapmodify)**

```

ldapmodify -x -D "cn=admin,dc=empresa,dc=local" -W -f canvi.ldif

```

Serveix per **fer canvis a objectes existents**.

#### **Què fa cada part?**

| Part                                | Explicació                                      |
|------------------------------------|--------------------------------------------------|
| `ldapmodify`                        | Eina per **modificar objectes**                 |
| `-x`                                | Autenticació simple                             |
| `-D "cn=admin,dc=empresa,dc=local"` | Usuari administrador                            |
| `-W`                                | Demana contrasenya                              |
| `-f canvi.ldif`                     | Fitxer LDIF amb les modificacions               |

#### **Exemples de fitxer canvi.ldif**

Reemplaçar un correu:

```

dn: cn=pau.girona,ou=Informatica,dc=empresa,dc=local
changetype: modify
replace: mail
mail: [pau.girona@empresa.local](mailto:pau.girona@empresa.local)

```

Afegir un telèfon:

```

dn: cn=pau.girona,ou=Informatica,dc=empresa,dc=local
changetype: modify
add: telephoneNumber
telephoneNumber: +34931223344

```

#### **Què fa?**

- Localitza l’objecte corresponent  
- Aplica els canvis (reemplaçar, afegir o eliminar atributs)

És com dir:

> “Actualitza aquesta fitxa amb aquesta informació.”

---

### **5.4. Resum**

| Ordre        | Funció                  | Equivalent       |
|--------------|--------------------------|------------------|
| `ldapsearch` | Buscar objectes          | Llegeix fitxes   |
| `ldapadd`    | Crear objectes           | Nova fitxa       |
| `ldapmodify` | Modificar objectes       | Actualitza fitxa |

Totes aquestes operacions les processa **slapd**.

# **6. Comparació**

| Sistema          | Server            | Funció                                  |
| ---------------- | ----------------- | --------------------------------------- |
| Active Directory | Domain Controller | Servidor que gestiona AD                |
| LDAP (OpenLDAP)  | slapd             | Servidor que gestiona el directori LDAP |

**slapd = Domain Controller però en format lliure i Linux.**

---

# **7. Resum final**

`slapd` és:

* el **servidor LDAP** d’OpenLDAP,
* un procés que s’executa en Linux,
* el responsable de gestionar el directori,
* la peça central de qualsevol instal·lació LDAP,
* l'alternativa oberta a un **Domain Controller** d’Active Directory.

Sense `slapd`, un directori LDAP **no existeix**.
