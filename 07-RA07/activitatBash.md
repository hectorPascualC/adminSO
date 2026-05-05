# Pràctica RA07 - Automatització de tasques en Linux amb Bash

## Relació amb la RA07

Aquesta pràctica treballa principalment aquests aspectes de la RA07:

- ús d’estructures bàsiques del llenguatge
- creació i execució d’un script en Bash
- comprovació i correcció bàsica d’errors
- ús d’ordres d’administració del sistema
- administració de comptes d’usuari, processos i serveis
- documentació del guió creat

## Objectiu de la pràctica

L’objectiu és crear un **script Bash** que permeti automatitzar una sèrie de comprovacions i tasques bàsiques d’administració en un sistema Linux.

Al final de la pràctica hauries d’haver aconseguit això:

- crear un fitxer de script executable
- comprovar si existeix un usuari determinat
- comprovar si un procés concret està en execució
- comprovar l’estat d’un servei del sistema
- generar un fitxer de còpia de seguretat
- guardar el resultat de l’execució en un fitxer de log

## Escenari de treball

Treballarem amb aquest escenari mínim:

- **1 màquina Linux** (preferentment Ubuntu o Debian)
- accés a terminal
- permisos suficients per consultar serveis i fitxers del sistema
- editor de text per crear l’script

## Enunciat

Has de crear un script anomenat:

```bash
ra07_tasca_automatica.sh
```

Aquest script haurà de fer, com a mínim, aquestes accions:

### 1. Mostrar un missatge d’inici
Quan s’executi, el guió ha d’indicar que la tasca ha començat.

### 2. Comprovar si existeix un usuari concret
El guió ha de comprovar si existeix un usuari amb el nom:

```bash
alumne
```

Si existeix, ha de mostrar un missatge indicant-ho.  
Si no existeix, també ho ha d’indicar.

### 3. Comprovar si hi ha un procés concret en execució
El guió ha de comprovar si hi ha un procés actiu amb el nom:

```bash
bash
```

Si està en execució, ho ha de mostrar per pantalla.  
Si no hi és, també ho ha d’indicar.

### 4. Comprovar l’estat d’un servei
El guió ha de comprovar l’estat d’un servei del sistema. Pots fer servir, per exemple:

```bash
cron
```

o bé un altre servei equivalent que existeixi al teu sistema.

El guió ha d’indicar si el servei està actiu o no.

### 5. Crear una còpia de seguretat
El guió ha de generar una còpia d’un fitxer de text dins del directori de treball.

Per exemple, si existeix un fitxer anomenat:

```bash
dades.txt
```

el guió n’ha de crear una còpia amb un nom semblant a:

```bash
dades_backup.txt
```

Si el fitxer original no existeix, el guió ho ha d’indicar.

### 6. Crear un fitxer de log
Tot el que fa el guió ha de quedar registrat en un fitxer de log, per exemple:

```bash
registre.log
```

Aquest fitxer ha de contenir com a mínim:

- data o moment d’execució
- resultat de la comprovació de l’usuari
- resultat de la comprovació del procés
- resultat de la comprovació del servei
- resultat de la còpia de seguretat

### 7. Mostrar un missatge final
Quan el guió acabi, ha d’indicar que l’execució ha finalitzat correctament.

## Requisits tècnics

L’script ha de complir aquestes condicions:

- ha d’estar escrit en **Bash**
- ha de començar amb la línia:

```bash
#!/bin/bash
```

- ha de contenir **variables**
- ha de contenir com a mínim una estructura **if**
- ha d’utilitzar ordres pròpies de Linux relacionades amb administració del sistema
- ha d’estar comentat mínimament perquè s’entengui què fa cada part
- ha de tenir permís d’execució


## Què has d’entregar 

### 1. El fitxer del script
```text
ra07_tasca_automatica.sh
```

### 2. Una captura o captures de pantalla
On es vegi:

- el contingut del script
- l’execució del script
- el resultat del log generat
- la comprovació que la còpia de seguretat s’ha creat correctament

## Criteris de valoració

Es valorarà especialment:

- que l’script s’executi correctament
- que faci totes les comprovacions demanades
- que utilitzi bé estructures bàsiques de Bash
- que el log reculli la informació mínima necessària
- que el codi estigui ordenat i entenedor
- que la pràctica quedi ben presentada

## Observació final

Aquesta pràctica està centrada en **scripting en Bash per a Linux**.  
L’objectiu no és fer una pràctica de configuració manual del sistema, sinó crear un **guió** que ajudi a automatitzar tasques bàsiques d’administració.
