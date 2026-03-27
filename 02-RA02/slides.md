---
marp: true
theme: default
paginate: true
size: 16:9
header: '0374 · Administració de sistemes operatius'
footer: 'RA02 · Administra processos del sistema'
style: |
  section {
    font-family: 'Aptos', 'Segoe UI', sans-serif;
    font-size: 28px;
    padding: 48px;
    background: #f8fafc;
    color: #0f172a;
  }
  h1, h2 {
    color: #0b3d91;
    margin-bottom: 0.35em;
  }
  h1 {
    font-size: 1.7em;
  }
  h2 {
    font-size: 1.25em;
  }
  strong {
    color: #0b3d91;
  }
  code {
    background: #e2e8f0;
    color: #0f172a;
    padding: 0.12em 0.28em;
    border-radius: 6px;
    font-size: 0.9em;
  }
  pre {
    font-size: 0.72em;
    border-radius: 10px;
    padding: 0.7em;
    border: 1px solid #cbd5e1;
  }
  blockquote {
    border-left: 6px solid #0b3d91;
    padding-left: 0.8em;
    color: #334155;
  }
  .cols {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 28px;
  }
  .box {
    background: white;
    border: 1px solid #cbd5e1;
    border-radius: 14px;
    padding: 18px 22px;
  }
  .small { font-size: 0.82em; }
  .tiny { font-size: 0.72em; }
---

# RA02
## Administra processos del sistema

**Objectiu:** entendre, controlar i documentar els processos del sistema amb criteris de seguretat i eficiència.

> Presentació resumida, pensada per classe i estudi autònom.

---

## Què treballa la RA02?

- concepte de **procés**
- **tipus, estats i cicle de vida**
- **interrupcions i excepcions**
- diferència entre **procés, fil i treball**
- **creació, manipulació i finalització** de processos
- control amb **comandes** i **eines gràfiques**
- **arrencada**, **dimonis**, **seguretat** i **documentació**

---

## 1. Què és un procés?

Un **procés** és un **programa en execució**.

No és només el fitxer del programa:

- té **codi** i **dades**
- ocupa **memòria**
- utilitza **recursos**
- rep un **PID**
- pot canviar d'**estat** durant la seva vida

**Idea clau:** el sistema operatiu administra processos per repartir CPU, memòria i recursos.

---

## 2. Tipus de processos

<div class="cols">
<div class="box">

**Segons la funció**

- processos d'**usuari**
- processos de **sistema**

</div>
<div class="box">

**Segons l'execució**

- **interactius**
- en **segon pla**
- **pare** i **fill**

</div>
</div>

**Exemple:** navegador, editor de text, servei de xarxa, gestor d'impressió.

---

## 3. Estructura bàsica d'un procés

Un procés acostuma a tenir:

- **PID** → identificador únic
- **PPID** → procés pare
- **estat** → preparat, execució, espera...
- **prioritat**
- **memòria associada**
- **fitxers o recursos oberts**

Sense aquesta informació, el sistema no podria gestionar-lo correctament.

---

## 4. Estats d'un procés

- **Nou**
- **Preparat**
- **En execució**
- **En espera / bloquejat**
- **Finalitzat**

### Cicle de vida

`nou → preparat → execució → espera → preparat → execució → finalitzat`

**Important:** un procés no està sempre executant-se; sovint està esperant torn o dades.

---

## 5. Interrupcions i excepcions

<div class="cols">
<div class="box">

**Interrupció**

Senyal que obliga la CPU a atendre un esdeveniment.

Exemples:
- teclat
- temporitzador
- xarxa

</div>
<div class="box">

**Excepció**

Situació especial durant l'execució d'una instrucció.

Exemples:
- divisió per zero
- accés il·legal a memòria
- instrucció no vàlida

</div>
</div>

---

## 6. Procés, fil i treball

- **Procés** → unitat completa d'execució amb recursos propis
- **Fil** (*thread*) → unitat més petita dins d'un procés
- **Treball** (*job*) → tasca controlada per la *shell*

### Resum ràpid

- un procés pot tenir **diversos fils**
- els fils **comparteixen memòria** del procés
- la *shell* pot controlar treballs amb `jobs`, `bg` i `fg`

---

## 7. Gestió de processos

Administrar processos vol dir:

- **crear-los**
- **veure'ls**
- **canviar-ne la prioritat**
- **aturar-los o reprendre'ls**
- **finalitzar-los**

### Comandes habituals

```bash
ps
ps aux
top
htop
pstree
kill PID
pkill nom
jobs
bg
fg
```

---

## 8. Exemples de control

```bash
# veure tots els processos
ps aux

# monitoritzar en temps real
top

# finalitzar un procés
kill 1234

# finalitzar-lo de forma forçada
kill -9 1234
```

**Compte:** `kill -9` s'ha d'utilitzar només quan un procés no respon.

---

## 9. Prioritats i eficiència

No tots els processos tenen la mateixa importància.

A Linux podem ajustar prioritats amb:

```bash
nice -n 10 ordre
renice 5 -p 1234
```

### Per què és útil?

- afavorir processos crítics
- limitar processos pesats
- millorar la resposta general del sistema

---

## 10. El sistema de fitxers i els processos

En Linux, molta informació del sistema es consulta amb el **sistema de fitxers virtual**.

### Directori clau

```bash
/proc
```

### Exemples

```bash
cat /proc/cpuinfo
cat /proc/meminfo
cat /proc/1/status
ls /proc
```

**Idea clau:** `/proc` permet registrar, identificar i analitzar processos.

---

## 11. Eines gràfiques de seguiment

A més de la línia d'ordres, es poden usar:

- **Monitor del sistema**
- **Gestor de tasques**
- eines pròpies de cada distribució

Permeten veure:

- ús de **CPU**
- ús de **memòria**
- processos actius
- processos bloquejats o sense resposta

---

## 12. Seqüència d'arrencada

Quan encenem l'ordinador, el sistema segueix diverses fases:

1. inicialització del maquinari
2. càrrega del gestor d'arrencada
3. càrrega del nucli
4. inici de serveis
5. execució de processos bàsics
6. inici de sessió

### A Linux

El procés **PID 1** acostuma a ser `systemd`.

---

## 13. Dimonis o serveis

Els **dimonis** (*daemons*) són processos en segon pla que ofereixen serveis.

### Exemples

- `sshd`
- `cron`
- serveis de xarxa
- serveis de registre

### Comandes útils

```bash
systemctl status ssh
systemctl status cron
journalctl -b
```

---

## 14. Seguretat davant processos no identificats

Hem de sospitar quan un procés:

- consumeix massa **CPU** o **RAM**
- té un **nom estrany**
- s'executa des d'una ruta inusual
- obre connexions inesperades
- reapareix constantment

### Accions bàsiques

- identificar **PID**, usuari i executable
- revisar **logs**
- comprovar si és un servei legítim
- aturar-lo si cal
- documentar la incidència

---

## 15. Documentació de processos

Documentar vol dir indicar:

- **nom** del procés
- **funció**
- **servei associat**
- com s'**inicia**
- com es **reinicia**
- riscos si falla
- relació amb altres processos

**Bona pràctica:** si es pot entendre, controlar i recuperar, està ben documentat.

---

## 16. Resum final

### Bloc 1 · Fonaments
Procés, tipus, estructura, estats.

### Bloc 2 · Execució interna
Interrupcions, excepcions, fils, prioritats.

### Bloc 3 · Administració pràctica
Comandes, eines gràfiques, control i seguiment.

### Bloc 4 · Relació amb el sistema
`/proc`, arrencada, `systemd`, dimonis.

### Bloc 5 · Administració segura
Detecció, resposta i documentació.

---

## Preguntes clau per estudiar

1. Què diferencia **procés**, **fil** i **treball**?
2. Quins són els **estats** d'un procés?
3. Per a què serveix **`/proc`**?
4. Quan faries servir **`kill -9`**?
5. Quina relació hi ha entre **arrencada**, **PID 1** i **dimonis**?
6. Quines mesures prendries davant un **procés sospitós**?

---

## Fi

**RA02 preparada en format Marp**

Si vols, al següent pas et puc fer també:

- una **versió més visual**
- una **versió més curta per exposar**
- o una **versió amb imatges del ZIP**
