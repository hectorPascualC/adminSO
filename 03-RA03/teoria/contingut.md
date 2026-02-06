# RA03 - Automatització de tasques del sistema

## Índex

- [3.0 - Introducció a la RA03](#3-0-introduccio-a-la-ra03)
  - [3.0.1 On som dins del mòdul i per què existeix la RA03](#3-0-1-on-som-dins-del-modul-i-per-que-existeix-la-ra03)
  - [3.0.2 Què entenem per “automatitzar”](#3-0-2-que-entenem-per-automatitzar)
  - [3.0.3 Automatitzar != programar (frontera conceptual important)](#3-0-3-automatitzar-programar-frontera-conceptual-important)
  - [3.0.4 Exemple simple](#3-0-4-exemple-simple)
  - [3.0.5 Per què és important l'automatització](#3-0-5-per-que-es-important-lautomatitzacio)
  - [3.0.6 Relació amb altres RAs (sense barrejar-les)](#3-0-6-relacio-amb-altres-ras-sense-barrejar-les)

- [3.1 - Avantatges de l'automatització de tasques](#3-1-avantatges-de-lautomatitzacio-de-tasques)
  - [3.1.1 El problema real: la repetició en administració de sistemes](#3-1-1-el-problema-real-la-repeticio-en-administracio-de-sistemes)
  - [3.1.2 Què aporta realment automatitzar](#3-1-2-que-aporta-realment-automatitzar)
  - [3.1.3 Avantatge 1: regularitat](#3-1-3-avantatge-1-regularitat)
  - [3.1.4 Avantatge 2: reducció d'errors humans](#3-1-4-avantatge-2-reduccio-d-errors-humans)
  - [3.1.5 Avantatge 3: estalvi de temps](#3-1-5-avantatge-3-estalvi-de-temps)
  - [3.1.6 Avantatge 4: control i traçabilitat](#3-1-6-avantatge-4-control-i-tracabilitat)

- [3.2 - Planificació de tasques del sistema](#3-2-planificacio-de-tasques-del-sistema)
  - [3.2.1 Què vol dir planificar una tasca](#3-2-1-que-vol-dir-planificar-una-tasca)
  - [3.2.2 Tipus de tasques segons el temps](#3-2-2-tipus-de-tasques-segons-el-temps)
  - [3.2.3 Planificació de tasques repetitives (visió conceptual)](#3-2-3-planificacio-de-tasques-repetitives-visio-conceptual)
  - [3.2.4 Primera aproximació a la sintaxi](#3-2-4-primera-aproximacio-a-la-sintaxi)
  - [3.2.5 Exemple senzill de tasca repetitiva](#3-2-5-exemple-senzill-de-tasca-repetitiva)
  - [3.2.6 Planificació de tasques puntuals (visió conceptual)](#3-2-6-planificacio-de-tasques-puntuals-visio-conceptual)
  - [3.2.7 Exemple conceptual de tasca puntual](#3-2-7-exemple-conceptual-de-tasca-puntual)
  - [3.2.8 Relació directa amb el criteri 3.2](#3-2-8-relacio-directa-amb-el-criteri-3-2)

- [3.3 - Restriccions i criteris de seguretat](#3-3-restriccions-i-criteris-de-seguretat)
  - [3.3.1 Per què la seguretat és clau quan automatitzem](#3-3-1-per-que-la-seguretat-es-clau-quan-automatitzem)
  - [3.3.2 Principi bàsic: mínima autorització](#3-3-2-principi-basic-minima-autoritzacio)
  - [3.3.3 Context d'execució](#3-3-3-context-dexecucio)
  - [3.3.4 Restriccions habituals](#3-3-4-restriccions-habituals)
  - [3.3.5 Bones pràctiques mínimes](#3-3-5-bones-practiques-minimes)
  - [3.3.6 Relació directa amb el criteri 3.3](#3-3-6-relacio-directa-amb-el-criteri-3-3)
  - [3.3.7 Exemple conceptual d'automatització segura](#3-3-7-exemple-conceptual-dautomatitzacio-segura)

- [3.4 - Planificació de tasques repetitives i puntuals](#3-4-planificacio-de-tasques-repetitives-i-puntuals)
  - [3.4.1 Per què aquest punt existeix (relació amb 3.2)](#3-4-1-per-que-aquest-punt-existeix-relacio-amb-3-2)
  - [3.4.2 Tasques repetitives: definició i aplicació](#3-4-2-tasques-repetitives-definicio-i-aplicacio)
  - [3.4.3 Tasques puntuals: definició i aplicació](#3-4-3-tasques-puntuals-definicio-i-aplicacio)
  - [3.4.4 Relació directa amb el criteri 3.4](#3-4-4-relacio-directa-amb-el-criteri-3-4)

- [3.5 - Automatització de l'administració de comptes](#3-5-automatitzacio-de-ladministracio-de-comptes)
  - [3.5.1 Què vol dir automatitzar comptes (dins la RA03)](#3-5-1-que-vol-dir-automatitzar-comptes-dins-la-ra03)
  - [3.5.2 Exemples de tasques automatitzables](#3-5-2-exemples-de-tasques-automatitzables)
  - [3.5.3 Exemples reals d'automatització de comptes](#3-5-3-exemples-reals-dautomatitzacio-de-comptes)
  - [3.5.4 Relació directa amb el criteri 3.5](#3-5-4-relacio-directa-amb-el-criteri-3-5)
  - [3.5.5 Relació amb la planificació per comandes](#3-5-5-relacio-amb-la-planificacio-per-comandes)
  - [3.5.6 Frontera amb RA07 (sense intrusió)](#3-5-6-frontera-amb-ra07-sense-intrusio)

- [3.6 - Instal·lació i configuració d'eines gràfiques](#3-6-instal-lacio-i-configuracio-deines-grafiques)
  - [3.6.1 Per què existeixen eines GUI](#3-6-1-per-que-existeixen-eines-gui)
  - [3.6.2 Instal·lació o activació segons el sistema](#3-6-2-instal-lacio-o-activacio-segons-el-sistema)
  - [3.6.3 Configuració bàsica](#3-6-3-configuracio-basica)
  - [3.6.4 Relació directa amb el criteri 3.6](#3-6-4-relacio-directa-amb-el-criteri-3-6)

- [3.7 - Ús d'eines gràfiques per a la planificació](#3-7-us-deines-grafiques-per-a-la-planificacio)
  - [3.7.1 Què permet fer una eina gràfica](#3-7-1-que-permet-fer-una-eina-grafica)
  - [3.7.2 Creació d'una tasca (mateixos conceptes que per comandes)](#3-7-2-creacio-duna-tasca-mateixos-conceptes-que-per-comandes)
  - [3.7.3 Gestió de tasques existents](#3-7-3-gestio-de-tasques-existents)
  - [3.7.4 Visualització i comprovació](#3-7-4-visualitzacio-i-comprovacio)
  - [3.7.5 Relació directa amb el criteri 3.7](#3-7-5-relacio-directa-amb-el-criteri-3-7)

- [3.8 - Documentació de tasques automàtiques](#3-8-documentacio-de-tasques-automatiques)
  - [3.8.1 Per què cal documentar](#3-8-1-per-que-cal-documentar)
  - [3.8.2 Què s'ha de documentar (mínim)](#3-8-2-que-sha-de-documentar-minim)
  - [3.8.3 Plantilla mínima de documentació](#3-8-3-plantilla-minima-de-documentacio)
  - [3.8.4 Verificació i evidències](#3-8-4-verificacio-i-evidencies)
  - [3.8.5 Manteniment i actualització](#3-8-5-manteniment-i-actualitzacio)
  - [3.8.6 Relació amb seguretat i auditoria](#3-8-6-relacio-amb-seguretat-i-auditoria)
  - [3.8.7 Relació directa amb el criteri d'avaluació 3.8](#3-8-7-relacio-directa-amb-el-criteri-davaluacio-3-8)


## 3.0 - Introducció a la RA03

### 3.0.1 On som dins del mòdul i per què existeix la RA03

En administració de sistemes, una gran part de la feina **no és crear coses noves**, sinó **repetir tasques**:

*   revisar sistemes
*   executar manteniments
*   aplicar canvis periòdics
*   garantir que certes accions es facin **sempre igual**

La **RA03** apareix per donar resposta a una pregunta molt concreta:

> _Com podem fer que el sistema faci per si sol tasques repetitives, de manera controlada i eficient, sense dependre sempre d'una persona?_

Aquesta RA **no va de programar**, va de **gestionar el sistema perquè treballi de forma automàtica**.


### 3.0.2 Què entenem per “automatitzar”  

> Configurar el sistema perquè **executi ordres o processos en un moment determinat**, sense intervenció manual directa, utilitzant **eines pròpies del sistema operatiu**

Automatitzar **no és** crear lògica complexa, ni programar comportaments. Això vindrà **més endavant (RA07)**


### 3.0.3 Automatitzar != programar (frontera conceptual important)


### Exemple 

**Situació real:**  
Cada dia a les 02:00 del matí s'ha de:

*   executar una ordre
*   fer una tasca de manteniment
*   generar un registre

**Opció A - Manual**  
Un administrador entra cada nit i escriu la comanda

**Opció B - Automatitzada (RA03)**  
El sistema té configurat:

*   **què** ha de fer
*   **quan** ho ha de fer
*   **amb quins permisos**

No hi ha cap programa nou, només **planificació**


### 3.0.4 Exemple simple 

Encara **no estem explicant cron-crontab**, només el concepte

```text
Cada dia a les 02:00
→ Executa aquesta ordre
→ Sense que ningú estigui davant de l'ordinador
```

Aquest tipus de configuració és **exactament** el que treballa la RA03.

No ens interessa encara:

*   com s'escriu la línia exacta
*   quina comanda concreta és
*   si hi ha bucles o condicions

Això vindrà a la RA07


### 3.0.5 Per què és important l'automatització  

Des del punt de vista professional, l'automatització resol problemes reals:

1.  **Regularitat**  
    La tasca s'executa sempre, encara que ningú hi pensi
2.  **Fiabilitat**  
    No depèn de cansament, oblit o torns
3.  **Eficiència**  
    El temps de l'administrador es pot dedicar a tasques més importants
4.  **Traçabilitat**  
    Les tasques automatitzades es poden documentar i revisar

### 0.6 Relació amb altres RAs (sense barrejar-les)

Per situar-nos:

*   **RA03**  
    → Quan i com el sistema executa tasques de manera automàtica
*   **RA07**  
    → Què fa exactament el sistema mitjançant scripts i lògica

A la RA03:

*   treballem **planificació**
*   treballem **gestió**
*   treballem **documentació**


## 3.1 - Avantatges de l'automatització de tasques

### 3.1.1 El problema real: la repetició en administració de sistemes

En un sistema informàtic moltes tasques són:

*   repetitives
*   freqüents
*   crítiques si les oblidem

El problema no és **saber fer-les**, sinó **fer-les sempre** en el moment correcte.


#### Exemple real 

Un administrador ha de:

*   comprovar l'estat del sistema
*   fer neteja periòdica
*   executar ordres de manteniment

Si això es fa **manualment**, depèn de:

*   la memòria de la persona
*   el temps disponible
*   la càrrega de feina del dia

Aquí és on entra l'automatització.

### 3.1.2 Què aporta realment automatitzar 

Quan automatitzem una tasca del sistema, **no estem fent res nou**:  estem decidint **quan** i **com** s'executa una ordre que ja existeix

La diferència és el **resultat final**


### Comparació conceptual

**Abans (manual):**

*   algú entra al sistema
*   executa una ordre
*   ho fa quan se'n recorda

**Després (automatitzat):**

*   el sistema executa l'ordre
*   sempre a la mateixa hora
*   sense dependre de ningú


### 3.1.3 Avantatge 1: regularitat

La regularitat és el primer gran avantatge.

Una tasca automatitzada:

*   s'executa **sempre**
*   a l'hora prevista
*   amb el mateix comportament

Això és especialment important en tasques de:

*   manteniment
*   comprovacions periòdiques
*   rutines del sistema

Un sistema regular és **més estable** que un sistema depenent de persones

### 3.1.4 Avantatge 2: reducció d'errors humans

Els errors humans solen ser:
*   oblit
*   presses
*   cansament
*   confusió de comandes


Quan una tasca està automatitzada:

*   no s'escriu malament la comanda
*   no s'executa dues vegades
*   no s'executa fora d'horari

El sistema **no s'equivoca**, simplement fa el que té configurat

### 3.1.5 Avantatge 3: estalvi de temps 

Un administrador de sistemes **no hauria d'estar fent tasques mecàniques**

Cada tasca automatitzada:

*   allibera temps
*   permet dedicar-se a incidències reals
*   millora la gestió global del sistema

#### Exemple senzill

Si una tasca dura:

*   2 minuts
*   però s'ha de fer cada dia

En un mes: són més d'1 hora de feina repetitiva

Automatitzar-la és una decisió **professional**


### 1.6 Avantatge 4: control i traçabilitat

Una tasca automatitzada:

*   es pot **documentar**
*   es pot **revisar**
*   es pot **desactivar**
*   es pot **modificar**


Això permet saber:

*   què fa el sistema
*   quan ho fa
*   per què ho fa

Això és important en:

*   entorns professionals
*   treball en equip
*   auditories


### 1.7 Exemple amb pseudo-configuració

**Sense entrar en sintaxi real**:

```text
Tasca programada:
- Cada dia a les 02:00
- Executa un script de manteniment
- S'executa com a administrador
```

## 3.2 - Planificació de tasques del sistema

### 3.2.1 Què vol dir planificar una tasca

Planificar una tasca **no és programar** el que fa el sistema, sinó:

1.  **Quan** s'ha d'executar
2.  **Amb quina freqüència**
3.  **Amb quins permisos**

El sistema ja sap **què** ha de fer (una ordre existent), nosaltres li diem **quan** ho ha de fer


### 3.2.2 Tipus de tasques segons el temps

A la RA03 mirarem **dos grans tipus de planificació**

#### Tasques repetitives

Són tasques que:

*   s'executen de manera regular
*   segueixen un patró de temps
*   formen part del manteniment habitual del sistema

Exemples reals:

*   cada dia
*   cada setmana
*   cada mes
*   cada dilluns a una hora concreta

Aquest tipus de tasques es planifiquen amb **eines de planificació periòdica**

#### Tasques puntuals

Són tasques que:

*   s'executen **una sola vegada**
*   en un moment concret del futur
*   no es tornen a repetir

Exemples reals:

*   executar una ordre aquesta nit
*   fer una acció un dia concret
*   llançar una tasca fora d'horari laboral

Aquest tipus de tasques es planifiquen amb **eines de planificació puntual**



### 3.2.3 Planificació de tasques repetitives (visió conceptual)

```text
Tasca: manteniment
Freqüència: cada dia
Hora: 02:00
```

El sistema interpreta:

*   que la tasca **s'ha de repetir**
*   sempre en el mateix horari
*   sense intervenció humana

És exactament el que significa la planificació periòdica

### 3.2.4 Primera aproximació a la sintaxi 

Exemple en un sistema Linux però **sense entrar en detalls tècnics**

```text
minut hora dia_mes mes dia_setmana ordre
```

El temps **es descriu amb camps**


### 3.2.5 Exemple senzill de tasca repetitiva

Suposem una tasca simple:

> Executar una ordre **cada dia a les 2 del matí**

```text
0 2 * * * ordre
```

Què interpretem:

*   hi ha una **hora**
*   hi ha una **freqüència**
*   hi ha una **ordre associada**

Això **no és programació**, és **configuració temporal**


### 3.2.6 Planificació de tasques puntuals (visió conceptual)

Ara pensem en una altra situació:

> Executa aquesta ordre **una sola vegada**, aquesta nit

Aquí **no volem repetició**

Conceptualment seria:

```text
Data: avui
Hora: 22:00
Acció: executar una ordre
```

El sistema guarda aquesta informació i executa la tasca **quan toca** només una vegada

### 3.2.7 Exemple conceptual de tasca puntual

```text
Executa aquesta ordre a les 22:00 d'avui
```

El que és important:

*   no hi ha repetició
*   la tasca desapareix després d'executar-se
*   és útil per accions excepcionals


### 3.2.8 Diferència clau entre repetitiva i puntual


| Aspecte | Repetitiva | Puntual |
| --- | --- | --- |
| S'executa | Moltes vegades | Una sola vegada |
| Patró de temps | Sí | No |
| Ús habitual | Manteniment | Accions puntuals |
| Intervenció humana | No | No |

### 3.3- Restriccions i criteris de seguretat

#### 3.3.1 Per què la seguretat és clau quan automatitzem

Automatitzar una tasca vol dir **donar-li poder al sistema** perquè executi ordres **sense supervisió humana directa**    
Amb l'automatització disposem **d'eficiència**, però també existeix **un risc**

> Una tasca automatitzada mal configurada pot fer danys de manera automàtica, i el pitjor: replicar-se


Per això, abans de crear moltes tasques automàtiques, cal tindre present:

**Qui pot automatitzar què, i amb quins límits?**  

Entrem en l'àrea d'usuaris i permisos

#### 3.3.2 El principi de mínim privilegi 

Un dels principis bàsics en seguretat és el **principi de mínim privilegi**:

> Cada usuari o procés ha de tenir **només els permisos imprescindibles** per fer la seva feina

Aplicat a l'automatització:

*   no totes les tasques han de tenir permisos elevats
*   no tots els usuaris han de poder planificar tasques
*   una tasca automatitzada **no hauria de fer més del necessari**


Aquest principi apareix sovint en entorns reals i auditories


#### 3.3.3 Qui pot planificar tasques en un sistema

En un sistema multiusuari:

*   hi ha **usuaris normals**
*   hi ha **usuaris administradors**
*   hi ha **serveis del sistema**

No tots han de tenir dret a:

*   crear tasques automàtiques
*   modificar tasques existents
*   executar ordres sensibles de manera programada

Això evita:

*   abusos
*   errors greus
*   problemes de seguretat


#### 3.3.5 Riscos habituals d'una mala automatització

Quan no s'aplica criteri de seguretat, poden aparèixer problemes com:

1.  Tasques que s'executen amb massa permisos
2.  Ordres automàtiques difícils de controlar
3.  Accions que afecten tot el sistema sense supervisió
4.  Dificultat per saber **qui va crear què**


Aquests riscos passen sovint en entorns reals


#### 3.3.6 Bones pràctiques bàsiques en automatització

Sense entrar encara en configuracions concretes, un administrador hauria de seguir criteris com:

1.  Automatitzar només el que cal
2.  Assignar permisos ajustats a cada tasca
3.  Evitar automatitzacions opaques o difícils d'entendre
4.  Mantenir control i visibilitat sobre les tasques creades

Aquestes pràctiques depenen del **criteri professional**


#### 3.3.7 Exemple conceptual d'automatització segura

Tenim dues situacions:

**Situació A (incorrecta):**

*   una tasca automatitzada
*   amb permisos elevats
*   sense justificació clara

**Situació B (correcta):**

*   una tasca automatitzada
*   amb permisos ajustats
*   documentada i controlada

La diferència **és **de criteri**.

## 3.5 — Automatització de l'administració de comptes

### 3.5.1 Què vol dir “administrar comptes” en un sistema

Quan parlem d'**administració de comptes**, ens referim a tasques relacionades amb:

* Altes d'usuaris
* Baixes d'usuaris
* Modificacions de comptes
* Manteniment periòdic associat als comptes

Aquestes tasques **no són excepcionals**, formen part del dia a dia d'un administrador.


El problema apareix quan aquestes accions:

* S'han de fer sovint
* Segueixen sempre el mateix patró
* Es fan fora d'horari

Aquí és on **té sentit automatitzar** però amb criteri

---

### 3.5.2 Automatitzar comptes no és crear comptes màgicament

**Automatitzar (RA03)**
Planificar l'execució d'ordres d'administració de comptes

**Programar (RA07)**
Crear scripts:
* bucles
* condicions
* lògica complexa


En aquesta RA:

* **no** escrivim scripts
* **no** fem bucles
* **no** prenem decisions automàtiques

Només **planifiquem accions ja definides** amb programes del sistema  

---

### 3.5.3 Exemples reals d'automatització de comptes 

#### Exemple 1 — Tasca periòdica relacionada amb comptes

> Cada nit, el sistema ha de:
>
> * revisar comptes inactius
> * executar una ordre de manteniment

Això és una **tasca planificada**, no un programa  


Conceptualment:

```text
    Tasca: manteniment de comptes
    Freqüència: diària
    Execució: automàtica
```

---

#### Exemple 2 — Acció puntual sobre comptes

> Avui a les 23:00 s'ha de:
>
> * executar una ordre administrativa sobre comptes
> * fora de l'horari lectiu o laboral

Això és una **tasca puntual**, planificada una sola vegada  

```text
Tasca: acció administrativa
Hora: 23:00
Execució: única
```

Nomeś hi ha **planificació**  

---

### 3.5.4 Exemple de codi senzill amb cron

Aquí **no expliquem encara la sintaxi completa**, només veiem **el significat**  

```text
0 23 * * * ordre_administrativa
```

El que interpretem:

* S'executa a una hora concreta
* Afecta administració de comptes
* No conté lògica

---

### 3.5.5 Per què cal especial cura amb els comptes

Els comptes d'usuari:

* Donen accés al sistema
* Tenen permisos
* Poden afectar la seguretat global


Per tant:

* No totes les tasques han d'anar automatitzades
* Cal documentar molt bé què fa cada tasca
* S'ha de controlar qui les crea

---

### 3.5.6 Automatització responsable  

Automatitzar l'administració de comptes **no només és fer-ho tot automàticament**, s'ha de tenir en compte:

1. Decidir quines accions són repetitives
2. Planificar-les amb horari controlat
3. Executar-les amb permisos ajustats
4. Documentar-les correctament


---

## 3.6 Eines gràfiques de planificació  

### 3.6.1 Per què existeixen eines gràfiques de planificació

Tot i que utilktzar línia d'ordres és molt potent, **no sempre és l'opció més adequada**, sobretot quan:

*   Treballem amb equips grans
*   Hi ha diferents administradors
*   Cal mantenir una visió clara de les tasques
*   Es necessita reduir errors humans

Les **eines gràfiques de planificació** ens deixa:

*   Visualitzar les tasques existents
*   Crear-ne de noves de manera guiada
*   Gestionar-les sense memoritzar sintaxi


Aquestes eines **no substitueixen** el coneixement del sistema, però **faciliten la gestió diària**  


### 3.6.2 Instal·lació d'eines gràfiques de planificació

En molts sistemes les eines gràfiques ja venen instal·lades o es poden afegir fàcilment

L'objectiu aquí **no és memoritzar paquets**, sinó entendre el procés general:

1.  El sistema permet instal·lar una eina gràfica
2.  Aquesta eina es connecta amb el sistema de planificació
3.  Mostra la informació de manera visual


L'eina **no crea un sistema nou**, sinó que **gestiona el que ja existeix**  

### 3.6.3 Què es pot fer amb una eina gràfica

Quan obrim una eina gràfica de planificació podem:

*   Veure totes les tasques planificades
*   Identificar l'usuari que les executa
*   Comprovar horaris i freqüències
*   Activar o desactivar tasques


Això ens dona un **esquema visual**, molt útil en entorns reals  


### 3.6.4 Creació d'una tasca amb eina gràfica (visió conceptual)

Crear una tasca amb una eina gràfica sol seguir un **assistents per passos**:

1.  Definir el nom de la tasca
2.  Indicar quan s'ha d'executar
3.  Assignar l'ordre o acció
4.  Confirmar permisos


Aquesta forma de treball:

*   Redueix errors
*   Ajuda a entendre el procés
*   És ideal per aprendre

### 3.6.5 Comparació: eina gràfica vs línia d'ordres

Aquest punt és **molt habitual en exàmens** i pràctica  


| Aspecte | Línia d'ordres | Eina gràfica |
| --- | --- | --- |
| Rapidesa | Alta | Mitjana |
| Visualització | Baixa | Alta |
| Facilitat | Mitjana | Alta |
| Risc d'error | Major | Menor |
| Aprenentatge | Més exigent | Més guiat |

Cap opció és “millor” en absolut:  depèn del context i de l'objectiu  

### 3.6.6 Gestió i manteniment de tasques existents

Les eines gràfiques no només serveixen per crear tasques, també podem:

*   Modificar horaris
*   Revisar configuracions
*   Desactivar tasques temporals
*   Eliminar tasques obsoletes



## 3.7 - Utilitza eines gràfiques per a la planificació de tasques

Les eines gràfiques de planificació de tasques permeten definir i gestionar tasques automàtiques mitjançant una interfície visual, sense necessitat d'escriure directament comandes del sistema.

Aquestes eines no introdueixen mecanismes nous de planificació, sinó que actuen com una capa d'interfície sobre els mateixos sistemes interns que utilitza la planificació per comandes.

El funcionament final de la tasca programada és el mateix, independentment de si s'ha configurat mitjançant una eina gràfica o mitjançant línia d'ordres.

---

### 3.7.1 - Definició de tasques mitjançant interfície gràfica

Mitjançant una eina gràfica, l'administrador pot definir tasques automàtiques indicant de manera visual:

- el moment d'execució de la tasca
- l'acció que s'ha d'executar
- l'usuari amb el qual s'executa
- les condicions associades a l'execució

Aquests elements es configuren a través de camps, menús i assistents que guien el procés de creació de la tasca.

---

### 3.7.2 - Gestió de tasques existents

Les eines gràfiques permeten consultar i gestionar les tasques ja definides al sistema, mostrant-ne informació com ara:

- estat de la tasca
- periodicitat
- acció associada
- últimes execucions

Aquesta visualització facilita la supervisió de les tasques programades i la detecció d'errors de configuració.

---

### 3.7.3 - Relació entre eines gràfiques i planificació per comandes

Les eines gràfiques i les comandes del sistema treballen sobre els mateixos conceptes bàsics de planificació:

- quan s'executa una tasca
- què executa
- amb quin context d'usuari
- en quines condicions

La diferència principal rau en la forma d'interacció amb el sistema, no en el resultat obtingut.

Una tasca creada mitjançant una eina gràfica pot ser equivalent a una tasca definida amb una comanda, i viceversa.

---

### 3.7.4 - Avantatges de l'ús d'eines gràfiques

L'ús d'eines gràfiques pot resultar especialment útil en situacions com:

- administradors amb poca experiència en línia d'ordres
- configuracions puntuals
- entorns on es prioritza la claredat visual
- necessitat de revisar ràpidament l'estat de les tasques

La interfície visual redueix errors de sintaxi i facilita la comprensió de la configuració.

---

### 3.7.5 - Limitacions de les eines gràfiques

Tot i els seus avantatges, les eines gràfiques també presenten limitacions:

- menor flexibilitat en configuracions avançades
- menor eficiència en la creació massiva de tasques
- dificultat per reutilitzar configuracions de manera automatitzada

Per aquest motiu, en entorns professionals sovint es combinen eines gràfiques i comandes segons les necessitats del sistema.




## 3.8 - Documentació de tasques automàtiques

### 3.8.1 Per què documentar és part de l'automatització

Una tasca automatitzada **no és només una configuració tècnica**. És una decisió administrativa que:

*   Afecta el sistema
*   Pot tenir impacte en usuaris
*   Pot durar anys

**Si no està documentada**, és com si no existís



Un sistema sense documentació és un sistema difícil de mantenir  

### 3.8.2 Què pot passar si no documentem

Exemple de situació molt habitual en entorns reals.


**Situació:**

*   Una tasca s'executa cada nit
*   Ningú recorda qui la va crear
*   Ningú sap exactament què fa
*   Ningú sap si encara és necessària

Això provoca:

*   errors difícils de diagnosticar
*   pèrdua de temps
*   riscos de seguretat
*   dependència d'una sola persona

Tot això **no es tracta d'un problema tècnic**, és un problema de **manca de documentació**  


### 3.8.3 Què s'ha de documentar d'una tasca automàtica

Documentar **no vol dir escriure un manual llarg**, sinó deixar clara la informació essencial  

Una tasca automatitzada hauria de tenir documentat:

1.  **Què fa**
2.  **Quan s'executa**
3.  **Amb quin usuari o permisos**
4.  **Per què existeix**


Aquesta informació permet que qualsevol altre administrador:

*   Entengui la tasca
*   La pugui revisar
*   Decideixi si cal mantenir-la o eliminar-la


### 3.8.4 Exemple de documentació senzilla 

Un exemple **realista** però senzill:

```text
Nom de la tasca: Manteniment de comptes
Descripció: Executa una ordre de manteniment relacionada amb comptes d'usuari
Freqüència: Diària
Hora: 02:00
Usuari d'execució: sistema
Motiu: Evitar acumulació de comptes inactius
```

Podem verure que és un exemple:

*   És curt
*   És clar
*   No entra en detalls tècnics innecessaris
*   Compleix perfectament el criteri RA03

### 3.8.5 Documentació i treball en equip

En entorns professionals:

*   No hi ha un sol administrador
*   Hi ha torns
*   Hi ha canvis de personal


Lavors la documentació ens permet:

*   Continuïtat del servei
*   Menys dependència d'una persona concreta
*   Millor manteniment del sistema

A l'entorn laboral es tracta d'una **garantia de qualitat** 

### 3.8.6 Relació amb seguretat i auditoria

La documentació també ens serveix per:

*   Auditories internes
*   Revisions de seguretat
*   Comprovacions de bones pràctiques


Una tasca documentada es pot:

*   Justificar
*   Revisar
*   Defensar davant d'una auditoria




