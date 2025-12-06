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

1. Inicia Windows 10 o 11
2. Assigna IP dins de la mateixa xarxa i DNS → IP del servidor AD
3. A Configuración → Sistema → Información → **Unirse a un dominio**
4. Domini: `iestorreroja.local`
5. Inicia sessió amb “alopez” per comprovar

**Resultat esperat:** Els equips apareixen a **OU=Equipos**.


## 🔸**4. Aplicar una Política de Grup (GPO)**

Crea una GPO que:

* **Treure les opcions d'apagar, reiniciar, suspendre i hibernar del menú Inicio**.

Procediment:

1. GPMC → Domini → Crear GPO “Restricció Energia”
2. Editar →

   * Equipo → Plantillas administrativas → Menú Inicio
   * Habilitar “Quitar y evitar el acceso a los comandos Apagar, Reiniciar, Suspender e Hibernar”.


# **Entrega**

* Captures de cada pas
* Explicació breu de problemes trobats
* Validació d'inici de sessió amb usuaris del domini
