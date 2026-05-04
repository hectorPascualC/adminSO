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

# Definició

Un llenguatge de guions és un llenguatge pensat per escriure ordres en forma de fitxer perquè el sistema les executi una darrere l’altra

---

# Context dels llenguatges de guions

La idea central és convertir tasques habituals en:

- seqüències repetibles
- ordres controlables
- execucions automàtiques
- administració més eficient

---

# Dos entorns generals

- **GNU/Linux** amb **Bash** o scripts de **shell**
- **Windows** amb **PowerShell**

En aquesta RA, el desenvolupament pràctic es pot centrar sobretot en **GNU/Linux**

---

# Què aporten els guions a l'administració

Permeten:

- gestionar **usuaris**
- controlar **processos**
- administrar **serveis**
- reutilitzar seqüències d’ordres
- reduir tasques manuals repetitives

---

# 7.1 Context dels llenguatges de guions

Els guions queden lligats a aquestes idees bàsiques:

- administració des de línia d’ordres
- combinació de comandes per a tasques del sistema
- reutilització d’ordres dins d’un mateix script
- execució controlada o programada quan convingui

---

# 7.2 Bash com a entorn de guions

En GNU/Linux, el llenguatge de guions més habitual és **Bash**

La primera línia habitual d’un script és:

```bash
#!/bin/bash
```

Això indica quin intèrpret ha d’executar el fitxer

---

# Exemple mínim de script Bash

```bash
#!/bin/bash
echo "Iniciant comprovació del sistema"
whoami
date
```

Aquí es veu que diverses ordres es poden reunir dins d’un sol fitxer executable

---

# 7.3 Estructures bàsiques del llenguatge

- ordres amb **nom + paràmetres**
```bash
ls -l /home
# ls és la comanda
# -l és un paràmetre
# /home és la ruta sobre la qual actua
```

- comandes combinades
```bash
ps aux | grep ssh
# ps aux mostra processos
# grep ssh filtra els que contenen ssh
```

---

# 7.3 Estructures bàsiques del llenguatge

- filtres i selecció de resultats
```bash
cat /etc/passwd | grep hector
# cat /etc/passwd mostra el contingut del fitxer
# grep hector selecciona només les línies on surt hector
```

- variables
```bash
USUARI="hector"
echo $USUARI
# USUARI guarda un valor
# echo $USUARI mostra el contingut de la variable
```

---

# 7.3 Estructures bàsiques del llenguatge

- condicionals `if`
```bash
if id "$USUARI" >/dev/null 2>&1; then
    echo "L'usuari existeix"
else
    echo "L'usuari no existeix"
fi
```

- bucles `for` o `while`
```bash
COMPTADOR=1
while [ $COMPTADOR -le 3 ]; do
    echo $COMPTADOR
    COMPTADOR=$((COMPTADOR+1))
done
```

---

# 7.4 Administració de comptes d'usuari amb Bash

Ordres útils:

```bash
id alumnera07
getent passwd alumnera07
cat /etc/passwd
```

Aquestes ordres permeten comprovar i consultar comptes del sistema

---

# Exemple de comprovació d'usuari

```bash
USUARI="alumnera07"

if id "$USUARI" >/dev/null 2>&1; then
    echo "L'usuari $USUARI existeix"
    getent passwd "$USUARI"
else
    echo "L'usuari $USUARI no existeix"
fi
```

---

# 7.5 Administració de processos amb Bash

Ordres habituals:

```bash
ps aux
pgrep bash
ps -ef | grep ssh
```

Això permet consultar, filtrar i comprovar processos des del terminal

---

# Exemple de comprovació de procés

```bash
PROCES="bash"

if pgrep "$PROCES" >/dev/null 2>&1; then
    echo "El procés $PROCES està en execució"
else
    echo "El procés $PROCES no està en execució"
fi
```

---

# 7.6 Administració de serveis amb Bash

Ordres habituals:

```bash
systemctl status ssh
systemctl is-active ssh
systemctl start ssh
systemctl stop ssh
systemctl restart ssh
```

---

# Exemple de comprovació de servei

```bash
SERVEI="ssh"

if systemctl is-active "$SERVEI" >/dev/null 2>&1; then
    echo "El servei $SERVEI està actiu"
else
    echo "El servei $SERVEI no està actiu"
fi
```

---

# 7.7 Consulta d'ordres i ajuda a GNU/Linux

Eines útils abans d’escriure un guió:

```bash
man ps
man systemctl
help
which bash
type echo
```

Això permet consultar què fa cada ordre i reutilitzar-la dins del script

---

# 7.8 Documentació dels guions

Exemple d’inici documentat:

```bash
#!/bin/bash
# Pràctica RA07
# Autor: Nom Cognoms
# Objectiu: Comprovar usuaris, processos i serveis
```

Els comentaris amb `#` ajuden a entendre el guió després

---

# Comprovació i depuració bàsica

Accions bàsiques:

- revisar sintaxi i ordres
- comprovar valors de variables
- verificar condicions `if`
- detectar errors de permisos
- provar l’execució real del fitxer

---

# Donar permisos i executar

```bash
chmod +x ra07_admin_linux.sh
./ra07_admin_linux.sh
```

Aquest és el pas necessari perquè el guió es pugui executar com a fitxer

---

# 7.9 Execució programada de guions

Com a aplicació complementària, un guió també es pot deixar preparat per executar-se més endavant

Eines habituals:

- **cron**
- **crontab**
- **anacron**
- **at**

---

# Cron i crontab

```bash
service crond status
apt-get install cron
crontab -l
crontab -e
```

Exemple de programació:

```bash
30 0 * * * root find /tmp -type f -empty -delete
```

---

# Anacron i at

```bash
apt-get install anacron
apt-get install at
at 12am tomorrow < script.sh
```

Aquestes eines permeten llançar scripts en moments concrets

---

# 7.11 Orientació pràctica de la RA07

Bloc principal de la unitat:

- **GNU/Linux** com a entorn de pràctica
- **Bash** com a llenguatge de guions principal
- scripts sobre **usuaris**, **processos** i **serveis**
- PowerShell com a context complementari

---

# Idea final

Si les pràctiques i l’examen s’han de centrar en Linux, la RA07 es pot desplegar de manera molt coherent al voltant de:

- **Bash**
- ordres del sistema
- scripts d’administració
- comprovació i documentació del guió
