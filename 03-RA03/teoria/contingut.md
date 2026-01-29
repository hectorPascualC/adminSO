# RA03 - Automatització de tasques del sistema

## 3.0 — Introducció a la RA03

### 3.0.1 On som dins del mòdul i per què existeix la RA03

En administració de sistemes, una gran part de la feina **no és crear coses noves**, sinó **repetir tasques**:

*   revisar sistemes
*   executar manteniments
*   aplicar canvis periòdics
*   garantir que certes accions es facin **sempre igual**

La **RA03** apareix per donar resposta a una pregunta molt concreta:

> _Com podem fer que el sistema faci per si sol tasques repetitives, de manera controlada i eficient, sense dependre sempre d’una persona?_

Aquesta RA **no va de programar**, va de **gestionar el sistema perquè treballi de forma automàtica**.


### 3.0.2 Què entenem per “automatitzar” en RA03

En el context de la RA03, **automatitzar** vol dir:

> Configurar el sistema perquè **executi ordres o processos en un moment determinat**, sense intervenció manual directa, utilitzant **eines pròpies del sistema operatiu**.

📌 Punt clau (Estrategias):  
Automatitzar **no és** crear lògica complexa, ni programar comportaments.  
Això vindrà **més endavant (RA07)**.


### 3.0.3 Automatitzar ≠ programar (frontera conceptual important)

A classe, aquesta és **la confusió més habitual**, així que ho deixem clar des del principi.

![https://www.researchgate.net/publication/3421718/figure/fig1/AS%3A394703557152775%401471116069563/Structure-of-a-typical-industrial-automation-system.png](https://www.researchgate.net/publication/3421718/figure/fig1/AS%3A394703557152775%401471116069563/Structure-of-a-typical-industrial-automation-system.png)

![https://www.craftsmensoftware.com/wp-content/uploads/2024/04/1_hNBda1K1ZwtxUaCBulgGpw.webp](https://www.craftsmensoftware.com/wp-content/uploads/2024/04/1_hNBda1K1ZwtxUaCBulgGpw.webp)

![https://www.slideteam.net/media/catalog/product/cache/1280x720/i/m/implementing_automation_vs_manual_process_in_organization_slide01.jpg](https://www.slideteam.net/media/catalog/product/cache/1280x720/i/m/implementing_automation_vs_manual_process_in_organization_slide01.jpg)

### Exemple conceptual

**Situació real:**  
Cada dia a les 02:00 del matí s’ha de:

*   executar una ordre
*   fer una tasca de manteniment
*   generar un registre

**Opció A — Manual**  
Un administrador entra cada nit i escriu la comanda.

**Opció B — Automatitzada (RA03)**  
El sistema té configurat:

*   **què** ha de fer
*   **quan** ho ha de fer
*   **amb quins permisos**

👉 No hi ha cap programa nou, només **planificació**.


### 3.0.4 Exemple molt simple (sense entrar encara en detall)

Encara **no estem explicant cron**, només el concepte.

```text
Cada dia a les 02:00
→ Executa aquesta ordre
→ Sense que ningú estigui davant de l’ordinador
```

Aquest tipus de configuració és **exactament** el que treballa la RA03.

No ens interessa encara:

*   com s’escriu la línia exacta
*   quina comanda concreta és
*   si hi ha bucles o condicions

Això vindrà després, **pas a pas**.


### 3.0.5 Per què aquesta RA és clau per a un administrador de sistemes

Des del punt de vista professional, la RA03 resol problemes reals:

1.  **Regularitat**  
    La tasca s’executa sempre, encara que ningú hi pensi.
2.  **Fiabilitat**  
    No depèn del cansament, de l’oblit o de torns.
3.  **Eficiència**  
    El temps de l’administrador es pot dedicar a tasques de més valor.
4.  **Traçabilitat**  
    Les tasques automatitzades es poden documentar i revisar.

Aquests quatre punts apareixeran **de forma recurrent** durant tota la RA.

### 0.6 Relació amb altres RAs (sense barrejar-les)

Per situar-nos bé dins del mòdul:

*   **RA03**  
    → _Quan_ i _com_ el sistema executa tasques de manera automàtica.
*   **RA07**  
    → _Què_ fa exactament el sistema mitjançant guions i lògica.

A la RA03:

*   treballem **planificació**
*   treballem **gestió**
*   treballem **documentació**

📌 Aquesta separació no és casual:  
és **exactament** el que demana el currículum.




## 1 — Avantatges de l’automatització de tasques
=====================================================

**(RA03 · Punt 3.1)**

* * *

1.1 El problema real: la repetició en administració de sistemes
---------------------------------------------------------------

En un sistema informàtic, moltes tasques **no són difícils**, però són:

*   repetitives
*   freqüents
*   crítiques si s’obliden

El problema no és **saber fer-les**, sinó **fer-les sempre**, en el moment correcte.

![https://www.claysys.com/app/uploads/2023/01/Automation-Of-Tasks.png](https://www.claysys.com/app/uploads/2023/01/Automation-Of-Tasks.png)

![https://www.projectmanager.com/wp-content/uploads/2019/08/Server-Maintenance-Screenshot.jpg](https://www.projectmanager.com/wp-content/uploads/2019/08/Server-Maintenance-Screenshot.jpg)

![https://cdn.prod.website-files.com/64366cea971df61149ca1025/654b5c4a184e6c606f1032e1_Maintenance-workflow-diagram.jpg](https://cdn.prod.website-files.com/64366cea971df61149ca1025/654b5c4a184e6c606f1032e1_Maintenance-workflow-diagram.jpg)

### Exemple real d’aula / empresa

Un administrador ha de:

*   comprovar l’estat del sistema
*   fer neteja periòdica
*   executar ordres de manteniment

Si això es fa **manualment**, depèn de:

*   la memòria de la persona
*   el temps disponible
*   la càrrega de feina del dia

Aquí és on entra l’automatització.

* * *

1.2 Què aporta realment automatitzar (no teoria, pràctica)
----------------------------------------------------------

Quan automatitzem una tasca del sistema, **no estem fent res nou**:  
estem decidint **quan** i **com** s’executa una ordre que ja existeix.

La diferència és el **resultat final**.

![https://www.researchgate.net/publication/338359641/figure/fig1/AS%3A873279363493890%401585217433372/Manual-vs-Automated-processes.png](https://www.researchgate.net/publication/338359641/figure/fig1/AS%3A873279363493890%401585217433372/Manual-vs-Automated-processes.png)

![https://www.cetdigit.com/hs-fs/hubfs/FIg%201%2C%20The%20Benefitsof%20Digital%20Process%20Automation.png?height=5029&name=FIg+1%2C+The+Benefitsof+Digital+Process+Automation.png&width=5332](https://www.cetdigit.com/hs-fs/hubfs/FIg%201%2C%20The%20Benefitsof%20Digital%20Process%20Automation.png?height=5029&name=FIg+1%2C+The+Benefitsof+Digital+Process+Automation.png&width=5332)

![https://cdn.prod.website-files.com/6345a30a1a28da441e842abc/68b69bb1217d054cf8893803_table%20practice%20%281%29.png](https://cdn.prod.website-files.com/6345a30a1a28da441e842abc/68b69bb1217d054cf8893803_table%20practice%20%281%29.png)

### Comparació conceptual

**Abans (manual):**

*   algú entra al sistema
*   executa una ordre
*   ho fa quan se’n recorda

**Després (automatitzat):**

*   el sistema executa l’ordre
*   sempre a la mateixa hora
*   sense dependre de ningú

* * *

1.3 Avantatge 1: regularitat
----------------------------

La regularitat és el primer gran avantatge.

Una tasca automatitzada:

*   s’executa **sempre**
*   a l’hora prevista
*   amb el mateix comportament

Això és especialment important en tasques de:

*   manteniment
*   comprovacions periòdiques
*   rutines del sistema

📌 **Idea clau per l’alumne:**  
Un sistema regular és **més estable** que un sistema depenent de persones.

* * *

1.4 Avantatge 2: reducció d’errors humans
-----------------------------------------

Els errors humans no solen ser errors tècnics, sinó:

*   oblit
*   presses
*   cansament
*   confusió de comandes

![https://kodexolabs.com/wp-content/uploads/2025/07/How-Does-AI-Reduce-Human-Error-1024x745.webp](https://kodexolabs.com/wp-content/uploads/2025/07/How-Does-AI-Reduce-Human-Error-1024x745.webp)

![https://help.autodesk.com/sfdcarticles/img/0EM3A0000008EJe](https://help.autodesk.com/sfdcarticles/img/0EM3A0000008EJe)

![https://kodexolabs.com/wp-content/uploads/2025/07/Key-Technologies-Reducing-Human-Error.webp](https://kodexolabs.com/wp-content/uploads/2025/07/Key-Technologies-Reducing-Human-Error.webp)

Quan una tasca està automatitzada:

*   no s’escriu malament la comanda
*   no s’executa dues vegades
*   no s’executa fora d’horari

El sistema **no s’equivoca**, simplement fa el que té configurat.

* * *

1.5 Avantatge 3: estalvi de temps (el temps de l’admin és limitat)
------------------------------------------------------------------

Un administrador de sistemes **no hauria d’estar fent tasques mecàniques**.

Cada tasca automatitzada:

*   allibera temps
*   permet dedicar-se a incidències reals
*   millora la gestió global del sistema

### Exemple senzill

Si una tasca dura:

*   2 minuts
*   però s’ha de fer cada dia

En un mes:

*   són més d’1 hora de feina repetitiva

Automatitzar-la és una decisió **professional**, no mandra.

* * *

1.6 Avantatge 4: control i traçabilitat
---------------------------------------

Una tasca automatitzada:

*   es pot **documentar**
*   es pot **revisar**
*   es pot **desactivar**
*   es pot **modificar**

![https://static-docs.nocobase.com/task_management20241106949.drawio.svg](https://static-docs.nocobase.com/task_management20241106949.drawio.svg)

![https://cdn.sanity.io/images/rdn92ihu/production/db61fcea7cb5fea3e3168cecdbc8caa28f5588ed-2086x593.png?auto=format&fit=max](https://cdn.sanity.io/images/rdn92ihu/production/db61fcea7cb5fea3e3168cecdbc8caa28f5588ed-2086x593.png?auto=format&fit=max)

![https://d2ds8yldqp7gxv.cloudfront.net/Blog%2BExplanatory%2BImages/PROJECT%2BSCHEDULINGb.jpg](https://images.openai.com/thumbnails/url/NC9np3icu5mVUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw5K9cpwTa8y0i2MtzCt9PVNMfc3c_MOMKxILs0sTC5OTDfxjnS2SErPCygt9_EocCkOVysGAHyVJpc)

Això permet saber:

*   què fa el sistema
*   quan ho fa
*   per què ho fa

Aquest punt és clau en:

*   entorns professionals
*   treball en equip
*   auditories

* * *

1.7 Exemple conceptual amb pseudo-configuració
----------------------------------------------

Encara **sense entrar en sintaxi real**, pensem-ho així:

```text
Tasca: manteniment del sistema
Freqüència: cada dia
Hora: 02:00
Execució: automàtica
Responsable: sistema
```

Això **ja és automatització**, encara que no hàgim vist cap comanda concreta.

* * *

1.8 Connexió directa amb el criteri d’avaluació 3.1
---------------------------------------------------

Tot el que hem vist en aquesta fase respon exactament al criteri:

> **3.1** Descriu els avantatges de l’automatització de les tasques repetitives en el sistema.

L’alumne ha de ser capaç de:

*   explicar **per què** s’automatitza
*   justificar **quan té sentit**
*   entendre **què millora respecte al treball manual**

Encara **no**:

*   configurar eines
*   escriure ordres
*   programar res

I això és **correcte**.

* * *

### ✅ Tancament de la FASE 1



🧭 FASE 2 — Planificació de tasques del sistema
===============================================

**(RA03 · Punts 3.2 i 3.4)**

* * *

2.1 Què vol dir “planificar” una tasca
--------------------------------------

Planificar una tasca **no és programar** el que fa el sistema, sinó decidir:

1.  **Quan** s’ha d’executar
2.  **Amb quina freqüència**
3.  **Amb quins permisos**

És a dir, el sistema ja sap **què** ha de fer (una ordre existent),  
nosaltres li diem **quan** ho ha de fer.

![https://cdn.vertex42.com/ExcelTemplates/Images/project-timeline-template-with-milestones.png](https://cdn.vertex42.com/ExcelTemplates/Images/project-timeline-template-with-milestones.png)

![https://media.geeksforgeeks.org/wp-content/uploads/20250920114635603424/seven_state.webp](https://media.geeksforgeeks.org/wp-content/uploads/20250920114635603424/seven_state.webp)

![https://raw.github.com/wiki/takumakanari/cronv/images/outputs/cronv-1d.png](https://raw.github.com/wiki/takumakanari/cronv/images/outputs/cronv-1d.png)

* * *

2.2 Tipus de tasques segons el temps
------------------------------------

A la RA03 treballem **dos grans tipus de planificació**.

### 2.2.1 Tasques repetitives

Són tasques que:

*   s’executen de manera regular
*   segueixen un patró de temps
*   formen part del manteniment habitual del sistema

Exemples reals:

*   cada dia
*   cada setmana
*   cada mes
*   cada dilluns a una hora concreta

Aquest tipus de tasques es planifiquen amb **eines de planificació periòdica**.

* * *

### 2.2.2 Tasques puntuals

Són tasques que:

*   s’executen **una sola vegada**
*   en un moment concret del futur
*   no es tornen a repetir

Exemples reals:

*   executar una ordre aquesta nit
*   fer una acció un dia concret
*   llançar una tasca fora d’horari laboral

Aquest tipus de tasques es planifiquen amb **eines de planificació puntual**.

![https://i.sstatic.net/bawml.png](https://images.openai.com/thumbnails/url/8JcFyHicu5mVUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw70Mq2wdCn2MDAKd8kwCHdO9yrONfOLCjT2SfUNzM4oS6sMiyr3DCrNdTHMzyhPKo-PVCsGAG6LJpg)

![https://marcgg.com/assets/blog/automation-win.png](https://marcgg.com/assets/blog/automation-win.png)

![https://media.geeksforgeeks.org/wp-content/uploads/20250920114635603424/seven_state.webp](https://media.geeksforgeeks.org/wp-content/uploads/20250920114635603424/seven_state.webp)

* * *

2.3 Planificació de tasques repetitives (visió conceptual)
----------------------------------------------------------

Abans de veure cap comanda, pensem-ho de manera lògica:

```text
Tasca: manteniment
Freqüència: cada dia
Hora: 02:00
```

El sistema interpreta:

*   que la tasca **s’ha de repetir**
*   sempre en el mateix horari
*   sense intervenció humana

Això és exactament el que resol la planificació periòdica.

* * *

2.4 Primera aproximació a la sintaxi (sense memoritzar)
-------------------------------------------------------

Ara sí, veiem **com es representa això** en un sistema Linux,  
però **sense entrar encara en detalls tècnics**.

```text
minut hora dia_mes mes dia_setmana ordre
```

No cal memoritzar-ho ara.  
L’objectiu és **entendre que el temps es descriu amb camps**.

![https://linuxhandbook.com/content/images/2020/06/crontab-explanation.png](https://linuxhandbook.com/content/images/2020/06/crontab-explanation.png)

![https://tecadmin.net/wp-content/uploads/2013/03/crontab-2.png](https://tecadmin.net/wp-content/uploads/2013/03/crontab-2.png)

![https://ahmadawais.com/wp-content/uploads/2017/06/crontab.png](https://ahmadawais.com/wp-content/uploads/2017/06/crontab.png)

* * *

2.5 Exemple senzill de tasca repetitiva
---------------------------------------

Suposem una tasca molt simple:

> Executar una ordre **cada dia a les 2 del matí**

```text
0 2 * * * ordre
```

📌 Què ens interessa aquí (no la sintaxi exacta):

*   hi ha una **hora**
*   hi ha una **freqüència**
*   hi ha una **ordre associada**

Això **no és programació**, és **configuració temporal**.

* * *

2.6 Planificació de tasques puntuals (visió conceptual)
-------------------------------------------------------

Ara pensem en una altra situació:

> “Executa aquesta ordre **una sola vegada**, aquesta nit”

Aquí **no volem repetició**.

![https://www.computernetworkingnotes.com/wp-content/uploads/linux-tutorials/images/lt133-10-command-with-option.png](https://www.computernetworkingnotes.com/wp-content/uploads/linux-tutorials/images/lt133-10-command-with-option.png)

![https://tecadmin.net/wp-content/uploads/2013/03/linux-at-command.jpg](https://tecadmin.net/wp-content/uploads/2013/03/linux-at-command.jpg)

![https://docs.temporal.io/diagrams/temporal-cron-job.svg](https://docs.temporal.io/diagrams/temporal-cron-job.svg)

Conceptualment seria:

```text
Data: avui
Hora: 22:00
Acció: executar una ordre
```

El sistema guarda aquesta informació  
i executa la tasca **quan toca**, només una vegada.

* * *

2.7 Exemple conceptual de tasca puntual
---------------------------------------

Sense entrar encara en detalls:

```text
Executa aquesta ordre a les 22:00 d’avui
```

El que és important que l’alumne entengui:

*   no hi ha repetició
*   la tasca desapareix després d’executar-se
*   és útil per accions excepcionals

* * *


2.8 Diferència clau entre repetitiva i puntual
----------------------------------------------

Aquest punt és **fonamental per a l’examen i la pràctica**.

![https://sitedrive.com/hs-fs/hubfs/problems%20and%20solutions%20table.png?height=700&name=problems+and+solutions+table.png&width=1200](https://sitedrive.com/hs-fs/hubfs/problems%20and%20solutions%20table.png?height=700&name=problems+and+solutions+table.png&width=1200)

![https://media2.dev.to/dynamic/image/width%3D1280%2Cheight%3D720%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3hgfujf28hkns84t6y74.png](https://media2.dev.to/dynamic/image/width%3D1280%2Cheight%3D720%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3hgfujf28hkns84t6y74.png)

![https://www.researchgate.net/publication/320629966/figure/tbl1/AS%3A725141252227075%401549898555273/Comparison-of-various-scheduling-algorithms.png](https://www.researchgate.net/publication/320629966/figure/tbl1/AS%3A725141252227075%401549898555273/Comparison-of-various-scheduling-algorithms.png)

| Aspecte | Repetitiva | Puntual |
| --- | --- | --- |
| S’executa | Moltes vegades | Una sola vegada |
| Patró de temps | Sí | No |
| Ús habitual | Manteniment | Accions puntuals |
| Intervenció humana | No | No |

* * *

2.9 Connexió directa amb els criteris d’avaluació
-------------------------------------------------

Tot el que hem vist fins ara cobreix directament:

*   **3.2** Utilitza comandes del sistema per a la planificació de tasques
*   **3.4** Realitza planificacions de tasques repetitives o puntuals

📌 En aquest punt, l’alumne ha de ser capaç de:

*   diferenciar tipus de tasques
*   entendre quan usar cada eina
*   interpretar una planificació ja existent

Encara **no**:

*   escriure scripts
*   afegir lògica
*   combinar ordres complexes

* * *

### ✅ Tancament de la FASE 2


🧭 FASE 3 — Restriccions i criteris de seguretat
================================================

**(RA03 · Punt 3.3)**

* * *

3.1 Per què la seguretat és clau quan automatitzem
--------------------------------------------------

Automatitzar una tasca vol dir **donar-li poder al sistema** perquè executi ordres **sense supervisió humana directa**.  
Això té un avantatge clar (eficiència), però també **un risc**.

> _Una tasca automatitzada mal configurada pot fer danys de manera automàtica._

![https://media.excellentwebworld.com/wp-content/uploads/2025/09/03052958/steps-to-develop-a-cybersecurity-risk-management-plan.webp](https://media.excellentwebworld.com/wp-content/uploads/2025/09/03052958/steps-to-develop-a-cybersecurity-risk-management-plan.webp)

![https://ztalk-admin.zybisys.com/assets/blogs/images/b4fe6f32-5f43-49e0-8a29-2ba3446599a2.png](https://ztalk-admin.zybisys.com/assets/blogs/images/b4fe6f32-5f43-49e0-8a29-2ba3446599a2.png)

![https://www.riskinsight-wavestone.com/wp-content/uploads/2019/07/image1.png](https://www.riskinsight-wavestone.com/wp-content/uploads/2019/07/image1.png)

Per això, abans de crear moltes tasques automàtiques, cal respondre a una pregunta bàsica:

**Qui pot automatitzar què, i amb quins límits?**

* * *

3.2 El principi de mínim privilegi (idea clau)
----------------------------------------------

Un dels principis bàsics en seguretat és el **principi de mínim privilegi**:

> Cada usuari o procés ha de tenir **només els permisos imprescindibles** per fer la seva feina.

Aplicat a l’automatització:

*   no totes les tasques han de tenir permisos elevats
*   no tots els usuaris han de poder planificar tasques
*   una tasca automatitzada **no hauria de fer més del necessari**

![https://delinea.com/hs-fs/hubfs/Delinea/blog-images/In-Post%20Graphic/delinea-blog-least-privilege-example-golden-image.jpg?height=415&name=delinea-blog-least-privilege-example-golden-image.jpg&width=700](https://delinea.com/hs-fs/hubfs/Delinea/blog-images/In-Post%20Graphic/delinea-blog-least-privilege-example-golden-image.jpg?height=415&name=delinea-blog-least-privilege-example-golden-image.jpg&width=700)

![https://www.tools4ever.com/wp-content/uploads/2023/01/What-Is-POLP-Infographic-1.png.webp](https://www.tools4ever.com/wp-content/uploads/2023/01/What-Is-POLP-Infographic-1.png.webp)

![https://www.gooddata.com/docs/permissions/permission-hierarchy-example-1.webp](https://www.gooddata.com/docs/permissions/permission-hierarchy-example-1.webp)

Aquest principi apareixerà sovint en entorns reals i auditories.

* * *

3.3 Qui pot planificar tasques en un sistema
--------------------------------------------

En un sistema multiusuari:

*   hi ha **usuaris normals**
*   hi ha **usuaris administradors**
*   hi ha **serveis del sistema**

No tots han de tenir dret a:

*   crear tasques automàtiques
*   modificar tasques existents
*   executar ordres sensibles de manera programada

Això evita:

*   abusos
*   errors greus
*   problemes de seguretat

* * *

3.4 Control d’accés a la planificació
-------------------------------------

Els sistemes operatius disposen de **mecanismes de control** per decidir:

*   quins usuaris poden planificar tasques
*   quins no

![https://www.computernetworkingnotes.com/wp-content/uploads/linux-tutorials/images/lt134-07-cron-execution-process.png](https://www.computernetworkingnotes.com/wp-content/uploads/linux-tutorials/images/lt134-07-cron-execution-process.png)

![https://learn-attachment.microsoft.com/api/attachments/168082-image.png?platform=QnA](https://learn-attachment.microsoft.com/api/attachments/168082-image.png?platform=QnA)

![https://doc.igrafx.com/__attachments/50070338/PermissionsDiagram.jpg?inst-v=ed65549a-1501-4b1c-85a2-887a08df34e1](https://doc.igrafx.com/__attachments/50070338/PermissionsDiagram.jpg?inst-v=ed65549a-1501-4b1c-85a2-887a08df34e1)

Conceptualment, el sistema treballa amb:

*   **llistes d’autorització**
*   **llistes de restricció**

No entrarem encara en la gestió concreta d’aquests fitxers;  
l’important és entendre **que existeixen** i **per a què serveixen**.

* * *

3.5 Riscos habituals d’una mala automatització
----------------------------------------------

Quan no s’aplica criteri de seguretat, poden aparèixer problemes com:

1.  Tasques que s’executen amb massa permisos
2.  Ordres automàtiques difícils de controlar
3.  Accions que afecten tot el sistema sense supervisió
4.  Dificultat per saber **qui va crear què**

![https://irp.cdn-website.com/35fcf6c5/dms3rep/multi/Automated%2BIncident%2BResponse_%2BWhat%2BIt%2BIs-%2BTools%2Band%2BUse%2BCases-8c69ab35.png](https://images.openai.com/thumbnails/url/G3JDXXicu5mZUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw70Li_Lqsgv8_YKNq_yMg6uijLzz0k3LPHSdfXy9vD3KC8MLs-2KA6JqEyu8AhxKkpWKwYAbP0muw)

![https://miro.medium.com/v2/resize%3Afit%3A1400/1%2A5v5Jxv1j-bfn5Xq9Oh2FAg.png](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2A5v5Jxv1j-bfn5Xq9Oh2FAg.png)

![https://media.licdn.com/dms/image/v2/D5612AQHU_PwUalXkwQ/article-cover_image-shrink_720_1280/B56ZeNK8POGQAM-/0/1750420146077?e=2147483647&t=tHqG3M0qqSIpuU1ppIqWLA_mIy0oFGpWmkxmhAyHmGo&v=beta](https://images.openai.com/thumbnails/url/Z7Ctknicu5mZUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw4O8i8rtAg1D4s3Ni4N9A6uys60TDFzcUtLDrCMKPBPLfNzKcpPds3w8HTLMzL1d01XKwYAU8YlvQ)

Aquests riscos **no són teòrics**: passen sovint en entorns reals.

* * *

3.6 Bones pràctiques bàsiques en automatització
-----------------------------------------------

Sense entrar encara en configuracions concretes, un administrador hauria de seguir criteris com:

1.  Automatitzar només el que cal
2.  Assignar permisos ajustats a cada tasca
3.  Evitar automatitzacions opaques o difícils d’entendre
4.  Mantenir control i visibilitat sobre les tasques creades

Aquestes pràctiques no depenen de l’eina, sinó del **criteri professional**.

* * *

3.7 Exemple conceptual d’automatització segura
----------------------------------------------

Pensem en dues situacions:

![https://cdn.brainmanager.io/mhsby0p2mye84iodc9ecgcyudsmf](https://cdn.brainmanager.io/mhsby0p2mye84iodc9ecgcyudsmf)

![https://www.datocms-assets.com/55802/1734707205-security-automation-best-practices-with-examples-1.png?auto=+compress%2Cformat&fit=max&q=90&w=1400](https://www.datocms-assets.com/55802/1734707205-security-automation-best-practices-with-examples-1.png?auto=+compress%2Cformat&fit=max&q=90&w=1400)

![https://files.codingninjas.in/article_images/privileged-and-non-privileged-instructions-1-1641925194.jpg](https://files.codingninjas.in/article_images/privileged-and-non-privileged-instructions-1-1641925194.jpg)

**Situació A (incorrecta):**

*   una tasca automatitzada
*   amb permisos elevats
*   sense justificació clara

**Situació B (correcta):**

*   una tasca automatitzada
*   amb permisos ajustats
*   documentada i controlada

La diferència **no és tècnica**, és **de criteri**.

* * *

3.8 Relació directa amb el criteri d’avaluació 3.3
--------------------------------------------------

Tot el que hem treballat en aquesta fase respon directament al criteri:

> **3.3** Estableix restriccions de seguretat.

L’alumne ha de ser capaç de:

*   explicar per què no tothom pot automatitzar
*   justificar l’ús de restriccions
*   detectar situacions d’automatització insegura

Encara **no** cal:

*   modificar fitxers de sistema
*   crear scripts
*   fer configuracions avançades

Això vindrà **quan toqui**.

* * *

### ✅ Tancament de la FASE 3



```text
0 23 * * * ordre_administrativa
```

📌 El que ens interessa:

*   s’executa a una hora concreta
*   afecta administració de comptes
*   no conté lògica

Això és **totalment RA03**.

* * *

4.5 Per què cal especial cura amb els comptes
---------------------------------------------

Els comptes d’usuari:

*   donen accés al sistema
*   tenen permisos
*   poden afectar la seguretat global

![https://cynomi.com/wp-content/uploads/2024/05/risk-assesment.png](https://cynomi.com/wp-content/uploads/2024/05/risk-assesment.png)

![https://delinea.com/hs-fs/hubfs/Delinea/diagrams/delinea-diagram-pam-lifecycle.jpg?height=592&name=delinea-diagram-pam-lifecycle.jpg&width=650](https://delinea.com/hs-fs/hubfs/Delinea/diagrams/delinea-diagram-pam-lifecycle.jpg?height=592&name=delinea-diagram-pam-lifecycle.jpg&width=650)

![https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/images/uac-windows-logon-process.gif](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/images/uac-windows-logon-process.gif)

Per això:

*   no totes les tasques han d’anar automatitzades
*   cal documentar molt bé què fa cada tasca
*   s’ha de controlar qui les crea

Aquest punt connecta directament amb la **FASE 3 (seguretat)**.

* * *

4.6 Automatització responsable (criteri professional)
-----------------------------------------------------

Automatitzar l’administració de comptes **no és fer-ho tot automàticament**, sinó:

1.  Decidir quines accions són repetitives
2.  Planificar-les amb horari controlat
3.  Executar-les amb permisos ajustats
4.  Documentar-les correctament

![https://www.researchgate.net/publication/351708436/figure/fig3/AS%3A1025359852085252%401621476247925/Block-diagram-responsible-for-the-process.png](https://www.researchgate.net/publication/351708436/figure/fig3/AS%3A1025359852085252%401621476247925/Block-diagram-responsible-for-the-process.png)

![https://www.netsuite.com/portal/assets/img/business-articles/accounting-software/infographic-ap-automation-best-practices.jpg](https://www.netsuite.com/portal/assets/img/business-articles/accounting-software/infographic-ap-automation-best-practices.jpg)

![https://www.servicenow.com/content/dam/servicenow-assets/public/en-us/images/ds-what-is-pages/what-do-system-administrators-do.png](https://www.servicenow.com/content/dam/servicenow-assets/public/en-us/images/ds-what-is-pages/what-do-system-administrators-do.png)

Aquest és el comportament esperat d’un **administrador professional**.

* * *

4.7 Relació directa amb el criteri d’avaluació 3.5
--------------------------------------------------

Tot el que hem vist aquí respon al criteri:

> **3.5** Automatitza l’administració de comptes.

L’alumne ha de ser capaç de:

*   explicar **com** es poden automatitzar accions sobre comptes
*   justificar **per què** es fan de manera planificada
*   distingir clarament automatització de programació

No cal:

*   crear scripts
*   fer lògica
*   automatitzar decisions complexes

* * *

### ✅ Tancament de la FASE 4


🧭 FASE 5 — Eines gràfiques per a la planificació de tasques
============================================================

**(RA03 · Punts 3.6 i 3.7)**

* * *

5.1 Per què existeixen eines gràfiques de planificació
------------------------------------------------------

Tot i que la línia d’ordres és molt potent, **no sempre és l’opció més adequada**, sobretot quan:

*   treballem amb equips grans
*   hi ha diferents administradors
*   cal mantenir una visió clara de les tasques
*   es necessita reduir errors humans

Les **eines gràfiques de planificació** permeten:

*   visualitzar les tasques existents
*   crear-ne de noves de manera guiada
*   gestionar-les sense memoritzar sintaxi

![https://i.sstatic.net/3gorh.png](https://images.openai.com/thumbnails/url/6iWg2Xicu5mZUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw4O9g_3zQwIr4oq8jWpKEnMsXA2Mchx9SzOc4kwTDF0T3MMda4KCjP2Dyo2yQoqLTVRKwYAVv0lyA)

![https://dmsiworks.com/wp-content/uploads/Graphical-Scheduler-002.png](https://dmsiworks.com/wp-content/uploads/Graphical-Scheduler-002.png)

![https://www.nirsoft.net/utils/taskschedulerview.png](https://www.nirsoft.net/utils/taskschedulerview.png)

Aquestes eines **no substitueixen** el coneixement del sistema,  
però **faciliten la gestió diària**.

* * *

5.2 Instal·lació d’eines gràfiques de planificació
--------------------------------------------------

En molts sistemes, les eines gràfiques:

*   ja venen instal·lades
*   o es poden afegir fàcilment

L’objectiu aquí **no és memoritzar paquets**, sinó entendre el procés general:

1.  El sistema permet instal·lar una eina gràfica
2.  Aquesta eina es connecta amb el sistema de planificació
3.  Mostra la informació de manera visual

![https://i.sstatic.net/XLuzu.png](https://images.openai.com/thumbnails/url/Dgbp1Xicu5mVUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw5MrQjLL0qqcov31PWuDHEyz7QwCo_QtTQKdjf2N9cNMvXyKslzzKks8avMyjdwyjJyVCsGAHGPJZQ)

![https://kb.dmsiworks.com/wp-content/uploads/Graphical-Scheduler-UG.png](https://kb.dmsiworks.com/wp-content/uploads/Graphical-Scheduler-UG.png)

![https://klariti.com/wp-content/uploads/2011/06/sys-admin-guide-template-word-1.gif](https://klariti.com/wp-content/uploads/2011/06/sys-admin-guide-template-word-1.gif)

📌 Punt clau:  
L’eina **no crea un sistema nou**, simplement **gestiona el que ja existeix**.

* * *

5.3 Què es pot fer amb una eina gràfica
---------------------------------------

Quan obrim una eina gràfica de planificació, normalment podem:

*   veure totes les tasques planificades
*   identificar l’usuari que les executa
*   comprovar horaris i freqüències
*   activar o desactivar tasques

![https://activedirectorypro.com/wp-content/uploads/2022/07/list-scheduled-tasks-gui-tool-3-1.webp](https://activedirectorypro.com/wp-content/uploads/2022/07/list-scheduled-tasks-gui-tool-3-1.webp)

![https://cdn.backup4all.com/images/kb/task-scheduler.webp](https://cdn.backup4all.com/images/kb/task-scheduler.webp)

![https://www.nirsoft.net/utils/taskschedulerview.png](https://www.nirsoft.net/utils/taskschedulerview.png)

Això aporta **control visual**, molt útil en entorns reals.

* * *

5.4 Creació d’una tasca amb eina gràfica (visió conceptual)
-----------------------------------------------------------

Crear una tasca amb una eina gràfica sol seguir un **assistents per passos**:

1.  Definir el nom de la tasca
2.  Indicar quan s’ha d’executar
3.  Assignar l’ordre o acció
4.  Confirmar permisos

![https://cdn.prod.website-files.com/5f16d69f1760cdba99c3ce6e/674475e8a14d14a48e3795ae_6740877d931ebd8625845b81_674084cae565a8a0a6eaafc8_06.png](https://cdn.prod.website-files.com/5f16d69f1760cdba99c3ce6e/674475e8a14d14a48e3795ae_6740877d931ebd8625845b81_674084cae565a8a0a6eaafc8_06.png)

![https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/media/task-scheduler-flow.png](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/media/task-scheduler-flow.png)

![https://www.digitalcitizen.life/wp-content/uploads/2020/10/task_wizard_2.png](https://images.openai.com/thumbnails/url/pj649Hicu5mZUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw5xjTLL13X1N3DzsnCNKHPWNTWNrIjy9_TJTIvPNXQyCPTLLyn3SdTNMHErTiqoKrNQKwYAL2YlMw)

Aquesta forma de treball:

*   redueix errors
*   ajuda a entendre el procés
*   és ideal per aprendre

* * *

5.5 Comparació: eina gràfica vs línia d’ordres
----------------------------------------------

Aquest punt és **molt habitual en exàmens** i pràctica.

![https://ipwithease.com/wp-content/uploads/2020/06/GUI-VS-CLI-TABLE-NEW.jpg](https://ipwithease.com/wp-content/uploads/2020/06/GUI-VS-CLI-TABLE-NEW.jpg)

![https://miro.medium.com/1%2AnH-dpx2KxM0SPEq5w9n8NQ.png](https://miro.medium.com/1%2AnH-dpx2KxM0SPEq5w9n8NQ.png)

![https://phoenixnap.com/kb/wp-content/uploads/2023/01/cli-vs-gui-the-differences.png](https://phoenixnap.com/kb/wp-content/uploads/2023/01/cli-vs-gui-the-differences.png)

| Aspecte | Línia d’ordres | Eina gràfica |
| --- | --- | --- |
| Rapidesa | Alta | Mitjana |
| Visualització | Baixa | Alta |
| Facilitat | Mitjana | Alta |
| Risc d’error | Major | Menor |
| Aprenentatge | Més exigent | Més guiat |

Cap opció és “millor” en absolut:  
depèn del context i de l’objectiu.

* * *

5.6 Gestió i manteniment de tasques existents
---------------------------------------------

Les eines gràfiques no només serveixen per crear tasques, sinó també per:

*   modificar horaris
*   revisar configuracions
*   desactivar tasques temporals
*   eliminar tasques obsoletes

![https://i.sstatic.net/OKdlO.png](https://images.openai.com/thumbnails/url/DkOVPnicu5mZUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw5xN83ODspOqUr0TowviPTKCQjwy0pONzdwMnHL8w2tCHINyagKKM_Idk4Mz_f29DRRKwYAaEomYQ)

![https://support.quest.com/kbarticleimages/ER/360003099472/pic_1.png](https://support.quest.com/kbarticleimages/ER/360003099472/pic_1.png)

![https://winaero.com/blog/wp-content/uploads/2021/02/Disable-Scheduled-Task-in-Windows-10.png](https://winaero.com/blog/wp-content/uploads/2021/02/Disable-Scheduled-Task-in-Windows-10.png)

Això és molt útil quan:

*   el sistema evoluciona
*   canvien les necessitats
*   hi ha rotació d’administradors

* * *

5.7 Relació directa amb els criteris d’avaluació
------------------------------------------------

Aquesta fase cobreix directament:

*   **3.6** Instal·la i configura eines gràfiques per a la planificació de tasques
*   **3.7** Utilitza eines gràfiques per a la planificació de tasques

L’alumne ha de demostrar que:

*   sap per a què serveixen
*   sap quan és millor usar-les
*   sap interpretar la informació que mostren

No cal:

*   saber programar
*   crear guions
*   afegir lògica complexa

* * *

### ✅ Tancament de la FASE 5


🧭 FASE 6 — Documentació de tasques automàtiques
================================================

**(RA03 · Punt 3.8)**

* * *

6.1 Per què documentar és part de l’automatització
--------------------------------------------------

Una tasca automatitzada **no és només una configuració tècnica**.  
És una decisió administrativa que:

*   afecta el sistema
*   pot tenir impacte en usuaris
*   pot durar anys

Per això, **si no està documentada**, és com si no existís.

![https://images.prismic.io/superpupertest/a756e200-5efc-4e62-991f-e887df6872c1_Importance%2B_im3.png?auto=compress%2Cformat&dpr=3](https://images.openai.com/thumbnails/url/Mlt8-Hicu5mVUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw4Oqcosd0_zLwpwLch19ggwzEqL0M3KNEh0ck3LdSoyc_FP9igsDvPWdUsNK45wyzKrUisGAIt2Jo0)

![https://windowsforum.com/attachments/windowsforum-festo-security-advisory-undocumented-remote-functions-threaten-industrial-automa-webp.123098/](https://windowsforum.com/attachments/windowsforum-festo-security-advisory-undocumented-remote-functions-threaten-industrial-automa-webp.123098/)

![https://cdn.prod.website-files.com/64101fe133b4a090721c3381/653f74d83b02deefdbbc4c84_2X6eh_cHnWYUJlmL-GgYOpuAL9y-eYOelFHNONFMjXsXHIQHVYFpj0LP2HCvhALWtowhJzJlNX2UUcPErRmToxhl5yIm9X1fgp8dziy3jx9hJiIlRM5GuZV0KtV-idYalEc5t6loBpyv6VmTlYttGq8.png](https://cdn.prod.website-files.com/64101fe133b4a090721c3381/653f74d83b02deefdbbc4c84_2X6eh_cHnWYUJlmL-GgYOpuAL9y-eYOelFHNONFMjXsXHIQHVYFpj0LP2HCvhALWtowhJzJlNX2UUcPErRmToxhl5yIm9X1fgp8dziy3jx9hJiIlRM5GuZV0KtV-idYalEc5t6loBpyv6VmTlYttGq8.png)

📌 Idea clau per a l’alumne:

> _Un sistema sense documentació és un sistema difícil de mantenir._

* * *

6.2 Què pot passar si no documentem
-----------------------------------

Vegem una situació molt habitual en entorns reals.

![https://gegeek.com/wp-content/uploads/2017/11/1-2-1024x447.jpg](https://gegeek.com/wp-content/uploads/2017/11/1-2-1024x447.jpg)

![https://www.alfredapp.com/help/kb/automation-task-not-found/automation-task-options.png](https://www.alfredapp.com/help/kb/automation-task-not-found/automation-task-options.png)

![https://attuneops.io/wp-content/uploads/2025/01/Must-have-System-Administrator-Tools.jpg](https://attuneops.io/wp-content/uploads/2025/01/Must-have-System-Administrator-Tools.jpg)

**Situació:**

*   Una tasca s’executa cada nit
*   Ningú recorda qui la va crear
*   Ningú sap exactament què fa
*   Ningú sap si encara és necessària

Això pot provocar:

*   errors difícils de diagnosticar
*   pèrdua de temps
*   riscos de seguretat
*   dependència d’una sola persona

Tot això **no és un problema tècnic**, és un problema de **manca de documentació**.

* * *

6.3 Què s’ha de documentar d’una tasca automàtica
-------------------------------------------------

Documentar **no vol dir escriure un manual llarg**, sinó deixar clara la informació essencial.

Una tasca automatitzada hauria de tenir documentat:

1.  **Què fa**
2.  **Quan s’executa**
3.  **Amb quin usuari o permisos**
4.  **Per què existeix**

![https://www.smartsheet.com/sites/default/files/2023-09/IC-Weekly-Task-Planner-Template-for-Microsoft-Word.png](https://www.smartsheet.com/sites/default/files/2023-09/IC-Weekly-Task-Planner-Template-for-Microsoft-Word.png)

![https://cdn.prod.website-files.com/6209ea9aee1f965d7fce7c19/683949e9b1e1153fbba65740_blog-wide-1-1.webp](https://cdn.prod.website-files.com/6209ea9aee1f965d7fce7c19/683949e9b1e1153fbba65740_blog-wide-1-1.webp)

![https://www.altexsoft.com/static/blog-post/2024/4/30a328c0-e526-44fc-a9a9-5777c5f90b28.jpg](https://www.altexsoft.com/static/blog-post/2024/4/30a328c0-e526-44fc-a9a9-5777c5f90b28.jpg)

Aquesta informació permet que qualsevol altre administrador:

*   entengui la tasca
*   la pugui revisar
*   decideixi si cal mantenir-la o eliminar-la

* * *

6.4 Exemple de documentació senzilla (model ASIX02)
---------------------------------------------------

Un exemple **realista i assumible** per nivell ASIX02 podria ser:

```text
Nom de la tasca: Manteniment de comptes
Descripció: Executa una ordre de manteniment relacionada amb comptes d’usuari
Freqüència: Diària
Hora: 02:00
Usuari d’execució: sistema
Motiu: Evitar acumulació de comptes inactius
```

📌 Aquest exemple:

*   és curt
*   és clar
*   no entra en detalls tècnics innecessaris
*   compleix perfectament el criteri RA03

* * *

6.5 Documentació i treball en equip
-----------------------------------

En entorns professionals:

*   no hi ha un sol administrador
*   hi ha torns
*   hi ha canvis de personal

![https://docsvault.com/wordpress/wp-content/uploads/2022/08/Document-Collaboration-Tools.png](https://docsvault.com/wordpress/wp-content/uploads/2022/08/Document-Collaboration-Tools.png)

![https://4030614.fs1.hubspotusercontent-na1.net/hubfs/4030614/documentation%20system%20roles.jpg](https://4030614.fs1.hubspotusercontent-na1.net/hubfs/4030614/documentation%20system%20roles.jpg)

![https://www.slingshotapp.io/wp-content/uploads/2021/09/4.Document-Management-System-Types.png](https://www.slingshotapp.io/wp-content/uploads/2021/09/4.Document-Management-System-Types.png)

La documentació permet:

*   continuïtat del servei
*   menys dependència d’una persona concreta
*   millor manteniment del sistema

És una **garantia de qualitat**.

* * *

6.6 Relació amb seguretat i auditoria
-------------------------------------

La documentació també és clau en:

*   auditories internes
*   revisions de seguretat
*   comprovacions de bones pràctiques

![https://wallstreetmojo-files.s3.ap-south-1.amazonaws.com/2023/02/Audit-Documentation.png](https://wallstreetmojo-files.s3.ap-south-1.amazonaws.com/2023/02/Audit-Documentation.png)

![https://www.manageengine.com/products/active-directory-audit/how-to/images/how-to-monitor-scheduled-tasks-in-windows-1.png](https://www.manageengine.com/products/active-directory-audit/how-to/images/how-to-monitor-scheduled-tasks-in-windows-1.png)

![https://images.prismic.io/secureframe-com/Z4cshpbqstJ99eBK_InternalSecurityAuditProcess.png?auto=format%2Ccompress](https://images.prismic.io/secureframe-com/Z4cshpbqstJ99eBK_InternalSecurityAuditProcess.png?auto=format%2Ccompress)

Una tasca documentada:

*   és justificable
*   és revisable
*   és defensable davant d’una auditoria

* * *

6.7 Relació directa amb el criteri d’avaluació 3.8
--------------------------------------------------

Tot el que hem vist en aquesta fase respon exactament al criteri:

> **3.8** Documenta els processos programats com a tasques automàtiques.

L’alumne ha de ser capaç de:

*   explicar **per què** cal documentar
*   saber **què** s’ha de documentar
*   fer una documentació clara i funcional

No cal:

*   eines complexes
*   formats corporatius avançats
*   documentació excessiva

* * *

6.8 Tancament global de la RA03
-------------------------------

Amb aquesta fase tanquem tot el recorregut de la RA03:

![https://cdn.educba.com/academy/wp-content/uploads/2020/01/automation-testing-life-cycle-1.jpg](https://cdn.educba.com/academy/wp-content/uploads/2020/01/automation-testing-life-cycle-1.jpg)

![https://www.cflowapps.co.uk/wp-content/uploads/2025/06/Best-Practices-for-an-End-to-End-Process-visual-selection-1.png](https://www.cflowapps.co.uk/wp-content/uploads/2025/06/Best-Practices-for-an-End-to-End-Process-visual-selection-1.png)

![https://testrigor.com/wp-content/uploads/2023/11/Process-automation-2.png](https://testrigor.com/wp-content/uploads/2023/11/Process-automation-2.png)

Hem vist:

*   **per què** automatitzar (FASE 1)
*   **com** planificar tasques (FASE 2)
*   **amb quins límits** de seguretat (FASE 3)
*   **què** automatitzar en comptes (FASE 4)
*   **amb quines eines** gestionar-ho (FASE 5)
*   **com documentar-ho** correctament (FASE 6)

Això és exactament el que demana la **RA03** segons la Generalitat.

* * *

### ✅ Tancament de la FASE 6 (i de la RA03)



