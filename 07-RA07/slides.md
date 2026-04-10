---
marp: true
paginate: true
theme: default

header: "RA07 - Llenguatges de guions en sistemes operatius"
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

# RA07  
## Llenguatges de guions en sistemes operatius

Administració de Sistemes Operatius  
ASIX02

---

# Context dels llenguatges de guions

Segons el llibre, la idea central és convertir tasques habituals en:

- seqüències repetibles
- ordres controlables
- execucions automàtiques
- administració més eficient

Això apareix sobretot en **PowerShell** i en eines de **GNU/Linux**.

---

# Dos entorns principals

El llibre treballa la RA07 des de dos blocs:

- **Windows** amb **PowerShell**
- **GNU/Linux** amb **cron**, **crontab**, **anacron** i **at**

En tots dos casos, el propòsit és administrar el sistema a partir de comandes i scripts.

---

# Què aporten els guions a l'administració

Permeten:

- gestionar **usuaris**
- controlar **processos**
- administrar **serveis**
- programar execucions automàtiques
- reduir tasques manuals repetitives

---

# 7.1 Context dels llenguatges de guions

Dins d'aquesta RA, els guions queden lligats a tres idees bàsiques:

- administració des de línia d'ordres
- combinació de comandes per a tasques del sistema
- automatització d'ordres i scripts en moments concrets

---

# 7.2 Estructures bàsiques del llenguatge

El llibre no fa una teoria abstracta llarga, però sí mostra estructures bàsiques com:

- ordres amb **nom + paràmetres**
- comandes combinades
- filtres i selecció de resultats
- expressions temporals de **cron**

---

# Estructura bàsica a PowerShell

La unitat bàsica és el **cmdlet**.

Exemple del llibre:

```powershell
Get-Command -CommandType cmdlet | Measure-Object
```

Això mostra que les ordres es poden combinar i processar.

---

# Idea clau de PowerShell

Segons el llibre, **PowerShell està basat en objectes**.

Això implica que:

- no treballa només amb text
- les ordres retornen informació estructurada
- després aquesta informació es pot filtrar o reutilitzar

---

# Estructura bàsica a cron

En GNU/Linux, el llibre mostra aquesta estructura:

```text
minute hour day_of_month month day_of_week command_to_be_executed
```

La línia es divideix en:

- la **programació**
- el **comando de ejecución**

---

# Símbols habituals a cron

El llibre destaca aquests símbols:

- `*` qualsevol valor
- `,` llista de valors
- `-` rang de valors
- `/` pas de valor
- `#` comentari

---

# 7.3 PowerShell com a entorn de guions

El llibre el presenta com:

- línia de comandes de **Windows**
- entorn basat en **scripts**
- eina per combinar ordres pròpies
- recurs d'administració del sistema

No és només una consola: també és un entorn per construir seqüències administratives.

---

# Formes d'executar PowerShell

El llibre indica dues maneres principals:

- **PowerShell ISE**
- **consola de text**

Això permet treballar tant en un entorn visual com directament des de terminal.

---

# 7.4 Administració de comptes d'usuari

Un dels exemples més clars del llibre és la gestió d'usuaris amb PowerShell.

Això connecta directament amb la RA07 perquè mostra scripts per a:

- consultar comptes
- crear usuaris
- canviar contrasenyes
- eliminar usuaris

---

# Veure usuaris locals

```powershell
Get-LocalUser
```

Aquesta ordre permet consultar els comptes locals existents al sistema.

---

# Crear un usuari

```powershell
#Ejecutar PowerShell como administrador
#Crear la contraseña con SecureString
$pass=ConvertTo-SecureString "1234Asdf_" -asplaintext -force
#Crear usuario con contraseña
New-LocalUser usuario -Password $pass
```

El procés combina diversos passos dins d'una mateixa seqüència.

---

# Canviar contrasenya i eliminar usuari

```powershell
$pass2=ConvertTo-SecureString "11234Aaaa_" -asplaintext -force
Set-LocalUser -Name usuario -Password $pass2
```

```powershell
Remove-LocalUser usuario
```

Això mostra com l'administració d'usuaris es pot convertir en ordres repetibles.

---

# 7.5 Administració de processos

El llibre mostra que PowerShell també serveix per:

- consultar processos
- filtrar-los per nom
- veure informació detallada
- aturar-los o iniciar-los

És l'equivalent per comandes a moltes accions de l'administrador de tasques.

---

# Consultar processos

```powershell
Get-Process | more
```

```powershell
Get-Process -Name win*
```

```powershell
Get-Process -Name explorer | Format-List *
```

---

# Aturar i iniciar processos

```powershell
Stop-Process -id 2456 -confirm
```

```powershell
Start-Process notepad.exe
```

El paràmetre `-confirm` reforça el control abans d'executar una acció sensible.

---

# 7.6 Administració de serveis

El tercer bloc pràctic de PowerShell és la gestió de serveis.

El llibre mostra ordres per:

- consultar serveis
- filtrar-los
- treballar amb equips remots
- iniciar, aturar o reiniciar serveis

---

# Consultar serveis

```powershell
Get-Service
```

```powershell
Get-Service -Name se*
```

```powershell
Get-Service -ComputerName ServerPpal
```

---

# Controlar un servei

Exemple del llibre amb el servei de la cua d'impressió:

```powershell
Stop-Service -Name spooler
Start-Service -Name spooler
Suspend-Service -Name spooler
Restart-Service -Name spooler
```

---

# 7.7 Execució de guions en GNU/Linux

A GNU/Linux, el llibre orienta la RA07 cap a l'execució automàtica d'ordres i scripts.

L'objectiu és indicar:

- **què** s'executa
- **quan** s'executa
- quin **programa o script** es llança

---

# Eines principals a GNU/Linux

En aquest bloc apareixen sobretot:

- **cron**
- **crontab**
- **anacron**
- **at**
- **Webmin**
- **GNOME schedule**

---

# 7.8 Cron

El llibre defineix **cron** com un dimoni que comprova si hi ha alguna ordre, programa o script que s'hagi d'executar segons l'hora configurada.

També recorda que la **zona horària** ha d'estar ben definida.

---

# Comprovar o instal·lar cron

```bash
/etc/rc.d/init.d/crond status
```

```bash
service crond status
```

```bash
apt-get install cron
```

---

# Elements relacionats amb cron

El llibre enumera aquests elements:

- `crond`
- `/etc/crontab`
- `/etc/init.d/cron`
- `crontab`
- `/var/log/cron`

Aquests fitxers i ordres formen l'entorn bàsic de treball.

---

# 7.9 Crontab

**Crontab** és l'arxiu especial que conté la programació de tasques que executarà cron.

Sintaxi general:

```bash
crontab [-l e r u] fichero
```

---

# Operacions bàsiques amb crontab

El llibre destaca aquests paràmetres:

- `-l` mostra la configuració
- `-e` edita la configuració
- `-r` esborra la configuració
- `-u usuario` indica l'usuari propietari

---

# Exemple de crontab

```bash
crontab -e
30 0 * * * root find /tmp -type f -empty -delete
```

Aquest exemple programa una execució diària a les **00:30**.

---

# Anacron

El llibre explica que **cron** només funciona si el sistema està encès en el moment previst.

**Anacron** resol aquest problema perquè:

- revisa tasques no executades
- les llança quan el sistema arrenca
- no requereix funcionament continu

---

# Elements bàsics d'anacron

Directoris habituals:

```text
/etc/cron.*
```

Instal·lació:

```bash
apt-get install anacron
```

---

# At

El llibre diferencia **at** de cron:

- **cron** → execucions periòdiques
- **at** → execució única en un moment determinat

Això el converteix en una eina útil per llançar un script una sola vegada.

---

# Exemple amb at

Instal·lació:

```bash
apt-get install at
```

Sintaxi mostrada pel llibre:

```text
HH[:]MM[am|pm] [Mes día] programa_script
```

Exemple:

```bash
at 12am tomorrow < script.sh
```

---

# Control d'accés a at

Fitxers indicats pel llibre:

- `/etc/at.allow`
- `/etc/at.deny`

Aquests fitxers controlen quins usuaris poden utilitzar aquesta ordre.

---

# 7.10 Consulta de cmdlets i funcions

La curricular demana consultar i utilitzar llibreries de funcions.

Segons el llibre, aquesta idea es veu sobretot en la consulta de **cmdlets** de PowerShell:

```powershell
Get-Command -CommandType cmdlet | Measure-Object
```

---

# Idea clau sobre les funcions disponibles

Abans de crear o adaptar un guió, cal saber:

- quines ordres ofereix l'entorn
- quines funcions ja existeixen
- quines es poden combinar

Per això un script també es construeix reutilitzant ordres disponibles.

---

# 7.11 Documentació dels guions

La curricular també demana documentar els guions creats.

El llibre permet entendre que cal deixar identificat:

- què fa el guió
- quan s'executa
- quin usuari o servei afecta
- quines comprovacions s'han fet

---

# Elements documentals que mostra el llibre

A cron i crontab apareixen recursos útils per documentar:

- comentaris amb `#`
- consulta amb `crontab -l`
- edició amb `crontab -e`
- revisió de logs a `/var/log/cron`

---

# 7.12 Eines gràfiques i implantació

El llibre no limita el treball amb guions a la consola.

Mostra també:

- **PowerShell ISE** a Windows
- **Webmin** a GNU/Linux
- **GNOME schedule** a GNU/Linux

---

# Comprovació i revisió bàsica

A partir dels exemples del llibre, convé:

- comprovar l'estat dels serveis
- revisar la sintaxi abans d'executar
- consultar configuracions amb `crontab -l`
- revisar registres com `/var/log/cron`
- usar confirmació en accions delicades

---

# Implantació en sistemes lliures i propietaris

El llibre mostra dues implantacions clares:

- **Windows** → PowerShell
- **GNU/Linux** → cron, crontab, anacron i at

Els guions no s'usen igual als dos entorns, però en tots dos serveixen per administrar el sistema.

---

# Idea final

Segons el llibre, la RA07 es concreta sobretot en:

- ús de **PowerShell** per gestionar usuaris, processos i serveis
- ús de **cmdlets** com a base dels guions
- ús de **cron**, **crontab**, **anacron** i **at** per automatitzar ordres i scripts
- revisió de sintaxi, estat i registres abans de donar per bona una execució
- documentació mínima de les tasques creades
