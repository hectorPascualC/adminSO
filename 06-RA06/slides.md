---
marp: true
paginate: true
theme: default

header: "RA06 - Integració de sistemes operatius"
footer: "Mòdul 0374 - Administració de Sistemes Operatius"

style: |
    header, footer {
        display: block;
        width: 92vw;
        font-size: .45rem;
        color: #bbbbbcff;
        z-index: 10;
    }
    header { text-align: right !important; padding-right: 0 !important; }
    section {
        display: flex !important;
        flex-direction: column !important;
        justify-content: flex-start !important;
    }

---

# RA06  
## Integració de sistemes operatius lliures i propietaris

Administració de Sistemes Operatius  
ASIX02

---

# Context de la integració de sistemes

En una xarxa real sovint conviuen:

- equips **Windows**
- servidors **Linux**
- impressores de xarxa
- clients amb sistemes diferents

Per això cal garantir la **interoperabilitat**.

---

# Què vol dir integrar sistemes

Integrar sistemes operatius vol dir:

- connectar equips diferents dins d'una mateixa xarxa
- compartir recursos comuns
- accedir a serveis de manera compatible
- treballar de manera coordinada

No es tracta que tots siguin iguals, sinó que **es puguin entendre**.

---

# 6.1 Concepte d'integració de sistemes operatius

La integració real apareix quan els equips poden:

- accedir a carpetes i fitxers compartits
- utilitzar impressores de xarxa
- autenticar-se i treballar amb permisos adequats

Això és el que anomenem **interoperabilitat**.

---

# Exemple real

Una situació típica:

- usuaris amb **Windows**
- servidor principal amb **Linux**
- carpeta comuna en xarxa
- impressora compartida

Si tots els equips poden utilitzar aquests recursos, la integració és correcta.

---

# 6.2 Escenaris heterogenis de xarxa

Un escenari heterogeni és una xarxa on conviuen equips diferents pel que fa a:

- sistema operatiu
- serveis instal·lats
- gestió de permisos
- manera d'accedir als recursos

És la situació més habitual en empreses i centres educatius.

---

# Exemple d'escenari heterogeni

```
Clients Windows ─────┐
                     ├── Xarxa local ─── Servidor Linux ─── Recursos compartits
Client Linux ────────┘
```

El servidor és el punt central i els clients han de poder accedir-hi encara que siguin diferents.

---

# 6.3 Recursos compartits en xarxa

Un recurs compartit és qualsevol element d'un sistema que es posa a disposició d'altres equips.

Els més habituals són:

- carpetes
- fitxers
- impressores
- espais d'emmagatzematge

---

# Compartició d'arxius

Quan una carpeta del servidor es comparteix correctament:

- els clients autoritzats hi poden entrar
- poden llegir documents
- poden modificar-los si tenen permisos
- la informació queda centralitzada

Això evita duplicacions i facilita el treball comú.

---

# Compartició d'impressores

Una impressora compartida permet que diversos equips puguin imprimir sobre el mateix recurs.

Avantatges:

- menys maquinari duplicat
- administració centralitzada
- ús des de sistemes diferents

---

# 6.4 Seguretat d'accés als recursos compartits

Compartir un recurs no vol dir obrir-lo a tothom.

Cal decidir:

- qui hi pot accedir
- què hi pot fer
- amb quins permisos

La seguretat és clau tant en carpetes com en impressores compartides.

---

# Permisos habituals

En un recurs compartit poden aparèixer permisos com:

- **lectura**
- **lectura i escriptura**
- **control total**

No tots els usuaris han de tenir el mateix nivell d'accés.

---

# Usuaris i grups

Per controlar l'accés és habitual treballar amb:

- usuaris
- grups d'usuaris

Això simplifica la gestió i evita haver de configurar permisos un per un.

---

# Idea clau de seguretat

En qualsevol recurs compartit s'hauria d'aplicar aquest criteri:

- donar només els permisos necessaris
- evitar permisos excessius
- separar usuaris que consulten d'usuaris que modifiquen

Això és el **principi de mínim privilegi**.

---

# 6.5 Connectivitat en entorns heterogenis

Abans de compartir recursos, cal assegurar que client i servidor es poden comunicar.

Cal comprovar:

- adreçament IP correcte
- mateixa xarxa o ruta entre equips
- resolució de noms
- absència de bloquejos al tallafoc

---

# Exemple bàsic de comprovació

Una prova inicial habitual és:

```bash
ping 192.168.1.10
```

Això confirma comunicació mínima, tot i que encara no garanteix que el servei compartit funcioni.

---

# 6.6 Serveis de xarxa per compartir recursos

Per compartir carpetes o impressores no n'hi ha prou amb crear-les.

Cal un **servei de xarxa** que actuï entre servidor i clients.

En aquesta RA els dos serveis principals són:

- **Samba**
- **CUPS**

---

# Servei vs recurs

És important diferenciar:

- **servei** → mecanisme que permet la compartició
- **recurs** → element concret que s'ofereix a la xarxa

Exemples:

- Samba → servei / carpeta compartida → recurs
- CUPS → servei / impressora → recurs

---

# 6.7 Samba

**Samba** és el servei més habitual quan un servidor Linux ha de compartir carpetes amb equips Windows.

Permet:

- compartir directoris
- controlar l'accés amb usuaris
- definir permisos de lectura o escriptura
- integrar Linux i Windows dins d'una mateixa xarxa

---

# Què permet Samba

En termes pràctics, Samba permet:

- publicar carpetes a la xarxa
- validar l'accés dels usuaris
- fer visibles recursos Linux des de Windows
- mantenir control sobre permisos i accessos

Això el converteix en una peça clau de la interoperabilitat.

---

# Exemple d'ús

Imaginem una carpeta del servidor Linux:

```text
/srv/compartit
```

Si es configura amb Samba, un equip Windows la podrà veure com un recurs de xarxa i accedir-hi amb les credencials corresponents.

---

# 6.8 Configuració de recursos compartits

Quan el servei ja està instal·lat, cal configurar el recurs que realment es vol oferir.

Això implica decidir:

- què es compartirà
- qui hi podrà accedir
- amb quins permisos

---

# Exemple conceptual de recurs compartit

Una configuració típica pot partir d'una carpeta com aquesta:

```bash
sudo mkdir -p /srv/compartit
```

Després s'ajusten permisos i es defineix el recurs dins de la configuració de Samba.

---

# Relació entre nom i ruta del recurs

```text
Nom del recurs a la xarxa  →  compartit
Ruta real al servidor      →  /srv/compartit
Accés                      →  usuaris autoritzats
Permisos                   →  lectura o lectura/escriptura
```

El client veu el recurs publicat, no l'estructura interna completa del servidor.

---

# Usuaris i autenticació

Un recurs compartit no s'hauria de deixar obert indiscriminadament.

La compartició depèn de la relació entre:

- directori real al servidor
- usuari que hi accedeix
- servei que valida l'accés
- permisos finals sobre el recurs

---

# Configuració d'impressores compartides

En la impressió, el raonament és semblant.

Primer cal:

- definir la impressora al sistema
- gestionar-la amb **CUPS**
- decidir si es comparteix o no
- provar-ne l'accés des dels clients

---

# 6.9 Proves de funcionament

Després d'instal·lar el servei i configurar el recurs, cal comprovar que tot funciona de veritat.

Això vol dir verificar:

- accés des de sistemes diferents
- autenticació correcta
- lectura i escriptura quan pertoqui
- ús real del recurs

---

# Proves sobre una carpeta compartida

Una seqüència raonable de comprovació és:

- localitzar el servidor
- accedir al recurs compartit
- introduir credencials si cal
- comprovar lectura
- provar creació o modificació de fitxers

---

# Proves sobre impressores compartides

En una impressora compartida convé verificar:

- que el client la detecta
- que la pot afegir o connectar-hi
- que pot enviar una pàgina de prova
- que la cua de treball es processa correctament

---

# Treball en grup en un entorn mixt

Una bona prova no és només una connexió puntual.

També interessa comprovar:

- diversos usuaris entrant a la mateixa carpeta
- diferents equips imprimint sobre la mateixa impressora
- que cada usuari veu només allò que li correspon

---

# 6.10 Documentació de la configuració

Quan el servei ja funciona, cal documentar el que s'ha configurat.

La documentació hauria d'incloure com a mínim:

- servei instal·lat
- servidor
- recurs compartit
- ruta real del recurs
- usuaris o grups autoritzats
- permisos aplicats
- proves realitzades

---

# Exemple simple de documentació

```
Servei: Samba
Servidor: srv-linux
Recurs compartit: compartit
Ruta real: /srv/compartit
Usuaris autoritzats: professorat, administracio
Permisos: lectura i escriptura
Clients de prova: Windows 11, Ubuntu Desktop
Resultat de les proves: accés correcte i creació de fitxers verificada
```

---

# Exemple de documentació d'impressora

```
Servei: CUPS
Servidor: srv-linux
Impressora: aula1
Tipus d'accés: compartida en xarxa
Clients de prova: Windows 11, Ubuntu Desktop
Resultat: pàgina de prova impresa correctament
```

---

# Idea final

La interoperabilitat no és només una idea teòrica.

És un procés complet que passa per:

- entendre l'escenari heterogeni
- instal·lar i configurar serveis
- compartir recursos reals
- provar-los des de sistemes diferents
- documentar el resultat final
