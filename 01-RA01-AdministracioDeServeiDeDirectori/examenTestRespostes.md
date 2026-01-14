# 1. Quin és l’objectiu principal d’un servei de directori?

A) Emmagatzemar fitxers
B) Gestionar identitats i permisos 
C) Fer còpies de seguretat
D) Executar aplicacions

**Per què és correcta (B):** Un servei de directori està pensat per centralitzar **identitat, permisos i accés** (usuaris, grups, equips, OUs), no per emmagatzemar fitxers ni executar programes. 
**On surt a la teoria:** `01-introduccioGeneral_Als_ServeisDeDirectori.md` → apartats sobre “Problemes que resol” i “Objectes/atributs” (identitat i permisos centralitzats). 

**Per què les altres són incorrectes:**

* **A**: emmagatzemar fitxers és funció de FS/servidor de fitxers, no d’un directori.
* **C**: còpies de seguretat és una tasca de backup, no la funció nuclear del directori.
* **D**: executar aplicacions és funció del SO/serveis, no del directori.

---

## 2. Quin protocol s’utilitza principalment per consultar la informació d’un servei de directori?

A) HTTP
B) FTP
C) LDAP 
D) SMB

**Per què és correcta (C):** LDAP és el protocol estàndard per fer **consultes** a un directori; és literalment el “Directory Access Protocol” en versió lleugera. 
**On surt a la teoria:** `01-introduccioGeneral_Als_ServeisDeDirectori.md` → comparació “Protocol: SQL vs LDAP” i context LDAP. 

**Per què les altres són incorrectes:**

* **A (HTTP)**: protocol web; no és el protocol de consulta de directori.
* **B (FTP)**: transferència de fitxers, no consultes de directori.
* **D (SMB)**: compartició de fitxers/impressió, no consulta d’objectes del directori.

---

## 3. Quin element representa una entitat com un usuari o un equip dins d’un directori?

A) Fitxer
B) Objecte 
C) Procés
D) Servei

**Per què és correcta (B):** La teoria defineix que usuaris/equips/grups són **objectes** del directori, descrits per atributs. 
**On surt a la teoria:** `01-introduccioGeneral_Als_ServeisDeDirectori.md` → “Components essencials: Objectes”. 

**Per què les altres són incorrectes:**

* **A**: un fitxer és un element del sistema de fitxers, no del model de directori.
* **C**: un procés és execució en memòria (SO), no entitat de directori.
* **D**: un servei pot existir com a recurs, però el model bàsic del directori és “objecte + atributs”.

---

## 4. Quin tipus d’estructura utilitza un servei de directori per organitzar la informació?

A) Taules relacionals
B) Llistes planes
C) Arbre jeràrquic (DIT) 
D) Fitxers JSON

**Per què és correcta (C):** La teoria remarca que el directori organitza la informació en un **arbre jeràrquic DIT** (no taules). 
**On surt a la teoria:** `01-introduccioGeneral_Als_ServeisDeDirectori.md` → “Arbre jeràrquic (DIT)” i comparació SQL vs Directori. 

**Per què les altres són incorrectes:**

* **A**: és propi de BBDD SQL.
* **B**: un directori no és una llista plana; depèn de jerarquia (DN/RDN/OU/DC).
* **D**: JSON pot ser un format d’intercanvi, però no és l’estructura del directori.

---

## 5. Quin servei fa el paper de servidor de directori en sistemes Linux amb OpenLDAP?

A) Active Directory
B) slapd 
C) Kerberos
D) Samba

**Per què és correcta (B):** OpenLDAP s’executa com a servei **slapd**, que processa operacions LDAP i gestiona el DIT. 
**On surt a la teoria:** `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “Equivalent en LDAP: slapd”. 

**Per què les altres són incorrectes:**

* **A**: AD és l’ecosistema Microsoft, no el dimoni Linux.
* **C**: Kerberos és autenticació, no servidor de directori.
* **D**: Samba és compatibilitat/serveis SMB; pot integrar-se, però no és el servidor LDAP.

---

## 6. Quin fitxer conté la base de dades del directori en Active Directory?

A) SAM
B) registry.dat
C) NTDS.dit 
D) users.db

**Per què és correcta (C):** La teoria defineix **NTDS.dit** com el fitxer on es desa la BBDD d’AD (usuaris, grups, OUs, etc.). 
**On surt a la teoria:** `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “NTDS.dit - BBDD AD”. 

**Per què les altres són incorrectes:**

* **A (SAM)**: existeix en Windows, però AD com a directori d’un domini treballa amb NTDS.dit al DC.
* **B** i **D**: no són el fitxer de BBDD del directori segons el temari.

---

## 7. Quin protocol d’autenticació utilitza Active Directory com a sistema principal?

A) NTLM
B) Kerberos 
C) LDAP
D) RADIUS

**Per què és correcta (B):** AD utilitza **Kerberos** per autenticació (tickets, KDC, etc.).
**On surt a la teoria:** `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “Sistema d’autenticació: Kerberos” i slides de Kerberos.

**Per què les altres són incorrectes:**

* **A (NTLM)**: el temari el presenta com a **compatibilitat** quan Kerberos no és possible. 
* **C (LDAP)**: LDAP és protocol de consulta del directori, no el sistema principal d’autenticació.
* **D (RADIUS)**: no és el mecanisme central d’AD segons aquests materials.

---

## 8. Què representa un DN (Distinguished Name) dins d’un directori?

A) El nom curt d’un usuari
B) L’identificador complet d’un objecte dins del DIT 
C) Un tipus de dada
D) Un grup de seguretat

**Per què és correcta (B):** DN = “adreça completa” d’un objecte (uid/ou/dc...). 
**On surt a la teoria:** `03-controladorsDeDomini_slides.md` → “DN (Distinguished Name): Identificador complet”. 

**Per què les altres són incorrectes:**

* **A**: això s’assemblaria més a l’RDN o a un identificador curt (però DN és complet).
* **C**: “tipus de dada” és sintaxi/atribut, no DN.
* **D**: un grup és un objecte; el DN és la seva adreça, no “un grup”.

---

## 9. Quin component del directori defineix quins objectes poden existir i quins atributs poden tenir?

A) DIT
B) Objecte
C) Esquema 
D) Global Catalog

**Per què és correcta (C):** L’**esquema** defineix objectClass, atributs, sintaxi i regles. 
**On surt a la teoria:** `02-esquemaAD_slides.md` → “Components principals de l’esquema”. 

**Per què les altres són incorrectes:**

* **A (DIT)**: és l’estructura jeràrquica, no les regles del que pot existir.
* **B**: l’objecte és una instància; el que “permet” o “limita” és l’esquema.
* **D (GC)**: accelera cerques/autenticacions; no defineix classes/atributs.

---

## 10. Quin servei d’Active Directory permet realitzar cerques ràpides a través de tots els dominis d’un bosc?

A) DNS
B) KDC
C) Global Catalog 
D) FSMO

**Per què és correcta (C):** El **GC** manté còpia parcial dels altres dominis i índex per cerques globals. 
**On surt a la teoria:** `03-controladorsDeDomini_slides.md` → “Global Catalog: índex global del bosc”. 

**Per què les altres són incorrectes:**

* **A (DNS)**: localitza serveis/DCs, però no fa l’índex de cerques del directori.
* **B (KDC)**: emet tickets (Kerberos), no és servei de cerques.
* **D (FSMO)**: rols d’operacions úniques, no servei de cerca.

---

## 11. Quin element permet localitzar automàticament els controladors de domini dins d’un entorn Active Directory?

A) LDAP
B) Kerberos
C) DNS amb registres SRV 
D) Global Catalog

**Per què és correcta (C):** AD depèn de DNS i **registres SRV** per localitzar DCs (_ldap._tcp..., _kerberos._tcp...). 
**On surt a la teoria:** `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “DNS integrat” + exemples de SRV. 

**Per què les altres són incorrectes:**

* **A**: LDAP és consulta, no “descobriment” de DCs.
* **B**: Kerberos autentica quan ja tens el camí/servei; la localització ve del DNS.
* **D**: GC és un rol/servei de directori, no el mecanisme general de descoberta de DCs.

---

## 12. Quin és el paper principal del Global Catalog (GC) en un bosc d’Active Directory?

A) Emmagatzemar còpies completes de tots els dominis
B) Accelerar cerques i autenticacions interdomini 
C) Gestionar els rols FSMO
D) Substituir el DNS del domini

**Per què és correcta (B):** El temari indica que el GC suporta **cerques ràpides** i **autenticacions interdomini** (token/grups universals). 
**On surt a la teoria:** `03-controladorsDeDomini_slides.md` → detalls del GC (còpia parcial + índex + suport autenticació). 

**Per què les altres són incorrectes:**

* **A**: diu que GC té còpia completa del domini local i parcial dels altres, no completa de tots. 
* **C**: FSMO és un altre mecanisme (rols), no funció del GC.
* **D**: DNS continua essent necessari (descoberta/nom), no el substitueix.

---

## 13. Quin tipus de replicació utilitza Active Directory per defecte?

A) Replicació mestre-esclau
B) Replicació síncrona centralitzada
C) Replicació multimàster amb excepcions FSMO 
D) Replicació manual per administrador

**Per què és correcta (C):** AD és **multimàster** (tots els DC accepten canvis) però hi ha operacions restringides pels **FSMO**. 
**On surt a la teoria:** `03b-multimaster.md` → “AD utilitza replicació multimàster” + restriccions FSMO. 

**Per què les altres són incorrectes:**

* **A** i **B**: contradiuen l’explicació multimàster.
* **D**: la teoria insisteix que la replicació és automàtica entre DCs.

---

## 14. Quina diferència clau hi ha entre la replicació intra-site i inter-site en Active Directory?

A) Intra-site utilitza TLS i inter-site no
B) Inter-site és més freqüent que intra-site
C) Intra-site és ràpida i no comprimida 
D) Inter-site no replica contrasenyes

**Per què és correcta (C):** Intra-site: **ràpida, freqüent i no comprimida**; Inter-site: **optimitzada i comprimida** amb intervals més llargs. 
**On surt a la teoria:** `03-controladorsDeDomini_slides.md` → “Replicació AD: intra-site / inter-site”. 

**Per què les altres són incorrectes:**

* **A**: la diferència del temari és rendiment/compressió/interval, no “TLS sí/no”.
* **B**: és al revés: intra-site és més freqüent.
* **D**: el temari no estableix aquesta regla; replica canvis del directori.

---

## 15. Quin rol FSMO és responsable de la compatibilitat amb sistemes antics i del control de l’horari del domini?

A) Schema Master
B) RID Master
C) PDC Emulator 
D) Infrastructure Master

**Per què és correcta (C):** La taula de rols FSMO assigna al **PDC Emulator** la “compatibilitat amb NT i control d’horari”. 
**On surt a la teoria:** `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → taula “Rols FSMO”. 

**Per què les altres són incorrectes:**

* **A**: esquema.
* **B**: blocs RID per SIDs.
* **D**: sincronització entre dominis (metadades d’objectes), no horari/compatibilitat.

---

## 16. En OpenLDAP, quin concepte equival funcionalment a l’arquitectura Domain / Tree / Forest d’Active Directory?

A) NTDS.dit
B) SAM
C) DIT (Directory Information Tree) 
D) Global Catalog

**Per què és correcta (C):** El temari compara explícitament “AD: Domain/Tree/Forest” vs “LDAP: DIT”. 
**On surt a la teoria:** `03-controladorsDeDomini_slides.md` → taula “AD vs LDAP”. 

**Per què les altres són incorrectes:**

* **A** i **B**: fitxer/element Windows; no és l’arquitectura LDAP.
* **D**: GC és servei propi d’AD per cerques/autenticació, no estructura LDAP.

---

## 17. Quina afirmació sobre el simple bind en LDAP és correcta?

A) No envia mai la contrasenya
B) És segur per defecte
C) Requereix TLS per no enviar credencials en clar 
D) Utilitza Kerberos internament

**Per què és correcta (C):** El temari diu que el **simple bind** envia “DN + contrasenya” i que sense TLS seria “text pla / prohibit”; per tant cal TLS.
**On surt a la teoria:** `03-controladorsDeDomini_slides.md` → “Simple bind… necessita TLS/SSL” i `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “Simple Bind: prohibit si no hi ha TLS”.

**Per què les altres són incorrectes:**

* **A**: contradicció directa (sí que envia contrasenya).
* **B**: no, justament és insegur sense xifrat.
* **D**: Kerberos seria SASL+GSSAPI, no simple bind.

---

## 18. Quin mecanisme permet a OpenLDAP replicar informació entre servidors de manera controlada?

A) FSMO
B) syncrepl 
C) NTLM
D) KDC

**Per què és correcta (B):** El temari dedica un apartat a **syncrepl** com a mecanisme de replicació LDAP.
**On surt a la teoria:** `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “syncrepl (client → servidor)” i slides “Replicació LDAP: Syncrepl”.

**Per què les altres són incorrectes:**

* **A (FSMO)**: és exclusiu d’AD per operacions úniques.
* **C (NTLM)**: autenticació antiga Windows, no replicació LDAP.
* **D (KDC)**: component Kerberos per tickets, no replicació.

---

## 19. Quin fitxer o base de dades és crític protegir en un controlador de domini Active Directory?

A) boot.ini
B) registry.dat
C) NTDS.dit 
D) ldif.conf

**Per què és correcta (C):** NTDS.dit és la BBDD del directori al DC (usuaris, grups, configuració del domini/bosc) i per això és element crític.
**On surt a la teoria:** `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “NTDS.dit - BBDD AD” i slides “DC emmagatzema NTDS.dit”.

**Per què les altres són incorrectes:**

* **A** i **B**: no són el repositori del directori segons temari.
* **D (ldif.conf)**: LDIF és format/entrada LDAP; no és la BBDD d’AD.

---

## 20. Quina eina és específica per diagnosticar l’estat de la replicació en Active Directory?

A) klist
B) dcdiag
C) repadmin /replsummary 
D) ldapsearch

**Per què és correcta (C):** El temari llista `repadmin /replsummary` com a eina de diagnosi de **replicació**.
**On surt a la teoria:** `03-controladorsDeDomini_slides.md` → “Eines de diagnosi: repadmin /replsummary → replicació” i `03-controladorsDeDomini_i_Arquitectures_ADLDAP.md` → “Comprovar replicació: repadmin /replsummary”.

**Per què les altres són incorrectes:**

* **A (klist)**: serveix per veure tickets Kerberos, no replicació. 
* **B (dcdiag)**: revisa salut del DC en general (no és “l’eina específica de replicació”).
* **D (ldapsearch)**: és per consultes LDAP, no per auditar replicació AD.