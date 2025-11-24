# **02. Esquema del servei de directori**

## **2.1. Concepte d’esquema dins un servei de directori**

En un servei de directori, l’**esquema** és el conjunt de **regles que defineixen quins objectes poden existir** i **quins atributs pot tenir cada objecte**.
És l’equivalent al *model de dades* en una base de dades relacional, però adaptat a un directori jeràrquic.

L’esquema determina:

* Quines **classes d’objectes** (objectClass) poden crear-se
* Quins **atributs obligatoris** ha de contenir cada objecte
* Quins **atributs opcionals** pot incloure
* Quines **relacions jeràrquiques** pot tenir dins el DIT
* Quins **tipus de dades** són vàlids per a cada atribut
* Com s’ha d’interpretar un objecte dins del directori

A efectes pràctics: **l’esquema és el “contracte” que tots els objectes del directori han de complir**.

---

## **2.2. Components de l’esquema: objectes, classes i atributs**

### **a) Atributs (attributes)**

Cada entrada del directori està formada per atributs.
Un atribut té:

* **Nom** (sn, cn, uid…)
* **Tipus de dada** (string, integer, DN…)
* **Regla de sintaxi**
* **Multiplicitat** (si pot tenir diversos valors)

Exemples d’atributs habituals en LDAP:

| Atribut           | Significat                        |
| ----------------- | --------------------------------- |
| **cn**            | Common Name                       |
| **sn**            | Surname                           |
| **uid**           | Identificador únic d’usuari       |
| **mail**          | Correu electrònic                 |
| **userPassword**  | Contrasenya de l’usuari           |
| **homeDirectory** | Carpeta personal en sistemes Unix |

Aquests atributs provenen de l’esquema estàndard *inetOrgPerson*, molt utilitzat a OpenLDAP.

---

### **b) Classes d'objecte (objectClass)**

Cada objecte del directori pertany a una o més **objectClass**, que defineixen:

* Quins atributs són obligatoris (**MUST**)
* Quins atributs són opcionals (**MAY**)

Exemples de classes habituals:

| objectClass            | Descripció                              |
| ---------------------- | --------------------------------------- |
| **top**                | Classe base que tenen tots els objectes |
| **organizationalUnit** | Representa una OU                       |
| **inetOrgPerson**      | Classe avançada per a usuaris           |
| **posixAccount**       | Defineix atributs d’un usuari Unix      |
| **posixGroup**         | Grups compatibles amb sistemes Linux    |

Les classes poden ser:

* **Estructurals (structural):** defineixen la naturalesa de l’objecte
* **D’auxili (auxiliary):** afegeixen atributs extra
* **Abstractes:** no es poden instanciar directament

---

## **2.3. El DIT: Directory Information Tree**

L’esquema es materialitza en un arbre jeràrquic anomenat **DIT**.

Exemple estàndard:

```
dc=empresa,dc=local
│
├── ou=Usuaris
│     ├── uid=jordib
│     ├── uid=mpuig
│
├── ou=Grups
│     ├── cn=administradors
│     └── cn=professors
│
└── ou=Equips
      ├── cn=PC-A101
      └── cn=Portatil-12
```

Aquest arbre no és arbitrari:
**està condicionat per l’esquema.**

Per exemple, no pots crear un objecte amb classe `posixAccount` dins d’una localització que no permeti aquesta classe.

---

## **2.4. Nomenclatura LDAP i identificadors DN**

Els objectes del directori es descriuen amb **DN (Distinguished Name)**, que és l’“adreça completa” de l’objecte dins del DIT.

Exemple:

```
uid=mpuig,ou=Usuaris,dc=empresa,dc=local
```

Elements del DN:

* **RDN (Relative Distinguished Name):** la part més específica
  ex: `uid=mpuig`
* **DC (Domain Component):** components del domini
* **OU (Organizational Unit):** unitats organitzatives
* **CN (Common Name):** nom comú

La composició del DN també ve determinada per l’esquema.

---

## **2.5. Fitxers LDIF i representació de l’esquema**

L’esquema i els objectes es poden definir en format **LDIF (LDAP Data Interchange Format)**, un text estructurat que permet:

* Crear objectes
* Modificar objectes
* Importar massiva d’entrades
* Definir esquemes personalitzats

Exemple d’usuari en LDIF:

```ldif
dn: uid=jordib,ou=Usuaris,dc=empresa,dc=local
objectClass: inetOrgPerson
objectClass: posixAccount
cn: Jordi Bonet
sn: Bonet
uid: jordib
uidNumber: 1001
gidNumber: 1001
homeDirectory: /home/jordib
mail: jbonet@empresa.local
```

L’esquema determina quins atributs són vàlids en aquest LDIF.

---

## **2.6. Esquemes estàndard i personalitzats**

OpenLDAP inclou esquemes estàndard:

* **core.schema**
* **cosine.schema**
* **inetorgperson.schema**
* **nis.schema**

A Windows Server, els esquemes incorporen:

* **User**
* **Group**
* **Computer**
* **OrganizationalUnit**
* **Contact**

Es poden **ampliar els esquemes** per necessitats corporatives, però cal fer-ho amb extrema precaució:

* nous objectClass
* nous atributs
* sintaxi definida
* OID únics

---

## **2.7. Bones pràctiques en el disseny de l’esquema**

1. **Utilitzar sempre esquemes estàndard** abans de crear-ne de nous.
2. **Evitar duplicar atributs** que ja existeixen en altres esquemes.
3. **Crear OU segons estructura organitzativa real**, no per persones concretes.
4. **Estandarditzar nomenclatura** (uid, cn, OU, DC…).
5. **Limitar la profunditat del DIT**: massa nivells dificulten l’administració.
6. **Planificar l’esquema abans de començar**, igual que un model de dades.

Aquestes bones pràctiques permeten un directori:

* escalable
* segur
* mantenible
* interoperable


