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
## Automatització de tasques del sistema

Administració de Sistemes Operatius  
ASIX02

---

# 3.0.1 - Context de la RA03

- Què vol dir automatitzar tasques
- Per què és necessari en administració de sistemes
- Relació amb el manteniment del sistema

---

# 3.0.2 - Objectius de la RA03

- Comprendre el concepte d'automatització
- Planificar tasques amb comandes del sistema
- Aplicar criteris bàsics de seguretat
- Utilitzar eines gràfiques de planificació
- Documentar tasques automàtiques

---

# 3.0.3 - Estructura del contingut

- 3.1 - Avantatges de l'automatització
- 3.2 - Planificació de tasques
- 3.3 - Seguretat
- 3.4 - Administració de comptes
- 3.5 - Eines gràfiques
- 3.6 - Documentació

---

# 3.1 - Avantatges de l'automatització de tasques

---

# 3.1.1 - Què és una tasca repetitiva

Una **tasca repetitiva** és una acció del sistema que:

- Es fa **sempre de la mateixa manera**
- Es repeteix **cada cert temps**
- No requereix decisió humana cada vegada  

---

# 3.1.1 - Què és una tasca repetitiva

Exemples típics:

- Còpies de seguretat:  
    - Mateixa carpeta d'origen   
    - Mateixa carpeta de destí   
    - Mateixa hora   
    - Mateixa acció  

---  

# 3.1.1 - Què és una tasca repetitiva

- Neteja de fitxers temporals: 
    - Mateixa carpeta (temp, tmp, etc.)
    - Mateixa acció (esborrar)
    - Mateix interval de temps (setmanal)  

---

# 3.1.1 - Què és una tasca repetitiva

- Generació de llistats o informes  
    - Mateixa comanda o programa  
    - Mateix format de sortida  
    - Mateix dia i hora   

---

# 3.1.2 - Per què automatitzar tasques

Automatitzar vol dir:

> Deixar programat que el sistema faci una tasca  
> **sense intervenció manual** de l'administrador


L'administrador:
- Defineix **què** s'ha de fer
- Defineix **quan** s'ha de fer
- El sistema s'encarrega de l'execució

---

# 3.1.3 - Avantatges principals de l'automatització

L'automatització permet:

1. **Estalviar temps**
2. **Reduir errors humans**
3. **Garantir regularitat**
4. **Millorar el manteniment del sistema**

Aquestes millores són especialment importants en sistemes amb molts usuaris o servidors  

---

# 3.1.4 - Visió professional en administració de sistemes

En administració de sistemes:

- Les tasques manuals no escalen bé
- Les tasques repetitives han d'estar automatitzades
- L'administrador passa de **fer** a **supervisar**

Això és la base del treball professional en sistemes.

---

# 3.1.4 - Exemple / Esquema - Per què automatitzar

**Manual repetitiu:**
[Repetició] -> [Temps] -> [Errors] -> [Inconsistència]

**Automatitzat:**
[Planificació] -> [Execució regular] -> **[Log]** -> [Control / Auditoria]

**Idea clau**:
Automatitzar = fer que el sistema executi una tasca **igual cada cop**
i deixi evidència (logs) perquè es pugui verificar

---

# 3.2 - Planificació de tasques del sistema

---

# 3.2.1 - Què vol dir planificar una tasca

Planificar una tasca vol dir:

- Indicar al sistema **quina acció** ha d'executar
- Indicar **quan** s'ha d'executar
- Fer-ho **sense intervenció manual** cada vegada

L'administrador defineix la planificació el sistema executa la tasca.

---

# 3.2.2 - Planificació i execució manual

Execució manual:
- L'administrador llança la comanda
- Només passa una vegada

Planificació:
- La comanda queda registrada
- El sistema l'executa automàticament
- Pot repetir-se o ser puntual

---

# 3.2.3 - Tipus de tasques segons el temps

Segons **quan** s'executen, distingim:

1. Tasques puntuals  
2. Tasques repetitives

Aquesta distinció és clau en l'administració del sistema.

---
# 3.2.4 - Tasques puntuals

Una tasca puntual:

- S'executa **una sola vegada**
- En un moment concret
- No es torna a repetir

Exemples:
- Executar una neteja aquesta nit
- Llançar una comanda un dia concret
- Fer una acció fora d'horari laboral

---

# 3.2.5 - Tasques repetitives

Una tasca repetitiva:

- S'executa **de manera regular**
- Segueix un patró de temps
- Forma part del manteniment habitual

Exemples:
- Còpies de seguretat diàries: incremental diària
- Neteja setmanal de fitxers
- Generació periòdica d'informes

---

# 3.2.6 - Què guarda el sistema quan planifiquem

Quan planifiquem una tasca, el sistema guarda:

- La comanda o acció a executar
- El moment o la freqüència
- L'usuari amb què s'executarà
- Les condicions bàsiques d'execució

No s'executa ara: queda **programada**.

---

# 3.2.7 - Visió conceptual Linux / Windows

Encara que les eines siguin diferents:

- Linux i Windows
- Línia d'ordres o interfície gràfica

El concepte és el mateix:
> Programar una acció perquè el sistema la faci sol

La diferència és **l'eina**

---

# 3.2.8 - Exemple / Esquema - Model mental d'una tasca planificada

Tasca planificada = 4 peces:

* (1) QUAN  -> Trigger (hora / calendari / esdeveniment)
* (2) QUÈ   -> Action  (comanda / programa / script)
* (3) QUI   -> Context (usuari amb permisos)
* (4) ON    -> A on -> Genera output  (logs / fitxers resultants)

---

# 3.2.8 - Exemple / Esquema - Model mental d'una tasca planificada 

Esborrar cada dia a les 3:00 fitxers de la carpeta temporal de més de 7 dies:

Linux (cron):
```
0 3 * * * /usr/bin/find /tmp/fitxers_temporals -type f -mtime +7 -delete
```

Windows (schtasks):
```
schtasks /Create /SC DAILY /ST 03:00 /TN "NetejaTemp" /TR "powershell.exe -Command \"Get-ChildItem 
C:\Temp -File | Where-Object {$_.LastWriteTime -lt (Get-Date).AddDays(-7)} | Remove-Item\""
```
---

# 3.2.9 - Exemple / Esquema - Puntual vs Repetitiva

**PUNTUAL (1 cop)**
- Objectiu: executar una acció una vegada
- Exemple: còpia de seguretat avui a les 22:00

**REPETITIVA (cada X temps)**
- Objectiu: manteniment continu
- Exemple: neteja logs cada dia a les 02:00

Esquema temporal:

Puntual:     |-----------X
Repetitiva:  |----X----X----X----X---->  

---

# 3.3 - Restriccions de seguretat en tasques automatitzades

---

# 3.3.1 - Per què la seguretat és clau

Les tasques automatitzades:

- S'executen **sense supervisió directa**
- Poden afectar fitxers, usuaris o serveis
- S'executen de manera recurrent

Un error de seguretat es pot repetir automàticament moltes vegades!!!

---

# 3.3.2 - Principi de mínims privilegis

Una tasca automàtica ha de:

- Executar-se amb **el mínim de permisos possibles**
- Fer només allò que necessita
- No utilitzar privilegis elevats si no és imprescindible

Això redueix riscos i errors greus.

---

# 3.3.3 - Usuari d'execució

Quan planifiquem una tasca cal decidir:

- Amb **quin usuari** s'executa
- Quins **permisos** té aquest usuari
- A quins recursos pot accedir

No totes les tasques han de córrer com a administrador

---

# 3.3.4 - Riscos habituals

Alguns riscos comuns són:

- Executar tasques amb permisos excessius
- Modificar fitxers crítics del sistema
- Llançar scripts sense control
- No saber exactament què fa la tasca

Aquests errors són típics en sistemes reals.

---

# 3.3.5 - Seguretat i automatització

Automatitzar **no** vol dir:

- Donar més permisos
- Perdre el control
- Ometre la revisió

Automatitzar vol dir:
> Planificar amb criteri i responsabilitat.

---

# 3.3.6 - Exemple / Esquema - Mínim privilegi

**Exemple dolent:**
Usuari: admin/root
Permisos: tot
Impacte: qualsevol error > dany gran

**Exemple bo:**
Usuari: usuari_tasca
Permisos: només carpeta X i comanda Y
Impacte: error limitat i controlable

---

# 3.4 - Automatització de l'administració de comptes

---

# 3.4.1 - Què vol dir automatitzar comptes

Automatitzar comptes **no** vol dir:

- Crear sistemes complexos d'usuaris
- Fer gestió avançada de permisos
- Substituir l'administrador

Vol dir:
> Automatitzar tasques simples i repetitives relacionades amb comptes d'usuari

---

# 3.4.2 - Tasques típiques automatitzables

**Exemples de tasques habituals:**

- Generar llistats d'usuaris
- Comprovar comptes existents
- Exportar informació de comptes
- Fer revisions periòdiques

**Aquestes tasques:**
- Es repeteixen
- Consumeixen temps
- No requereixen decisió complexa

---

# 3.4.3 - Automatització != gestió avançada

**És important separar:**

Automatització (RA03):
- Accions simples
- Execució programada
- Suport a l'administració

Gestió avançada (RA07): administrar molts elements, amb regles, polítiques i control
- Alta i baixa massiva d'usuaris
- Polítiques complexes
- Control detallat de permisos

---

# 3.4.4 - Relació amb la seguretat

**Encara que siguin tasques simples:**

- Cal controlar amb quin usuari s'executen
- Cal limitar l'accés a la informació
- Cal evitar exposar dades sensibles

Automatitzar també implica responsabilitat.

---

# 3.4.5 - Exemple / Esquema - Automatització de comptes 

**Objectiu:**
Automatitzar tasques simples i repetitives sobre comptes,
sense entrar en gestió avançada (domini/polítiques complexes).

**Flux simple:**
[Entrada] -> [Acció sobre compte] -> [Verificació] -> [Log]

**Exemple conceptual:**
- Entrada: llista d'usuaris "nous"
- Acció: crear compte local o activar/desactivar
- Verificació: comprovar existència/estat
- Log: deixar evidència (fitxer)

---

# 3.5 - Eines gràfiques per a la planificació de tasques

---

# 3.5.1 - Per què utilitzar eines gràfiques

**Les eines gràfiques permeten:**

- Facilitar la planificació de tasques
- Evitar errors de sintaxi
- Visualitzar millor la configuració
- Fer l'administració més accessible

Són especialment útils en entorns educatius o d'iniciació  

---

# 3.5.2 - Relació amb la planificació per comandes

**Les eines gràfiques:**

- Fan el **mateix** que les comandes
- Canvia la manera d'interactuar
- No canvia el resultat final
- Task Scheduler, Crontab-UI (https://github.com/alseambusher/crontab-ui)

El sistema continua executant tasques automàtiques programades  

---

# 3.5.3 - Tipus d'accions que permeten

**Amb eines gràfiques es pot:**

- Crear tasques puntuals
- Crear tasques repetitives
- Definir horaris i freqüències
- Assignar l'usuari d'execució

Sempre sota els mateixos criteris de seguretat vistos abans

--- 

# 3.5.4 - Exemple / Esquema 

**GUI és útil quan:**
- Cal visualitzar molts paràmetres
- Cal reduir errors de sintaxi
- Cal manteniment per part d'altres admins

---

# 3.5.5 - Exemple / Esquema - Per què GUI (eina gràfica)

**Comparació ràpida:**

CLI:
+ ràpid per experts
+ Fàcil de repetir i automatitzar
- errors per sintaxi

GUI:
+ més visual
+ guia (assistent)
- Menys eficient si s’han de crear moltes tasques: obrir finestres, clicar botons, omplir formularis, avançar per assistents, confirmar opcions  

---

# 3.5.6 - Exemple / Esquema - Parts d'una tasca en una GUI

**Quan crees una tasca en una eina gràfica, sempre hi ha:**

(1) General
- nom, descripció, usuari

(2) Triggers
- quan s'executa (dia/hora/esdeveniment)

(3) Actions
- què executa (programa/comanda)

(4) Conditions / Settings
- condicions (energia, xarxa, repetició)
- comportament si falla

**Esquema:**

[General] -> [Triggers] -> [Actions] -> [Settings] -> [Historial/Logs]

---

# 3.6 - Documentació de tasques automàtiques

---

# 3.6.1 - Per què documentar tasques

**Les tasques automàtiques:**
- No es veuen executar cada dia
- Poden fallar sense avís immediat
- Poden ser modificades per altres persones

**Sense documentació:**
- Es perd el control del sistema
- Augmenta el risc d'errors

---

# 3.6.2 - Què vol dir documentar

**Documentar una tasca vol dir:**

- Deixar constància que existeix
- Explicar **què fa**
- Indicar **quan s'executa**
- Indicar **amb quin usuari**

No cal documentació extensa, però sí **clara i útil**  

---

# 3.6.3 - Informació mínima a documentar

**Com a mínim, cal indicar:**
- Nom o identificador de la tasca
- Acció o comanda que executa
- Planificació (moment o freqüència)
- Usuari d'execució
- Finalitat de la tasca

Aquesta informació permet entendre i revisar la tasca  

---

# 3.6.4 - Relació amb seguretat i manteniment

**La documentació permet:**
- Revisar permisos i riscos
- Detectar errors de configuració
- Facilitar auditories
- Millorar el manteniment del sistema

Una tasca documentada és una tasca controlada  

---

# 3.8 - Exemple / Esquema - Plantilla mínima de documentació

**Plantilla:**

Markdown, PDF, documents tipus Word / LibreOffice, Wiki corporativa (Confluence, GitLab Wiki...)

```
/automatitzacions
 ├─ backup_diari.md
 ├─ neteja_temp.md
 └─ README.md

```

Nom de la tasca:
Objectiu:
Quan s'executa (trigger):
Què executa (action):
Amb quin usuari (context):
On deixa evidència (log/output):
Riscos i controls:
Com verificar que ha funcionat:
Com desfer-ho (rollback bàsic):

**Idea clau:**
Si algú altre llegeix això, ha de poder entendre la tasca i verificar-la sense preguntar-te  

---

