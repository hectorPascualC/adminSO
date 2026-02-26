# RA03 - Pràctica 03 - Planificació de tasques amb Webmin

Mòdul: Administració de Sistemes Operatius
Resultat d’aprenentatge: RA03 — Gestiona l’automatització de tasques del sistema

Criteris associats:

* 3.6 Instal·la i configura eines gràfiques de planificació
* 3.7 Utilitza eines gràfiques per a la planificació de tasques

---

# 1. Objectiu

Aprendre a instal·lar i utilitzar una eina gràfica d’administració per gestionar tasques automàtiques en un servidor Linux.

En aquesta pràctica utilitzarem:

> Webmin

---

# 2. Context de treball

* Ubuntu Server
* Sense entorn gràfic
* Accés remot via navegador web
* Connexió mitjançant SSH des del client

---

# 3. Instal·lació de Webmin

Has de:

1. Instal·lar Webmin al servidor
2. Verificar que el servei està actiu
3. Comprovar que escolta al port `10000`
4. Accedir via navegador a:

```
https://IP_DEL_SERVIDOR:10000
```

---

# 4. Exercicis

## 4.1 Exercici 1 — Instal·lació i accés

* Instal·lar Webmin
* Verificar estat del servei amb `systemctl`
* Accedir via navegador
* Iniciar sessió amb usuari amb permisos d’administració
* Adjuntar captura de pantalla del panell principal

---

## 4.2 Exercici 2 — Tasca periòdica amb GUI

Crear una tasca periòdica des de:

```
System → Scheduled Cron Jobs
```

La tasca ha de:

* Executar-se cada 5 minuts
* Executar la comanda:

```
echo "WEBMIN CRON $(date)" >> /home/USUARI/cron_logs/webmin_cron.log
```

Després:

* Verificar que el fitxer es crea
* Comprovar el contingut amb `cat`
* Executar `crontab -l` per comprovar que la tasca s’ha afegit correctament

---

## 4.3 Exercici 3 — Tasca puntual

Crear una tasca que:

* S’executi una sola vegada (execució diferida)
* Escrigui:

```
TASCA WEBMIN EXECUTADA
```

* Desa el resultat a:

```
/home/USUARI/cron_logs/webmin_at.log
```

Verificar l’execució:

* Comprovar que el fitxer s’ha creat
* Explicar si Webmin utilitza cron o at per aquesta funcionalitat

---

## 4.4 Exercici 4 — Relació amb CLI

Executar:

```
crontab -l
```

I explicar:

* Si les tasques creades amb Webmin apareixen
* Quina relació hi ha entre Webmin i cron
* Si Webmin substitueix cron o simplement el gestiona
* Avantatges i limitacions d’utilitzar GUI en entorns server

---

# 5. Reflexió final

1. Avantatges d’utilitzar Webmin
2. Inconvenients en entorns professionals
3. Diferències entre CLI i GUI
4. En quin tipus d’empresa utilitzaries Webmin?
5. És recomanable instal·lar eines web d’administració en servidors crítics? Justifica-ho.

---

# 6. Lliurament

L’alumne haurà de lliurar:

* Document PDF o Markdown
* Captures de Webmin
* Resultat de `crontab -l` i `atq
* Contingut dels logs generats
* Explicació tècnica de la relació GUI → cron
* Explicació tècnica de la relació GUI → at

