# PART TEÒRICA (TEST)

# 1. Automatitzar una tasca

**Resposta correcta**

**C)** Programar una acció perquè s’executi sense intervenció directa

**Per què és correcta**

Automatitzar significa programar l’execució d’una acció sense intervenció manual.

Document referència
`contingut.md`

Frase utilitzada:

> “Automatitzar una tasca significa programar una acció perquè el sistema l’executi automàticament.”

---

**Per què les altres són incorrectes**

A) Eliminar supervisió
✖ Automatitzar **no elimina supervisió**, només executa automàticament.

B) Executar manualment
✖ És exactament el contrari d’automatitzar.

D) Fer còpies de seguretat
✖ És **un exemple d’automatització**, però no la definició.

---

# 2. Línia cron

```
0 3 * * *
```

**Resposta correcta**

**B)** Cada dia a les 03:00

**Per què**

El format cron és:

```
minut hora dia mes dia-setmana
```

Document
`slidesCron.md`

Frase:

> “La sintaxi de cron segueix el format minut hora dia mes dia-setmana.”

---

**Per què les altres són incorrectes**

A) Cada 3 minuts
✖ El primer camp seria */3.

C) Cada 3 hores
✖ L’hora seria */3.

D) Dia 3 del mes
✖ El tercer camp seria 3.

---

# 3. Redirecció >>

**Resposta correcta**

**B)** La tasca pot fallar

**Per què**

Si el directori no existeix, la redirecció no pot crear-lo.

Document
`tasca02Manual.md`

Frase:

> “Si el directori de destinació no existeix, la redirecció pot provocar error en l’execució.”

---

**Incorrectes**

A) Crear carpeta
✖ `>>` no crea directoris.

C) Redirigir a /tmp
✖ No existeix aquest comportament.

D) No passa res
✖ Generaria error.

---

# 4. Principi de seguretat

**Resposta correcta**

**C)** Least Privilege

Document
`slides.md`

Frase:

> “Principi de least privilege: executar processos amb el mínim privilegi necessari.”

---

Incorrectes

A) Redundància
✖ Alta disponibilitat.

B) Multimàster
✖ Arquitectura replicació.

D) Snapshot
✖ Estat del sistema.

---

# 5. Webmin

**Resposta correcta**

**C)** És una interfície gràfica que gestiona cron i at

Document
`tasca03Manual.md`

Frase:

> “Webmin proporciona una interfície gràfica per gestionar cron i at.”

---

Incorrectes

A) Substitueix cron
✖ cron continua executant tasques.

B) Escriu al kernel
✖ modifica configuracions del sistema.

D) Només funciona amb root
✖ pot utilitzar altres usuaris amb permisos.

---

# 6. Rutes absolutes

**Resposta correcta**

**A)** cron té un entorn limitat

Document
`slidesCron.md`

Frase:

> “Cron s’executa amb un entorn reduït i és recomanable utilitzar rutes absolutes.”

---

Incorrectes

B) velocitat
✖ no té relació.

C) redireccions
✖ no és el motiu.

D) scripts
✖ cron pot executar qualsevol comanda.

---

# 7. Auditoria de tasques

**Resposta correcta**

**B)** Que tinguin logs verificables

Document
`contingut.md`

Frase:

> “Les tasques automatitzades han de generar logs per poder verificar l’execució.”

---

Incorrectes

A) rapidesa
✖ no és criteri auditoria.

C) root
✖ és risc.

D) GUI
✖ irrellevant.

---

# 8. Error cada minut

**Resposta correcta**

**C)** Pot repetir-se i escalar

Document
`slidesCron.md`

Frase:

> “Una tasca incorrecta en cron pot repetir-se constantment i amplificar l’error.”

---

Incorrectes

A) cap
✖ error repetit.

B) una vegada
✖ cron repeteix.

D) bloqueja kernel
✖ no necessàriament.

---

# 9. Webmin cron

**Resposta correcta**

**B)** Escriu al crontab

Document
`tasca03Manual.md`

Frase:

> “Webmin crea o modifica les entrades del crontab.”

---

Incorrectes

A) executa directament
✖ cron és qui executa.

C) base de dades Webmin
✖ utilitza configuració sistema.

D) només quan administrador està connectat
✖ cron funciona independentment.

---

# 10. Tasques at pendents

**Resposta correcta**

**B)** atq

Document
`slides.md`

Frase:

> “La comanda atq permet veure les tasques programades amb at.”

---

Incorrectes

A) crontab -l
✖ només cron.

C) ps aux
✖ processos.

D) systemctl status at
✖ estat del servei.

---

# PART PRÀCTICA

---

# A) Automatització periòdica

### Comanda

```bash
*/10 * * * * /bin/df -h >> /home/sysadmin/logs/espai_disc.log 2>&1
```

---

### Per què es redirigeixen errors

Per capturar errors en el mateix log.

Document
`tasca02Manual.md`

Frase:

> “La redirecció 2>&1 permet guardar errors i sortida al mateix fitxer.”

---

### Per què generar logs

Per auditoria i verificació.

Document
`contingut.md`

Frase:

> “Els logs permeten verificar l’execució de tasques automatitzades.”

---

### Risc privilegis elevats

Pot executar accions amb impacte global.

Document
`slides.md`

Frase:

> “Executar tasques amb privilegis elevats pot incrementar el risc de seguretat.”

---

# B) Tasca puntual amb at

### Comanda

```bash
echo "/bin/ps aux >> /home/sysadmin/logs/processos_snapshot.log 2>&1" | at now + 2 minutes
```

---

### Justificació

Document
`slides.md`

Frase:

> “La comanda at permet programar una tasca puntual en el futur.”

Document
`tasca03Manual.md`

Frase:

> “També es pot utilitzar at amb pipe per evitar el mode interactiu.”

---

# C) Verificació

---

### 1. Comprovar cron

```bash
crontab -l
```

Document
`slidesCron.md`

Frase:

> “crontab -l mostra les tasques cron de l’usuari.”

---

### 2. Tasques at

```bash
atq
```

Document
`slides.md`

Frase:

> “atq permet veure tasques pendents.”

---

### 3. Verificar execució

```bash
cat /home/sysadmin/logs/espai_disc.log
```

o

```bash
less /home/sysadmin/logs/espai_disc.log
```

Document
`tasca02Manual.md`

Frase:

> “Els logs generats permeten verificar l’execució de la tasca.”


