# **2. Esquema del AD**

## **2.1. Introducció a l'esquema d'un AD**

En un servei de directori, ja sigui **Active Directory (AD)** o **OpenLDAP**, l'**esquema** és el component fonamental que defineix quins objectes pot contenir el directori i quina forma han de tenir

És el **model de dades** del directori, l'equivalent al *plànol estructural* que garanteix:
* coherència
* integritat
* compatibilitat entre les entrades

L'esquema determina:
* quines **classes d'objecte** poden existir
* quins **atributs obligatoris** (MUST) i opcionals (MAY) conté cada objecte
* la **sintaxi** i tipus de dades que poden tenir els atributs
* la **jerarquia** i relacions dins del DIT (Directory Information Tree)
* les **regles de validació** i restriccions

Sense un esquema, el directori seria un contenidor caòtic, incoherent i insegur

L'esquema és el mecanisme que garanteix que cada entrada compleix una definició formal i validable

<br>
<br>

## **2.2. Components de l'esquema: atributs, classes i regles**

### **2.2.1. Atributs (Attributes)**

Un atribut és una **unitat bàsica d'informació** dins d'un objecte

Un atribut té:
* **Nom:** cn, uid, mail, sn…
* **Sintaxi:** el format que pot tenir el valor
* **Regles de cardinalitat:** SINGLE-VALUE o MULTI-VALUE
* **Comparadors:** com s'avaluen per ordenar o comparar

Ex real:
```
attributetype ( 2.5.4.3 NAME 'cn'
    DESC 'Common Name'
    SYNTAX 1.3.6.1.4.1.1466.115.121.1.15
    SINGLE-VALUE )
```

Interpretació:
* **2.5.4.3**      → OID oficial de l'atribut
* **NAME 'cn'**    → nom intern
* **DESC**         → descripció humana
* **SYNTAX**       → tipus de dada (DirectoryString)
* **SINGLE-VALUE** → només pot aparèixer una vegada

---

### **2.2.2. Sintaxis importants de l'esquema**

Els atributs han de complir una sintaxi que defineix **quins valors són vàlids**

**a. IA5String**
Cadena ASCII simple
Útil per valors que no poden contenir accents ni caràcters especials
Ex: `uid`.

**b. DirectoryString**
Cadena Unicode en UTF-8
És la sintaxi més flexible i habitual
Ex: `cn`, `sn`, `description`

**c. DN (Distinguished Name)**
Cadena que representa un **camí complet dins del DIT**
Ex:
```
uid=jlopez,ou=Usuaris,dc=empresa,dc=local
```

**d) NumericString**
Només conté dígits
Ex: `employeeNumber`

**e) Boolean**
Només pot ser:
```
TRUE / FALSE
```

---

### **2.2.3. Classes d'objecte (objectClass)**

Una **objectClass** defineix:
* la naturalesa de l'objecte
* els atributs obligatoris (MUST)
* els atributs opcionals (MAY)
* si és estructural, abstracte o auxiliar

Les classes d'objecte es classifiquen en:

**1. Estructurals (Structural)**
Defineixen objectes concrets

Ex:
* inetOrgPerson
* posixAccount
* organizationalUnit
* device

**2. Auxiliars (Auxiliary)**
Afegeixen atributs addicionals a una classe estructural

Ex:
* shadowAccount
* sambaSamAccount

**3. Abstractes (Abstract)**
Base sobre la qual hereten altres classes

Ex:
* top

<br>
<br>

## **2.3. Classes predeterminades més habituals**

### **OpenLDAP**

**Classes d'usuari:**
* **inetOrgPerson** → usuaris humans (atributs personals i de contacte)
* **posixAccount**  → atributs per login Unix
* **shadowAccount** → control de contrasenyes Unix

**Classes de grup:**
* **posixGroup** → grups Unix

**Infraestructura i dispositius:**
* **organizationalUnit**
* **device**
* **ipHost**

### **Active Directory**

**Usuaris i grups:**
* **user**
* **group**

**Infraestructura:**
* **computer**
* **organizationalUnit**
* **contact**

<br>
<br>

## **2.4. Ex complet d'un esquema real**

A continuació tens un fragment real d'esquema (simplificat):
```
objectclass ( 2.16.840.1.113730.3.2.2
    NAME 'inetOrgPerson'
    SUP organizationalPerson
    STRUCTURAL
    MUST ( cn $ sn )
    MAY  ( userPassword $ mail $ telephoneNumber $ uid $
           givenName $ displayName )
)
```

Interpretació:
* Es tracta de la classe **inetOrgPerson**
* Hereta d'**organizationalPerson**
* Ha de tenir obligatòriament **cn** i **sn**
* Pot tenir atributs addicionals com **mail**, **uid**, **telephoneNumber**


## **2.5. Combinació de classes: un objecte amb múltiples funcions**

LDAP permet que un mateix objecte contingui **múltiples objectClass**, de manera que un usuari pot complir molts rols alhora

Ex:
```
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
```

Això permet:
* Autenticació al directori     → `inetOrgPerson`
* Login Unix (UID/GID)          → `posixAccount`
* Control de contrasenya Unix   → `shadowAccount`
* Definir homeDirectory i shell → `posixAccount`

**Un sol objecte > un usuari LDAP i un usuari Linux simultàniament**

---

## **2.6. El DIT (Directory Information Tree)**

El DIT és l'arbre jeràrquic que organitza totes les entrades del directori
És comparable a un *arbre de carpetes*

Ex:
```
dc=empresa,dc=local
 ├── ou=Usuaris
 │     ├── uid=aribas
 │     └── uid=mpuig
 ├── ou=Grups
 │     ├── cn=professors
 │     └── cn=administradors
 └── ou=Equips
       ├── cn=PC01
       └── cn=PC02
```

La profunditat i organització del DIT afecta directament:
* rendiment
* permisos
* administració
* seguretat

---

## **2.7. Nomenclatura LDAP: DN, RDN i components**

### 🔹 **DN (Distinguished Name)**

És l'identificador **únic i absolut** d'una entrada

Ex:
```
uid=mcarbo,ou=Usuaris,dc=empresa,dc=local
```

### 🔹 Parts del DN:

* **RDN:** `uid=mcarbo`
* **Contenidor superior:** `ou=Usuaris`
* **Arrel del domini:** `dc=empresa,dc=local`

**DN = RDN + contenidors + DCs**

---

## **2.8. Grups POSIX**

Un grup POSIX és un grup compatible amb sistemes Unix

Atributs:

* **cn:** nom del grup
* **gidNumber:** identificador numèric del grup
* **memberUid:** usuaris membres (UIDs)

Ex:
```
dn: cn=professors,ou=Grups,dc=empresa,dc=local
objectClass: posixGroup
gidNumber: 5020
memberUid: aribas
memberUid: mpuig
```

---

## **2.9. LDIF: definició, funcions i Ex**

**LDIF (LDAP Data Interchange Format)** és un format de text per:
* crear objectes
* modificar objectes
* importar dades massives
* definir esquemes
* fer backup de parts del directori

Ex: alta d'un usuari POSIX + inetOrgPerson
```
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
userPassword: {SSHA}34KJ34lkj324lkj23l4kj==
```

---

## **2.10. Diferència entre esquemes estàndard i esquemes personalitzats**

* Esquemes estàndard d'OpenLDAP 
* **core.schema**
* **inetorgperson.schema**
* **nis.schema**
* **cosine.schema**

* Esquema per defecte d'Active Directory 
* **user**
* **group**
* **computer**
* **organizationalUnit**

* Esquemes personalitzats. Quan cal un atribut propi, cal crear-lo

Ex real (simplificat):
```
attributetype ( 1.3.6.1.4.1.99999.1.1
    NAME 'departamentID'
    DESC 'Identificador intern de departament'
    SYNTAX 1.3.6.1.4.1.1466.115.121.1.15 )
```

Això defineix un atribut personalitzat **departamentID**

---

## **2.11. Bones pràctiques en la definició de l'esquema**

### **1. No crear OUs amb noms de persones**

Perquè:
* són estructures temporals
* la persona pot marxar o canviar de rol
* trenca la lògica d'organització
* dificulta permisos i polítiques

---

### **2. Separar clarament els tipus d'objectes**

Ex correcte:
```
ou=Usuaris
ou=Equips
ou=Grups
ou=Departaments
```

Evita confusions i facilita:
* delegació
* permisos
* scripts
* manteniment

---

### **3. Política estricta de nomenclatura**

Objectius:
* evitar duplicats
* facilitar l'administració
* mantenir consistència global

Bones pràctiques:
* `uid` sempre en minúscules
* grups en plural (`cn=professors`)
* equips amb prefix (`PC-A101`)

---

### **4. No modificar esquemes estàndard**

És molt perillós perquè:
* pot trencar l'inici del servei (slapd)
* pot fer inservible AD
* és inreversible sense backup
* afecta tota la infraestructura

---

### **5. Evitar DIT massa profunds**

Un DIT excessivament llarg:
```
OU=Barcelona → OU=Edifici A → OU=Planta 3 → OU=Pasillo Est → ...
```

Provoca:
* polítiques difícils de gestionar
* permisos incontrolables
* recerques lentes
* confusió a llarg termini

