# **CAPÍTOL 2 – ESQUEMA DEL SERVEI DE DIRECTRECTORI**

## **2.1. Introducció a l’esquema dins un servei de directori**

En qualsevol servei de directori, ja sigui **Active Directory (AD)** o **OpenLDAP**, l’**esquema** és el component que defineix la **estructura lògica de la informació**

Actua com el *contracte formal* que determina:

* quins **tipus d’objectes** poden existir
* quins **atributs** han de tenir aquests objectes
* quines **restriccions** i **sintaxi** s’aplica a cada atribut
* com es relacionen els objectes dins del **DIT (Directory Information Tree)**

Podem comparar **l’esquema del directori** amb:

* el **model entitat-relació** en una base de dades
* el **JSON Schema** en documents JSON
* la **classe base** en programació orientada a objectes

Sense un esquema, el directori seria un contenidor desordenat i inconsistent.


> *L’esquema és una part fonamental del directori, ja que proporciona la definició formal de les entrades i determina el conjunt d’objectes que poden existir*

## **2.2. Components fonamentals de l’esquema**

### **2.2.1 Attributes**

Cada objecte del directori està format per atributs.

Els atributs tenen:

* **Nom:** cn, sn, uid, mail, userPassword…
* **Sintaxi:** cadena, DN, enter, booleà…
* **Regles:**

  * atribut obligatori (MUST)
  * atribut opcional (MAY)
  * atribut únic o repetible

El llibre defineix sintaxis com:

* *IA5String*
* *DirectoryString*
* *DN*
* *NumericString*
* *Boolean*

Exemple d’atribut en definició d’esquema:

```
attributetype ( 2.5.4.3 NAME 'cn'
    DESC 'Common Name'
    SYNTAX 1.3.6.1.4.1.1466.115.121.1.15
    SINGLE-VALUE )
```

---

### **2.2.2 objectClass**

Una classe defineix el tipus d’objecte i quins atributs pot contenir

Tipus:

* **Structural:** defineixen objectes (person, inetOrgPerson, posixAccount)
* **Auxiliary:** afegeixen atributs extra a objectes existents
* **Abstract:** no es poden instanciar (top és la base de totes)

Exemple (classe inetOrgPerson):

```
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
```

Atributs obligatoris:

* cn
* sn

Atributs opcionals:

* mail
* uid
* userPassword
* telephoneNumber

---

### **2.2.3 Relació entre classes d’objecte**

Un objecte LDAP pot contenir **múltiples objectClass**, fet que li dona molta flexibilitat.
Ex:

```
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
```

Això permet que **un mateix objecte** serveixi per:

* autenticar-se al directori
* tenir UID/GID per login Unix
* tenir homeDirectory definit
* tenir shell assignada

---

## **2.3. DIT (Directory Information Tree)**

DIT és la representació jeràrquica del directori.

> *DIT és un arbre jeràrquic similar a l’estructura d’un sistema d’arxius*

Ex:

```
dc=empresa,dc=local
 ├── ou=Usuaris
 │     ├── uid=aribas
 │     ├── uid=mcarbo
 │     └── uid=jdevesa
 ├── ou=Grups
 │     ├── cn=professors
 │     └── cn=administradors
 └── ou=Equips
       ├── cn=PC01
       └── cn=PC02
```

Característiques:

* **Organitzat per OU** (Organizational Unit)
* Entrades identificades amb **DN**
* Noms formats per **RDN + contenedor superior**

L’esquema defineix **què pot existir** en cada OU.

---

## **2.4. Nomenclatura LDAP: DN, RDN, DC, OU, CN**

La composició del DN (Distinguished Name) és clau en l’esquema

Ex:

```
dn: uid=mcarbo,ou=Usuaris,dc=empresa,dc=local
```

Components:

* **uid=mcarbo** → RDN
* **ou=Usuaris** → unitat organitzativa
* **dc=empresa,dc=local** → domini base

Ex de diferents objectes:

### **Usuari**

```
dn: uid=jlopez,ou=Personal,dc=empresa,dc=local
cn: Jordi López
sn: López
mail: jlopez@empresa.local
```

### **Grup POSIX**

```
dn: cn=professors,ou=Grups,dc=empresa,dc=local
objectClass: posixGroup
gidNumber: 5010
memberUid: jlopez
memberUid: mcarbo
```

### **Equip**

```
dn: cn=PC-A101,ou=Equips,dc=empresa,dc=local
objectClass: device
objectClass: ipHost
ipHostNumber: 192.168.1.45
```

---

## **2.5. Representació d’objectes amb LDIF**

El format LDIF és fonamental per:

* importar usuaris massius
* exportar dades
* modificar entrades
* fer còpies de seguretat
* automatitzar canvis

Ex:

### **Alta d’un usuari POSIX + inetOrgPerson**

```ldif
dn: uid=mpuig,ou=Usuaris,dc=empresa,dc=local
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: Marc Puig
sn: Puig
uid: mpuig
mail: mpuig@empresa.local
uidNumber: 1105
gidNumber: 1105
homeDirectory: /home/mpuig
loginShell: /bin/bash
userPassword: {SSHA}h324h2kjh423kj4.....
```

---

## **2.6. Esquemes estàndard i personalitzats**

Diferenciació entre:

### ✔ Esquemes inclosos amb OpenLDAP:

* core
* cosine
* nis
* inetorgperson

### ✔ Esquema estès d’AD:

* user
* group
* contact
* computer
* organizationalUnit

### ✔ Esquemes personalitzats

Quan una organització necessita atributs específics.

Ex:

```
attributetype ( 1.3.6.1.4.1.99999.1.1
    NAME 'departamentID'
    DESC 'Identificador intern de departament'
    SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 )
```

Crear esquemes personals requereix:

* assignar OID únic
* definir sintaxi acceptable
* evitar conflictes amb atributs existents
* carregar l’esquema amb `ldapadd` o en `/etc/openldap/schema/`

---

## **2.7. Bones pràctiques de disseny d’esquema**

### **2.7.1 No adaptar l’esquema a persones, sinó a rols / organització**

→ No crear OUs “Pere”, “Marta”, “Jordi”.

### **2.7.2 Separar clarament tipus d’objectes**

* OU per usuaris
* OU per equips
* OU per grups
* OU administratius (ex: IT, RH)

### **2.7.3 Definir una política estricta de nomenclatura**

Exemples del llibre:

* `uid=mpuig`
* `cn=professors`
* `cn=PC-A101`

### **2.7.4 No modificar esquemes estàndard a menys que sigui imprescindible**

És molt perillós en AD i LDAP.

### **2.7.5 Estructura de DIT poc profunda però ben organitzada**

Evitar arbres com:

```
OU=Barcelona > OU=Edifici A > OU=Planta 3 > OU=Pasillo Est > ...
```
