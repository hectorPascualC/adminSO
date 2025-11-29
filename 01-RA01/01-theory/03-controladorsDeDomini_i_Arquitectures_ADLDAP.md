# **03 - Controladors de domini i arquitectures AD/LDAP**

Els controladors de domini són l'element central d'un servei de directori modern, tant en entorns **Microsoft Active Directory** com en entorns basats en **OpenLDAP**. La seva funció és garantir:

* La identitat
* L'autenticació
* La seguretat
* La disponibilitat dels recursos corporatius

---

## **3.1. Concepte de Controlador de Domini (DC)**

Un **Controlador de Domini (Domain Controller)** és un servidor que conté la **base de dades del directori** i que s'encarrega de:

* Autenticació d'usuaris i equips
* Autorització d'accés a recursos
* Replicació d'informació entre servidors
* Aplicació de polítiques de seguretat
* Gestió d'objectes del domini

En AD, els DC utilitzen **NTDS.dit**, **Kerberos**, **DNS** i un sistema de replicació multimàster

En LDAP, el paper equivalent és el servidor **slapd**, que gestiona el DIT i processa totes les operacions del protocol LDAP.

### **Diagrama simplificat del rol d'un DC**

```
       ┌─────────────────────────────────┐
       │      Controlador de Domini      │
       │           (AD / LDAP)           │
       ├─────────────────────────────────┤
       │ Autenticació (Kerberos / SASL)  │
       │ Autorització d'objectes         │
       │ Base de dades del directori     │
       │ Replicació entre servidors      │
       └───────────────┬─────────────────┘
                       │
            Autenticació / Consultes
                       │
               ┌───────▼───────┐
               │   Clients     │
               │ Windows/Linux │
               └───────────────┘
```

---

## **3.2. Components principals d'un DC (Active Directory)**

Segons el capítol dedicat a Directori Actiu del llibre, un controlador de domini d'AD incorpora diversos components crítics.

### **a. NTDS.dit - BBDD AD**

És el fitxer on es desa:

* Usuaris
* Grups
* Equips
* Unitats organitzatives (OU)
* Polítiques i configuració
* Informació global del bosc i dominis

### **b. Sistema d'autenticació: Kerberos**

Active Directory fa servir **Kerberos** per:

* Evitar contrasenyes en text pla
* Garantir autenticació segura
* Gestionar tickets de sessió

Ex de comprovació d'un ticket Kerberos:

```
klist
```
Què és [**klist**](./03a-klist.md)?

Sortida típica:

```
Client: joan@EMPRESA.LOCAL
Server: krbtgt/EMPRESA.LOCAL
Start Time: 23/11/2025 10:05
End Time:   23/11/2025 20:05
```

### **c. DNS integrat**

AD depèn del DNS per:

* Localitzar controladors
* Resoldre serveis (*SRV records*)
* Gestionar la jerarquia del domini

Ex de registres:

```
_ldap._tcp.dc._msdcs.empresa.local
_kerberos._tcp.dc._msdcs.empresa.local
```

### **d. Global Catalog**

Un DC pot actuar com a **Global Catalog**, que conté una còpia parcial del directori per accelerar cerques en tot el bosc

---

## **3.3. Rols FSMO (Flexible Single Master Operations)**

Tot i que AD utilitza una replicació [**multimàster**](./03b-multimaster.md), hi ha operacions que només pot realitzar **un únic servidor**.

Els rols FSMO són cinc:

| Rol                       | Funció                                   |
| ------------------------- | ---------------------------------------- |
| **Schema Master**         | Gestiona l'esquema del bosc              |
| **Domain Naming Master**  | Gestió de dominis del bosc               |
| **RID Master**            | Assigna blocs RID per crear SIDs únics   |
| **PDC Emulator**          | Compatibilitat amb NT i control d'horari |
| **Infrastructure Master** | Sincronització entre dominis             |

Comanda de consulta:
```
netdom query fsmo
```
Què fa aquesta [**consulta**](./03d-fsmoQuery.md)?

---

## **3.4. Arquitectura AD: Domini, Arbre i Bosc**

### **a. Domini**

Unitat bàsica d'administració:

```
empresa.local
```

### **b. Arbre (Tree)**

Què és un [**arbre**](./03e-arbre.md)?

Conjunt de dominis jeràrquics:

```
empresa.local
 └── barcelona.empresa.local
```

### **c. Bosc (Forest)**

Què és un [**bosc**](./03c-bosc.md)

Conjunt d'arbres que comparteixen:

* Esquema
* Global Catalog
* Confiança integrada

### **Diagrama conceptual**

```
Bosc
└── Arbre
    ├── Domini principal (empresa.local)
    └── Subdominis (barcelona.empresa.local, valencia.empresa.local)
```

---

## **3.5. Equivalent en LDAP: Arquitectura del servidor slapd**

En entorns Linux, un DC s'equipara al servidor **OpenLDAP (slapd)**

* En AD, el servidor central és el **Domain Controller**
* En LDAP, el servidor central és el **slapd** (daemon de LDAP)
* És una **equivalència conceptual**, NO funcional
* És per això que un ldap-server *no és* estrictament un DC, però **fa el rol de “servidor de directori” dins Linux**, igual que AD en Windows.
* Quan instal·les OpenLDAP (slapd), no estàs instal·lant un sistema complet d’identitat integral com AD, sinó un servei més que conviu al servidor
* OpenLDAP és una peça del puzzle, no un ecosistema complet com Active Directory

```diff
+--------------------------------+
| Sistema Linux (Ubuntu, RHEL…)  |
|                                |
|  ├── slapd (OpenLDAP)          |  ← Directori
|  ├── Kerberos (opcional)       |  ← Autenticació
|  ├── nslcd / sssd              |  ← Integració d’usuaris
|  ├── Samba                     |  ← Compartició / compatibilitat
|  ├── TLS/Certbot               |  ← Xifrat
|  └── Altres serveis            |
+--------------------------------+

```

Quan instal·les un DC amb AD tens:

* Servei de directori (NTDS.dit)
* Autenticació Kerberos
* LDAP integrat
* Gestió de polítiques (GPO)
* Replicació multimàster
* DNS integrat per defecte
* GC (Global Catalog)
* SAM
* RPC per a operacions de domini
* Validació d’equips/usuari
* Control d’accés complet

És un ecosistema

### **a. Funcions principals**

* Processar operacions LDAP
* Gestionar el DIT (arbre)
* Emmagatzemar objectes segons l'esquema
* Xifrar comunicacions (TLS)
* Autenticar usuaris (simple bind / SASL)

### **b. Ex d'entrada LDAP (LDIF)**

```ldif
dn: uid=jordi,ou=Usuaris,dc=empresa,dc=local
objectClass: inetOrgPerson
objectClass: posixAccount
cn: Jordi Bonet
sn: Bonet
uid: jordi
uidNumber: 1101
gidNumber: 1101
homeDirectory: /home/jordi
mail: jordi@empresa.local
```

### **c. Comanda de consulta**

```
ldapsearch -x -b "dc=empresa,dc=local" "(uid=jordi)"
```

---

## **3.6. Replicació: AD multimàster i OpenLDAP**

Tant AD com LDAP implementen mecanismes de replicació per garantir disponibilitat i tolerància a fallades

### **a. Active Directory**

* Replicació multimàster
* Intra-site (ràpida)
* Inter-site (optimitzada)

#### **Intra-site**

* Els DC estan **al mateix lloc físic** o subxarxa
* La replicació és molt freqüent (15 segons – 5 min)
* No es comprimeix: es prioritza la velocitat

#### **Inter-site**

* Els DC estan en **seus diferents**
* La replicació és més espaiada (per defecte 180 min)
* S'aplica compressió per reduir consum d’amplada de banda

Comprovar l'estat per **auditar l’estat de la replicació entre DCs**:

```
repadmin /replsummary
```

### **b. OpenLDAP**

Permet:

#### **syncrepl** (client → servidor)
És un mecanisme en què un servidor LDAP actua com **client de replicació**, i va demanant canvis al provider
```
syncrepl rid=001
  provider=ldap://ldap1.empresa.local
  type=refreshAndPersist
  searchbase="dc=empresa,dc=local"
```

#### **multi-master replication**
Tots els nodes poden escriure i replicar-se mútuament
Equivalent conceptual a AD, però:

* OpenLDAP necessita configuració explícita
* Requereix resolució de conflictes

#### **mirror mode**
Dos servidors, un és “actiu” i un “espera”:

```
LDAP1 (actiu) ↔ LDAP2 (mirall)
```

Si cau l’actiu, el mirall pot prendre el relleu

---

## **3.7. Seguretat i models d'autenticació**

### **a. Active Directory**

#### Kerberos
El veurem més endavant

#### **NTLM (compatibilitat)**

* Protocol d’autenticació antic.
* Es fa servir quan Kerberos NO és possible.
* Es manté només per compatibilitat amb dispositius antics.

#### **TLS entre controladors**

Tot i que la comunicació en AD és interna, molts entorns:

* habiliten TLS en LDAPS (port 636),
* o xifren en RPC amb Kerberos.

### **b. LDAP**

#### **Simple Bind**

* Envia DN + contrasenya.
* No xifrat → **prohibit** si no hi ha TLS.
* Xifrat (TLS) → acceptable.

#### **SASL + GSSAPI (Kerberos)**

* Autenticació moderna.
* Usa Kerberos igual que AD.
* No cal enviar contrasenya, només tickets.

#### TLS/SSL

Per connexions segures

Prova TLS:

```
openssl s_client -connect ldap.empresa.local:636
```

---

## **3.8. Eines d'administració i diagnòstic**

### **Eines AD**

#### **dsa.msc**

* GUI de gestió d’usuaris i equips.

#### **gpmc.msc**

* GUI per editar i aplicar GPO.

#### **dcdiag**

* CLI per revisar salut d’un DC.

#### **repadmin**

* CLI dedicada a replicació.

#### **RSAT**

* Paquet d’eines instal·lable des de Windows 10/11 per administrar AD remotament.

**Totes aquestes eines ja vénen en un DC, i RSAT s’instal·la a Windows 10/11**

### **Eines LDAP**

#### **ldapsearch / ldapadd / ldapmodify**

* Eines CLI incloses amb `ldap-utils`
* Funcionen per terminal
* S’instal·len amb:

```
apt install ldap-utils
```

#### **slapcat / slapadd**

* Exporten/importen la base LDAP completa.
* Funcionen localment sobre el fitxer MDB.

#### **phpLDAPadmin**

* Interfície web (GUI).
* S’instal·la al servidor amb:

```
apt install phpldapadmin
```

