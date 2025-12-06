# 📁 **CAPÍTOL 4 - Instal·lació, configuració i personalització d’un servei de directori**

## 🔹**4.1. Instal·lació d’Active Directory (AD DS) - Exemple complet**

**Exemple real (Windows Server 2019)**

### **1. Preconditions**

Servidor: `SRV-AD01`  
IP fixa: `192.168.1.10`  
DNS: `127.0.0.1` (o el mateix servidor)  

### **2. Instal·lar el rol `Active Directory Domain Services`**

1. Obrir *Administrador del servidor*
2. “Agregar roles y características”
3. Seleccionar: **Servicios de dominio de Active Directory (AD DS)**
4. Continuar i instal·lar


## 🔹**4.2. Promoció del servidor a controlador de domini (DC)**

Exemple: crear un domini *nou* anomenat:

```
priserver.local
```

### Passos:

1. Després de la instal·lació del rol, apareix un avís:
   **“Promover este servidor a controlador de dominio”**
2. Seleccionar:

```
Crear un nuevo bosque
Nombre del dominio: priserver.local
```

3. Introduir la contrasenya DSRM
4. Acceptar la instal·lació de DNS
5. Ubicacions per defecte per a:
   * NTDS.dit
   * LOGS
   * SYSVOL
6. Instal·lar i reiniciar


## 🔹**4.3. Personalització inicial d’AD**

**Crear OU (organization units)**

Exemple d’estructura recomanada per un institut:

```
priserver.local
│
├── OU=Profesores
├── OU=Alumnos
├── OU=Departamentos
└── OU=Equipos
```

### 🔸Crear usuaris - Exemple

A *Usuarios y equipos de Active Directory* → carpeta OU → clic dret:

→ **Nuevo → Usuario**

Exemple:

```
Nombre: Laura García
Usuario: lgarcia
Contraseña: Temporal123*
```


### 🔸Crear grups - Exemple

A OU → *Nuevo → Grupo*

```
Nombre: Contabilidad
Tipo: Seguridad
Ámbito: Global
```

### 🔸Afegir equips al domini

1. A un client Windows:
   *Panel de control → Sistema → Cambiar configuración → Dominio*

2. Escriure:

```
Dominio: priserver.local
```

3. INICIAR SESSIÓ com:

```
priserver\Administrador
```

A AD apareix un objecte:

```
CN=EQUIPO01,OU=Equipos,DC=priserver,DC=local
```





