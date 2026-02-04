# Pràctica 1 — Identificació i anàlisi de tasques repetitives del sistema

**Resultat d’aprenentatge:** RA03 — Gestiona l’automatització de tasques del sistema
**Criteris d’avaluació associats:**

* **3.1** Descriu els avantatges de l’automatització de les tasques repetitives del sistema
* **3.2** Utilitza comandes del sistema per a la planificació de tasques (nivell conceptual)
* **3.4** Realitza planificacions de tasques repetitives o puntuals (anàlisi, no execució)


## Intro

### Objectiu de la pràctica

L’objectiu d’aquesta pràctica és que aprenguis a identificar tasques repetitives que es realitzen manualment en el sistema i analitzar si poden ser automatitzades, explicant els avantatges i possibles riscos de l’automatització.
A través d’aquesta pràctica s’ha de comprendre:

* què és una tasca repetitiva,
* per què és convenient automatitzar-la,
* quins avantatges aporta l'automatització,
* i quins riscos pot tenir una mala automatització.

**En aquesta pràctica no s'implementaran tasques automàtiques**, només se’n farà l’anàlisi i justificació.

### Context de treball

La pràctica s’ha de realitzar sobre **un sistema operatiu real o virtual**, a escollir entre:

* Linux (Ubuntu Desktop o Ubuntu Server)
* Windows o Windows Server

No és obligatori utilitzar VirtualBox, però es pot fer servir si es prefereix treballar en un entorn de proves

---

## Tasques a realitzar

### 1. Anàlisi inicial del sistema

Descriu breument el sistema operatiu utilitzat indicant:

* tipus de sistema (Linux o Windows),
* ús previst del sistema: usuari, servidor, entorn VB...
* tasques habituals que s’hi duen a termeS

---

### 2. Identificació de tasques repetitives

Identifica **com a mínim 3 tasques repetitives reals** del sistema analitzat

Per ajudar-te a detectar aquest tipus de tasques, fixa't en **accions de manteniment i control** que es realitzen de manera periòdica i sempre segueixen el mateix procediment.

A continuació es mostren **exemples orientatius** de tasques que habitualment són repetitives:

* còpies de seguretat de fitxers o carpetes importants
* neteja de fitxers temporals o antics
* comprovació d’espai en disc
* revisió de logs del sistema o d’aplicacions
* generació d’informes periòdics
* actualitzacions del sistema o de components concrets

L'important no és l’exemple escollit, sinó **justificar per què la tasca és repetitiva**

---

### 3. Classificació de les tasques

Per a cada tasca identificada, indica si es tracta d’una:

* tasca **repetitiva**, o
* tasca **puntual**.

Justifica breument la classificació realitzada.

---

### 4. Justificació de l’automatització

Per a cada tasca, explica:

* per què és adequada (o no) per ser automatitzada,
* quin avantatge aportaria l'automatització (estalvi de temps, reducció d’errors, regularitat, control, ...

Relaciona aquesta justificació amb el criteri **3.1** de la RA03

---

### 5. Anàlisi bàsica de riscos

Per a cada tasca analitzada, descriu de manera **conceptual**:

* quin risc podria tenir una mala automatització,
* quin tipus d’error podria produir-se,
* quin seria l’impacte aproximat de l’error (baix, mitjà o alt).

No cal entrar en gestió avançada de permisos ni polítiques complexes.

---

## Taula-model d’identificació de tasques repetitives

Utilitza la taula següent per documentar les tasques identificades:

| Tasca identificada | Freqüència (diària, setmanal, puntual…) | Justificació (per què és repetitiva) |
| ------------------ | --------------------------------------- | ------------------------------------ |
|                    |                                         |                                      |
|                    |                                         |                                      |
|                    |                                         |                                      |

---

## Lliurament

Cal lliurar **un document escrit** (PDF o Markdown) que inclogui:

* introducció breu,
* desenvolupament dels punts 1 a 5,
* taula de tasques completada,
* conclusions finals

