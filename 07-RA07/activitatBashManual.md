# Pràctica 1 - Automatització de tasques amb un script Bash en Linux

## Manual detallat

## 1 - Preparar l'entorn de treball

Crea una carpeta general per a la pràctica dins del teu directori personal:

```bash
mkdir -p ~/ra07_script/{dades,logs,backups}
```

### Quines carpetes tindràs

- `dades` → carpeta d'origen per a la còpia
- `logs` → carpeta on desarem el registre
- `backups` → carpeta on desarem la còpia comprimida

### Comprovar que existeixen

```bash
ls -R ~/ra07_script
```

### Què hauries de veure

Una estructura semblant a aquesta:

```text
/home/usuari/ra07_script:
backups  dades  logs
```

---

## 2 - Comprovar que Bash està disponible

Comprova que Bash existeix i està disponible al sistema:

```bash
bash --version
```

---

## 3 - Preparar dades de prova

Ara crearem dos fitxers de text dins de la carpeta `dades`. Aquests fitxers serviran perquè després el guió pugui crear una còpia comprimida real.

Escriu:

```bash
echo "Fitxer 1 de prova" > ~/ra07_script/dades/prova1.txt
echo "Fitxer 2 de prova" > ~/ra07_script/dades/prova2.txt
```

### Què fa cadascuna d'aquestes ordres

- `echo` escriu un text
- `>` redirigeix aquest text a un fitxer
- si el fitxer no existeix, el crea

### Comprovar que els fitxers existeixen

```bash
ls -l ~/ra07_script/dades
```

---

## 4 - Crear el fitxer del guió

Ara crearem el fitxer principal de la pràctica.

```bash
touch ~/ra07_script/activitat_ra07.sh
```

### Comprovar que el fitxer existeix

```bash
ls -l ~/ra07_script/activitat_ra07.sh
```

Has de veure el fitxer dins de la carpeta `ra07_script`.

---

## 5 - Obrir el fitxer i escriure l'estructura inicial

```bash
nano ~/ra07_script/activitat_ra07.sh
```

Escriu aquest bloc inicial exactament així:

```bash
#!/bin/bash

# Pràctica RA07
# Autor: Nom Cognoms
# Data: dd/mm/aaaa
# Objectiu: Automatitzar comprovacions i una còpia de seguretat bàsica a Linux

echo "Iniciant activitat RA07..."
```

### Explicació 
- `#!/bin/bash` indica quin intèrpret ha d'executar el guió
- `echo` mostra un missatge per pantalla

### Per què és important la primera línia

La línia:

```bash
#!/bin/bash
```

És molt important. Si falta o està mal escrita, el sistema pot no interpretar correctament el fitxer com a script de Bash.

Guarda i surt de nano

---

## 6 - Afegir variables de treball

Torna a obrir el fitxer:

```bash
nano ~/ra07_script/activitat_ra07.sh
```

Just sota dels comentaris inicials, afegeix variables:

```bash
USUARI="root"
PROCES="bash"
SERVEI="cron"
ORIGEN="$HOME/ra07_script/dades"
DESTI="$HOME/ra07_script/backups"
DIR_LOGS="$HOME/ra07_script/logs"
DATA=$(date +%F_%H-%M-%S)
FITXER_LOG="$DIR_LOGS/informe_$DATA.log"
FITXER_BACKUP="$DESTI/copia_$DATA.tar.gz"
```

### Significat de cada variable

- `USUARI` → usuari que volem comprovar
- `PROCES` → procés que volem buscar
- `SERVEI` → servei que volem revisar
- `ORIGEN` → carpeta que volem comprimir
- `DESTI` → carpeta on es desarà la còpia
- `DIR_LOGS` → carpeta on es desarà el registre
- `DATA` → data i hora de l'execució
- `FITXER_LOG` → nom complet del log
- `FITXER_BACKUP` → nom complet de la còpia comprimida

### Per què ho fem amb variables

Perquè així el guió és més fàcil d'entendre i d'adaptar.

Si més endavant vols comprovar un altre usuari, un altre procés o un altre servei, només cal canviar el valor de la variable i no tot el guió.

---

## 7 - Afegir el bloc de comprovació de l'usuari

A sota del missatge inicial, afegeix aquest bloc:

```bash
echo "----- COMPROVACIÓ D'USUARI -----" | tee -a "$FITXER_LOG"
if id "$USUARI" >/dev/null 2>&1; then
    echo "L'usuari $USUARI existeix." | tee -a "$FITXER_LOG"
    getent passwd "$USUARI" | tee -a "$FITXER_LOG"
else
    echo "L'usuari $USUARI no existeix." | tee -a "$FITXER_LOG"
fi
```

### Què fa aquest bloc

1. mostra un títol per pantalla i al log
2. comprova si l'usuari existeix
3. si existeix, mostra la seva informació bàsica
4. si no existeix, mostra un missatge d'error controlat

### Explicació detallada

#### Línia del títol

```bash
echo "----- COMPROVACIÓ D'USUARI -----" | tee -a "$FITXER_LOG"
```

- `echo` escriu el text
- `tee -a` fa dues coses alhora:
  - mostra el text per pantalla
  - l'afegeix al fitxer de log
- `-a` vol dir que afegeix contingut al final, sense esborrar el que ja hi havia

#### Condicional `if`

```bash
if id "$USUARI" >/dev/null 2>&1; then
```

- `id "$USUARI"` intenta consultar aquest usuari
- `>/dev/null 2>&1` amaga la sortida normal i els errors
- `if ... then` comprova si l'ordre s'ha executat bé

#### Compleix condició

```bash
echo "L'usuari $USUARI existeix." | tee -a "$FITXER_LOG"
getent passwd "$USUARI" | tee -a "$FITXER_LOG"
```

- informa que l'usuari existeix
- `getent passwd` mostra la línia d'aquest usuari a la base de dades del sistema

#### No compleix condició

```bash
echo "L'usuari $USUARI no existeix." | tee -a "$FITXER_LOG"
```

Mostra un missatge si l'usuari no existeix.

#### Tancament

```bash
fi
```

Tanca el `if`.

---

## 8 - Afegir el bloc de comprovació del procés

Ara afegeix aquest segon bloc sota del bloc anterior:

```bash
echo "----- COMPROVACIÓ DE PROCÉS -----" | tee -a "$FITXER_LOG"
if pgrep "$PROCES" >/dev/null 2>&1; then
    echo "El procés $PROCES està en execució." | tee -a "$FITXER_LOG"
    pgrep "$PROCES" | tee -a "$FITXER_LOG"
else
    echo "El procés $PROCES no està en execució." | tee -a "$FITXER_LOG"
fi
```

### Què fa aquest bloc

1. mostra el títol del bloc
2. comprova si hi ha algun procés amb aquest nom
3. si n'hi ha, informa i mostra els PID trobats
4. si no n'hi ha, informa que no està en execució

### Explicació de l'ordre principal

```bash
pgrep "$PROCES"
```

- `pgrep` busca processos pel nom
- si en troba, retorna un o més PID
- si no en troba, retorna error i el `if` entra a l'`else`

### Què és un PID

És el número identificador d'un procés  

---

## 9 - Afegir el bloc de comprovació del servei

Ara afegeix aquest tercer bloc:

```bash
echo "----- COMPROVACIÓ DE SERVEI -----" | tee -a "$FITXER_LOG"
if systemctl list-units --type=service --all | grep -q "$SERVEI"; then
    echo "El servei $SERVEI existeix al sistema." | tee -a "$FITXER_LOG"
    systemctl is-active "$SERVEI" 2>/dev/null | tee -a "$FITXER_LOG"
else
    echo "El servei $SERVEI no existeix o no està disponible." | tee -a "$FITXER_LOG"
fi
```

### Què fa aquest bloc

1. mostra el títol del bloc
2. comprova si el servei apareix al sistema
3. si existeix, intenta mostrar si està actiu o no
4. si no existeix, ho informa

### Explicació de les ordres

#### Línia de comprovació

```bash
systemctl list-units --type=service --all | grep -q "$SERVEI"
```

- `systemctl list-units --type=service --all` mostra serveis
- `grep -q` busca el nom sense mostrar la sortida, només dient si s'ha trobat o no

#### Línia de l'estat

```bash
systemctl is-active "$SERVEI" 2>/dev/null | tee -a "$FITXER_LOG"
```

- `systemctl is-active` intenta mostrar l'estat del servei
- sovint retornarà coses com `active`, `inactive` o `failed`

---

## 10 - Afegir el bloc de còpia de seguretat

Ara afegeix aquest bloc:

```bash
echo "----- CÒPIA DE SEGURETAT -----" | tee -a "$FITXER_LOG"
if [ -d "$ORIGEN" ]; then
    tar -czf "$FITXER_BACKUP" -C "$ORIGEN" .
    if [ $? -eq 0 ]; then
        echo "Còpia creada correctament a: $FITXER_BACKUP" | tee -a "$FITXER_LOG"
    else
        echo "Error en crear la còpia comprimida." | tee -a "$FITXER_LOG"
    fi
else
    echo "La carpeta d'origen no existeix." | tee -a "$FITXER_LOG"
fi
```

### Què fa aquest bloc

1. comprova que la carpeta d'origen existeix
2. crea una còpia comprimida `.tar.gz`
3. comprova si la còpia s'ha creat correctament
4. informa del resultat i el desa al log

### Explicació de la condició inicial

```bash
if [ -d "$ORIGEN" ]; then
```

- `[ -d ... ]` comprova si la ruta existeix i és un directori

### Explicació de la còpia comprimida

```bash
tar -czf "$FITXER_BACKUP" -C "$ORIGEN" .
```

- `tar` crea arxius agrupats
- `-c` crea una còpia nova
- `-z` la comprimeix amb gzip
- `-f` indica el nom del fitxer de sortida
- `-C "$ORIGEN"` fa que treballi des d'aquella carpeta
- `.` vol dir que es comprimirà el contingut d'aquell directori

### Explicació del segon `if`

```bash
if [ $? -eq 0 ]; then
```

- `$?` guarda el codi de sortida de l'última ordre executada
- si val `0`, l'ordre ha anat bé
- si no val `0`, hi ha hagut algun error

---

## 11 - Afegir el bloc final del guió

Per tancar el script, afegeix això al final:

```bash
echo "----- FINAL DE L'EXECUCIÓ -----" | tee -a "$FITXER_LOG"
echo "Log desat a: $FITXER_LOG" | tee -a "$FITXER_LOG"
echo "Activitat RA07 finalitzada."
```

### Què fa aquest bloc

- tanca el guió amb un títol final
- indica on s'ha desat el log
- mostra un missatge final per pantalla

---

## 12 - Contingut complet del guió

Quan hagis acabat tots els blocs, el contingut complet del fitxer hauria de quedar semblant a això:

```bash
#!/bin/bash

# Pràctica RA07
# Autor: Nom Cognoms
# Data: dd/mm/aaaa
# Objectiu: Automatitzar comprovacions i una còpia de seguretat bàsica a Linux

USUARI="root"
PROCES="bash"
SERVEI="cron"
ORIGEN="$HOME/ra07_script/dades"
DESTI="$HOME/ra07_script/backups"
DIR_LOGS="$HOME/ra07_script/logs"
DATA=$(date +%F_%H-%M-%S)
FITXER_LOG="$DIR_LOGS/informe_$DATA.log"
FITXER_BACKUP="$DESTI/copia_$DATA.tar.gz"

echo "Iniciant activitat RA07..."

echo "----- COMPROVACIÓ D'USUARI -----" | tee -a "$FITXER_LOG"
if id "$USUARI" >/dev/null 2>&1; then
    echo "L'usuari $USUARI existeix." | tee -a "$FITXER_LOG"
    getent passwd "$USUARI" | tee -a "$FITXER_LOG"
else
    echo "L'usuari $USUARI no existeix." | tee -a "$FITXER_LOG"
fi

echo "----- COMPROVACIÓ DE PROCÉS -----" | tee -a "$FITXER_LOG"
if pgrep "$PROCES" >/dev/null 2>&1; then
    echo "El procés $PROCES està en execució." | tee -a "$FITXER_LOG"
    pgrep "$PROCES" | tee -a "$FITXER_LOG"
else
    echo "El procés $PROCES no està en execució." | tee -a "$FITXER_LOG"
fi

echo "----- COMPROVACIÓ DE SERVEI -----" | tee -a "$FITXER_LOG"
if systemctl list-units --type=service --all | grep -q "$SERVEI"; then
    echo "El servei $SERVEI existeix al sistema." | tee -a "$FITXER_LOG"
    systemctl is-active "$SERVEI" 2>/dev/null | tee -a "$FITXER_LOG"
else
    echo "El servei $SERVEI no existeix o no està disponible." | tee -a "$FITXER_LOG"
fi

echo "----- CÒPIA DE SEGURETAT -----" | tee -a "$FITXER_LOG"
if [ -d "$ORIGEN" ]; then
    tar -czf "$FITXER_BACKUP" -C "$ORIGEN" .
    if [ $? -eq 0 ]; then
        echo "Còpia creada correctament a: $FITXER_BACKUP" | tee -a "$FITXER_LOG"
    else
        echo "Error en crear la còpia comprimida." | tee -a "$FITXER_LOG"
    fi
else
    echo "La carpeta d'origen no existeix." | tee -a "$FITXER_LOG"
fi

echo "----- FINAL DE L'EXECUCIÓ -----" | tee -a "$FITXER_LOG"
echo "Log desat a: $FITXER_LOG" | tee -a "$FITXER_LOG"
echo "Activitat RA07 finalitzada."
```

---

## 13 - Revisar el contingut complet abans d'executar-lo

Mostra el contingut del fitxer:

```bash
cat ~/ra07_script/activitat_ra07.sh
```

Revisa amb calma aquests punts:

- la primera línia és `#!/bin/bash`
- les variables estan ben escrites
- totes les cometes estan ben obertes i tancades
- cada `if` té el seu `fi`
- no hi ha cap línia tallada o mig esborrada

---

## 14 - Comprovar sintaxi abans d'executar

Abans d'executar un script és molt bona pràctica revisar-ne la sintaxi 

Fes-ho així:

```bash
bash -n ~/ra07_script/activitat_ra07.sh
```

### Què fa aquesta ordre

- `bash -n` revisa la sintaxi del fitxer
- no l'executa
- si no mostra res, normalment vol dir que la sintaxi és correcta

### Si hi ha error

Et dirà en quina línia hi ha el problema. En aquest cas, torna a obrir el fitxer i corregeix-lo 

---

## 15 - Donar permisos d'execució

Ara fes executable el guió:

```bash
chmod +x ~/ra07_script/activitat_ra07.sh
```

### Comprovar el resultat

```bash
ls -l ~/ra07_script/activitat_ra07.sh
```

Has de veure una `x` als permisos del fitxer

Exemple:

```text
-rwxr-xr-x ... activitat_ra07.sh
```

---

## 16 - Executar el guió

Executa'l així:

```bash
~/ra07_script/activitat_ra07.sh
```

### Què hauries de veure

Diversos missatges per pantalla, un per cada bloc del guió.

Per exemple, podries veure alguna cosa semblant a això:

```text
Iniciant activitat RA07...
----- COMPROVACIÓ D'USUARI -----
L'usuari root existeix.
...
----- COMPROVACIÓ DE PROCÉS -----
El procés bash està en execució.
...
----- COMPROVACIÓ DE SERVEI -----
active
----- CÒPIA DE SEGURETAT -----
Còpia creada correctament a: ...
----- FINAL DE L'EXECUCIÓ -----
Log desat a: ...
Activitat RA07 finalitzada.
```

No cal que el resultat sigui exactament igual. El que importa és que l'script executi tots els blocs sense errors greus

---

## 17 - Comprovar que el log s'ha creat

Ara revisa la carpeta de logs:

```bash
ls -l ~/ra07_script/logs
```

Hauria d'aparèixer un fitxer semblant a aquest:

```text
informe_2026-05-04_19-30-00.log
```

### Obrir el log

```bash
cat ~/ra07_script/logs/informe_*.log
```

### Què ha de contenir

Com a mínim, informació sobre:

- l'usuari comprovat
- el procés comprovat
- el servei comprovat
- la còpia de seguretat creada

---

## 18 - Comprovar que la còpia comprimida s'ha creat

Ara revisa la carpeta `backups`:

```bash
ls -l ~/ra07_script/backups
```

Hauria d'aparèixer un fitxer `.tar.gz`.

### Comprovar el contingut de la còpia sense descomprimir-la

```bash
tar -tzf ~/ra07_script/backups/copia_*.tar.gz
```

### Què hauries de veure

Els fitxers de prova, per exemple:

```text
./prova1.txt
./prova2.txt
```

---

## 19 - Fer una prova d'adaptació controlada

Ara comprovarem que el guió és adaptable

Torna a obrir el fitxer:

```bash
nano ~/ra07_script/activitat_ra07.sh
```

Canvia, per exemple, aquesta línia:

```bash
PROCES="bash"
```

per aquesta altra:

```bash
PROCES="sshd"
```

Guarda el fitxer i torna a executar-lo.

### Què estàs comprovant amb això

Que el mateix script es pot reutilitzar per a un altre procés canviant només una variable  

També pots provar de canviar:

- `USUARI`
- `SERVEI`

per fer comprovacions diferents 

---

## 20 - Depuració bàsica del guió

Si vols veure què va executant el script línia a línia, pots fer servir aquest mode de depuració:

```bash
bash -x ~/ra07_script/activitat_ra07.sh
```

### Què fa

- `bash -x` mostra cada ordre a mesura que s'executa
- és molt útil per detectar en quin punt falla el guió

Això és especialment útil si:

- una variable no té el valor esperat
- una condició `if` no entra on tu pensaves
- una ruta no és correcta

---

## 21 - Guia de resolució de problemes

Si alguna cosa falla, segueix aquesta guia.

### Cas 1 - El fitxer no existeix

Comprova-ho amb:

```bash
ls -l ~/ra07_script/activitat_ra07.sh
```

Si no existeix, torna'l a crear amb `touch`.

### Cas 2 - El guió no té permís d'execució

Comprova-ho amb:

```bash
ls -l ~/ra07_script/activitat_ra07.sh
```

Si no té `x`, executa:

```bash
chmod +x ~/ra07_script/activitat_ra07.sh
```

### Cas 3 - Error `bad interpreter`

Revisa la primera línia del fitxer:

```bash
#!/bin/bash
```

Ha d'estar sola i ben escrita.

### Cas 4 - Error de sintaxi

Comprova la sintaxi amb:

```bash
bash -n ~/ra07_script/activitat_ra07.sh
```

I si cal, revisa el fitxer amb:

```bash
nano ~/ra07_script/activitat_ra07.sh
```

### Cas 5 - No es crea el log

Comprova que existeix la carpeta de logs:

```bash
ls -ld ~/ra07_script/logs
```

### Cas 6 - No es crea la còpia comprimida

Comprova que existeix la carpeta d'origen:

```bash
ls -ld ~/ra07_script/dades
```

I comprova que hi ha fitxers dins:

```bash
ls -l ~/ra07_script/dades
```

### Cas 7 - El servei no dona resultat

Pots provar amb un altre servei real del sistema. Per veure'n alguns:

```bash
systemctl list-units --type=service --all | head
```

### Cas 8 - El procés no apareix

Pots mirar quins processos hi ha amb:

```bash
ps aux | head
```

I després canviar la variable `PROCES`.

### Cas 9 - Vols revisar el guió complet

```bash
cat ~/ra07_script/activitat_ra07.sh
```

---

## 23 - Checklist final d'autorevisió

Abans de donar la pràctica per acabada, comprova això:

- [ ] he creat la carpeta `~/ra07_script`
- [ ] dins hi ha `dades`, `logs` i `backups`
- [ ] he creat dades de prova dins de `dades`
- [ ] he creat el fitxer `activitat_ra07.sh`
- [ ] el fitxer comença amb `#!/bin/bash`
- [ ] he afegit comentaris inicials al guió
- [ ] he definit variables dins del fitxer
- [ ] he utilitzat almenys un `if`
- [ ] el guió comprova un usuari
- [ ] el guió comprova un procés
- [ ] el guió comprova un servei
- [ ] el guió crea una còpia comprimida
- [ ] el guió desa un log
- [ ] he comprovat la sintaxi amb `bash -n`
- [ ] el fitxer té permís d'execució
- [ ] he executat el guió correctament
- [ ] puc demostrar que s'ha creat el log
- [ ] puc demostrar que s'ha creat la còpia de seguretat
- [ ] he fet almenys una prova canviant una variable

