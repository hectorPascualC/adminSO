---
marp: true
paginate: true
theme: default

header: "RA01 – Servei de Directori · Capítol 3 – Controladors de Domini i Arquitectures AD/LDAP"
footer: "Mòdul 0374 – Administració de Sistemes Operatius"

style: |
    header, footer {
        display: block;
        width: 100vw;
        font-size: .45rem;
        color: #bbbbbcff;
        z-index: 10;
    }

    header {
        text-align: right !important;   
        padding-right: 50px;           
    }
---

<!-- Slide 1 -->
# RA01: Capítol 3
## Controladors de Domini i Arquitectures AD/LDAP

ASIX - Administració de Sistemes Operatius i Xarxes 
Professor: Hèctor Pascual

---

<!-- Slide 2 -->
# 🔸 Objectius del capítol

- Entendre què és un **Controlador de Domini (DC)**  
- Comprendre l’arquitectura d’**Active Directory**  
- Distingir domini, arbre i bosc  
- Explicar el concepte de **Global Catalog**  
- Entendre la **replicació AD**  
- Conèixer els principis bàsics de **LDAP / OpenLDAP**  
- Comparar AD i LDAP a nivell arquitectònic

---

<!-- Slide 3 -->
# 🔸 Què és un Controlador de Domini?

Un **DC (Domain Controller)** és el servidor que:

- emmagatzema la base del directori (NTDS.dit)  
- valida usuaris i equips  
- emet tickets Kerberos  
- replica la informació  
- aplica polítiques de seguretat

És el **nucli del servei de directori** a Windows.

---

<!-- Slide 4 -->
# 🔸 Funcions d’un DC

- Autenticació (Kerberos)  
- Autorització  
- Replicació entre DCs  
- Gestió del domini  
- Global Catalog  
- Validació de grups i permisos

---

<!-- Slide 5 -->
# 🔸 Arquitectura AD: Domini - Arbre - Bosc

- **Domini**: unitat bàsica d’administració  
- **Arbre** : conjunt de dominis amb espai de noms comú  
- **Bosc**  : conjunt d’arbres que comparteixen esquema i GC

---

<!-- Slide 6 -->
# 🔸 Exemple d’arbre i bosc

```

empresa.local
├── barcelona.empresa.local
└── madrid.empresa.local

institut.cat
├── premia.institut.cat
└── viladecans.institut.cat

```

Dos arbres → **un bosc** si comparteixen:  
✔ Esquema  
✔ Global Catalog  
✔ Confiances internes

---

<!-- Slide 7 -->
# 🔸 Global Catalog (GC)

Servei especialitzat que manté:

- una còpia **completa** del domini local  
- una còpia **parcial** dels altres dominis  
- índexs per a cerques ràpides  
- validació de grups universals  
- suport a autenticacions interdominis

És **l'índex global del bosc**

---

<!-- Slide 8 -->
# 🔸 Autenticació Kerberos

Quan un usuari inicia sessió:

1. Envia credencials al KDC  
2. Rep el **Ticket-Granting Ticket (TGT)**  
3. Demana tickets per accedir a serveis

Comprovació:

```bash
klist
```

```
Client: joan@empresa.local
Server: krbtgt/empresa.local
Start Time: 10:05
End Time:   20:05
```

---

<!-- Slide 9 -->

# 🔸 Replicació AD

### **Intra-site**

* ràpida
* no comprimida
* intervals curts (segons/minuts)

### **Inter-site**

* optimitzada
* comprimida
* intervals llargs (minuts/hores)

---

<!-- Slide 10 -->

# 🔸 Eines de diagnosi en AD

* `dcdiag`                → estat del DC
* `repadmin /replsummary` → replicació
* `netdom query fsmo`     → rols FSMO
* `dsa.msc`               → gestió d’usuaris/equips
* `gpmc.msc`              → polítiques (GPO)

---

<!-- Slide 11 -->

# 🔸 Introducció a LDAP

LDAP = **Lightweight Directory Access Protocol**

Servei de directori en entorns Linux:

* s’executa en el servei **slapd**
* organitza informació en un **DIT**
* usa fitxers LDIF
* permet consultes i autenticació

---

<!-- Slide 12 -->

# 🔸 DN, RDN i DIT

### **DN (Distinguished Name)**

Identificador complet:

```
uid=mpuig,ou=Professorat,dc=ins,dc=cat
```

### **RDN**

```
uid=mpuig
```

### **DIT**

Arbre jeràrquic d’objectes.

---

<!-- Slide 13 -->

# 🔸 AD vs LDAP

| Aspecte      | AD                    | LDAP                    |
| ------------ | --------------------- | ----------------------- |
| Arquitectura | Domini / Arbre / Bosc | DIT                     |
| Autenticació | Kerberos + NTLM       | Simple bind / SASL      |
| Replicació   | Multimàster + FSMO    | syncrepl / MMR / mirror |
| Gestió       | GUI + RSAT            | CLI + phpldapadmin      |

---

<!-- Slide 14 -->

# 🔸 Replicació LDAP

### **Syncrepl**

* client → servidor
* refresca i persisteix

### **Multi-Master Replication**

* diversos servidors poden escriure

### **Mirror Mode**

* servidor actiu + servidor mirall

---

<!-- Slide 15 -->

# 🔸 Exemple syncrepl

```bash
syncrepl rid=001
  provider=ldap://ldap1.empresa.local
  type=refreshAndPersist
  searchbase="dc=empresa,dc=local"
```

---

<!-- Slide 16 -->

# 🔸 Autenticació LDAP

### Simple bind

* DN + contrasenya
* necessita TLS

### SASL (GSSAPI)

* usa Kerberos
* més segur
* no cal enviar contrasenya

---

<!-- Slide 17 -->

# 🔸 Eines LDAP

* `ldapsearch`             → consultes
* `ldapadd` / `ldapmodify` → gestió
* `slapcat` / `slapadd`    → export/import
* `phpldapadmin`           → gestió web

---

<!-- Slide 18 -->

# 🔸 Resum final

* Un DC és el servidor central del domini
* AD s’organitza en dominis, arbres i boscos
* El GC accelera cerques i autenticació
* Kerberos és el nucli del logon
* LDAP proporciona un directori flexible
* AD i LDAP comparteixen conceptes però tenen rols diferents

