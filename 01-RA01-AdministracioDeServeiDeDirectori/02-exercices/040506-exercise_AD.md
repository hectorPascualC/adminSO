# **PRÀCTICA 4-5-6 - Active Directory (Windows Server)**

### **Objectius de la pràctica**

* Instal·lar i configurar un servidor Windows Server com a controlador de domini.
* Crear i organitzar l’estructura lògica del directori (OU, usuaris, grups i equips).
* Unir equips clients al domini i verificar el funcionament de l’autenticació centralitzada.
* Aplicar polítiques de grup bàsiques (GPO) per controlar opcions dels usuaris o equips.

---

# **Material necessari**

* 1 màquina virtual **Windows Server 2019 o 2022**
* 1 màquina virtual **Windows 10 o 11** com a client
* Connexió virtual en mode **Internal Network** o **Host-Only**
* ISO corresponent de Windows Server i Windows Client


# **Enunciat**

Durant aquesta pràctica hauràs de desplegar un entorn funcional de **Directori Actiu (Active Directory Domain Services)** i realitzar administració bàsica del domini. El resultat final ha de ser una infraestructura capaç d’autenticar usuaris, gestionar permisos i aplicar polítiques a través del domini


## **1. Instal·lació del rol AD DS**

Instal·la i configura els serveis de domini d’Active Directory al servidor Windows Server.
Un cop instal·lat el rol, promociona el servidor a **controlador de domini** creant un **nou bosc** i un **nou domini**

Defineix el nom del domini segons les indicacions del professor o utilitza un nom de domini propi.

## **2. Creació de l'estructura d’Organizational Units (OU)**

Un cop creat el domini, dissenya una estructura mínima d’OU que contingui:

* Una OU per als **professors**
* Una OU per als **alumnes**
* Una OU per als **equips**
* Una OU per als **grups del domini**

Crea aquestes OU dins del domini i deixa-les preparades per a les següents tasques


## **3. Creació d’usuaris del domini**

A la OU corresponent, crea almenys **dos usuaris** (un de professors i un d’alumnes).
Assigna'ls una contrasenya inicial i configura la política perquè **hagin de canviar-la en el primer inici de sessió**

Tots els usuaris creats hauran d’autenticar-se posteriorment des del client Windows


## **4. Creació de grups del domini**

Dins de l'OU destinada als grups, crea almenys dos grups de **tipus seguretat** i de **àmbit global**:

* Un grup per als professors
* Un grup per als alumnes

Afegeix cada usuari al grup que li correspongui.


## **5. Unió d’un equip Windows al domini**

Configura la màquina virtual del client Windows perquè:

1. Rebi la configuració de xarxa adequada
2. Reconegui el servidor Windows Server com a DNS
3. S’uneixi al domini creat

Un cop afegit l'equip al domini, comprova que apareix correctament dins de l'OU d’equips (o mou-lo manualment si cal)


## **6. Verificació del funcionament del Directori Actiu**

Realitza les següents comprovacions:

* Inicia sessió al client Windows amb un usuari del domini.
* Comprova que la contrasenya pot ser canviada.
* Verifica que el servidor AD rep correctament els registres de l’equip.

Inclou captures de pantalla de totes les validacions.


## **7. Aplicació d’una Política de Grup (GPO)**

Crea i aplica una GPO al domini o a una OU concreta
L’objectiu de la GPO serà:

Que **elimini les opcions d’Apagar, Reiniciar, Suspendre i Hibernar del menú Inicio** en tots els equips del domini (o només en els que tu decideixis).

A fer:

* Crear la GPO
* Editar-la amb la configuració corresponent
* Enllaçar-la a l’àmbit correcte
* Validar que s’aplica des del client Windows


## **8. Documentació i entrega**

El lliurament de la pràctica ha d’incloure:

* Explicació resumida de cada tasca realitzada
* Captures de pantalla de totes les configuracions i validacions
* Respostes a les preguntes d’anàlisi que indiqui el professor
* Estructura final del domini i OU
* Verificació del funcionament de la GPO

