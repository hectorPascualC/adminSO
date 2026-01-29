✅ Alternativa correcta (Estrategias): codi i scripts com a **AMPLIACIÓ DOCENT** (marcat)
----------------------------------------------------------------------------------------

Fem codi **creat per nosaltres**, però **clarament separat** del COS, amb etiqueta visible:

> 🧑‍🏫 **AMPLIACIÓ DOCENT (NO surt al llibre)**

Així:

*   no inventem fonts
*   mantens auditoria neta
*   tens codi i scripts per reduir text i fer classe/pràctica

A continuació et deixo **un paquet de codi** per posar **exactament** als punts on havíem detectat (FASE 2 principalment), **marcat com a ampliació docent**.

* * *

🧑‍🏫 AMPLIACIÓ DOCENT (NO surt al llibre) — Codi per RA03
==========================================================

FASE 2 (3.2 i 3.4) — Planificació de tasques amb comandes
---------------------------------------------------------

### Linux — `cron` (tasca repetitiva)

```bash
# Exemple: cada dia a les 02:30, escriu una línia al log
30 2 * * * /usr/bin/echo "$(date) - tasca programada" >> /var/log/ra03_tasques.log
```

### Linux — `at` (tasca puntual)

```bash
# Exemple: executa una ordre una sola vegada d'aquí 5 minuts
echo "/usr/bin/echo 'execució puntual' >> /tmp/ra03_at.log" | at now + 5 minutes
```

### Windows — `schtasks` (tasca repetitiva)

```bat
:: Exemple: cada dia a les 09:00 executa un script
schtasks /Create /TN "RA03_LogDiari" /SC DAILY /ST 09:00 ^
/TR "cmd /c echo %DATE% %TIME% >> C:\Temp\ra03_log.txt"
```

### Windows — `schtasks` (tasca puntual)

```bat
:: Exemple: una sola execució en una data/hora concreta
schtasks /Create /TN "RA03_ExecPuntual" /SC ONCE /SD 02/01/2026 /ST 18:30 ^
/TR "cmd /c echo Execució puntual >> C:\Temp\ra03_once.txt"
```

* * *

FASE 3 (3.3) — Restriccions de seguretat (exemples mínims)
----------------------------------------------------------

### Linux — executar amb usuari limitat (idea clau)

```bash
# Exemple: fer que la tasca s’executi amb un usuari sense privilegis
sudo -u usuari_limitat /usr/bin/echo "tasca amb permisos limitats" >> /tmp/ra03_perm.log
```

### Windows — executar amb un usuari concret (conceptual)

> En Windows, les tasques es poden configurar perquè s’executin amb un usuari específic i només amb els permisos necessaris (no cal “Administrador” si no és imprescindible).

_(No poso més perquè si entrem en credencials / “run whether user is logged on” ja es converteix en manual de GUI.)_

* * *

FASE 4 (3.5) — Administració de comptes (sense RA07)
----------------------------------------------------

### Linux — acció simple planificada (exemple segur)

```bash
# Exemple: llistar usuaris i deixar una “foto” diària (administració bàsica)
cut -d: -f1 /etc/passwd > /var/log/ra03_usuaris_snapshot.txt
```

### Windows — acció simple planificada (exemple segur)

```bat
:: Exemple: exportar llistat d'usuaris locals (administració bàsica)
net user > C:\Temp\ra03_usuaris_locals.txt
```

* * *

FASE 6 (3.8) — Documentació mínima d’una tasca (plantilla simple)
-----------------------------------------------------------------

```text
Nom de la tasca: ________________________
Objectiu: _______________________________
Quan s’executa: __________________________
Ordre/acció: _____________________________
Usuari d’execució: _______________________
Sortida/registre: ________________________
Notes: __________________________________
```

* * *

On inserir-ho EXACTAMENT al document (sense tocar l’índex)
==========================================================

### A la FASE 2

*   Just després del bloc “2.1 Concepte de planificació…”
    *   Inserir `cron` + `at` (o Windows `schtasks`, segons el teu entorn)
*   Just després del bloc “2.2 Tasques puntuals i repetitives…”
    *   Inserir el segon parell d’exemples (puntual/repetitiva)

**Sempre amb l’etiqueta**: 🧑‍🏫 AMPLIACIÓ DOCENT (NO surt al llibre)

* * *
