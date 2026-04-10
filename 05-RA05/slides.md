---
marp: true
theme: default
paginate: true
size: 16:9
header: '0374 · Administració de sistemes operatius'
footer: 'RA05 · Administra servidors d’impressió'
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

# RA05
## Administra servidors d’impressió

**Objectiu:** entendre com funciona la impressió en xarxa i saber instal·lar, configurar, gestionar i documentar un servidor d’impressió.

> Presentació resumida, pensada per classe i estudi autònom.

---

## Què treballa la RA05?

- funció dels **sistemes i servidors d’impressió**
- **ports i protocols** d’impressió
- eines integrades del sistema operatiu
- configuració d’un **servidor d’impressió en entorn web**
- **impressores lògiques** i **grups d’impressió**
- gestió de **cues i treballs**
- impressió compartida entre **sistemes heterogenis**
- **documentació** de la configuració

---

## 1. Què és un servidor d’impressió?

Un **servidor d’impressió** és el sistema que centralitza la gestió de les impressores d’una xarxa.

### Què fa?

- rep peticions d’impressió
- administra les **cues de treball**
- envia els treballs a la impressora adequada
- aplica permisos i opcions de configuració
- facilita la compartició a diversos usuaris

**Idea clau:** permet ordenar, controlar i compartir la impressió en xarxa.

---

## 2. Per què és útil en una empresa o centre?

- evita configurar cada impressora manualment a cada equip
- centralitza **drivers**, permisos i cues
- facilita el manteniment
- permet compartir una mateixa impressora entre molts usuaris
- ajuda a controlar incidències i consum

### Sense servidor d’impressió...

- més errors de configuració
- més temps de manteniment
- menys control sobre els treballs enviats

---

## 3. Conceptes bàsics d’impressió

<div class="cols">
<div class="box">

**Impressora física**

Dispositiu real connectat per USB, xarxa o Wi‑Fi.

</div>
<div class="box">

**Impressora lògica**

Configuració creada al sistema per enviar treballs a una impressora física o virtual.

</div>
</div>

### També hem de distingir

- **treball d’impressió**
- **cua d’impressió**
- **controlador o driver**
- **servidor d’impressió**

---

## 4. Ports i protocols d’impressió

### Ports habituals

- **IPP** → `631`
- **LPD/LPR** → `515`
- **SMB / impressora compartida** → `445` i, en alguns entorns antics, `139`
- **RAW / JetDirect** → `9100`

### Protocols habituals

- **IPP**
- **LPD/LPR**
- **SMB/CIFS**
- **RAW / AppSocket / JetDirect**

**Important:** cal saber quin protocol usa cada impressora o servidor.

---

## 5. Sistemes d’impressió

Els sistemes operatius incorporen eines pròpies per gestionar impressores.

### Exemples habituals

- **Linux** → sovint amb **CUPS**
- **Windows** → gestió integrada d’impressores i servidor d’impressió

### Funcions habituals

- afegir impressores
- compartir-les
- veure la cua
- pausar o reprendre treballs
- establir una impressora per defecte

---

## 6. CUPS i entorn web

En Linux, el sistema més habitual és **CUPS** (*Common UNIX Printing System*).

### Què permet?

- administrar impressores locals i de xarxa
- gestionar cues i treballs
- compartir impressores
- administrar-ho des d’un **entorn web**

### Interfície web típica

```text
http://localhost:631
```

**Idea clau:** la RA05 demana saber instal·lar i configurar un servidor d’impressió en entorn web.

---

## 7. Eines integrades al sistema operatiu

### A Linux

```bash
lpstat -p -d
lpq
lpr document.pdf
cancel ID_treball
lpadmin
```

### A Windows

- Configuració > Impressores i escàners
- Gestió d’impressió
- Propietats de la impressora
- Compartició i permisos

**No n’hi ha prou amb teoria:** cal saber usar comandes i eines gràfiques.

---

## 8. Exemples bàsics en Linux

```bash
# veure impressores disponibles
lpstat -p -d

# enviar un document a imprimir
lpr fitxer.pdf

# veure la cua
lpq

# cancel·lar un treball
cancel 12
```

**Compte:** l’administració avançada sol requerir permisos elevats.

---

## 9. Impressores lògiques

Una **impressora lògica** és la configuració que veu l’usuari al sistema.

### Es pot crear per...

- una impressora física concreta
- una impressora compartida per xarxa
- una impressora virtual, com ara PDF
- diferents configuracions d’una mateixa impressora física

### Exemple pràctic

Una mateixa impressora física pot tenir:
- una impressora lògica en **color**
- una impressora lògica en **blanc i negre**
- una impressora lògica amb **doble cara**

---

## 10. Grups d’impressió

Els **grups d’impressió** permeten agrupar diverses impressores sota una mateixa lògica de servei.

### Avantatges

- repartir càrrega de treball
- millorar disponibilitat
- reduir temps d’espera
- simplificar la impressió per a l’usuari

### Idea pràctica

L’usuari envia el document a un grup i el sistema decideix quina impressora del grup l’atén.

---

## 11. Cues i treballs d’impressió

Cada impressora gestiona una **cua** de treballs.

### Operacions habituals

- veure la cua
- canviar l’ordre dels treballs
- pausar un treball
- reprendre’l
- cancel·lar-lo
- buidar la cua

### Per què és important?

Perquè molts problemes d’impressió no estan a la impressora, sinó a la **cua bloquejada** o a un treball que falla.

---

## 12. Compartició en xarxa entre sistemes diferents

La RA05 també demana compartir impressores entre **sistemes operatius diferents**.

### Escenaris habituals

- Windows comparteix una impressora i Linux hi accedeix
- Linux publica una impressora i Windows hi imprimeix
- impressora de xarxa accessible des de tots dos sistemes

### Requisits bàsics

- connectivitat de xarxa
- protocol compatible
- permisos correctes
- driver o controlador adequat

---

## 13. Seguretat i bones pràctiques

També cal aplicar criteris de seguretat.

### Recomanacions

- limitar qui pot imprimir o administrar
- protegir l’accés a la interfície web
- evitar compartir sense control
- revisar logs i incidències
- eliminar impressores antigues o duplicades
- documentar canvis i configuracions

**Important:** un servidor d’impressió és un servei de xarxa més i s’ha d’administrar amb criteri.

---

## 14. Què s’hauria de documentar?

- nom del servidor
- IP o nom de xarxa
- protocol utilitzat
- ports necessaris
- impressores creades
- impressora per defecte
- grups d’impressió
- permisos d’accés
- proves de funcionament
- incidències detectades

**Bona pràctica:** si un altre tècnic ho pot reproduir, la documentació és útil.

---

## 15. Relació amb els criteris d’avaluació

### La RA05 demana saber...

- descriure la funcionalitat del servidor d’impressió
- identificar **ports i protocols**
- usar **eines del sistema**
- configurar un servidor d’impressió **via web**
- crear **impressores lògiques**
- crear **grups**
- gestionar **cues i treballs**
- compartir entre **sistemes diferents**
- **documentar** tot el procés

---

## 16. Resum final

### Bloc 1 · Fonaments
Servidor d’impressió, impressora física i lògica.

### Bloc 2 · Comunicació
Protocols, ports i compartició en xarxa.

### Bloc 3 · Administració
CUPS, eines del sistema, cues i treballs.

### Bloc 4 · Organització
Impressores lògiques, grups i permisos.

### Bloc 5 · Tancament tècnic
Proves, incidències i documentació.

---

## Preguntes clau per estudiar

1. Quina diferència hi ha entre **impressora física** i **lògica**?
2. Quins **ports i protocols** són habituals en impressió?
3. Per a què serveix **CUPS**?
4. Què és una **cua d’impressió**?
5. Per què es creen **grups d’impressió**?
6. Què cal comprovar quan compartim una impressora entre **Windows i Linux**?
7. Quina documentació mínima hauries de deixar feta?

---

## Fi

**RA05 preparada en format Marp**

Després, si vols, podem fer:

- una versió més **visual**
- una versió amb **captures del llibre**
- o una versió més **curta per exposició**
