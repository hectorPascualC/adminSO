# Pràctica 1 - Compartició de carpetes en xarxa amb Samba

## Manual 

## Relació amb la RA06

Aquesta pràctica treballa principalment aquests aspectes de la RA06:

- compartició de recursos en xarxa entre sistemes diferents
- seguretat i control d'accés
- serveis de xarxa per compartir recursos
- configuració de recursos compartits
- comprovació del funcionament en un entorn heterogeni

## Objectiu de la pràctica

L'objectiu és configurar un servidor **Ubuntu Server** perquè comparteixi una carpeta en xarxa mitjançant **Samba**. Aquesta carpeta haurà de ser accessible des d'un altre equip client, preferentment **Windows**, tot i que també podria ser un altre Linux.

Al final de la pràctica hauries d'haver aconseguit això:

- el servidor té **Samba instal·lat i funcionant**
- existeix una carpeta compartida al servidor
- hi ha un usuari preparat per entrar al recurs
- des del client es pot accedir a la carpeta compartida
- pots demostrar si el recurs és només de lectura o també permet escriptura

## Escenari de treball

Treballarem amb aquest escenari mínim:

- **1 màquina Ubuntu Server** que actuarà com a servidor Samba
- **1 màquina client**, preferentment Windows
- les dues màquines han d'estar connectades a la mateixa xarxa

Esquema bàsic:

```text
Client Windows o Linux  ───── Xarxa local ───── Ubuntu Server amb Samba
```

## Com funciona la pràctica

Seqüència:

- primer comprovem la xarxa
- després instal·lem Samba
- després creem la carpeta real del servidor
- després preparem l'usuari i els permisos Linux
- després configurem la compartició a Samba
- després reiniciem i comprovem el servei
- després entrem des del client
- finalment provem lectura i escriptura

Si alguna cosa falla al final, has de tornar enrere i revisar cada capa en aquest mateix ordre

## Idea clau abans de començar

Perquè un client pugui entrar a una carpeta compartida han de funcionar alhora quatre coses:

- la xarxa entre client i servidor
- la carpeta real al servidor
- l'usuari que hi accedeix
- el servei Samba que publica el recurs

A més, has de recordar que hi ha **dues capes de permisos**.

Els **permisos Linux** són els permisos reals del directori dins del servidor. Els pots consultar amb una comanda com aquesta:

```bash
ls -ld /srv/compartit
```

Això controla què pot fer el propietari, el grup i la resta d'usuaris sobre la carpeta real

Els **permisos o restriccions Samba** són les condicions amb què Samba publica la carpeta a la xarxa. Per exemple:

```ini
read only = no
valid users = sambauser
```

Això controla si Samba deixa entrar a l'usuari i si la compartició és de només lectura o no.

Encara que Samba estigui ben configurat, si els permisos Linux són incorrectes, el client pot veure la carpeta però no podrà treballar-hi correctament.

## 1 - Comprovació inicial de la xarxa

Confirmar que **client i servidor es poden veure**

## 2 - Instal·lació de Samba

Comprovarem si Samba està instal·lat al servidor.

Escriu:

```bash
smbd --version
```

Si Samba està instal·lat, hauria d'aparèixer un número de versió, per exemple:

```text
Version 4.x.x-Ubuntu
```

Si la comanda no és reconeguda o no retorna cap versió, instal·la Samba amb:

```bash
sudo apt update
sudo apt install samba -y
```

## 3 - Comprovar que el servei està actiu

Després de la instal·lació, comprova l'estat del servei Samba:

```bash
systemctl status smbd
```

El resultat correcte hauria d'incloure una línia semblant a aquesta:

```text
active (running)
```

Si no està actiu, inicia'l amb:

```bash
sudo systemctl start smbd
```

I si vols assegurar que arrenqui automàticament en reiniciar el sistema, executa també:

```bash
sudo systemctl enable smbd
```

Després, torna a comprovar l'estat amb:

```bash
systemctl status smbd
```

No passis al següent pas fins que el servei aparegui com a actiu

## 4 - Creació de la carpeta compartida

Ara crearem la carpeta que es compartirà a la xarxa.

Executa:

```bash
sudo mkdir -p /srv/compartit
```

Aquesta carpeta serà el recurs que es publicarà mitjançant Samba.

Després comprova que existeix:

```bash
ls -ld /srv/compartit
```

Has de veure una línia amb informació del directori. En aquest moment encara és normal que aparegui com a propietari `root`

## 5 - Crear l'usuari i preparar permisos Linux

Per fer la pràctica de manera clara, prepararem un usuari específic.

Crea un usuari anomenat `sambauser`:

```bash
sudo adduser sambauser
```

El sistema et demanarà una contrasenya i dades opcionals

Farem que aquest usuari sigui el propietari de la carpeta compartida:

```bash
sudo chown -R sambauser:sambauser /srv/compartit
```

Dona permisos bàsics al directori:

```bash
sudo chmod -R 755 /srv/compartit
```

Aquesta configuració vol dir que el propietari tindrà r, w i x, mentre que el grup i la resta tindran r i x

Per comprovar el resultat:

```bash
ls -ld /srv/compartit
```

Ara hauries de veure que el propietari del directori és `sambauser`

Amb aquesta el propietari podrà escriure. Si després l'accés Samba es fa com aquest usuari, la carpeta permetrà operacions de w. Si no  hauràs d'ajustar permisos més endavant

## 6 - Afegir l'usuari a Samba

L'usuari existeix a Linux i però ha d'estar habilitat per entrar via Samba

Per crear la contrasenya Samba de l'usuari:

```bash
sudo smbpasswd -a sambauser
```

Et demanarà dues vegades la contrasenya

Assegura't que l'usuari queda activat:

```bash
sudo smbpasswd -e sambauser
```

Ara l'usuari ja existeix al sistema Linux, existeix a Samba i pot ser utilitzat per autenticar l'accés a la carpeta compartida

## 7 - Fer una còpia de seguretat del fitxer de configuració

Abans de tocar configuració de Samba fem una còpia del fitxer principal:

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

Això permetrà recuperar el fitxer original si t'equivoques

## 8 - Configurar la compartició a Samba

Editar el fitxer principal de configuració.

Obre'l:

```bash
sudo nano /etc/samba/smb.conf
```

Baixa fins al final del fitxer i afegeix aquest bloc:

```ini
[compartit]
   path = /srv/compartit
   browseable = yes
   read only = no
   valid users = sambauser
```

Això vol dir el següent:

- `compartit` serà el nom visible del recurs a la xarxa
- la ruta real serà `/srv/compartit`
- la carpeta serà visible
- la compartició no serà només de lectura
- només hi podrà accedir l'usuari `sambauser`

Guarda els canvis i surt de l'editor

## 9 - Comprovar la configuració abans de reiniciar

Abans de reiniciar Samba, comprova que no hi ha errors de sintaxi:

```bash
testparm
```

Aquesta ordre revisa el fitxer de configuració

Si tot està bé veuràs un missatge conforme la configuració és correcta

Si `testparm` mostra errors, torna a obrir `smb.conf`, revisa la sintaxi i repeteix la comprovació. No reiniciïs Samba fins que `testparm` no sigui correcte

## 10 - Reiniciar Samba

Quan la configuració sigui correcta, reinicia el servei:

```bash
sudo systemctl restart smbd
```

Després comprova de nou l'estat:

```bash
systemctl status smbd
```

Has de tornar a veure el servei com a `active (running)`

## 11 - Comprovar el tallafoc

Si tens el tallafoc actiu, cal permetre Samba

Comprova l'estat amb:

```bash
sudo ufw status
```

Si surt `inactive`, pots continuar. El tallafoc no està bloquejant res en aquest moment

Si surt `active`, permet Samba amb:

```bash
sudo ufw allow samba
```

Després torna a comprovar l'estat i verifica que la regla s'ha aplicat correctament

## 12 - Crear un fitxer de prova al servidor

Abans d'entrar des del client, crea un fitxer de prova dins de la carpeta compartida:

```bash
echo "Fitxer de prova Samba" | sudo tee /srv/compartit/prova.txt
```

Després comprova que existeix:

```bash
ls -l /srv/compartit
```

Has de veure el fitxer `prova.txt` dins del directori compartit

## 13 - Accés des del client Windows

Ara passa al client Windows.

Obre l'Explorador de fitxers, fes clic a la barra d'adreces de dalt i escriu exactament això:

```text
\\LA_IP_DEL_SERVIDOR\compartit
```

Windows intentarà obrir el recurs de xarxa. Pot passar dues coses:

- entra directament
- et demana credencials

Si et demana credencials:

- usuari: `sambauser`
- contrasenya: la contrasenya has definit

Si Windows recorda credencials antigues i continua fallant, tanca l'Explorador, torna a provar i assegura't que estàs entrant amb l'usuari correcte

Si tot va bé, hauries de veure el contingut de la carpeta, i el fitxer `prova.txt`

## 14 - Prova de lectura

Des del client, obre `prova.txt`

Si s'obre correctament, vol dir que el client té accés de lectura al recurs

## 15 - Prova d'escriptura

Ara intenta crear un fitxer nou des del client:

```text
prova_client.txt
```

Si el fitxer es desa correctament, la compartició permet escriptura

Per confirmar-ho, torna al servidor i comprova:

```bash
ls -l /srv/compartit
```

Hauries de veure també el fitxer `prova_client.txt`

Això demostra que el client no només veu la carpeta, sinó que hi pot treballar

## 16 - Prova opcional de només lectura

Aquesta prova serveix per entendre la diferència entre poder entrar a una compartició i poder-ne modificar el contingut.

Torna a obrir el fitxer de configuració:

```bash
sudo nano /etc/samba/smb.conf
```

Canvia aquesta línia:

```ini
read only = no
```

per aquesta:

```ini
read only = yes
```

Torna a comprovar la configuració amb:

```bash
testparm
```

Si no hi ha errors, reinicia Samba:

```bash
sudo systemctl restart smbd
```

Ara torna a provar des del client:

- entrar a la carpeta
- obrir un fitxer
- crear un fitxer nou
- modificar un fitxer existent

El comportament correcte ara hauria de ser aquest:

- el client pot entrar a la carpeta
- el client pot llegir fitxers
- el client no pot crear ni modificar contingut

## 17 - Guia de resolució de problemes

Si alguna cosa falla, segueix aquesta guia

Si el client no arriba al servidor, comprova de nou la xarxa amb `ping`. Si no hi ha resposta, revisa IPs, estat de les màquines, connexió de xarxa i configuració de la interfície

Si el servei Samba no està actiu, comprova-ho amb:

```bash
systemctl status smbd
```

Si no està en execució, arrenca'l:

```bash
sudo systemctl start smbd
```

Si hi ha un error de configuració a Samba, la millor comprovació és:

```bash
testparm
```

Si `testparm` falla, has de corregir el fitxer `smb.conf` abans de reiniciar el servei

Si sospites que el tallafoc bloqueja Samba:

```bash
sudo ufw status
```

Si cal:

```bash
sudo ufw allow samba
```

Si Windows demana credencials però les rebutja, refés la contrasenya Samba de l'usuari:

```bash
sudo smbpasswd -a sambauser
sudo smbpasswd -e sambauser
```

Si el recurs es veu però no deixa escriure, revisa dues coses:

- que a Samba tinguis `read only = no`
- que els permisos Linux del directori permetin escriptura a l'usuari que realment hi accedeix

Si el recurs no apareix a Windows mira:

- que la IP sigui correcta
- que `smbd` estigui actiu
- que `testparm` no doni errors
- que el tallafoc no bloquegi Samba

I torna a provar manualment amb aquesta ruta:

```text
\\LA_IP_DEL_SERVIDOR\compartit
```

## Checklist final d'autorevisió

Abans de donar la pràctica per acabada, comprova això:

- [ ] conec la IP del servidor
- [ ] el `ping` entre client i servidor funciona
- [ ] `smbd` està actiu
- [ ] la carpeta `/srv/compartit` existeix
- [ ] l'usuari `sambauser` existeix a Linux
- [ ] l'usuari `sambauser` està donat d'alta a Samba
- [ ] `testparm` no mostra errors
- [ ] el client pot entrar a `\\IP_DEL_SERVIDOR\compartit`
- [ ] puc llegir `prova.txt`
- [ ] puc escriure o puc demostrar que està en només lectura
