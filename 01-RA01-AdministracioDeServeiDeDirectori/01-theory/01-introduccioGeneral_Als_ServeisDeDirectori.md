# 📂 **01. Introducció general als serveis de directori**

## 🔸**1.1. Per què existeixen els serveis de directori?**
En qualsevol organització moderna conviuen molts elements digitals:
- Usuaris (professors, alumnes, personal tècnic…)  
- Ordinadors i portàtils  
- Servidors i màquines virtuals  
- Impressores i recursos compartits  
- Aplicacions corporatives (Moodle, VPN, ERP, NAS…)

Tots aquests elements necessiten **identitat digital** i **permisos d’accés**.

### ▫ Sense servei de directori
Si cada equip mantingués la seva pròpia llista d’usuaris:
- contrasenyes diferents a cada PC,  
- permisos inconsistents,  
- dificultat extrema per fer altes i baixes,  
- manca d’auditoria i de seguretat,  
- duplicació d’usuaris a desenes o centenars d’equips.

### ▫ Amb servei de directori
Apareixen beneficis immediats:
- autenticació centralitzada,  
- únic compte corporatiu,  
- perfils i permisos coherents,  
- aplicació uniforme de polítiques,  
- administració fàcil i escalable a centenars d’usuaris.

## 🔸**1.2. Què és un servei de directori? Definició**
Un servei de directori és una **base de dades jeràrquica**, optimitzada per consultes ràpides, que emmagatzema i gestiona la informació d’identitat, permisos i configuracions d’una organització.

### ▫ Característiques principals:
- **Arbre jeràrquic (DIT)** en lloc de taules SQL  
- Pensat per **consultes molt ràpides**  
- Manté **objectes** (usuaris, grups, equips…)  
- Cada objecte té **atributs** (nom, correu, UID…)  
- Inclou un **esquema** que defineix quins objectes i atributs existeixen  
- S’integra amb protocols d’autenticació i seguretat

### ▫ Comparació breu

| Aspecte | Base de dades SQL | Servei de directori |
|--------|-------------------|---------------------|
| Estructura | Taules | Arbre DIT |
| Ús | Dades d'aplicacions | Identitat i permisos |
| Protocol | SQL | LDAP |
| Optimització | CRUD | Consultes ràpides |
| Exemples | MySQL, PostgreSQL | Active Directory, OpenLDAP |

## 🔸**1.3. Evolució i context tecnològic**

### ▫ **X.500 (1988)**
Primer model formal d’un directori corporatiu. Molt complet però pesat.

### ▫ **LDAP (1993–97)**
Versió lleugera de X.500, dissenyada per funcionar sobre TCP/IP i integrar-se en entorns Unix i Internet.

Base de:
- OpenLDAP (Linux)
- Integracions amb milers d’aplicacions web

### ▫ **Active Directory (1999)**
Microsoft combina:
- LDAP per consultes,  
- Kerberos per autenticació,  
- DNS per localització del domini.

Es converteix ràpidament en l’estàndard empresarial.

## 🔸**1.4. Problemes reals que un servei de directori resol**

Exemple real d’un institut:

▫ **Sense directori:**
- 900 comptes repetits a cada aula  
- alumnes duplicats  
- professors sense permisos coherents  
- impressió i xarxa descontrolada  
- impossibilitat d’auditar

▫ **Amb directori:**
- identitat única per usuari  
- permisos centralitzats  
- gestió simplificada d’alumnes i professors  
- configuracions d’equip homogènies  
- seguretat i auditoria

## 🔸**1.5. Components essencials d’un servei de directori**

### **Objectes**
Un **objecte** és una representació digital d’una entitat real: usuari, equip, grup, impressora, servei…

És una “fitxa digital” amb atributs.

▫ Exemples:
- **Usuari** → nom, cognom, correu, UID…  
- **Grup** → membres que formen part del grup  
- **Equip** → nom, adreça, identitat de la màquina  
- **OU** → contenidor organitzatiu

---

### **Atributs**
Cada objecte es descriu amb atributs.

Exemples LDAP:
```
uid: hpascual
cn: Hector Pascual
sn: Pascual
mail: [hector.pascual@empresa.local](mailto:hector.pascual@empresa.local)
objectClass: inetOrgPerson

```

Exemples AD:
- sAMAccountName  
- userPrincipalName  
- memberOf  
- objectGUID  

### **Esquema**
L’esquema defineix:

1. Tipus d’objectes disponibles (objectClass).  
2. Atributs obligatoris (MUST).  
3. Atributs opcionals (MAY).  
4. Sintaxi i format de cada atribut.

Exemples:
- `inetOrgPerson` → requereix `cn` i `sn`  
- `user` d’Active Directory → requereix `sAMAccountName`, `objectClass`

## 🔸**1.6. El DIT: estructura jeràrquica del directori**

El **DIT (Directory Information Tree)** organitza tota la informació.

Exemple:

```
dc=empresa,dc=local
├── ou=Professors
│    ├── cn=Hector Pascual
│    └── cn=Montse Grau
├── ou=Alumnes
│    ├── cn=Joel Font
│    └── cn=Paula Ruiz
└── ou=Equips
├── cn=PC-A201
└── cn=PC-A202

```

▫Interpretació:
- `dc=` indica el domini (empresa.local)  
- Cada `ou=` és una unitat organitzativa (departaments, perfils…)  
- Cada `cn=` és un objecte concret (usuari, equip…)  
- Cada objecte té un **DN únic**, com una adreça postal digital.

## 🔸**1.7. Arquitectura general d’un servei de directori**

Un servei de directori està format per:

### **1.7.1 Clients**
Ordinadors, aplicacions i serveis que demanen autenticació o consulten informació.  
Exemples: Windows, Moodle, VPN, SAMBA, NAS…

### **1.7.2 Servidor de directori**
- **slapd** en OpenLDAP (Linux)  
- **Domain Controller** en Active Directory (Windows)

### **1.7.3 Protocols**
- **LDAP** → consultes i modificacions  
- **Kerberos** → autenticació  
- **TLS/SSL** → xifratge  
- **DNS** → localització del domini

## 🔸**1.8. Exemples bàsics d’operacions LDAP**

### Cerca d'objectes
```
ldapsearch -x -LLL -H ldap://localhost -b "dc=empresa,dc=local"

```

### Afegir un objecte
```

ldapadd -x -D "cn=admin,dc=empresa,dc=local" -W -f usuari.ldif

```

### Modificar un objecte
```

ldapmodify -x -D "cn=admin,dc=empresa,dc=local" -W -f canvi.ldif

```

Aquestes operacions les processa el servidor `slapd`.

