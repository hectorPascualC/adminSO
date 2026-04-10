# Pràctica 2 — Impressora compartida en xarxa amb CUPS

## Relació amb la RA06

Aquesta pràctica treballa principalment els aspectes següents de la RA06:

- serveis per compartir recursos en xarxa
- configuració d’impressores compartides
- ús de recursos des de sistemes diferents
- comprovació del funcionament en una xarxa heterogènia
- documentació de la configuració realitzada

## Objectiu

L’objectiu d’aquesta pràctica és instal·lar i configurar **CUPS** en un servidor Linux, afegir-hi una **impressora**, compartir-la en xarxa i comprovar que es pot utilitzar des d’un **equip client**, preferentment Windows.

## Entorn de treball

Per fer aquesta pràctica es recomana treballar amb:

- **1 màquina Ubuntu Server** que actuarà com a servidor d’impressió
- **1 màquina client**, preferentment **Windows**
- una impressora disponible per fer la prova
- connexió de xarxa entre servidor i client

Si no es disposa d’una impressora física, es pot fer la configuració igualment i treballar amb la detecció i la compartició del recurs fins on l’entorn ho permeti.

## Enunciat

El servidor Linux haurà d’actuar com a **servidor d’impressió** dins d’una xarxa local. Per fer-ho, haurà de gestionar una impressora mitjançant CUPS i posar-la a disposició d’altres equips.

Després de la configuració, un client de la xarxa haurà de poder localitzar la impressora compartida, afegir-la al seu sistema i imprimir una pàgina de prova.

## Tasques a realitzar

### 1. Comprovació inicial de la xarxa

Abans de configurar el servei, comprova que el client i el servidor es veuen correctament a la xarxa.

## 2. Instal·lació de CUPS

Comprova si **CUPS** està instal·lat al servidor Ubuntu. Si no hi és, instal·la’l.

Després, comprova que el servei està actiu i disponible al sistema.

## 3. Accés a la gestió del servidor d’impressió

Accedeix a la interfície o mecanisme de gestió de CUPS i comprova que el servidor d’impressió està operatiu.

## 4. Afegir una impressora al servidor

Afegeix una impressora al sistema.

En aquest punt has de comprovar:

- que el servidor detecta la impressora
- que queda registrada correctament
- que el sistema pot gestionar-la

## 5. Compartir la impressora en xarxa

Configura la impressora perquè pugui ser utilitzada des d’altres equips de la xarxa.

La configuració ha de deixar clar:

- que la impressora és visible com a recurs compartit
- que es pot utilitzar des d’un client
- que la configuració del servei permet l’accés remot

## 6. Comprovació local des del servidor

Abans d’anar al client, verifica des del mateix servidor que la impressora està ben configurada.

## 7. Accés des del client

Des d’un equip client, localitza la impressora compartida del servidor.

L’alumne haurà de:

- detectar la impressora a la xarxa
- afegir-la al client
- comprovar que queda disponible per imprimir

## 8. Impressió de prova

Un cop la impressora estigui configurada al client, envia una **pàgina de prova**.

L’objectiu és verificar que:

- el treball arriba al servidor
- la cua d’impressió es processa correctament
- la impressora respon com cal

## 9. Prova en entorn heterogeni

Sempre que sigui possible, aquesta part s’ha de fer des d’una **màquina Windows**, perquè això reforça justament la part d’interoperabilitat que treballa la RA06.

## Evidències que ha de lliurar l’alumne

L’alumne ha d’entregar evidències del treball realitzat. Com a mínim:

- captura de la instal·lació de CUPS o comprovació del servei
- captura de la impressora configurada al servidor
- captura que mostri que la impressora està compartida
- captura de la detecció de la impressora des del client
- captura de la impressora afegida al client
- evidència de la pàgina de prova o del treball enviat a la cua d’impressió

## Resultat final esperat

Al final de la pràctica, el servidor Linux hauria d’oferir una impressora compartida en xarxa gestionada amb CUPS, i un client de la xarxa hauria de poder afegir-la i imprimir una pàgina de prova.
