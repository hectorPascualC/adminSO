# **Annex: Configuració de Xarxa i Diagnòstic per a l'Entorn AD amb VirtualBox (Red Interna)**


## 1. Arquitectura de xarxa utilitzada

Dues màquines virtuals funcionant en **Red Interna**:

| Rol                            | Hostname          | IP final          | OS                  |
| ------------------------------ | ----------------- | ----------------- | ------------------- |
| **Controlador de Domini (WS)** | `WIN-BCNV4AEJS68` | **192.168.10.10** | Windows Server 2022 |
| **Client Windows 11 (WC)**     | `VBOXCLIENT`      | **192.168.10.20** | Windows 11 Pro      |

### Mode de xarxa utilitzat a VirtualBox

**Red interna (Internal Network)**

* Nom: `RED-AD`
* Aïllada d’Internet
* Només comunicació entre màquines virtuals
* Evita conflictes de rutes, DHCP externs i DNS del host

**És el mode recomanat per a muntar Active Directory en un entorn docent**
(Explicació completa al final)


# 2. Configuració a VirtualBox

## Rutes d’accés a la configuració:

**VirtualBox → Selecciona la MV → Configuración → Red → Adaptador 1**

Config final:

```
Modo de conexión: Red interna
Nombre: RED-AD
Cable conectado: connectat
```

⚠️ Cap altre adaptador (NAT/Bridge) ha d’estar activat


## 3. Configuració del Windows Server (WS)

### 3.1. Arribar a la configuració IPv4

Des de la interfície gràfica:

1. **Administrador del servidor**
2. Pestaña esquerra → **Servidor local**
3. A la dreta → **Ethernet** (en blau)
4. S’obre “Conexiones de red”
5. Clic dret → **Propiedades**
6. Selecciona **Protocolo de Internet versión 4 (TCP/IPv4)** → **Propiedades**

### 3.2. Configuració final utilitzada

    ```
    IP:       192.168.10.10
    Máscara:  255.255.255.0
    Gateway:  (vacío)
    DNS:      192.168.10.10
    ```

---

## 4. Configuració del Windows Client (WC)

### 4.1. Ruta d’accés

Windows 11:

1. **Inicio → Configuración**
2. **Red e Internet**
3. **Ethernet**
4. A “Asignación de IP” → **Editar**
5. A “Asignación de DNS” → **Editar**

### 4.2. Configuració final utilitzada

    ```
    IP:      192.168.10.20
    Máscara: 255.255.255.0
    Gateway: (vacío)
    DNS:     192.168.10.10
    ```


## 5. Configuració del DNS al Windows Server

### 5.1. Ruta d’accés a DNS Manager

Administrador del servidor → **Herramientas** → **DNS**

Després:

    ```
    Zonas de búsqueda directa → iestorreroja.local
    ```

### Registres que han de existir:

    ```
    iestorreroja.local   A     192.168.10.10
    win-bcnv4aejs68      A     192.168.10.10
    ```


## 6. Comprovacions i sortides esperades

### 6.1. Ping funcional entre màquines

    ```
    ping 192.168.10.10
    ```

**Sortida esperada:**

    ```
    Respuesta desde 192.168.10.10: bytes=32 tiempo<1ms TTL=128
    ```


### 6.2. Test de resolució DNS amb nslookup

    ```
    nslookup iestorreroja.local
    ```

### 🟡 Per què surt **Servidor: Unknown**?

Perquè **no vam crear la zona de búsqueda inversa (PTR)**.

En absència d’aquesta zona:

* El servidor DNS *no pot resoldre la seva pròpia IP cap a un nom*
* **Això NO afecta Active Directory**
* És només un detall estètic d’`nslookup`

Si la resolució directa (registre A) funciona —i funciona—, AD treballarà correctament.

### Sortida esperada (que és correcta):

    ```
    Servidor: Unknown
    Address: 192.168.10.10

    Nombre: iestorreroja.local
    Address: 192.168.10.10
    ```


## 7. Per què és recomanable muntar AD en `Red Interna`?

### 7.1 Aïllament complet

Active Directory manipula:

* DNS d’empresa
* Polítiques de seguretat
* WINS / Netlogon
* Paquets d’autenticació Kerberos

En una xarxa NAT o Bridge podria interferir amb:

* DHCP real del router
* DNS reals (8.8.8.8 o del proveïdor)
* Rutes externes

En **red interna no pot afectar el host ni la xarxa del centre**.


### 7.2: Control total sobre l’escenari

* Controla el 100% dels serveis
* No hi ha sorpreses externes
* Res no depèn d’Internet

Active Directory **no necessita Internet** per funcionar


### 7.3: Estabilitat de l'AD

Els dominis Active Directory són extremadament sensibles a:

* Renegociacions de rutes
* DNS incorrectes
* Canvis en IP del controlador

**Red interna = IPs fixes = AD estable**


### 7.4: Reproduïbilitat de les pràctiques

El mateix escenari funciona:

* a l'aula
* a l'ordinador de l'alumne
* a casa

Sense dependències.


## 8. Resum final (per incloure al manual)

```
Entorn AD utilitzant VirtualBox en red interna

WS:
  IP: 192.168.10.10
  Mask: 255.255.255.0
  Gateway: —
  DNS: 192.168.10.10

WC:
  IP: 192.168.10.20
  Mask: 255.255.255.0
  Gateway: —
  DNS: 192.168.10.10

Comprovacions:
  ping WS ↔ WC OK
  nslookup iestorreroja.local OK
  "Servidor: Unknown" és normal (no zona inversa)
  AD estable i funcional

Red interna recomanada per:
  - Aïllament
  - Estabilitat
  - Control docent
  - Reproduïbilitat
```


