---

marp: true
paginate: true
theme: default

header: "RA03 - Automatització de tasques del sistema"
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

# RA03  
## Planificació de tasques amb Linux

Administració de Sistemes Operatius  
ASIX02

---

# 3.1 - Planificació de tasques en Linux

En sistemes Linux la planificació de tasques es realitza principalment amb:

- cron → tasques repetitives
- at   → tasques puntuals

Totes dues funcionen com a serveis del sistema

---

# 3.2 - El servei cron

- Servei en segon pla
- S’executa automàticament en iniciar el sistema
- Llegeix configuracions programades
- Executa comandes en els moments definits

Comprovació del servei:

```bash
systemctl status cron
```

---

# 3.3 - Estructura d’una línia de crontab

```
* * * * * comanda
│ │ │ │ │
│ │ │ │ └── dia setmana (0-7)
│ │ │ └──── mes (1-12)
│ │ └────── dia mes (1-31)
│ └──────── hora (0-23)
└────────── minut (0-59)
```

Cada camp defineix quan s’executa la tasca

---

# 3.3 - Estructura d'una línia de crontab

| Valor | Dia       |
| ----- | --------- |
| 0     | Diumenge  |
| 1     | Dilluns   |
| 2     | Dimarts   |
| 3     | Dimecres  |
| 4     | Dijous    |
| 5     | Divendres |
| 6     | Dissabte  |
| 7     | Diumenge  |

* Antic sistema UNIX
* 0 o 7 → diumenge
* 1 → dilluns
* 6 → dissabte

---

# 3.4 - Gestió de tasques amb crontab

Editar tasques de l’usuari:

```bash
crontab -e
```

Llistar tasques:

```bash
crontab -l
```

Eliminar tasques:

```bash
crontab -r
```

---

# 3.5 - Exemple pràctic amb cron

Exemple: executar una comanda cada 5 minuts

```bash
*/5 * * * * date >> /home/usuari/cron_logs/cron_5min.log
```

Aquesta tasca:

* `*` tots els valors possibles de minuts (0 - 59 minuts)
* `/` interval o pas (step)
* Executa `date`
* Escriu el resultat en un fitxer
* Es repeteix automàticament
* `>>`  afegeix al final del fitxer

---

# 3.5 - Exemple pràctic amb cron

Altres exemples: 
* 1-30/2 -> de l’1 al 30, cada 2  

Errors possibles:
* 0 25-60/3 * * *
* bad hour
* errors in crontab file, can't install.
* si es desa en un fitxer del sistema, per exemple `/etc/crontab`
    * ignorarà la línia
    * la considerarà invàlida
    * pot deixar rastre al log → `journalctl -u cron`
        * journalctl → eina per consultar el journal de systemd
        * -u → unit (servei concret)
        * cron → nom del servei
        * `journalctl -u cron --since "10 minutes ago"` → si vols veure les últimes execucions recents dels últims 10 minuts

---

# 3.6 - Directoris de cron del sistema

El sistema també executa scripts ubicats a:

* /etc/cron.hourly
* /etc/cron.daily
* /etc/cron.weekly
* /etc/cron.monthly

Aquests directoris permeten automatització global

---

# 3.7 - Verificació de tasques cron

Consultar logs del servei:

```bash
journalctl -u cron
```

Comprovar fitxer generat:

```bash
cat /ruta/del/fitxer.log
```

La verificació és essencial en qualsevol automatització

---

# 3.8 - El servei at

* Servei per executar tasques puntuals
* Execució única
* No és repetitiu

Comprovació del servei:

```bash
systemctl status atd
```
* at → comanda per programar una tasca puntual
* d  → daemon (Un procés que s’executa en segon pla i proporciona un servei al sistema)

---

# 3.9 - Crear una tasca puntual amb at

Exemple:

```bash
echo "date >> /home/usuari/tasca_at.log" | at now + 1 minute
```

* programa una execució en un moment concret
* echo és una comanda de Bash (i altres shells) que serveix per:
    * mostrar text per pantalla
    * enviar text cap a un fitxer o cap a una altra comanda

---

# 3.10 - Verificació amb at

Tasques pendents (at queue):

```bash
atq
```

Logs del servei:

```bash
journalctl -u atd
```
* -u: filtrar els logs per una unitat concreta de systemd
    * un servei cron
    * un servei atd
    * un temporitzador
    * un dispositiu 

Comprovar resultat:

```bash
cat /home/usuari/tasca_at.log
```

---

# 3.11 - Diferència entre cron i at

cron:

* Tasques repetitives
* Execució periòdica
* [exemples cron](https://learning.lpi.org/en/learning-materials/102-500/107/107.2/107.2_01/)

at:

* Tasques puntuals
* Execució única
* [exemples at](https://learning.lpi.org/en/learning-materials/102-500/107/107.2/107.2_02/)

```

