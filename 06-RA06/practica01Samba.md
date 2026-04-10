# Pràctica 1 — Compartició de carpetes en xarxa amb Samba

## Relació amb la RA06

Aquesta pràctica treballa principalment els aspectes següents de la RA06:

- compartició de recursos en xarxa entre sistemes diferents
- seguretat i control d’accés
- serveis de xarxa per compartir recursos
- configuració de recursos compartits
- comprovació del funcionament en un entorn heterogeni

## Objectiu

L’objectiu d’aquesta pràctica és instal·lar i configurar un **servidor Samba** en un equip amb **Ubuntu Server**, crear una **carpeta compartida**, preparar els **usuaris i permisos** necessaris i comprovar l’accés al recurs des d’un **equip client** de la xarxa.

## Entorn de treball

Per fer aquesta pràctica es recomana treballar, com a mínim, amb aquest escenari:

- **1 màquina Ubuntu Server** que actuarà com a servidor
- **1 màquina client**, preferentment **Windows**, tot i que també es pot utilitzar Linux
- connexió de xarxa entre les dues màquines
- permisos d’administració al servidor

## Enunciat

En una xarxa local mixta, el servidor Linux ha de proporcionar un recurs compartit perquè els usuaris d’altres equips hi puguin accedir. Aquest recurs serà una carpeta comuna de treball.

L’objectiu és que el servidor comparteixi correctament aquesta carpeta mitjançant Samba i que un client de la xarxa hi pugui entrar amb l’usuari corresponent.

La carpeta haurà d’estar configurada amb un accés limitat, de manera que l’usuari client pugui consultar-ne el contingut i, segons la configuració aplicada, escriure-hi o només llegir.

## Tasques a realitzar

### 1. Comprovació inicial de la xarxa

Abans de començar la configuració del servei, comprova que el servidor i el client es veuen correctament a la xarxa.

Has de verificar:

- l’adreça IP del servidor
- l’adreça IP del client
- que hi ha connectivitat entre els dos equips

## 2. Instal·lació de Samba

Al servidor Ubuntu, comprova si **Samba** està instal·lat. Si no hi és, instal·la’l.

Després, comprova que el servei ha quedat disponible al sistema.

## 3. Creació de la carpeta compartida

Crea una carpeta específica per a aquesta pràctica.  
Per exemple:

```bash
/srv/compartit
```

Aquesta carpeta serà el recurs que es publicarà a la xarxa.

## 4. Assignació de permisos al directori

Configura els permisos del directori perquè l’usuari o usuaris autoritzats hi puguin accedir correctament.

En aquest punt has de decidir:

- quin usuari serà el propietari
- quin grup hi tindrà accés
- si el recurs serà només de lectura o també d’escriptura

## 5. Configuració del recurs a Samba

Edita el fitxer de configuració de Samba i afegeix-hi una nova compartició per a la carpeta creada.

La compartició ha de tenir:

- un **nom visible a la xarxa**
- una **ruta real** al sistema Linux
- unes **condicions d’accés**
- una configuració coherent amb els permisos del sistema

## 6. Alta o preparació de l’usuari de Samba

Prepara un usuari que pugui autenticar-se per accedir a la carpeta compartida.

Aquest usuari haurà de:

- existir al sistema
- quedar habilitat per a Samba
- tenir una contrasenya de Samba

## 7. Reinici i comprovació del servei

Després de la configuració, reinicia o recarrega Samba i comprova que el servei funciona correctament.

## 8. Accés des del client

Des d’un equip client de la xarxa, accedeix al recurs compartit.

Cal comprovar:

- que el recurs és visible
- que demana autenticació si està configurat així
- que l’usuari pot entrar-hi
- que pot llegir el contingut
- que pot escriure-hi només si els permisos ho permeten

## 9. Verificació final

Comprova que la configuració és coherent amb el comportament real del recurs.

Si la carpeta és de només lectura, el client no hauria de poder modificar res.  
Si és de lectura i escriptura, hauria de poder crear o editar fitxers.

## Evidències que ha de lliurar l’alumne

L’alumne ha d’entregar evidències del treball realitzat. Com a mínim:

- captura de la IP del servidor
- captura de la instal·lació de Samba o comprovació del servei
- captura de la carpeta creada al servidor
- fragment rellevant del fitxer de configuració de Samba
- captura de l’alta o configuració de l’usuari
- captura de l’accés des del client
- prova que demostri si el recurs permet o no escriptura

## Resultat final esperat

Al final de la pràctica, el sistema hauria de permetre que un client accedeixi des de la xarxa a una carpeta compartida allotjada en el servidor Linux mitjançant Samba, amb el nivell d’accés que s’hagi configurat.
