# Pràctica 2 - Creació i comprovació de tasques automàtiques amb `cron` i `at` (Ubuntu Server)

**Mòdul:** Administració de Sistemes Operatius
**Resultat d’aprenentatge:**
RA03 — Gestiona l’automatització de tasques del sistema

**Criteris d’avaluació associats:**

* **3.2** Utilitza comandes del sistema per a la planificació de tasques
* **3.4** Realitza planificacions de tasques repetitives o puntuals relacionades amb l’administració del sistema

## 1. Objectiu de la pràctica

L’objectiu d’aquesta pràctica és aprendre a **crear, executar i verificar tasques automàtiques del sistema** utilitzant eines de línia de comandes en un **entorn servidor Linux**, **Ubuntu Server**.

## 2. Preparació inicial del sistema

Abans de començar, comprova que els serveis necessaris estan disponibles

### 2.1 Comprovació del servei `cron`

```bash
systemctl status cron
```

Si no està actiu:

```bash
sudo systemctl enable --now cron
```

---

### 2.2 Instal·lació i activació del servei `at`

Comprova si `at` està instal·lat:

```bash
which at
```

Si no ho està:

```bash
sudo apt update
sudo apt install at
sudo systemctl enable --now atd
```

---

### 2.3 Creació de directoris de treball

Crea la següent estructura **exactament com s’indica**:

```bash
mkdir -p ~/cron_logs
mkdir -p ~/at_logs
```

Aquests directoris s’utilitzaran a tots els exercicis

## 3. Exercicis a realitzar

### 3.1 - Exercici 1 - Tasca repetitiva amb `cron` (cada 5 minuts)

**Objectiu:** crear una tasca automàtica repetitiva bàsica en un entorn servidor

Has de crear una tasca amb `cron` que compleixi **totes** les condicions següents:

* S’executa **cada 5 minuts**
* Escriu la data i hora actual
* Escriu el text:

  ```
  TASCA CRON 5 MIN
  ```
* Desa la sortida al fitxer:

  ```
  /home/USUARI/cron_logs/cron_5min.log
  ```

*(Substitueix USUARI pel teu nom d’usuari real del sistema)*

Després de crear la tasca:

* Espera com a mínim 5 minuts
* Comprova que el fitxer existeix
* Comprova que el contingut s’actualitza automàticament

---

### 3.2 - Exercici 2 — Tasca repetitiva diària amb `cron`

**Objectiu:** executar una comanda real del sistema de manera automàtica

Has de crear una tasca amb `cron` que:

* S’executi **cada dia a les 03:00**
* Executi la comanda:

  ```bash
  df -h
  ```
* Desa el resultat al fitxer:

  ```
  /home/USUARI/cron_logs/espai_disc.log
  ```

Després de crear la tasca:

* Mostra el contingut del `crontab`
* Explica breument **com comprovaries l’execució** sense esperar fins a les 03:00

---

### 3.3 - Exercici 3 — Tasca puntual amb `at`

**Objectiu:** crear una tasca automàtica que s’executi una sola vegada.

Has de crear una tasca amb `at` que:

* S’executi **1 minut després** de crear-la
* Escrigui el text:

  ```
  TASCA AT EXECUTADA
  ```
* Desa el resultat al fitxer:

  ```
  /home/USUARI/at_logs/tasca_at.log
  ```

Després de crear la tasca:

* Comprova que la tasca està programada
* Espera que s’executi
* Comprova que el fitxer s’ha creat correctament

---

### 3.4 - Exercici 4 — Verificació i comprovació en entorn servidor

**Objectiu:** demostrar que saps verificar tasques automàtiques en un sistema sense GUI.

Has de mostrar:

1. El contingut del `crontab` de l’usuari
2. El contingut dels fitxers:

   * `cron_5min.log`
   * `espai_disc.log`
   * `tasca_at.log`
3. Consultes de logs del sistema relacionades amb:

   * `cron`
   * `at`

No cal interpretar els logs, només demostrar que saps **consultar-los correctament**

## 4. Lliurament

Cal lliurar **un document escrit** (PDF) que inclogui:

* Breu introducció
* Ordres utilitzades
* Sortides de terminal o captures de pantalla
* Resultats obtinguts a cada exercici



