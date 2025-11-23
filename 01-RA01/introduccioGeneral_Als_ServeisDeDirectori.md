# **1. Introducció general als serveis de directori**

## **1.1. Per què existeixen els serveis de directori? Context i motivació**
En qualsevol organització moderna (universitat, institut, empresa tecnològica, administració pública...) la infraestructura digital està formada per nombrosos elements:

* Usuaris (treballadors, estudiants, personal extern...)
* Ordinadors personals i portàtils
* Servidors i màquines virtuals
* Impressores i recursos compartits
* Aplicacions internes: ERP, CRM, Moodle, VPN…

Tots aquests elements necessiten **identitat digital** i **permisos**.

### Problema sense servei de directori

Si cada equip mantingués la seva pròpia llista d’usuaris:

* Contrasenyes duplicades o desincronitzades
* Permisos inconsistents
* Caos administratiu
* Brexes de seguretat
* Dificultat extrema per escalar infraestructures
* Pèrdua de control en canvis de personal

Aquest escenari és insostenible a mitjà termini.

### Solució: serveis de directori

Els serveis de directori proporcionen:

* **Autenticació centralitzada**
* **Autoria i permisos** des d’un únic punt
* **Identitat única corporativa**
* **Jerarquia organitzada** per usuaris, equipament i recursos
* **Auditoria i seguretat integrades**
* **Automatització de processos**

---

## **1.2. Definició d’un servei de directori**

> *Un servei de directori és una base de dades jeràrquica, optimitzada per consultes ràpides, que emmagatzema informació d’identitat i configuracions, i permet administrar l’accés a recursos i sistemes d’una manera centralitzada.*

Conceptes clau:

* **Base de dades jeràrquica** (no relacional)
* **Optimitzada per consulta**, no per transaccions
* **Identitat i permisos** com a primera funció
* **Estructura d’arbre**
* **Esquema que defineix els objectes**
* **Integració amb protocols d’autenticació**

### Comparativa ràpida

| Aspecte    | Base de dades SQL | Servei de directori        |
| ---------- | ----------------- | -------------------------- |
| Estructura | Taules            | Arbre (DIT)                |
| Operacions | CRUD complex      | Consultes ràpides          |
| Ús         | Aplicacions       | Identitat/seguretat        |
| Protocol   | SQL               | LDAP                       |
| Exemples   | MySQL, PostgreSQL | Active Directory, OpenLDAP |

---

## **1.3. Evolució històrica**

### **X.500 (1988)**

Primer model formal de directori corporatiu. Molt robust, però pesat i poc pràctic.

### **LDAP (1993–1997)**

Neix com una alternativa lleugera a X.500:

* Lleuger
* Sobre TCP/IP
* Ideal per Internet
* Estàndard oberts
* Implementable en Linux (OpenLDAP)

### **Active Directory (1999)**

Microsoft integra:

* LDAP (consultes)
* Kerberos (autenticació)
* DNS (localització de serveis)
* RPC (gestió interna)

Active Directory esdevé el **estàndard empresarial**.

---

## **1.4. Problemes reals que un servei de directori resol**

### Sense servei de directori

Un institut amb 900 usuaris podria tenir:

* 900 comptes diferents per aula
* Contradictòries polítiques de seguretat
* Professors sense accés entre aules
* Alumnes duplicats a centenars d’equips
* Impressió descontrolada
* Incapacitat per auditar res

### Amb servei de directori

* Únic compte corporatiu
* Perfils sincronitzats
* Polítiques centralitzades
* Estacions de treball gestionades
* Control d’auditoria i permisos

---

## **1.5. Components essencials d’un servei de directori**

### **Objectes**

Un **objecte** és la unitat bàsica d’informació dins d’un servei de directori.  
Representa una **entitat real** de l’organització que necessita una identitat digital per autenticar-se, ser gestionada o rebre permisos.

En un directori, un objecte és com una **fitxa digital** composta per atributs, i ubicada dins del DIT (Directory Information Tree).

**Tot allò que té identitat dins del sistema és un objecte**, com ara:

- **Usuaris** (professors, alumnes, tècnics…)  
- **Grups** (departaments, rols, cursos…)  
- **Equips** (ordinadors de l’aula, portàtils, servidors)  
- **Impressores** (ressources compartides)  
- **Serveis** (aplicacions, bases de dades, carpetes SMB…)  
- **Unitats organitzatives (OU)** (contenidors lògics com “Professors”, “1r ASIX”, “Aules”)

### **Què defineix exactament un objecte?**

Un objecte està format per:

1. **objectClass**  
   Indica el *tipus* d’objecte (user, group, computer, inetOrgPerson…).  
   És com el “model” o “classe” en programació.

2. **Atributs**  
   Són els *camps* o *propietats* de l'objecte (nom, cognoms, correu, UID, sistema operatiu…).

3. **DN (Distinguished Name)**  
   El *camí complet* que indica on està situat l’objecte dins del directori.  

   Exemple:  
   `cn=Hector Pascual,ou=Professors,dc=torreroja,dc=cat`

4. **Esquema**  
   Indica **quins atributs són obligatoris**, quins són opcionals i quina sintaxi han de complir.

### **Exemples visuals**

#### **Usuari LDAP:**
```

dn: cn=Hector Pascual,ou=Professors,dc=torreroja,dc=cat
objectClass: inetOrgPerson
cn: Hector Pascual
sn: Pascual
mail: [hector.pascual@torreroja.cat](mailto:hector.pascual@torreroja.cat)

```

#### **Usuari AD:**
```

givenName: Hector
sn: Pascual
sAMAccountName: hpascual
userPrincipalName: [hpascual@torreroja.cat](mailto:hpascual@torreroja.cat)
memberOf: CN=Professors,OU=Docents,DC=torreroja,DC=cat
objectGUID: a4bf2c2e-d891-4b95-b92a-...

```

### **Atributs**

Propietats que descriuen objectes.

Exemple LDAP:

```
uid: hpascual
cn: Hector Pascual
sn: Pascual
mail: hector.pascual@torreroja.cat
objectClass: inetOrgPerson
```

Exemple AD:

```
givenName: Hector
sn: Pascual
sAMAccountName: hpascual
userPrincipalName: hpascual@torreroja.cat
memberOf: CN=Professors,OU=Docents,DC=torreroja,DC=cat
objectGUID: a4bf2c2e-d891-4b95-b92a-...
```

## **Esquema**

L’esquema d’un servei de directori és el conjunt de normes que defineixen com han de ser els objectes del directori, quins atributs tenen i quin tipus o format pot tenir cada atribut.

### **1. Tipus d’objectes**

Cada objecte en un directori té una o més `objectClass`.

**Exemples d’objectes segons l’esquema:**

- `inetOrgPerson` → persones en LDAP  
- `posixAccount` → comptes Unix  
- `groupOfNames` → grups en LDAP  
- `computer` → equips en Active Directory  
- `user` → usuaris en Active Directory  
- `organizationalUnit` → unitats organitzatives (OU)

L’esquema defineix **quins objectClass existeixen** i com es poden utilitzar.

---

### **2. Quins atributs són obligatoris**

Cada `objectClass` especifica els atributs obligatoris (**MUST**) i opcionals (**MAY**) que pot tenir un objecte.

#### **Exemple en LDAP → `inetOrgPerson` (persona)**

**Atributs obligatoris (MUST):**
- `cn` (common name)
- `sn` (surname)

**Atributs opcionals (MAY):**
- `mail`
- `telephoneNumber`
- `jpegPhoto`
- `description`

#### **Exemple en Active Directory → usuari (`user`)**

**Atributs obligatoris:**
- `sAMAccountName`
- `objectClass`
- `objectGUID`

**Atributs opcionals:**
- `mail`
- `department`
- `title`
- `memberOf`

Si intentes crear un objecte sense un atribut obligatori, **el servidor de directori mostrarà un error** i rebutjarà l’operació.

---

### **3. Quin format i quina sintaxi tenen els atributs**

Cada atribut té un tipus de dada i una sintaxi específica que ha de complir.

**Exemples de sintaxi:**

- `cn` → cadena de text (UTF-8)  
- `uidNumber` → número enter  
- `gidNumber` → número enter  
- `mail` → correu electrònic amb format RFC  
- `telephoneNumber` → número amb caràcters `+` i dígits  
- `objectGUID` → valor binari  
- `jpegPhoto` → dades binàries (imatges)  
- `userPassword` → hash (SSHA, SHA-1, Kerberos, NTLM…)

L’esquema **garanteix la coherència del directori** obligant que tots els atributs compleixin el tipus i la sintaxi establerta.



### **DIT (Directory Information Tree)**

El **DIT (Directory Information Tree)** és **l’arbre jeràrquic** que utilitzen LDAP i Active Directory per organitzar tots els objectes del directori.

És la **forma visual i estructural** del directori.

> **Igual que un arbre genealògic organitza una família, el DIT organitza tots els objectes del domini.**

---

#### **Anàlisi detallat d'un DIT d'exemple**

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
├── cn=PC-A202

```

---

#### **1. Arrel del directori**

```

dc=empresa,dc=local

```

Això és la **base** o **suffix** del directori.

- `dc` = *Domain Component*  
- `empresa.local` és el nom del domini (equivalent a *torreroja.cat*)

Aquesta és la **arrel** del DIT, equivalent al `/` d’un sistema de fitxers.

---

#### **2. Primer nivell: OU (Organizational Units)**

```

├── ou=Professors
├── ou=Alumnes
└── ou=Equips

```

Les **OU (Organizational Units)** són com carpetes o departaments dins del directori.

Serveixen per:

- Organitzar objectes per funció, departament o rol.  
- Aplicar polítiques diferents (en AD).  
- Delegar administració a persones concretes.  
- Separar alumnes, professors, equips, etc.

En aquest cas hi ha **3 OU principals**:

- **ou=Professors** → carpeta amb professors  
- **ou=Alumnes**    → carpeta amb alumnes  
- **ou=Equips**     → carpeta amb ordinadors  

---

#### **3. Segon nivell: objectes dins de cada OU**

Dins de cada OU hi ha **objectes** que representen persones o equips.

---

##### **3.1 Dins de Professors**

```

│    ├── cn=Hector Pascual
│    └── cn=Montse Grau

```

Són **objectes d’usuari** (`cn = Common Name`), que representen professors.

---

##### **3.2 Dins d’Alumnes**

```

│    ├── cn=Joel Font
│    └── cn=Paula Ruiz

```

També són **objectes d’usuari**, però ubicats a l’OU d’alumnes.

Això permet:

- aplicar permisos diferents  
- separar perfils  
- gestionar-los de manera independent  

---

##### **3.3 Dins d’Equips**


```
  ├── cn=PC-A201
  ├── cn=PC-A202
```


Objectes de tipus **computer** (en AD) o objectes genèrics d’equip (en LDAP).

Representen equips físics d’aula:

- També s’autentiquen al domini  
- Reben polítiques i configuracions  
- Tenen permisos assignats  

---

#### **4. Interpretació del DIT complet**

##### **Vista conceptual simplificada:**

```

DOMINI (empresa.local)
│
├── PROFESSORS
│     ├── Hector Pascual
│     └── Montse Grau
│
├── ALUMNES
│     ├── Joel Font
│     └── Paula Ruiz
│
└── EQUIPS
├── PC-A201
└── PC-A202

```

##### **Significat:**

- El directori gestiona **persones** i **ordinadors**.  
- Els objectes estan **organitzats per funció o categoria**.  
- La jerarquia reflecteix l’estructura de l’organització.  
- Cada objecte té un **DN únic** (identificador complet).

Exemples:

**Professor:**
```

cn=Hector Pascual,ou=Professors,dc=empresa,dc=local

```

**Equip:**
```

cn=PC-A201,ou=Equips,dc=empresa,dc=local

```

Un DN és com la **seva adreça postal** dins del directori.

---


## **1.6. Arquitectura general d’un servei de directori**

L’arquitectura d’un servei de directori defineix **com s’organitzen, comuniquen i funcionen** tots els components implicats en la gestió d’identitats, autenticació i permisos. Tant si estem parlant d’Active Directory com d’un directori LDAP pur, aquesta arquitectura sempre segueix el mateix patró: **clients que fan consultes**, **servidors que responen** i **protocols que regulen la comunicació**.

---

### **1. Clients**

Els **clients** són tots aquells dispositius, aplicacions o serveis que necessiten:

- autenticar-se,
- consultar informació del directori,
- validar credencials,
- obtenir permisos o configuracions.

No necessàriament són persones; poden ser equips, aplicacions, servidors o serveis de fons.

#### **Exemples de clients:**

- **Un equip Windows** que vol iniciar sessió en un domini Active Directory.  
- **Un servidor Moodle**, que valida usuaris utilitzant LDAP o AD.  
- **Un firewall o VPN**, que pregunta al directori si un usuari està autoritzat.  
- **Una aplicació corporativa**, que comprova si un usuari té permisos dins d’un grup.  
- **Un equip Linux**, configurat per autenticar-se contra un directori LDAP.

Cada client executa operacions com:
- Buscar informació.
- Validar contrasenya.
- Obtenir atributs (correu, grups, rols).
- Comprovar permisos.

> **En resum:** un client és qualsevol cosa que “pregunta” al directori.

---

### **2. Servidor de directori**

El servidor de directori és el **cor central** del sistema. Gestiona dades d’identitat, aplica polítiques i respon consultes exterior.

Segons la tecnologia pot ser:
- **slapd** → servidor LDAP pur (a Linux, OpenLDAP).  
- **Domain Controller (DC)** → en Active Directory, que integra:  
  - LDAP (consultes)  
  - Kerberos (autenticació)  
  - DNS (localització)  
  - serveis d’autoritat certificadora, si cal  

El servidor de directori s’encarrega de:
- Guardar i organitzar tots els objectes (usuaris, equips, grups…).  
- Resoldre consultes LDAP d’usuaris o aplicacions.  
- Validar la identitat d’un usuari o equip.  
- Emmagatzemar informació de permisos i membres de grup.  
- Replicar dades amb altres servidors (en AD).  
- Aplicar polítiques (GPO) a les màquines del domini.

> **Pots pensar-hi com el “cervell” del domini.**

---

### **3. Protocols**

Els protocols defineixen **com es comuniquen** el client i el servidor de directori. En un entorn corporatiu real hi intervenen diversos protocols, cadascun amb una funció molt específica:

---

#### **a) LDAP — Consultes i gestió d’objectes**

LDAP és el protocol principal per:

- buscar informació (search),
- crear objectes (add),
- modificar-los (modify),
- eliminar-los (delete).

S’utilitza en:

- Active Directory  
- OpenLDAP  
- AD-LDS  
- Sistemes Unix integrats a LDAP  

És lleuger, ràpid i optimitzat per a consultes.

```

Client → LDAP → Directori

```

---

#### **b) Kerberos — Autenticació segura**

En Active Directory, Kerberos és el mecanisme que:

- verifica la identitat dels usuaris,
- genera tickets de seguretat,
- permet Single Sign-On,
- evita enviar contrasenyes per la xarxa.

Kerberos no és opcional en AD:  
**és el protocol d’autenticació per defecte del domini**.

```

Client → Kerberos → Domain Controller (KDC)

```

---

#### **c) TLS/SSL — Xifratge**

Tant LDAP com Kerberos poden treballar amb TLS/SSL per protegir la comunicació.

- Permet *LDAPS* (LDAP xifrat).  
- Garanteix que la comunicació no és interceptada.  
- Essencial en entorns corporatius i centres educatius.

---

#### **d) DNS — Localització del domini i serveis**

DNS és imprescindible en Active Directory.

Serveix per:

- trobar el Domain Controller més proper,  
- resoldre noms d’host,  
- localitzar serveis Kerberos, LDAP, GC (Global Catalog).

Sense DNS, un client Windows **no pot ni iniciar sessió en el domini**.

```

Client → DNS → Quin servidor gestiona el domini?

```

---

### **Flux simplificat de funcionament**

Quan un usuari inicia sessió en un domini, realment passen **tres operacions separades**, encara que l’usuari ho percebi com una sola acció.

```

[Client] --LDAP-------> [Servidor de directori]
[Client] --Kerberos---> [Domain Controller]
[Client] --DNS--------> [Servidor DNS]

```

**Interpretació:**

1. El client **pregunta al DNS** quin és el DC del domini.  
2. El client **demana un ticket Kerberos** per autenticar-se.  
3. El client **consulta via LDAP** per obtenir atributs, permisos, etc.

Tot això passa en **mil·lisegons**, i és el que fa possible:

- iniciar sessió,  
- carregar polítiques,  
- accedir a recursos,  
- identificar usuaris a aplicacions,  
- gestionar tot des del servidor.




---

## **1.7. Diagrames avançats i esquemes explicatius**

---

### **1.7.1. Arquitectura del servei de directori**

```
                       ┌──────────────────────────┐
                       │   SERVIDOR DE DIRECTORI   │
                       │ (Active Directory/LDAP)   │
                       └───────────────┬───────────┘
                                       │
               ┌───────────────────────┼────────────────────────┐
               │                       │                        │
       ┌───────▼───────┐       ┌───────▼──────┐        ┌────────▼────────┐
       │ Base de dades  │       │   Esquema    │        │   DIT (Arbre)   │
       │ (objectes)     │       │ (models)     │        │  (estructura)   │
       └────────────────┘       └──────────────┘        └─────────────────┘
```

---

### **1.7.2. Identificació d’un objecte mitjançant DN**

```
dn: cn=Hector Pascual,
    ou=Professors,
    ou=Departaments,
    dc=iescarlesvallbona,
    dc=cat
```

---

### **1.7.3. Comparació visual AD vs LDAP**

```
Active Directory (Windows)             OpenLDAP (Linux)
──────────────────────────             ─────────────────
- Domini                               - Suffix (dc=)
- Arbre                                - DIT
- Forest                               - Instància LDAP
- Controlador de Domini (DC)           - slapd
- OU, usuaris, grups                   - OU, objectClass POSIX
- Kerberos                             - SASL/Kerberos opcional
- GPO                                  - No equivalent directe
```

---

### **1.7.3. Creació d’un usuari LDAP (LDIF)**

Fitxer:

```
dn: cn=pau.girona,ou=Informatica,dc=empresa,dc=local
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: Pau Girona
sn: Girona
uid: pgirona
uidNumber: 1101
gidNumber: 1101
homeDirectory: /home/pgirona
loginShell: /bin/bash
mail: pau.girona@empresa.local
userPassword: {SSHA}z7Jgwf…
```

Comanda:

```
ldapadd -x -D "cn=admin,dc=empresa,dc=local" -W -f usuari.ldif
```

---

### **1.7.5. Consultes LDAP avançades**

**Llista d’usuaris del departament d’informàtica:**

```
ldapsearch -LLL -x -b "ou=Informatica,dc=empresa,dc=local" "(objectClass=inetOrgPerson)"
```

**Cercar cognoms:**

```
ldapsearch -x "(sn=Grau)"
```

---

### **1.7.6. Exemple d’Active Directory (PowerShell)**

**Crear un usuari:**

```
New-ADUser -Name "Hector Pascual" `
 -GivenName "Hector" `
 -Surname "Pascual" `
 -SamAccountName "hpascual" `
 -UserPrincipalName "hpascual@empresa.local" `
 -Path "OU=Professors,OU=Departaments,DC=empresa,DC=local"
```

**Afegir-lo a un grup:**

```
Add-ADGroupMember -Identity "Departament-Informatica" -Members "hpascual"
```

---

### **1.7.7. Autenticació AD des de Linux**

```
kinit hpascual@EMPRESA.LOCAL
ldapsearch -Y GSSAPI -H ldap://dc01.empresa.local -b "dc=empresa,dc=local"
```
