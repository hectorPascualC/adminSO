# **SOLUCIÓ - Examen pràctic RA01**

## **1. Traducció d’estructura AD → LDAP**  

### Reformulació pregunta per evitar confusions  

A la interfície gràfica d’**Active Directory**, els administradors veuen una estructura simplificada del directori, sense mostrar explícitament els DN ni els atributs LDAP interns.

A la GUI d’Active Directory es veu el següent esquema:
[esquema]

**Tasca**
Representa **aquesta mateixa estructura** utilitzant la **notació LDAP explícita**, indicant:

* el **Base DN**
* la **OU**
* i els **DN dels usuaris**

*(No cal escriure un LDIF complet)*

### Resposta correcta

L’equivalent en LDAP de l'estructura és:

```text
dc=iestorreroja,dc=local
└── ou=Alumnos
    ├── uid=Usuari 1
    ├── uid=Usuari 2
    └── uid=Usuari 3
```
```text
dc=iestorreroja,dc=local
└── ou=Alumnos
    ├── cn=Usuari 1
    ├── cn=Usuari 2
    └── cn=Usuari 3
```

---

### Explicació
* En **AD**, el domini (`iestorreroja.local`) és l'arrel
* En **LDAP**, aquest domini es representa com un **Base DN** format per `dc` -> domain component: components formen el domini
* Les **OU** existeixen en tots dos sistemes de representació i serveixen per organitzar
* En LDAP tots els objectes del directori s'identifiquen amb un DN-Distinguished Name, que és una llista ordenada indicant **atribut=valor**
* A AD la interfície gràfica ja crea i utilitza els atributs automàticament, encara que no els vegis. En LDAP *pur* si tu no l'escrius aquest no existeix
* En aquest context, l'atribut més habitual per identificar un usuari és el uid: User ID  
* Sense atribut rebríem els errors
    ```
    ldapadd: invalid format
    ```
    o
    ```
    ldapadd: parse error
    ```
---

### D’on surt als documents

* **Manual OpenLDAP – estructura del DIT**
  `04-05-06-exercise-manual-openLDAP.md`
  Apartats:
  * *Configuració inicial d’OpenLDAP*
  * *Creació de l’estructura del directori (OU)*
  * Exemples de DN amb `dc`, `ou`, `uid`


## **2. Enllaç correcte d’una GPO amb Computer Configuration**

Una **GPO (Group Policy Object)** és **un objecte de política**, però **pot contenir dos tipus diferents de configuració** al seu interior:

```text
GPO
├── Computer Configuration
└── User Configuration
```

**No existeixen “dos tipus de GPO diferents”**
**Existeix una sola GPO**, amb **dos blocs de configuració possibles**  

* **Computer Configuration**
  * Conté configuracions que s’apliquen als **equips**
  * S’aplica quan l’equip arrenca
  * Només té efecte sobre **objectes equip**

* **User Configuration**
  * Conté configuracions que s’apliquen als **usuaris**
  * S’aplica quan l’usuari inicia sessió
  * Només té efecte sobre **objectes usuari**

Una mateixa GPO pot tenir:
* només Computer Configuration
* només User Configuration
* o tots dos blocs

Respecte la pregunta:
* La GPO **només conté configuració dins del bloc Computer Configuration**
* Els **equips** es troben a la `OU: Equipos`
* Els **usuaris** es troben a la `OU: Alumnos`

Encara que la GPO estigui enllaçada a `OU: Alumnos`, **no tindrà efecte**, perquè allà **no hi ha equips**

### Resposta correcta
**La GPO s’ha d’enllaçar a la OU: Equipos**, perquè només conté configuració d’equip (Computer Configuration) i aquest tipus de configuració només s’aplica als objectes equip que es troben dins de la unitat organitzativa on està enllaçada

---

### D’on surt als documents

* **Manual Active Directory – GPO**
  `04-05-06-exercise_AD_manual.md`
  Apartats:
  * *Aplicar una Política de Grup (GPO)*
  * *Computer Configuration vs User Configuration*
  * Enllaç de GPO a OU

---

## **3. Creació d’un usuari LDAP**  

### a) Pas a eliminar del procés

**C. Validar el directori LDAP abans de crear l’usuari**

---

### b) Ordre correcte dels passos

**B → D → A → E → F**

---

### c) Justificació tècnica de l'eliminació

Quan es crea un usuari el directori LDAP ja està creat, no es valida el directori complet ja que en un cas real podria portar molts recursos i temps. Imagina fer aquest *checking* cada cop que crees un usuari nou

---

### Explicació  

A les pràctiques el procés de creació d’un usuari LDAP és **seqüencial** i segueix el flux:

1. **B. Verificar que la OU existeix**
   L’usuari s’ha de crear dins d’una OU existent. LDAP no crea estructures automàticament.

2. **D. Escriure el fitxer LDIF**
   Es crea un **fitxer LDIF independent**, que descriu l’operació d’alta de l’usuari.

3. **A. Definir l’entrada de l’usuari amb els atributs corresponents**
   Dins del LDIF s’especifiquen:

   * el DN de l’usuari
   * les `objectClass`
   * els atributs obligatoris (`uid`, `cn`, `sn`, etc.)

4. **E. Executar l’ordre per afegir l’usuari al directori LDAP**
   Mitjançant `ldapadd`, el directori processa el LDIF i crea l’entrada.

5. **F. Comprovar que l’usuari s’ha creat correctament**
   Es valida la creació amb una consulta LDAP (`ldapsearch`).

---

### Per què el pas C no és correcte

* el **directori LDAP ja existeix** i no es valida a cada operació
* es valida és **l'entrada creada**, no el directori sencer
* LDAP valida automàticament cada operació quan l'executem. Les validacions posteriors es fan quan nosaltres realitzem operacions o consultes sobre les entrades creades

En resum:
* **no validem abans**
* **validem sempre després**

---

### D’on surt als documents

* **Manual OpenLDAP – creació d’usuaris**
  `04-05-06-exercise-manual-openLDAP.md`
  Apartats:

  * *Creació de l’estructura del directori (OU)*
  * *Creació d’usuaris LDAP*
  * Errors típics (`No such object`)

---

## **4. Procés de creació i validació d’una estructura LDAP**

### a) Pas a eliminar del procés

**C. Crear primer els usuaris directament sota el Base DN**

---

### b) Ordre correcte dels passos

**B → F → D → A → E**

---

### c) Justificació tècnica de l’eliminació

Els usuaris LDAP no s'han de crear directament sota el Base DN, sinó dins d’una unitat organitzativa (OU). El DIT requereix una estructura jeràrquica per organitzar els objectes

---

### Explicació  

Quan es crea una **estructura LDAP completa (DIT)** és **jeràrquica i ordenada**, tal com s’ha treballat a les pràctiques

El flux és:

1. **B. Definir la Base DN del domini**
   La Base DN representa l'arrel del directori LDAP
   És el punt sobre el qual penjarà tota l’estructura

2. **F. Escriure els LDIF corresponents**
   En LDAP totes les operacions es creen mitjançant **fitxers LDIF independents** indicant:

   * el DN de cada entrada
   * els `objectClass`
   * els atributs necessaris

3. **D. Crear les OU amb organizationalUnit**
   Les OU són els contenidors lògics del DIT
   **Han d’existir abans de crear usuaris**, ja que els usuaris no poden penjar-se directament de l'arrel

4. **A. Crear usuaris amb inetOrgPerson**
   Els usuaris són entrades LDAP que:

   * utilitzen `inetOrgPerson`
   * **sempre es creen dins d’una OU**
   * mai directament sota el Base DN

5. **E. Validar el DIT amb una consulta LDAP**
   La validació es fa mitjançant **consulta LDAP (`ldapsearch`)** mirant que:

   * les OU existeixen
   * els usuaris existeixen
   * els DN són correctes

En resum:
* **primer s’organitza**
* **després s’afegeixen els objectes**

---

### D’on surt als documents

* **Manual OpenLDAP – configuració inicial i validació**
  `04-05-06-exercise-manual-openLDAP.md`
  Apartats:

  * *Configuració inicial d’OpenLDAP*
  * *Verificació del servei LDAP*
  * *Creació del DIT*


---

## **5. Error de DNS en la unió al domini**

### a) Error conceptual

El client **no està utilitzant el DNS del domini**, sinó un DNS extern (8.8.8.8)

---

### b) Per què fallarà la unió al domini  

Perquè el client no està utilitzant el DNS del domini, i per tant no pot trobar el servidor del domini.
Sense poder localitzar el controlador de domini, la unió al domini no es pot completar.

---

### Explicació 

A les pràctiques hem vist que per unir un client a un domini:

* el client **ha de preguntar al DNS del domini**
* aquest DNS és el **mateix servidor on està AD**
* és el DNS que sap:
  * quin és el domini
  * quin servidor el gestiona

Quan el client té configurat **8.8.8.8**:
* Internet pot funcionar correctament
* però **el domini intern no existeix per a aquest DNS**

En resum:
* el client **no pot localitzar el domini**
* i la unió **no es pot completar**

---

### D’on surt als documents

* **Manual Active Directory – unió de clients al domini**
  `04-05-06-exercise_AD_manual.md`
  Apartats:

  * *Unir un Client Windows al Domini*
  * Configuració IP i DNS del client
  * `nslookup` i comprovacions DNS



