# RA07 - Respostes completes de l'examen

## PART TEÒRICA

1. **a**  
2. **b**  
3. **b**  
4. **c**  
5. **b**  
6. **a**  
7. **c**  
8. **b**  
9. **c**  
10. **b**  

---

## PART PRÀCTICA

## 1. Preparació de l’entorn i execució bàsica del guió

### a) Contingut complet del script

#### Amb explicacions
```bash
#!/bin/bash

USUARI="root"
PROCES="bash"
SERVEI="cron"
ORIGEN="$HOME/ra07_script/dades"
DESTI="$HOME/ra07_script/backups"
DIR_LOGS="$HOME/ra07_script/logs"
DATA=$(date +%F_%H-%M-%S)
FITXER_LOG="$DIR_LOGS/informe_$DATA.log"
FITXER_BACKUP="$DESTI/copia_$DATA.tar.gz"

echo "Iniciant activitat RA07"

echo "----- COMPROVACIÓ D'USUARI -----" | tee -a "$FITXER_LOG"
# mostra el títol del bloc per pantalla
# tee -a també afegeix aquest mateix text al fitxer de log sense esborrar el que ja hi havia

if id "$USUARI" >/dev/null 2>&1; then
# comprova si l'usuari guardat a la variable USUARI existeix al sistema
# id "$USUARI" intenta consultar aquest usuari
# >/dev/null fa que la sortida normal no es mostri
# 2>&1 fa que els errors tampoc es mostrin
# then indica que el bloc següent s'executa si la comanda ha anat bé

    echo "L'usuari $USUARI existeix." | tee -a "$FITXER_LOG"
    # mostra per pantalla i guarda al log que l'usuari existeix

    getent passwd "$USUARI" | tee -a "$FITXER_LOG"
    # mostra la línia de l'usuari a la base de dades d'usuaris del sistema
    # també la desa al log

else
# aquest bloc s'executa si l'usuari no existeix

    echo "L'usuari $USUARI no existeix." | tee -a "$FITXER_LOG"
    # mostra per pantalla i guarda al log que l'usuari no existeix

fi
# tanca l'estructura if


echo "----- COMPROVACIÓ DE PROCÉS -----" | tee -a "$FITXER_LOG"
# mostra el títol del bloc de procés per pantalla i també l'afegeix al log

if pgrep "$PROCES" >/dev/null 2>&1; then
# comprova si hi ha algun procés amb el nom guardat a la variable PROCES
# pgrep busca processos pel nom
# >/dev/null 2>&1 evita mostrar la sortida i els errors
# then indica que el bloc següent s'executa si s'ha trobat el procés

    echo "El procés $PROCES està en execució." | tee -a "$FITXER_LOG"
    # mostra per pantalla i desa al log que el procés està actiu

    pgrep "$PROCES" | tee -a "$FITXER_LOG"
    # mostra els PID dels processos trobats i també els desa al log

else
# aquest bloc s'executa si no s'ha trobat cap procés amb aquest nom

    echo "El procés $PROCES no està en execució." | tee -a "$FITXER_LOG"
    # mostra per pantalla i desa al log que el procés no està actiu

fi
# tanca l'estructura if


echo "----- COMPROVACIÓ DE SERVEI -----" | tee -a "$FITXER_LOG"
# mostra el títol del bloc de servei per pantalla i també l'afegeix al log

if systemctl is-active "$SERVEI" >/dev/null 2>&1; then
# comprova si el servei guardat a la variable SERVEI està actiu
# systemctl is-active retorna èxit si el servei està actiu
# >/dev/null 2>&1 evita mostrar la sortida i els errors
# then indica que el bloc següent s'executa si el servei està actiu

    echo "El servei $SERVEI està actiu." | tee -a "$FITXER_LOG"
    # mostra per pantalla i desa al log que el servei està actiu

else
# aquest bloc s'executa si el servei no està actiu o no existeix

    echo "El servei $SERVEI no està actiu." | tee -a "$FITXER_LOG"
    # mostra per pantalla i desa al log que el servei no està actiu

fi
# tanca l'estructura if


echo "----- CÒPIA DE SEGURETAT -----" | tee -a "$FITXER_LOG"
# mostra el títol del bloc de còpia de seguretat per pantalla i també l'afegeix al log

if [ -d "$ORIGEN" ]; then
# comprova si la ruta guardada a la variable ORIGEN existeix i és un directori
# -d vol dir "és un directori"
# then indica que el bloc següent s'executa si la carpeta existeix

    tar -czf "$FITXER_BACKUP" -C "$ORIGEN" .
    # crea una còpia comprimida en format .tar.gz
    # -c crea un arxiu nou
    # -z aplica compressió gzip
    # -f indica el nom del fitxer de sortida
    # -C "$ORIGEN" fa que tar treballi des de la carpeta ORIGEN
    # . indica que es comprimeix el contingut d'aquesta carpeta

    if [ $? -eq 0 ]; then
    # comprova si l'última comanda executada ha anat bé
    # $? és el codi de retorn de la comanda anterior
    # -eq 0 vol dir "igual a 0", que en Bash indica èxit

        echo "Còpia creada correctament a: $FITXER_BACKUP" | tee -a "$FITXER_LOG"
        # mostra per pantalla i desa al log que la còpia s'ha creat correctament

    else
    # aquest bloc s'executa si la compressió ha fallat

        echo "Error en crear la còpia comprimida." | tee -a "$FITXER_LOG"
        # mostra per pantalla i desa al log que hi ha hagut un error

    fi
    # tanca el segon if, el que comprova si tar ha anat bé

else
# aquest bloc s'executa si la carpeta d'origen no existeix

    echo "La carpeta d'origen no existeix." | tee -a "$FITXER_LOG"
    # mostra per pantalla i desa al log que no s'ha trobat la carpeta a copiar

fi
# tanca l'if principal del bloc de còpia de seguretat

echo "----- FINAL DE L'EXECUCIÓ -----" | tee -a "$FITXER_LOG"
echo "Log desat a: $FITXER_LOG" | tee -a "$FITXER_LOG"
echo "Activitat RA07 finalitzada"
```

#### Script net sense comentaris
```bash
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

if systemctl is-active "$SERVEI" >/dev/null 2>&1; then
    echo "El servei $SERVEI està actiu." | tee -a "$FITXER_LOG"
else
    echo "El servei $SERVEI no està actiu." | tee -a "$FITXER_LOG"
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
```

### b) Comandes necessàries

```bash
bash -n ~/ra07_script/activitat_ra07.sh
chmod +x ~/ra07_script/activitat_ra07.sh
~/ra07_script/activitat_ra07.sh
ls -l ~/ra07_script/logs
ls -l ~/ra07_script/backups
cat ~/ra07_script/logs/*.log
tar -tzf ~/ra07_script/backups/*.tar.gz
```

---

## 2. Anàlisi d’un fragment Bash amb errors

### a) Quatre errors sintàctics o lògics

1. Falta `then` al primer `if`.  
2. La comanda `tar` fa servir `-C "$DESTI"` en lloc de `-C "$ORIGEN"`.  
3. El missatge de l’`else` final és incorrecte: diu que la carpeta d’origen existeix.  
4. Es dona la còpia per bona sense comprovar si `tar` ha funcionat correctament.  

### b) Per què aquests errors farien fallar el guió o donar un resultat incorrecte

1. Sense `then`, el guió té error de sintaxi.  
2. Si `tar` comprimeix des de `"$DESTI"` en lloc de `"$ORIGEN"`, la còpia es faria des del directori equivocat.  
3. El missatge de l’`else` informa just el contrari del que passa realment.  
4. Sense comprovar el codi de retorn de `tar`, el guió pot dir que la còpia s’ha creat encara que hagi fallat.  

### Fragment amb comentaris indicant la línia correcta

```bash
if systemctl is-active "$SERVEI" >/dev/null 2>&1
# correcte: if systemctl is-active "$SERVEI" >/dev/null 2>&1; then

    echo "El servei $SERVEI està actiu"
else
    echo "El servei $SERVEI no està actiu"
fi

if [ -d "$ORIGEN" ]; then
    tar -czf "$FITXER_BACKUP" -C "$DESTI" .
    # correcte: tar -czf "$FITXER_BACKUP" -C "$ORIGEN" .

    echo "Còpia creada correctament a: $FITXER_BACKUP"
    # correcte: aquest missatge hauria d'anar després de comprovar el resultat de tar
    # correcte:
    # if [ $? -eq 0 ]; then
    #     echo "Còpia creada correctament a: $FITXER_BACKUP"
    # else
    #     echo "Error en crear la còpia comprimida."
    # fi

else
    echo "La carpeta d'origen existeix."
    # correcte: echo "La carpeta d'origen no existeix."
fi
```

### Versió corregida del bloc

```bash
if systemctl is-active "$SERVEI" >/dev/null 2>&1; then
    echo "El servei $SERVEI està actiu"
else
    echo "El servei $SERVEI no està actiu"
fi

if [ -d "$ORIGEN" ]; then
    tar -czf "$FITXER_BACKUP" -C "$ORIGEN" .
    if [ $? -eq 0 ]; then
        echo "Còpia creada correctament a: $FITXER_BACKUP"
    else
        echo "Error en crear la còpia comprimida."
    fi
else
    echo "La carpeta d'origen no existeix."
fi
```
