# Pràctica 2 - Impressora compartida en xarxa amb CUPS

## Relació amb la RA06

Aquesta pràctica treballa principalment aquests aspectes de la RA06:

- serveis per compartir recursos en xarxa
- configuració d'impressores compartides
- ús de recursos des de sistemes diferents
- comprovació del funcionament en una xarxa heterogènia
- documentació de la configuració realitzada

## Objectiu de la pràctica

L'objectiu és configurar un servidor **Ubuntu Server** perquè actuï com a **servidor d'impressió** mitjançant **CUPS**. La impressora haurà de quedar disponible a la xarxa i s'haurà de poder utilitzar des d'un altre equip client, preferentment **Windows**, tot i que també podria ser un altre Linux.

Al final de la pràctica hauries d'haver aconseguit això:

- el servidor té **CUPS instal·lat i funcionant**
- la impressora està donada d'alta al servidor
- la impressora està compartida en xarxa
- el client pot afegir la impressora
- pots enviar una pàgina de prova i comprovar que arriba a la cua d'impressió

## Escenari de treball

Treballarem amb aquest escenari mínim:

- **1 màquina Ubuntu Server** que actuarà com a servidor d'impressió
- **1 màquina client**, preferentment Windows
- **1 impressora disponible** per fer la pràctica
- connexió de xarxa entre servidor i client


## Resum pasos de la pràctica

- primer comprovem la xarxa
- després instal·lem CUPS
- després verifiquem que el servei està actiu
- després preparem el servidor perquè permeti administració i accés remot
- després comprovem el tallafoc
- després donem d'alta la impressora al servidor
- després la compartim en xarxa
- després la provem localment al servidor
- després l'afegim al client
- finalment fem una impressió de prova

Si alguna cosa falla, has de tornar enrere i revisar aquestes capes en aquest mateix ordre.

## Idea clau 

Perquè un client pugui imprimir mitjançant una impressora compartida, han de funcionar:

- la xarxa entre client i servidor
- el servei **CUPS** al servidor
- la impressora real o virtual donada d'alta al servidor
- la compartició de la impressora a la xarxa
- l'accés del client a la cua d'impressió

En aquesta pràctica has de recordar que **CUPS** és el sistema d'impressió habitual en Linux. S'encarrega de:

- gestionar impressores
- administrar cues d'impressió
- compartir impressores en xarxa
- processar els treballs enviats pels clients

El client no “utilitza CUPS per dins”, però sí que pot fer servir una impressora que està gestionada per CUPS en un servidor Linux.

## 1. Comprovació inicial de la xarxa

## 2. Instal·lació de CUPS

Comprova si CUPS està instal·lat al servidor.

Escriu:

```bash
lpstat -r
```

Si CUPS està instal·lat i el planificador està en funcionament, veuràs una sortida semblant a aquesta:

```text
scheduler is running
```

Si no, instal·la CUPS amb:

```bash
sudo apt update
sudo apt install cups -y
```

## 3. Comprovar que el servei està actiu

Després de la instal·lació, comprova l'estat del servei:

```bash
systemctl status cups
```

El resultat:

```text
active (running)
```

Si el servei no està actiu inicia:

```bash
sudo systemctl start cups
```

Si vols assegurar que arrenqui automàticament en reiniciar el sistema:

```bash
sudo systemctl enable cups
```

Torna a comprovar l'estat amb:

```bash
systemctl status cups
```

## 4. Preparar l'usuari administrador de CUPS

Per administrar impressores des de la interfície web de CUPS, és recomanable que l'usuari que utilitzis estigui dins del grup `lpadmin`

Comprova quin usuari estàs fent servir al servidor:

```bash
whoami
```

Afegeix aquest usuari al grup `lpadmin`:

```bash
sudo usermod -aG lpadmin EL_TEU_USER
```

## 5. Permetre administració i compartició remota a CUPS

En molts sistemes Ubuntu CUPS queda preparat per administrar-se només des del mateix servidor. Per aquesta pràctica, convé preparar CUPS per a accés i compartició remots

Executa:

```bash
sudo cupsctl WebInterface=yes
sudo cupsctl --remote-admin --remote-any --share-printers
```

Això fa 3 coses:

- activa la interfície web
- permet administració remota
- activa la compartició d'impressores

Reinicia CUPS:

```bash
sudo systemctl restart cups
```

Comprova l'estat:

```bash
systemctl status cups
```

## 6. Comprovar el tallafoc

Si el tallafoc del servidor està actiu, cal permetre l'accés al port de CUPS

Comprova l'estat amb:

```bash
sudo ufw status
```

Si surt `inactive`, pots continuar
Si surt `active`, permet el port de CUPS:

```bash
sudo ufw allow 631/tcp
```

Torna a comprovar l'estat:

```bash
sudo ufw status
```

Hauries de veure que el port `631/tcp` està permès

## 7. Accedir a la interfície web de CUPS

Ara has de comprovar que la interfície web respon correctament  

Obre un navegador des d'un altre equip de la xarxa per escriure:

```text
http://IP_DEL_SERVIDOR:631
```

Substitueix la IP per la del teu servidor.

Hauries de veure la pàgina principal de CUPS  

Si el navegador no carrega la pàgina revisa:

- que el servei `cups` estigui actiu
- que la IP del servidor sigui correcta
- que el tallafoc no bloquegi el port 631
- que hagis executat `cupsctl --remote-admin --remote-any --share-printers`

## 8. Detectar les impressores disponibles al servidor

Comprova si el sistema detecta el dispositiu d'impressió

Escriu:

```bash
lpinfo -v
```

Aquesta ordre mostra els dispositius d'impressió visibles pel sistema

Si tens una impressora connectada per USB al servidor, hauria d'aparèixer una línia relacionada amb USB. Si és una impressora de xarxa, podria aparèixer com a dispositiu descobert a la xarxa  

Si aquí no apareix cap impressora revisa:

- connexió física de la impressora
- si la impressora està encesa
- si el cable USB o la connexió de xarxa és correcta

## 9. Afegir la impressora a CUPS

Ara has de donar d'alta la impressora al servidor

Des de la interfície web de CUPS:

- `Administration`
- `Add Printer`

Quan el sistema et demani credencials posa l'usuari del grup `lpadmin`

Mostrarà les impressores disponibles

Selecciona la impressora

A dades de configuració, assigna un nom:

```text
aula1
```

Si hi ha una casella de tipus **Share This Printer** activa-la. Si no la marques o no actives la compartició, la impressora pot quedar instal·lada al servidor però no visible per als clients de la xarxa

## 10. Comprovar que la impressora ha quedat registrada

Torna a la terminal del servidor i comprova les impressores configurades:

```bash
lpstat -p -d
```

Exemple del que hauria de sortir:

```text
printer aula1 is idle. enabled since ...
system default destination: aula1
```

- la impressora existeix a CUPS
- la impressora està activa
- mostra la impressora per defecte

## 11. Compartir la impressora correctament

Tot i que la impressora com a compartida, convé revisar que CUPS la publiqui correctament

A CUPS entra a la impressora que acabes de crear i comprova:

- que apareix com a compartida
- que no està pausada
- que no té cap error de configuració

Si veus opcions com **Maintenance**, **Administration** o **Print Test Page**: la impressora està creada i gestionable.

## 12. Fer una prova local des del servidor

Mirar que el servidor ja pot utilitzar la impressora. Tens dues maneres:

- Una és des de la interfície web de CUPS, entrant a la impressora i buscant una opció com:
    - `Maintenance`
    - `Print Test Page`

- La segona és des de terminal. Per exemple, pots enviar un fitxer de text petit a la impressora:

```bash
echo "Prova CUPS des del servidor" | lp -d aula1
```

Substitueix `aula1` pel nom de la teva impressora

Si la impressora és correcta i la cua funciona, aquest treball hauria d'entrar a la cua d'impressió  

Per comprovar la cua:

```bash
lpstat -o
```

Si hi ha treballs en cua els veuràs

Si la impressora respon i imprimeix, ja tens la prova local superada

## 13. Comprovar que la cua d'impressió està disponible

Per veure tota la informació general de CUPS:

```bash
lpstat -t
```

Mostra:

- si el planificador està actiu
- quina impressora és la predeterminada
- quines impressores hi ha
- si hi ha treballs a la cua

És una de les **millors ordres de diagnosi per aquesta pràctica**

## 14. Afegir la impressora des del client Windows

Ara passa al client Windows  

Primer:

- obre **Configuració**
- entra a **Impressores i escàners**
- prem **Afegeix un dispositiu**

Si la impressora apareix selecciona-la

Si no apareix, fes servir el mètode manual

A la mateixa pantalla escull una de l'opció:

- **La impressora que vull no és a la llista**

Després selecciona l'opció per afegir una impressora per nom o per URL i escriu una adreça com aquesta:

```text
http://LA_TEVA_IP:631/printers/aula1
```
## 15. Si Windows demana un controlador

Quan Windows afegeix una impressora per URL, de vegades pot instal·lar-la automàticament. Altres vegades et demanarà un controlador  

Si això passa, prova en ordre:

- deixar que Windows la detecti automàticament
- si t'ofereix un controlador de tipus **Microsoft IPP Class Driver**, seleccionar-lo
- triar el controlador del fabricant si està disponible al sistema

## 16. Comprovar que el client veu la impressora

Quan la impressora quedi afegida a Windows, comprova que apareix a la llista d'impressores instal·lades  

## 17. Enviar una pàgina de prova des del client

Ara entra a les propietats de la impressora des del client Windows:

- **Imprimeix una pàgina de prova**

Envia aquesta pàgina.

## 18. Verificar la impressió des del servidor

Torna al servidor i comprova la cua:

```bash
lpstat -o
```

o

```bash
lpstat -t
```
Si la impressora imprimeix o el treball entra i surt correctament de la cua, està funcionant com toca.

## 19. Resolució de problemes

Si el client no arriba al servidor, torna a comprovar la xarxa amb `ping`. Si no hi ha resposta, revisa IPs, estat de les màquines, configuració de la xarxa...

Si la pàgina `http://IP_DEL_SERVIDOR:631` no obre:

- que el servei `cups` estigui actiu
- que el port 631 no estigui bloquejat
- que hagis executat `cupsctl --remote-admin --remote-any --share-printers`

Si CUPS funciona però no detecta cap impressora:

```bash
lpinfo -v
```

Si aquí no surt la impressora el problema no és de compartició, sinó de detecció del dispositiu  

Si la impressora existeix a CUPS però no imprimeix, comprova si la cua està pausada:

```bash
lpstat -p -d
```

Si la impressora està pausada o deshabilitada:

```bash
sudo cupsenable aula1
sudo cupsaccept aula1
```

Si Windows no troba la impressora, mira:

- la IP del servidor
- el nom exacte de la impressora
- que la URL sigui correcta
- que la impressora estigui compartida a CUPS
- que el tallafoc no bloquegi el port 631

Si Windows afegeix la impressora però la pàgina de prova no s'imprimeix:

- el controlador instal·lat al client
- la cua d'impressió al servidor
- si la impressora real està en estat correcte

## Checklist final d'autorevisió

Abans de donar la pràctica per acabada, comprova això:

- [ ] conec la IP del servidor
- [ ] el `ping` entre client i servidor funciona
- [ ] el servei `cups` està actiu
- [ ] puc obrir `http://IP_DEL_SERVIDOR:631`
- [ ] la impressora apareix detectada o registrada al servidor
- [ ] la impressora està compartida
- [ ] el client ha pogut afegir la impressora
- [ ] he enviat una pàgina de prova
- [ ] he comprovat la cua d'impressió al servidor
- [ ] puc demostrar que la impressora funciona des del client
