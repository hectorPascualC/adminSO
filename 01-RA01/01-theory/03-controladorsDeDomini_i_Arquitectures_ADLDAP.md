# **03 – Controladors de domini i arquitectures AD/LDAP**

Els controladors de domini són l’element central d’un servei de directori modern, tant en entorns **Microsoft Active Directory** com en entorns basats en **OpenLDAP**. La seva funció és garantir la identitat, l’autenticació, la seguretat i la disponibilitat dels recursos corporatius.

A continuació es presenten els conceptes fonamentals, els components principals i els mecanismes interns que defineixen el funcionament d’un controlador de domini, seguint el mateix estil, estructura i detall que el **Capítol 2 – Esquema del servei de directori** ().

---

## **3.1. Concepte de Controlador de Domini (DC)**

Un **Controlador de Domini (Domain Controller)** és un servidor que conté la **base de dades del directori** i que s’encarrega de:

* Autenticació d’usuaris i equips
* Autorització d’accés a recursos
* Replicació d’informació entre servidors
* Aplicació de polítiques de seguretat
* Gestió d’objectes del domini

En Active Directory, els DC utilitzen **NTDS.dit**, **Kerberos**, **DNS** i un sistema de replicació multimàster.
En LDAP, el paper equivalent és el servidor **slapd**, que gestiona el DIT i processa totes les operacions del protocol LDAP.

### **Diagrama simplificat del rol d’un DC**

```
       ┌─────────────────────────────────┐
       │      Controlador de Domini      │
       │           (AD / LDAP)           │
       ├─────────────────────────────────┤
       │ Autenticació (Kerberos / SASL)  │
       │ Autorització d’objectes         │
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

Segons el capítol dedicat a Directori Actiu del llibre, un controlador de domini d’AD incorpora diversos components crítics.

### **a) NTDS.dit – Base de dades AD**

És el fitxer on es desa:

* Usuaris
* Grups
* Equips
* Unitats organitzatives (OU)
* Polítiques i configuració
* Informació global del bosc i dominis

### **b) Sistema d’autenticació: Kerberos**

Active Directory fa servir **Kerberos v5** per:

* evitar contrasenyes en text pla,
* garantir autenticació segura,
* gestionar tickets de sessió.

Exemple de comprovació d’un ticket Kerberos:

```
klist
```

Sortida típica:

```
Client: joan@EMPRESA.LOCAL
Server: krbtgt/EMPRESA.LOCAL
Start Time: 23/11/2025 10:05
End Time:   23/11/2025 20:05
```

### **c) DNS integrat**

AD depèn del DNS per:

* localitzar controladors,
* resoldre serveis (*SRV records*),
* gestionar la jerarquia del domini.

Exemples de registres:

```
_ldap._tcp.dc._msdcs.empresa.local
_kerberos._tcp.dc._msdcs.empresa.local
```

### **d) Global Catalog**

Un DC pot actuar com a **Global Catalog**, que conté una còpia parcial del directori per accelerar cerques en tot el bosc.

---

## **3.3. Rols FSMO (Flexible Single Master Operations)**

Tot i que AD usa replicació multimàster, hi ha operacions que només pot realitzar **un únic servidor**.

Els rols FSMO són cinc:

| Rol                       | Funció                                   |
| ------------------------- | ---------------------------------------- |
| **Schema Master**         | Gestiona l’esquema del bosc              |
| **Domain Naming Master**  | Gestió de dominis del bosc               |
| **RID Master**            | Assigna blocs RID per crear SIDs únics   |
| **PDC Emulator**          | Compatibilitat amb NT i control d’horari |
| **Infrastructure Master** | Sincronització entre dominis             |

Comanda de consulta:

```
netdom query fsmo
```

---

## **3.4. Arquitectura AD: Domini, Arbre i Bosc**

### **a) Domini**

Unitat bàsica d’administració, per exemple:

```
empresa.local
```

### **b) Arbre (Tree)**

Conjunt de dominis jeràrquics:

```
empresa.local
 └── barcelona.empresa.local
```

### **c) Bosc (Forest)**

Conjunt d’arbres que comparteixen:

* esquema,
* Global Catalog,
* confiança integrada.

### **Diagrama conceptual**

```
Bosc
└── Arbre
    ├── Domini principal (empresa.local)
    └── Subdominis (barcelona.empresa.local, valencia.empresa.local)
```

---

## **3.5. Equivalent en LDAP: Arquitectura del servidor slapd**

En entorns Linux, un DC s’equipara al servidor **OpenLDAP (slapd)**.

### **a) Funcions principals**

* Processar operacions LDAP
* Gestionar el DIT (arbre)
* Emmagatzemar objectes segons l’esquema
* Xifrar comunicacions (TLS)
* Autenticar usuaris (simple bind / SASL)

### **b) Exemple d’entrada LDAP (LDIF)**

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

### **c) Comanda de consulta**

```
ldapsearch -x -b "dc=empresa,dc=local" "(uid=jordi)"
```

---

## **3.6. Replicació: AD multimàster i OpenLDAP**

Tant AD com LDAP implementen mecanismes de replicació per garantir disponibilitat i tolerància a fallades.

### **a) Active Directory**

* Replicació multimàster
* Intra-site (ràpida)
* Inter-site (optimitzada)

Comprovar l'estat:

```
repadmin /replsummary
```

### **b) OpenLDAP**

Permet:

* **syncrepl** (client → servidor)
* **multi-master replication**
* **mirror mode**

Exemple de definició syncrepl al fitxer de configuració:

```
syncrepl rid=001
  provider=ldap://ldap1.empresa.local
  type=refreshAndPersist
  searchbase="dc=empresa,dc=local"
```

---

## **3.7. Seguretat i models d’autenticació**

### **a) Active Directory**

* Kerberos
* NTLM (compatibilitat)
* TLS entre controladors

### **b) LDAP**

* Simple bind (no recomanat sense TLS)
* SASL + GSSAPI (Kerberos)
* TLS/SSL per connexions segures

Prova TLS:

```
openssl s_client -connect ldap.empresa.local:636
```

---

## **3.8. Eines d’administració i diagnòstic**

### **Eines AD**

| Eina         | Funció                    |
| ------------ | ------------------------- |
| **dsa.msc**  | Gestió d’usuaris i equips |
| **gpmc.msc** | Polítiques de grup (GPO)  |
| **dcdiag**   | Diagnòstic de DC          |
| **repadmin** | Replicació                |
| **RSAT**     | Administració remota      |

### **Eines LDAP**

| Eina                     | Funció                |
| ------------------------ | --------------------- |
| **ldapsearch**           | Consultes             |
| **ldapadd / ldapmodify** | Alta/modificació      |
| **slapcat / slapadd**    | Exportació/importació |
| **phpldapadmin**         | Gestió web            |

