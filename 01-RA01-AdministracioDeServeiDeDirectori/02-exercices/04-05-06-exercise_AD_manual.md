# 📝 **PRÀCTICA 4,5,6 - Active Directory (Windows Server)**

## 🔹**Objectius**

1. Instal·lar i promocionar un servidor AD DS
2. Configurar un domini funcional
3. Crear usuaris, grups i equips dins del domini
4. Fer que un client Windows s'autentiqui correctament al domini
5. Administrar polítiques bàsiques (GPO)


## 🔹**Infraestructura necessària**

* **1 MV Windows Server 2019/2022**
* **1 MV Windows 10/11 client**
* VB connectat a xarxa interna

Al client es recomana instal.lar:
* Windows 10/11 Pro
* Windows 10/11 Enterprise
* Windows 10/11 Education

Si no tens d'aquestes i tens la versió W11 Home, segueix aquesta guia per un cop instal.lat fer l'[upgrade a Pro](./040506-exercise_AD_manual_upgradeW11HomeToW11Pro.md)


## 🔸**1. Instal·lació del AD**

1. Instal·la Windows Server i configura
   - Clau de producte → clicka a **No tengo clave de producto**
   - Seleccionar SO  → clicka a **Windows Server 2022 Standard (Experiencia escritorio)**
   - Qué tipo de instalación quiere? → **Personalizar**
2. Un cop dintre de WS, a la pantalla d'escriptori, en el body, a Administrador del servidor → **Agregar roles y características**
3. Tipo de instalación → **Instalación basada en características o roles**
4. Selección de servidor → **Seleccionar un servidor del grupo de servidores** → Selecciona el teu servidor (ja està per defecte)
5. Roles de servidor → **Servicios de dominio de AD**
6. ¿Desea agregar las características requeridas...? → clicla a **Agregar características**
7. Características → **Siguiente**
8. AD DS → **Siguiente**
9. Confirmación 
   → clicka a **Reiniciar automáticamente**
   → clicka a **Instalar**
10. Un cop instal.lat *Servicios de dominio de Active Directory (AD DS)*, just a sota clicka a **Promociona este servidor a controlador de dominio**:
   * Nou bosc
   * Domini: `iestorreroja.local`
   * Nivell funcional: per defecte
   * Escriu una password per restauració del AD
11. Opciones de DNS: surtirà el missatge *No sepuede crear una delegación para este servidor DNS...* → Dixeu-lo tot tal i com està, no feu check a res, només clickeu a **Siguiente**
12. Opciones adicionales: hauria de sortir com a nom NetBIOS `IESTORREROJA`
13. Accepta ubicacions predefinides per:
   * Base de dades
   * Logs
   * SYSVOL
14. Revisar opciones → **Siguiente**
15. Comprobación de requisitos previos → **Instalar**

**Resultat esperat:** El servidor reinicia i ja forma el domini `iestorreroja.local`


## 🔸**2. Crear l'estructura d'OU i comptes**

### 2.1 Crear OUs
1. Herramientas → **Usuarios y Equipos de Active Directory**  
2. Obre el domini `iestorreroja.local`
3. Botó dret → Nuevo → Unidad organizativa
4. Crea:
   ```
   Profesores
   Alumnos
   Equipos
   Grupos
   ```

### 2.2 Crear usuaris

1. Selecciona OU creada → **Profesores**
2. Botó dret ratolí → **Nuevo** → **Usuario**
3. Omple les dades:
   Nombre: Ana
   Apellidos: López
   Nombre completo: (s’autocompleta)
   Nombre de inicio de sesión de usuario (pre-Windows 2000): alopez
   Nombre de inicio de sesión de usuario (UPN): alopez@iestorreroja.local
   → **Siguiente**
4. Posa una contrasenya temporal
   Marca → **El usuario debe cambiar la contraseña en el siguiente inicio de sesión**
5. Mateix procediment però per a `jperez`

### 2.3 Crear grups
1. Selecciona OU creada → **Grupos**
2. Botó dret ratolí → **Nuevo** → **Grupo**
3. Omple dades per a:
   * **Profesores** (Seguridad, Global)
   * **Alumnos** (Seguridad, Global)
4. Assignar Ana Lópex al grup Profesores:
   * Ves a: OU = Profesores
   * Doble clic a alopez
   * Pestanya Member Of
   * Clica Agregar
   * Escriu: `Profesores`
   * Comprobar nombres
   * Aplicar
   * Aceptar
5. Mateix procediment però per a `jperez`

## 🔸**3. Unir un Client Windows al Domini**

1. Inicia Windows 10 o 11 **edició Pro** (imprescindible per unir-se a un domini)

2. Assigna una IP fixa dins de la mateixa xarxa que el servidor AD i configura el DNS amb la IP del controlador de domini (WS):

   Exemple recomanat:

   ```
   IP: 192.168.10.20
   Máscara: 255.255.255.0
   Gateway: (en blanc)
   DNS: 192.168.10.10
   ```

3. Comprova que tens connectivitat client → servidor amb:

   ```
   ping 192.168.10.10
   ```

   Si no funciona, revisa la configuració de xarxa amb l’annex tècnic:
   ➜ [Guia de configuració de xarxa per la pràctica](./040506-exercise_AD_manual_activeDirectoryInternalNetworkGuide.md)

4. Comprova la resolució DNS del domini:

   ```
   nslookup iestorreroja.local
   ```

   > ⚠️ Si mostra “Servidor: Unknown”, **no és cap error**: indica que no s’ha creat la zona inversa (no és necessària per la pràctica)

5. Uneix el client al domini:

   * **Configuración → Sistema → Información → Configuración avanzada del sistema**
   * Pestanya **Nombre de equipo**
   * Botó **Cambiar…**
   * Selecciona **Dominio** i escriu:

     ```
     iestorreroja.local
     ```

6. Introdueix credencials amb permisos al domini:

   ```
   Usuario: Administrador
   Contraseña: (la del servidor WS)
   ```

7. Reinicia quan ho demani.

8. A la pantalla d’inici de sessió, selecciona *Inicio de sesión en iestorreroja* i entra amb:

   ```
   iestorreroja\alopez
   ```

**Resultat esperat:**
L’usuari inicia sessió al domini i el client queda registrat a l’Active Directory.

**Verificació de l'unió del client al domini**
Executar:
   ```
      whoami
   ```

Esperat:
   ```
   iestorreroja\alopez
   ```

## 🔸**4. Aplicar una Política de Grup (GPO)**

Crear una GPO que **elimini les opcions d’Apagar, Reiniciar, Suspendre i Hibernar del menú Inicio** en tots els equips del domini (o només en els que tu decideixis).

### **4.1 Accedir al Gestor de Polítiques de Grup (GPMC)**

1. A Windows Server, obre:

   ```
   Administrador del servidor → Herramientas → Administración de directivas de grupo
   ```
2. A l'arbre esquerre, desplega:

   ```
   Bosque → iestorreroja.local → Dominios → iestorreroja.local
   ```

---

### **4.2 Crear i vincular la nova GPO**

1. Clic dret sobre **iestorreroja.local**
2. Selecciona:

   ```
   Crear un GPO en este dominio y vincularlo aquí…
   ```
3. Nom de la GPO:

   ```
   RestriccioEnergia
   ```
4. Fes clic a **Aceptar**.

Això crea la GPO i la vincula de forma automàtica al domini.

---

### **4.3 Editar la GPO**

1. Obre la consola de gestió de GPO:

   * **Administrador del servidor**
     → **Herramientas**
     → **Administración de directivas de grupo** (GPMC)
   * (Alternativa: **Win + R** → `gpmc.msc`)

2. Navega fins al domini:

   * **Bosque: iestorreroja.local**
     → **Dominios**
     → **iestorreroja.local**

3. A la llista de GPOs vinculades, fes clic dret sobre **RestriccioEnergia**
   ```
   Administración de directivas de grupo  
       → Bosque: iestorreroja.local
           → Dominios
               → iestorreroja.local
                   → RestriccioEnergia
   ```

4. Selecciona **Editar**

5. Al panell de l’editor de polítiques:

   * **Equipo**
     → **Plantillas administrativas**
     → **Menú Inicio y barra de tareas**

6. Localitza i habilita la política:

   * **Quitar y evitar el acceso a los comandos Apagar, Reiniciar, Suspender e Hibernar**

7. Aplica la política al terminal

   Servidor (WS):
   ```
   gpupdate /force
   ```

   Client (WC):
   ```
   gpupdate /force
   ```

   Pot ser necessari reiniciar el client per veure l’efecte.

8. Comrpovació visual esperada
* Entra al Windows 11 amb un usuari del domini (ex: alopez)
* Després mira el menú d’inici i botó d’alimentació:
   * NO apareixen les opcions: Apagar, Reiniciar, Suspender, Hibernar
   * Només hauria d’aparèixer:
      → Cerrar sesión
      → O cap opció d'alimentació
Això confirma que la GPO ha fet efecte

---

### **4.4 Comprovació final: verificar la GPO des del client Windows 11**

A la màquina client:

1. Inicia sessió com a **alopez** o qualsevol usuari del domini.
2. Executa:

   ```
   cmd
   gpupdate /force
   ```

3. Comprova que les opcions d'apagat **ja no apareixen** al menú Inicio

---

### **4.5 Comprovació final: pertinença al domini**

Des del client:

   ```
   whoami
   ```

Sortida esperada:

   ```
   iestorreroja\alopez
   ```

Des del servidor:

Obre:

   ```
   Usuarios y equipos de Active Directory → Equipos
   ```

L'equip **ha d'aparèixer** en aquesta OU.

---

### **4.6 Assignar la GPO només als equips (opcional, manual)**

Els equips del domini inicialment apareixen a la OU:

```
iesteroreroja.local → Computers
```

Aquest és el comportament per defecte d'AD

### 🔹 **Perquè la GPO afecti només als equips (i no als usuaris)?**

És recomanable moure els equips a l'OU **Equipos**, creada per aquesta pràctica

### **Com fer-ho?**

1. Obre:

   ```
   Herramientas → Usuarios y equipos de Active Directory
   ```
2. Ves a:

   ```
   iestorreroja.local → Computers
   ```
3. Localitza l'equip del client (`04-05-06-AD-WC`).
4. Botó dret → **Mover…**
5. Tria:

   ```
   iestorreroja.local → Equipos
   ```

Ara la GPO serà aplicada només als equips.

---

### **4.7 Assignar la GPO només als equips (opcional, automàtic)**

Per evitar haver-los de moure manualment:

### Des del servidor, PowerShell com a administrador:

```
redircmp "OU=Equipos,DC=iestorreroja,DC=local"
```


# **Entrega**

* Captures de cada pas
* Explicació breu de problemes trobats
* Validació d'inici de sessió amb usuaris del domini
