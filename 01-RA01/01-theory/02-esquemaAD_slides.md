---
marp: true
paginate: true
theme: default

header: "RA01 - Servei de Directori · Esquema del Servei de Directori"
footer: "Mòdul 0374 - Administració de Sistemes Operatius"

style: |
    header, footer {
        display: block;
        width: 100vw;
        font-size: .45rem;
        color: #bbbbbcff;
        z-index: 10;
    }
    header { text-align: right !important; padding-right: 50px; }
---

<!-- Slide 1 -->
# RA01 - Esquema del Servei de Directori  
### Capítol 2 - Introducció a l'Esquemax    

**Mòdul 0374** - Administració de Sistemes Operatius  
**Professor**: **Hèctor Pascual**

---

<!-- Slide 2 -->
# 🔸1.0 Objectius del capítol

Al final del capítol sabràs:

- Definir què és un **esquema** en AD/LDAP  
- Distingir **objectClass**, **atributs**, **syntaxis**, **DIT**, **DN**, **RDN**  
- Llegir fragments reals d'esquema AD i LDAP  
- Crear i interpretar objectes via **LDIF**  
- Aplicar bones pràctiques de disseny d'estructura del directori

---

<!-- Slide 3 -->
# 🔸2.0 Què és l'esquema del directori?

L'**esquema** és el **model de dades formal** del directori

Defineix:

- Quin tipus d’objectes poden existir  
- Quins atributs pot tenir cada objecte  
- Quina sintaxi i quines regles segueixen  
- **Com s'organitza la informació internament**
   - En LDAP → dins del **DIT (Directory Information Tree)**
   - En AD → dins de l’arquitectura **Domain / Tree / Forest**
- Què és vàlid i què no en el directori

> És com l'esquema d'una BBDD però en format jeràrquic

---

<!-- Slide 4 -->
# 🔸3.0 Components principals de l'esquema

- **objectClass** → tipus d’objecte  
- **Atributs** → informació de cada objecte  
- **Sintaxis** → tipus de dades permeses  
- **Regles de cardinalitat** → SINGLE/MULTI VALUE  
- **Estructura jeràrquica interna del directori**
   - En LDAP → **DIT (Directory Information Tree)**
   - En AD   → **Domain / Tree / Forest**
- **DN / RDN** → adreces completes dels objectes

---

<!-- Slide 5 -->
# 🔸4.0 Atributs (nom, sintaxi i cardinalitat)

Un atribut conté:

- **Nom**: `cn`, `sn`, `uid`, `mail`…  
- **Sintaxi**: tipus de dada  
- **Cardinalitat**:  
  - `SINGLE-VALUE`  
  - `MULTI-VALUE`

Ex:

- `cn = "Marc Puig"`  
- `memberUid = { "mpuig", "aribas" }`

---

<!-- Slide 6 -->

# 🔸5.0 Sintaxis LDAP (tipus de dades)

### DirectoryString  
Text Unicode → `cn`, `sn`, `description`

### IA5String  
ASCII pur → `uid`, `loginName`

### NumericString  
Només dígits → `employeeNumber`

### Boolean  
`TRUE` / `FALSE`

---

<!-- Slide 7 -->

# 🔸5.1 Sintaxis en Active Directory (AD)

A diferència de LDAP, Active Directory **no utilitza les sintaxis X.500**

AD utilitza **tipus de dades propis de Microsoft**:

### 🔹 UnicodeString  
Per noms i textos (`cn`, `sAMAccountName`, `displayName`)

### 🔹 Integer / LargeInteger  
Per identificadors, timestamps (`userAccountControl`, `pwdLastSet`)

### 🔹 Boolean  
`TRUE` / `FALSE`

### 🔹 SID (Security Identifier)  
Identificador únic de seguretat a Windows

### 🔹 Object (DN + Binary)  
Enllaços interns entre objectes

> AD implementa LDAP, però la seva **sintaxi interna és diferent i més orientada a Windows**

---

<!-- Slide 7 -->

# 🔸6.0 Sintaxi DN (Distinguished Name)

Representa l'**adreça completa** d'un objecte:

```text
uid=mpuig,ou=Professorat,ou=Departament,dc=ins-torreraja,dc=cat
```

* **RDN:** `uid=mpuig`
* **OUs:** Professorat, Departament
* **Domini:** ins-torreraja.cat

> Adreça postal al revés: del més concret → al més general.

---

<!-- Slide 8 -->

# 🔸7.0 Exemple d'esquema AD real (classe `user`)

```text
classSchema
 cn: user
 objectClassCategory: 1
 subClassOf: organizationalPerson
 mayContain:
  - displayName
  - telephoneNumber
  - userPrincipalName
  - sAMAccountName
mustContain:
  - objectClass
systemMustContain:
  - objectSid
```

Comentaris:

* `objectClassCategory: 1` → estructural
* `objectSid` → identificador únic de Windows (SID)

---

<!-- Slide 9 -->

# 🔸8.0 Definició d'atribut (`attributetype`)

```text
attributetype ( 2.5.4.3
  NAME 'cn'
  DESC 'Common Name'
  SYNTAX DirectoryString
  SINGLE-VALUE )
```

Explicació:

* OID: `2.5.4.3`
* Sintaxi: **DirectoryString**
* Només un valor (`SINGLE-VALUE`)

---

<!-- Slide 10 -->

# 🔸9.0 Tipus d'objectClass

### Estructurals

Descriuen el tipus real (persona, equip, OU…)

### Auxiliars

Afegeixen atributs extra (`shadowAccount`, `sambaSamAccount`)

### Abstractes

Base d'herència (`top`)

---

<!-- Slide 11 -->

# 🔸10.0 ObjectClass més habituals

### En OpenLDAP

* `inetOrgPerson`
* `posixAccount`
* `shadowAccount`
* `posixGroup`
* `organizationalUnit`

### En Active Directory

* `user`
* `group`
* `computer`
* `organizationalUnit`

---

<!-- Slide 12 -->

# 🔸11.0 Exemple complet d'un objectClass

```text
objectclass ( 2.16.840.1.113730.3.2.2
  NAME 'inetOrgPerson'
  SUP organizationalPerson
  STRUCTURAL
  MUST ( cn $ sn )
  MAY  ( mail $ uid $ telephoneNumber )
)
```

* Obligatori (`MUST`): `cn`, `sn`
* Opcional (`MAY`): `uid`, `mail`…

---

<!-- Slide 13 -->

# 🔸12.0 Objecte multirol (multi-classes)

```text
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
```

Un sol objecte pot ser:

* Persona LDAP
* Usuari Unix
* Usuari amb política de contrasenya

---

<!-- Slide 14 -->

# 🔸13.0 El DIT (Directory Information Tree)

```text
dc=empresa,dc=local
 ├── ou=Usuaris
 │     ├── uid=aribas
 │     └── uid=mpuig
 ├── ou=Grups
 │     └── cn=professors
 └── ou=Equips
       └── cn=PC01
```

---

<!-- Slide 15 -->

# 🔸14.0 DN i RDN: identificador únic

```text
uid=mcarbo,ou=Usuaris,dc=empresa,dc=local
```

* **RDN:** `uid=mcarbo`
* **Contenidor:** `ou=Usuaris`
* **Domini:** `dc=empresa,dc=local`

> DN = RDN + OUs + DCs

---

<!-- Slide 16 -->

# 🔸15.0 Grups POSIX (Unix/Linux)

```text
objectClass: posixGroup
cn: professors
gidNumber: 5020
memberUid: aribas
memberUid: mpujol
```

Atributs clau:

* `gidNumber`
* `memberUid`

---

<!-- Slide 17 -->

# 🔸16.0 LDIF: format estàndard

```text
dn: uid=jroca,ou=Usuaris,dc=empresa,dc=local
objectClass: inetOrgPerson
objectClass: posixAccount
cn: Jordi Roca
sn: Roca
uid: jroca
mail: jroca@empresa.local
```

Serveix per:

* Crear objectes
* Modificar
* Importar massivament
* Definir esquemes

---

<!-- Slide 18 -->

# 🔸17.0 Executar LDIF

### Linux (OpenLDAP)

```bash
ldapadd -x -D "cn=admin,dc=empresa,dc=local" -W -f alta.ldif
```

### Windows (AD)

```cmd
ldifde -i -f alta.ldif
```

---

<!-- Slide 19 -->

# 🔸18.0 Esquemes estàndard i personalitzats

### OpenLDAP:

* `core.schema`
* `inetorgperson.schema`
* `nis.schema`
* `cosine.schema`

### Active Directory:

* `user`, `group`, `computer`, `organizationalUnit`

> Esquemes personalitzats només quan és imprescindible.

---

<!-- Slide 20 -->

# 🔸19.0 Bones pràctiques

* No crear OUs amb noms de persones
* Separar **usuaris / equips / grups**
* `uid` sempre en minúscules
* Grups en plural (`professors`)
* Evitar DITs massa profunds
* No modificar esquemes estàndard

---

<!-- Slide 21 -->

# 🔸20.0 Resum final

* L'esquema defineix “què pot existir”
* objectClass + atributs = model de dades
* Sintaxis = validesa dels camps
* DIT + DN/RDN = estructura i adreces
* LDIF = creació i modificació d'objectes
* AD i LDAP comparteixen base X.500 però implementació diferent

**FI**

---


