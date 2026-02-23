# RA03 - Pràctica 02 Manual - Automatització amb `cron` i `at` (Ubuntu)

# 🔹 PART 1 - Tasca repetitiva amb cron

## **Exercici 1 - Crear una tasca que s’executi cada 5 minuts**

### 🔹 Enunciat

Crea una tasca amb `cron` que:

* S’executi cada 5 minuts

* Escrigui el text:

  ```
  TASCA CRON 5 MIN
  ```

* Afegeixi la data actual

* Desa el resultat a:

```
/home/vboxuser/cron_logs/cron_5min.log
```

## 🔹 Resolució

### 1. Crear la carpeta

```bash
mkdir -p /home/vboxuser/cron_logs
```

### Per què ho fem?

Cron no crea carpetes automàticament.

Si la carpeta no existeix, la tasca fallarà perquè el fitxer no es pot crear.

A més:

> En entorns automàtics sempre hem de garantir que la ruta existeix.

---

### 2. Editar el crontab

```bash
crontab -e
```

Afegir:

```bash
*/5 * * * * echo "TASCA CRON 5 MIN $(date)" >> /home/vboxuser/cron_logs/cron_5min.log
```

## Explicació de la línia

### `*/5 * * * *`

Cron té 5 camps:

```
minut hora dia mes dia_setmana
```

`*/5` significa:

> Divideix el rang complet de minuts (0–59) en intervals de 5

Per això s’executa a: 0, 5, 10, 15, 20...

No és “cada 5 minuts des que l’has creat”, sinó:

> Cada minut múltiple de 5

---

### `$(date)`

Això és **substitució de comanda**.

Significa:

1. Executa `date`
2. Substitueix el resultat dins la línia

Per això cada línia del log tindrà una data diferent.

---

### `>>`

Redirecció en mode append.

Significa:

> Afegeix al final del fitxer.

Si utilitzéssim `>`:

* Es sobreescriuria el fitxer cada vegada.

---

### Ruta absoluta

```
/home/vboxuser/cron_logs/cron_5min.log
```

Per què no posem:

```
cron_logs/cron_5min.log
```

Perquè cron:

* No té directori actual
* No és interactiu
* No sap on estàs

Per això:

> En tasques automàtiques SEMPRE ruta absoluta


## 🔹 Comprovació

```bash
crontab -l
```

Mostra el contingut del crontab.

Després de 5 minuts:

```bash
cat /home/vboxuser/cron_logs/cron_5min.log
```

---

# 🔹 PART 2 - Monitorització d’espai de disc


## **Exercici 2 - Registrar l’espai de disc cada 3 minuts**

### 🔹 Enunciat

Crea una tasca amb cron que:

* S’executi cada 3 minuts
* Executi la comanda:

```bash
/bin/df -h
```

* Desa el resultat a:

```
/home/vboxuser/cron_logs/espai_disc.log
```

---

## 🔹 Resolució

Editar crontab:

```bash
crontab -e
```

Afegir:

```bash
*/3 * * * * /bin/df -h >> /home/vboxuser/cron_logs/espai_disc.log 2>&1
```


## Explicació

### `/bin/df -h`

Per què posem `/bin/df` i no `df`?

Quan treballes al terminal:

* El sistema busca el programa dins la variable PATH

Cron no sempre carrega el mateix PATH

Per tant:

> És bona pràctica indicar el binari executable complet.

---

### Què fa `df -h`?

`df` = disk free

Mostra:

* Espai total
* Espai utilitzat
* Espai lliure
* Percentatge d’ús
* Punt de muntatge

Opció `-h`:

> Human readable (MB, GB...)

Automatitzar això serveix per:

* Detectar discos plens
* Analitzar creixement de dades
* Prevenir caigudes del sistema

---

### `2>&1`

Quan executem una comanda, Linux crea un procés.

Cada procés té canals:

| Número | Nom    | Funció           |
| ------ | ------ | ---------------- |
| 1      | stdout | Sortida normal   |
| 2      | stderr | Sortida d’errors |

`2>&1` significa:

> Envia els errors (2) al mateix lloc on va la sortida normal (1)

Per què és important?

Si `df` genera un error i no el redirigeixes:

* Potser no el veuràs
* El log quedarà incomplet


# PART 3 - Tasca puntual amb at

## **Exercici 3 - Crear una tasca puntual**

### 🔹 Enunciat

Crea una tasca amb `at` que:

* S’executi 1 minut després de crear-la
* Escrigui:

```
TASCA AT EXECUTADA
```

* Desa el resultat a:

```
/home/vboxuser/at_logs/tasca_at.log
```

---

## 🔹 Resolució

### 1. Crear carpeta

```bash
mkdir -p /home/vboxuser/at_logs
```

---

### 2. Crear tasca

```bash
at now + 1 minute
```

Escriure:

```bash
echo "TASCA AT EXECUTADA" >> /home/vboxuser/at_logs/tasca_at.log
```

Finalitzar amb:

```
CTRL + D
```


## Explicació 

### Per què CTRL+D?

`at` està llegint des de stdin.

Quan premem CTRL+D:

> Indiquem final d’entrada

Sense això, la tasca no es guarda

---

### Diferència amb cron

| cron                | at                   |
| ------------------- | -------------------- |
| Tasques repetitives | Execució única       |
| Servei continu      | S’executa una vegada |

---

### Com comprovar que està programada

```bash
atq
```

Mostra la cua de tasques pendents

---

### Com comprovar execució

```bash
cat /home/vboxuser/at_logs/tasca_at.log
```

# Errors habituals explicats

* No posar `/` inicial → ruta relativa
* No crear carpeta
* No indicar binari complet
* Oblidar `2>&1`
* Oblidar CTRL+D


