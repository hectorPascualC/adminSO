# RA07 - Llenguatges de guions en sistemes operatius

## Nota prèvia
Aquest document s’ha redactat seguint la mateixa línia de contingut de la RA06 i s’ha construït **només a partir del llibre aportat**. Per això, el desenvolupament se centra en els blocs del llibre que tracten directament:

- **PowerShell** en entorns Windows
- **Gestió de processos** i **automatització de tasques** en GNU/Linux
- eines de planificació com **cron**, **crontab**, **anacron** i **at**

No s’hi afegeix teoria externa. Quan algun punt curricular no apareix desenvolupat com a apartat propi al llibre, s’explica únicament fins on el llibre el mostra de manera explícita.

## Índex
- [7.1 Context dels llenguatges de guions en l’administració de sistemes](#71-context-dels-llenguatges-de-guions-en-ladministració-de-sistemes)
- [7.2 Estructures bàsiques del llenguatge segons el llibre](#72-estructures-bàsiques-del-llenguatge-segons-el-llibre)
- [7.3 PowerShell com a entorn de guions en Windows](#73-powershell-com-a-entorn-de-guions-en-windows)
- [7.4 Administració de comptes d’usuari amb PowerShell](#74-administració-de-comptes-dusuari-amb-powershell)
- [7.5 Administració de processos amb PowerShell](#75-administració-de-processos-amb-powershell)
- [7.6 Administració de serveis amb PowerShell](#76-administració-de-serveis-amb-powershell)
- [7.7 Execució de guions i automatització en GNU/Linux](#77-execució-de-guions-i-automatització-en-gnulinux)
- [7.8 Cron i el format d’execució de comandes i scripts](#78-cron-i-el-format-dexecució-de-comandes-i-scripts)
- [7.9 Crontab, anacron i at](#79-crontab-anacron-i-at)
- [7.10 Consulta de cmdlets i llibreries de funcions segons el llibre](#710-consulta-de-cmdlets-i-llibreries-de-funcions-segons-el-llibre)
- [7.11 Documentació dels guions creats](#711-documentació-dels-guions-creats)
- [7.12 Eines gràfiques, comprovació i implantació en entorns lliures i propietaris](#712-eines-gràfiques-comprovació-i-implantació-en-entorns-lliures-i-propietaris)

## 7.1 Context dels llenguatges de guions en l’administració de sistemes

En l’administració de sistemes, un llenguatge de guions no s’ha d’entendre només com una manera d’escriure ordres a mà, sinó com una forma de convertir tasques habituals de gestió en seqüències repetibles, controlables i més eficients. El llibre ho mostra des de dos entorns diferents: d’una banda, **Windows**, on apareix **PowerShell** com a eina de línia d’ordres i també com a entorn basat en scripts; i, de l’altra, **GNU/Linux**, on la idea d’automatització es concreta sobretot en la planificació de tasques amb eines com **cron**, **crontab**, **anacron** i **at**.

A Windows, el llibre explica que Microsoft ha anat incorporant una línia de comandes que ha evolucionat fins a arribar a PowerShell. Aquesta eina no es limita a executar ordres simples, sinó que permet administrar característiques del sistema operatiu i actuar també com a entorn basat en scripts. Això vol dir que el mateix administrador pot combinar comandes i construir seqüències per facilitar la gestió del sistema.

A GNU/Linux, el llibre planteja el mateix problema des d’un altre angle. Hi ha moltes tasques que no interessa executar manualment cada vegada: còpies de seguretat, neteges periòdiques, revisions del sistema o execució programada de processos. Per això apareixen eines específiques de planificació que permeten definir **quan** s’ha d’executar una ordre i **quin programa o script** s’ha de llançar.

Per tant, dins de la RA07, el paper dels llenguatges de guions queda lligat a tres idees bàsiques que sí apareixen clarament al llibre:

- administració del sistema des de línia de comandes
- combinació d’ordres per gestionar usuaris, processos i serveis
- automatització d’execucions en moments concrets o de forma periòdica

Això connecta directament amb el sentit pràctic d’aquesta RA: no es tracta només de conèixer comandes soltes, sinó d’entendre com es poden fer servir com a base per construir tasques d’administració més estables i repetibles.

## 7.2 Estructures bàsiques del llenguatge segons el llibre

El llibre no presenta una teoria abstracta llarga sobre sintaxi de llenguatges de scripting, però sí que mostra diverses **estructures bàsiques de treball** que permeten entendre com s’organitzen els guions en els dos entorns que tracta.

En **PowerShell**, la unitat bàsica de treball és el **cmdlet**. El llibre explica que totes les ordres reben aquest nom i que, per consultar els cmdlets disponibles, es pot executar una ordre com aquesta:

```powershell
Get-Command -CommandType cmdlet | Measure-Object
```

Això ja ens mostra diversos trets importants:

- l’ordre té un nom principal
- es poden afegir **paràmetres**
- es poden combinar diverses parts en una mateixa línia
- es poden filtrar o processar resultats

A més, el llibre insisteix en una idea clau: **PowerShell està basat en objectes**. Això el diferencia d’altres línies de comandes més tradicionals, perquè la informació que es rep no és només text, sinó objectes sobre els quals després es poden aplicar més ordres.

En **GNU/Linux**, l’estructura bàsica que el llibre desenvolupa amb més detall és la de **cron**. Aquí el llenguatge no es presenta com una sintaxi de programació general, sinó com una expressió de planificació temporal. El llibre indica que les línies de programació es divideixen en dues parts:

- la **programació**
- el **comando de execución**

La forma general que mostra és aquesta:

```text
minute hour day_of_month month day_of_week command_to_be_executed
```

Cada camp té un rang concret:

- `minute` → 0-59
- `hour` → 0-23
- `day of month` → 1-31
- `month` → 1-12 o JAN-DEC
- `day of week` → 0-6 o SUN-SAT
- `command to be execute` → ordre que s’ha d’executar

El llibre també incorpora símbols especials que formen part d’aquesta estructura:

- `*` indica qualsevol valor
- `,` separa llistes de valors
- `#` s’utilitza com a comentari
- `-` indica un rang de valors
- `/` indica un pas de valor

Per tant, quan en aquesta RA es parla d’**estructures del llenguatge**, el que sí es pot afirmar a partir del llibre és que aquestes estructures es concreten, sobretot, en:

- ordres amb **nom + paràmetres** a PowerShell
- filtres, seleccions i comandes combinades
- expressions temporals formades per camps a cron
- referència a un programa o script que serà executat

## 7.3 PowerShell com a entorn de guions en Windows

El llibre presenta **PowerShell** com una peça central dins de l’administració de Windows. Explica que Microsoft ha inclòs una línia de comandes que ha evolucionat al llarg dels seus sistemes operatius i que actualment està present en tots els sistemes Microsoft, sempre que hi hagi disponible el framework **.NET** en què es basa.

Però el punt més important per a aquesta RA és un altre: PowerShell **no es defineix només com una línia de comandes**, sinó també com un **entorn basat en scripts**, on es poden desenvolupar comandes pròpies o combinar-ne diverses per facilitar la gestió del sistema.

Aquesta idea és essencial, perquè converteix PowerShell en molt més que una consola per escriure ordres aïllades. En termes pràctics, l’administrador pot fer servir PowerShell per:

- consultar informació del sistema
- gestionar usuaris
- administrar processos
- gestionar serveis
- encadenar ordres per resoldre tasques d’administració

El llibre indica dues maneres d’executar PowerShell:

- amb un entorn gràfic anomenat **PowerShell ISE**
- amb una **consola de text** semblant a la línia de comandes tradicional

Això és important perquè mostra dues formes diferents de treballar:

- una més orientada a l’escriptura i organització de comandes dins d’un entorn visual
- una altra més directa, ràpida i centrada en la consola

També explica que PowerShell recull totes les característiques de `cmd.exe`, però en va més enllà. Segons el llibre, també permet accedir a elements interns més profunds de Windows i gestionar-los des d’una lògica més potent.

Dins del context de la RA07, això significa que PowerShell és el bloc del llibre que més clarament respon a aquests punts:

- creació de guions a partir de comandes combinades
- interpretació d’ordres del sistema
- adaptació d’ordres per a tasques d’administració
- implantació d’aquest tipus de treball dins de sistemes propietaris

## 7.4 Administració de comptes d’usuari amb PowerShell

Un dels primers exemples clars que ofereix el llibre és l’ús de PowerShell per a l’**administració de comptes d’usuari**. Aquest és un dels nuclis pràctics més directament alineats amb la RA07, perquè mostra com un conjunt d’ordres pot servir per resoldre una tasca administrativa concreta.

El llibre parteix d’una idea bàsica: una compte d’usuari està formada per un nom i una contrasenya, i aquests elements formen el conjunt de credencials que serveixen per identificar una persona dins del sistema.

A partir d’aquí, mostra diverses ordres relacionades amb la gestió d’usuaris:

### Veure comptes d’usuari

```powershell
Get-LocalUser
```

Aquesta ordre permet consultar els usuaris locals existents al sistema.

### Crear un usuari

El llibre explica que aquesta acció s’ha de fer com a administrador i mostra una seqüència com aquesta:

```powershell
#Ejecutar PowerShell como administrador
#Crear la contraseña con SecureString
$pass=ConvertTo-SecureString "1234Asdf_" -asplaintext -force
#Crear usuario con contraseña
New-LocalUser usuario -Password $pass
```

Aquí es veu amb claredat que l’administració d’usuaris no es resol amb una única ordre aïllada, sinó amb una petita seqüència d’accions:

- preparar la contrasenya
- convertir-la al format requerit
- crear el compte amb la contrasenya assignada

### Canviar la contrasenya d’un usuari

```powershell
#El usuario tiene que existir
$pass2=ConvertTo-SecureString "11234Aaaa_" -asplaintext -force
Set-LocalUser -Name usuario -Password $pass2
```

### Eliminar un usuari

```powershell
#Es necesario abrir PowerShell como administrador
Remove-LocalUser usuario
```

Aquest bloc mostra molt bé una de les aplicacions reals del scripting dins del sistema operatiu: transformar la gestió manual d’usuaris en ordres repetibles i fàcils de tornar a executar. Encara que el llibre ho presenti en exemples simples, la idea de fons és clarament la mateixa que demana la RA07.

## 7.5 Administració de processos amb PowerShell

El segon gran bloc que presenta el llibre dins de PowerShell és la **gestió de processos**. Igual que passava amb els usuaris, aquí també es veu com la línia de comandes es pot utilitzar per fer tasques que, d’una altra manera, sovint es farien des d’una interfície gràfica.

El llibre diu explícitament que, igual que amb l’administrador de tasques de Windows, amb PowerShell es poden gestionar processos prescindint de la interfície gràfica del sistema.

### Consultar processos

Per obtenir la llista de processos que s’estan executant:

```powershell
Get-Process | more
```

El llibre també mostra que es pot filtrar per nom:

```powershell
Get-Process -Name win*
```

I que es pot obtenir informació més detallada d’un procés concret:

```powershell
Get-Process -Name explorer | Format-List *
```

Això fa visible una altra idea molt pròpia d’aquesta RA: la línia de comandes no només serveix per llançar ordres, sinó també per **consultar**, **filtrar** i **adaptar** la informació obtinguda.

### Detenir un procés

El llibre mostra dues possibilitats. Una és detenir processos pel seu nom. L’altra és fer-ho pel seu identificador.

Exemple amb identificador i confirmació:

```powershell
Stop-Process -id 2456 -confirm
```

El mateix llibre explica que l’argument `-confirm` força que el sistema demani confirmació abans de detenir el procés. Això encaixa amb la idea de control i comprovació abans d’executar accions sensibles.

### Iniciar un nou procés

Per iniciar un procés nou, el llibre mostra l’ús de `Start-Process` i posa com a exemple el bloc de notes:

```powershell
Start-Process notepad.exe
```

En aquest cas es veu clarament com PowerShell pot actuar com a entorn de llançament i control de processos del sistema.

## 7.6 Administració de serveis amb PowerShell

El tercer bloc de PowerShell que desenvolupa el llibre és la **gestió de serveis**. Aquest apartat és especialment important, perquè connecta de manera molt directa amb el punt curricular que parla de guions per a l’administració de serveis del sistema operatiu.

El llibre explica que obtenir els serveis d’un equip local o remot és senzill si s’empra el cmdlet **Get-Service**.

### Consultar serveis

Sense paràmetres, s’obtenen tots els serveis:

```powershell
Get-Service
```

També es poden filtrar per nom:

```powershell
Get-Service -Name se*
```

I el llibre remarca que el paràmetre `ComputerName` permet consultar serveis en equips remots. Per exemple:

```powershell
Get-Service -ComputerName ServerPpal
```

### Aturar, iniciar, suspendre i reiniciar serveis

El llibre dona una sèrie d’ordres molt clares per a un cas concret, el servei de la cua d’impressió:

```powershell
Stop-Service -Name spooler
Start-Service -Name spooler
Suspend-Service -Name spooler
Restart-Service -Name spooler
```

Amb això es veu una altra característica important dels guions d’administració: treballar sobre un mateix objecte o servei repetint un patró d’accions molt clar.

En termes de RA07, aquest bloc del llibre permet desenvolupar de manera directa els continguts relacionats amb:

- administració de serveis del sistema
- adaptació d’ordres a necessitats concretes
- execució controlada d’accions administratives
- ús de comandes en sistemes propietaris

## 7.7 Execució de guions i automatització en GNU/Linux

A la part de GNU/Linux, el llibre aborda el problema dels guions des d’una perspectiva orientada a la **execució automàtica d’ordres i scripts**. Aquí no es posa tant l’accent en una consola orientada a objectes com en PowerShell, sinó en la necessitat d’indicar al sistema **què** s’ha d’executar, **quan** s’ha d’executar i quin **programa o script** serà el que realment es llançarà.

El llibre diu que programar una tasca és una de les accions més comunes i freqüents quan s’administra un sistema operatiu. També remarca que, a Windows, això es pot fer amb **Tareas Programadas**, mentre que a GNU/Linux aquest procediment es fa habitualment des de terminal.

L’objectiu d’aquestes eines és permetre execucions automàtiques en:

- un període de temps concret
- dates determinades
- tasques periòdiques
- processos relacionats amb el control del sistema
- execució d’ordres o scripts personalitzats

Aquest últim punt és especialment important, perquè el mateix llibre ja fa servir l’expressió **scripts personalizados**, cosa que encaixa de manera directa amb el nucli de la RA07.

En aquest bloc, les eines principals que apareixen són:

- **cron**
- **crontab**
- **anacron**
- **at**
- utilitats gràfiques com **Webmin** i **GNOME schedule**

Per tant, dins de GNU/Linux, el llibre vincula l’ús de guions sobretot amb la possibilitat de deixar preparades execucions automàtiques d’ordres i scripts del sistema. Això és important perquè aquí la planificació no apareix com un tema separat d’organització, sinó com el mecanisme que permet posar en funcionament un guió administratiu en el moment previst.

## 7.8 Cron i el format d’execució de comandes i scripts

El primer gran element que desenvolupa el llibre és **cron**. El defineix com un dimoni que s’executa des del moment en què arrenca el sistema i que comprova si hi ha alguna ordre, programa o script que hagi de ser executat d’acord amb l’hora configurada.

El llibre insisteix en una advertència important: la **zona horària** ha d’estar ben configurada, perquè altrament les execucions poden no coincidir amb allò que s’havia previst.

També identifica el servei amb el nom **crond** i mostra diferents maneres de comprovar-ne l’estat:

```bash
/etc/rc.d/init.d/crond status
```

```bash
/etc/init.d/crond status
```

```bash
service crond status
```

I, si cal instal·lar-lo:

```bash
apt-get install cron
```

### Fitxers i elements implicats

El llibre enumera diversos elements importants relacionats amb cron:

- el dimoni `crond`
- el fitxer de configuració per al root: `/etc/crontab`
- el fitxer d’inici i parada del dimoni: `/etc/init.d/cron`
- l’ordre de programació de tasques: `crontab`
- el sistema d’informes o logs: `/var/log/cron`

### Sintaxi de la programació

Per programar una tasca, el llibre diu que s’ha d’afegir una línia amb una expressió de cron. Aquesta línia queda estructurada així:

```text
minute hour day_of_month month day_of_week command_to_be_executed
```

Els valors possibles i els símbols especials ja s’han vist a l’apartat 7.2, però aquí prenen sentit complet, perquè el llibre els presenta com la base real de la planificació.

Això converteix cron en un exemple molt clar de llenguatge de guió orientat a l’administració: no és un llenguatge generalista, però sí una sintaxi definida que permet interpretar una expressió temporal i automatitzar l’execució d’ordres i scripts del sistema.

## 7.9 Crontab, anacron i at

### Crontab

Un cop definida la programació, el llibre explica que s’ha de col·locar en un lloc on el dimoni la pugui llegir. Aquí entra en joc **crontab**, que es descriu com un arxiu especial que conté la programació de tasques que executarà cron.

El llibre també explica que l’ordre `crontab` és la responsable de gestionar els fitxers de planificació assignats a cada usuari. La sintaxi general que dona és aquesta:

```bash
crontab [-l e r u] fichero
```

I en detalla aquests paràmetres:

- `-l` mostra el fitxer de configuració de l’usuari
- `-e` edita el fitxer de configuració de l’usuari
- `-r` esborra el fitxer de configuració de l’usuari
- `-u usuario` especifica quin és l’usuari propietari de la tasca

El llibre també mostra un exemple clar d’edició i una programació concreta:

```bash
crontab -e
30 0 * * * root find /tmp -type f -empty -delete
```

En aquest cas, la planificació indica una execució diària a les 00:30 per eliminar fitxers buits del directori `/tmp`.

### Anacron

El llibre explica una limitació de cron: només funciona si el sistema està encès en el moment previst. Si una tasca s’havia de fer mentre el sistema estava apagat, no s’executarà.

Per resoldre aquest problema apareix **anacron**, que es defineix com un planificador que no requereix que el sistema estigui funcionant contínuament. Quan el sistema arrenca, revisa quines tasques no s’han executat i les realitza.

Segons el llibre, les tasques d’anacron solen residir als directoris del tipus:

```text
/etc/cron.*
```

I la instal·lació del servei es fa amb:

```bash
apt-get install anacron
```

### At

Finalment, el llibre diferencia **at** de cron. Mentre que cron està pensat per a execucions periòdiques, **at** serveix per programar una tasca **una sola vegada**, en un moment determinat.

La instal·lació es fa amb:

```bash
apt-get install at
```

El llibre també indica dos fitxers de control d’accés molt importants:

- `/etc/at.allow` → si existeix, només els usuaris que hi apareixen poden executar `at`
- `/etc/at.deny` → si existeix aquest fitxer i no l’anterior, els usuaris que hi apareixen no poden executar `at`

La sintaxi general que mostra és:

```text
HH[:]MM[am|pm] [Mes día] programa_script
```

I dona aquest exemple:

```bash
at 12am tomorrow < script.sh
```

Aquest és, probablement, un dels exemples més directes del llibre en què apareix de manera explícita la idea d’executar un **script** preparat per l’usuari.

## 7.10 Consulta de cmdlets i llibreries de funcions segons el llibre

Un dels punts que la curricular inclou de manera explícita és la **consulta i ús de llibreries de funcions**. El llibre no desenvolupa aquest tema com un capítol independent llarg, però sí aporta una base clara des de **PowerShell**: el treball amb **cmdlets** i la seva consulta.

A l’inici del bloc dedicat a PowerShell, el llibre explica que totes les ordres reben el nom de **cmdlets**. També mostra una manera directa de consultar-los:

```powershell
Get-Command -CommandType cmdlet | Measure-Object
```

Aquest exemple és important perquè permet entendre dues idees molt relacionades amb el criteri curricular:

- abans d’escriure o adaptar un guió, cal saber **quines ordres o funcions** ofereix l’entorn
- el treball amb scripts no parteix sempre de zero, sinó que sovint consisteix a **consultar**, **seleccionar** i **combinar** funcions ja disponibles

Per tant, encara que el llibre no utilitzi una teoria extensa sobre biblioteques o mòduls com a apartat separat, sí que mostra una lògica clara de consulta del conjunt de funcions disponibles dins de l’entorn de scripting. En el cas de Windows, aquesta idea queda representada sobretot per la consulta de cmdlets de PowerShell.

Això permet relacionar el contingut del llibre amb una idea central de la RA07: un guió d’administració no es construeix només escrivint instruccions noves, sinó també reutilitzant les ordres i funcions que el sistema ja posa a disposició de l’administrador.

## 7.11 Documentació dels guions creats

La curricular de la RA07 també demana **documentar els guions creats**. El llibre no dedica un apartat exclusiu i llarg a la documentació formal dels scripts, però sí dona elements suficients per entendre quina informació convé deixar reflectida quan es treballa amb ordres programades o amb seqüències administratives.

En el cas de **cron**, per exemple, el mateix format obliga a identificar dades essencials de la tasca:

- quan s’executa
- quina ordre, programa o script es llança
- si la programació és periòdica o puntual
- quin usuari la té associada, en el cas que pertoqui

El llibre també indica que el símbol `#` s’utilitza com a **comentari** dins de les expressions de cron. Aquest detall és petit, però és molt rellevant des del punt de vista documental, perquè permet afegir aclariments i fer més entenedor el contingut d’un fitxer de configuració o d’un conjunt de programacions.

A més, quan el llibre mostra ordres com aquestes:

```bash
crontab -l
crontab -e
```

o la revisió de fitxers com:

```text
/var/log/cron
```

està deixant clar que treballar amb scripts i programacions no consisteix només a crear-los, sinó també a **revisar-los**, **mantenir-los** i poder entendre posteriorment què fan i quan s’executen.

En el cas de **PowerShell**, la documentació també es pot entendre a partir de la manera com el llibre presenta les seqüències d’exemple: ordres separades, objectiu concret de cada pas i relació clara entre acció i resultat. Això no és una metodologia externa afegida, sinó una conseqüència directa de com el mateix llibre mostra la creació d’ordres administratives sobre usuaris, processos i serveis.

Per tant, dins d’aquesta RA07, documentar els guions creats significa sobretot deixar identificats aquests elements mínims:

- finalitat del guió o de la tasca programada
- ordres o cmdlets que utilitza
- usuari, servei o procés sobre el qual actua
- moment d’execució, si es tracta d’una tasca programada
- comprovacions fetes després de l’execució

Aquesta lectura es manté fidel al llibre, perquè surt directament dels exemples de cron, crontab, logs i seqüències de PowerShell que s’hi mostren.

## 7.12 Eines gràfiques, comprovació i implantació en entorns lliures i propietaris

El llibre no limita el treball amb guions a la consola pura. També presenta eines gràfiques que poden servir de suport.

### Entorn propietari: Windows

A Windows, el llibre distingeix dues formes d’execució de PowerShell:

- **PowerShell ISE**, com a entorn gràfic
- la **consola de text**, com a entorn més directe

Això mostra que els guions es poden treballar tant en format més visual com des de línia d’ordres pura.

### Entorn lliure: GNU/Linux

A GNU/Linux, el llibre menciona eines gràfiques com:

- **Webmin**
- **GNOME schedule**

Segons el llibre, Webmin és una interfície web per a l’administració del sistema en entorns Unix. Entre altres funcions, permet gestionar tasques programades o planificades sense necessitat de recórrer directament a fitxers de text o a comandes.

GNOME schedule apareix com una utilitat gràfica de planificació des de l’entorn GNOME i la seva instal·lació es fa amb:

```bash
apt-get install gnome-schedule
```

### Comprovació, depuració bàsica i adaptació segons el que mostra el llibre

El llibre no desenvolupa un bloc teòric independent sobre “depuració de scripts” com si fos un tema separat, però sí mostra diverses formes de **comprovació** i **control** que formen part del treball real amb guions i ordres d’administració.

En concret, al llibre apareixen aquestes idees:

- comprovar l’estat d’un servei abans o després d’automatitzar-lo, com en el cas de `crond`
- revisar la sintaxi dels camps de cron abans de donar per bona una planificació
- editar i consultar la configuració amb `crontab -e` i `crontab -l`
- fer servir paràmetres de confirmació en accions delicades, com `Stop-Process -id 2456 -confirm`
- filtrar resultats i mostrar només allò que interessa per entendre millor què està passant
- revisar fitxers de registre com `/var/log/cron`

Dit d’una altra manera, el llibre sí permet treballar la idea de **prova**, **comprovació**, **adaptació** i **correcció bàsica**, però sense convertir-ho en una teoria externa separada del que mostren els exemples reals del sistema.

### Implantació en sistemes lliures i propietaris

Un dels aspectes més útils del llibre per a la RA07 és que mostra clarament la implantació dels guions en dos entorns diferents:

- **sistemes propietaris**, mitjançant PowerShell en Windows
- **sistemes lliures**, mitjançant cron, crontab, anacron, at i eines gràfiques en GNU/Linux

Això permet tancar la RA07 amb una idea important: els llenguatges de guions no s’utilitzen igual a tots els sistemes, però en tots dos casos serveixen per una mateixa finalitat general:

- administrar el sistema
- reduir tasques manuals repetitives
- controlar usuaris, processos i serveis
- programar execucions automàtiques
- fer la gestió del sistema més previsible i més repetible

### Tancament del bloc

Segons el llibre, el treball amb llenguatges de guions en sistemes operatius es concreta sobretot en aquests punts:

- ús de **PowerShell** per administrar usuaris, processos i serveis a Windows
- ús de **cmdlets** i consulta d’ordres disponibles com a base per construir guions
- ús de **cron**, **crontab**, **anacron** i **at** per automatitzar l’execució d’ordres i scripts a GNU/Linux
- suport d’eines gràfiques com **PowerShell ISE**, **Webmin** i **GNOME schedule**
- necessitat de revisar sintaxi, estats, registres i resultats abans de donar per bona una execució
- conveniència de deixar identificada la finalitat i configuració dels guions i de les tasques programades

Aquest és el marc que el llibre aporta per desenvolupar la RA07 sense afegir-hi contingut extern.
