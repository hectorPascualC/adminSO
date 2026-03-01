# RA03 – Pràctica 03 - Manual complet — Automatització de tasques amb Webmin (Ubuntu Server)

Mòdul: Administració de Sistemes Operatius
RA03 — Gestiona l’automatització de tasques del sistema
Criteris: 3.6 + 3.7

---

# PART 0 — Arquitectura de treball

En aquesta pràctica treballarem amb dues màquines:

* **Servidor** → Ubuntu Server (sense entorn gràfic)
* **Client** → Ordinador des del qual accedim per SSH i navegador

Esquema real:

```text
Client
   │
   ├── SSH (port 22)
   │
   └── Navegador HTTPS (port 10000)
            │
            ▼
      Ubuntu Server + Webmin
```

---

# PART 1 — Connexió SSH (pas imprescindible)

Abans d’instal·lar Webmin, cal poder administrar el servidor remotament per poder copiar i pegar les comandes 
d'instal.lació des de client a servidor per SSH

---

## 1. Instal·lar SSH al servidor (si no està instal·lat)

Dins Ubuntu Server:

```bash
sudo apt update
sudo apt install openssh-server
```

---

## 2. Activar el servei SSH

```bash
sudo systemctl enable --now ssh
```

Verificar:

```bash
sudo systemctl status ssh
```

Ha d’aparèixer:

```
Active: active (running)
```

---

## 3. Saber la IP del servidor

```bash
ip a
```

Buscar una línia com:

```
inet 192.168.x.x
```

Aquesta és la IP.

---

## 4. Connectar des del client

### Si el client és Linux / macOS:

```bash
ssh usuari@IP_DEL_SERVIDOR
```

Exemple:

```bash
ssh vboxuser@192.168.1.50
```

### Si el client és Windows:

Des de PowerShell:

```powershell
ssh vboxuser@192.168.1.50
```

---

Si és la primera vegada, acceptar la clau amb `yes`.

Ara ja estàs treballant remotament al servidor.

---

# PART 2 — Instal·lació de Webmin (mètode simplificat)

Instal·larem Webmin des d’un paquet `.deb` per evitar afegir repositoris externs.

**Webmin és una interfície web d’administració de sistemes Linux**

---

## 1. Anar a /tmp

Per què?

Perquè `apt` utilitza l’usuari intern `_apt`, que no pot accedir a `/home`.

```bash
cd /tmp
```

---

## 2. Descarregar Webmin

```bash
wget https://prdownloads.sourceforge.net/webadmin/webmin_2.105_all.deb
```

---

## 3. Instal·lar Webmin

```bash
sudo apt install ./webmin_2.105_all.deb
```

---

## 4. Verificar servei

```bash
sudo systemctl status webmin
```

Ha d’indicar:

```
Active: active (running)
```

---

## 5. Comprovar port 10000

```bash
ss -tuln | grep 10000
```

Ha de mostrar que està escoltant.

---

# PART 3 — Accés via navegador

Des del navegador del client:

```
https://IP_DEL_SERVIDOR:10000
```

Exemple:

```
https://192.168.1.50:10000
```

Acceptar certificat autosignat.

Iniciar sessió amb usuari amb permisos sudo.

---

# PART 4 — Crear carpeta de logs

Les tasques no creen carpetes automàticament.

```bash
mkdir -p /home/USUARI/cron_logs
```

---

# PART 5 — Crear tasca periòdica (cada 5 minuts)

A Webmin:
```
System → Scheduled Cron Jobs → **Create a new scheduled cron job**
```

Crear nova tasca:
* Frequency: Every 5 minutes
* Executar com: usuari actual (en el meu cas vboxuser)

Command:
```bash
echo "WEBMIN CRON $(date)" >> /home/USUARI/cron_logs/webmin_cron.log
```

A **When to execute**:

**Opció simple**

* A “Simple schedule” busca una opció tipus **Every 5 minutes**

**Opció manual**

* Selecciona minuts: **0, 5, 10, 15, 20, ... 55**

  * (has de fer Ctrl+click per seleccionar múltiples minuts; Webmin ho recorda al peu del formulari)   
  * si selecciones només 5, ho efectuarà al minut 5 de cada hora: 10:05, 11:05, 12:05...

Cliquem finalment a `Create

---

## Verificació des de Webmin

En aquesta pantalla s’ha de comprovar: que apareix la nova tasca a la llista:
```
System → Scheduled Cron Jobs
```

Verificació del fitxer generat:
```
Tools → File Manager
```

Navega fins a:
```
/home/EL_TEU_USER/cron_logs/
```
---

## Comprovar des de CLI

```bash
crontab -l
```

La tasca creada via Webmin apareix al crontab.

---

# PART 6 — Crear tasca puntual

Des de Webmin → System → Scheduled Commands

Configurar execució única amb data/hora concreta:
* Run on date
* Run at time

Per saber el dia i hora actuals ens hem de fixar en la predeterminada del servidor Ubuntu
que ens surt marcat a `Current Time`

Commands to execute:
```bash
echo "TASCA WEBMIN EXECUTADA $(date)" >> /home/USUARI/cron_logs/webmin_at.log
```

---

## Verificar execució
A la pantalla nova sortirà una taula amb un ID per tasca i les seves propietats

Per veure les tasques creades:
```
System → Scheduled Commands
```

Si surt directament la pantalla de `New Scheduled Command`, la tasca ja s'ha executat i s'ha esborrat del sistema
En cas contrari sortiran totes les tasques pendents

## Verificació per CLI
```bash
atq
```
---

# PART 7 — Explicació tècnica

Webmin és una interfície gràfica

No substitueix cron

Relació real:

```
Webmin → escriu crontab → cron executa
```

El mateix per a `at`

---

# PART 8 — Errors habituals

* No tenir SSH actiu
* No tenir IP accessible
* No crear carpeta logs
* No utilitzar ruta absoluta
* Firewall bloquejant port 10000

---

# PART 9 — Conclusió professional

En aquesta pràctica hem:

* Instal·lat una eina gràfica en servidor 
* Creat tasques amb GUI 
* Verificat relació GUI → cron
* Verificat relació GUI → at
* Confirmat execució real al sistema


