# **Configuració inicial del Windows Server**

Abans de poder instal·lar i promocionar AD, cal preparar correctament el servidor per **SConfig**, que apareix automàticament en iniciar Windows Server

### 🔸**1 Canviar el nom del servidor**

1. A SConfig → Opció **2) Computer name**
2. Assigna un nom recomanat, per exemple:

   ```
   SRV-AD01
   ```
3. Reinicia el servidor quan ho demani.

---

### 🔸**2 Configurar IP fixa i DNS**

1. A SConfig → Opció **8) Network Settings**
2. Selecciona l’adaptador de xarxa (normalment 1).
3. Tria **1) Set Network Adapter Address** → **Static**
4. Assigna una IP dins de la xarxa interna de VirtualBox, per exemple:

   ```
   IP: 192.168.10.10
   Mask: 255.255.255.0
   Gateway: (en blanc en xarxa interna)
   ```
5. Opció **2) Set DNS Server**

   * DNS primari:

     ```
     192.168.10.10
     ```

     *(és la IP del mateix servidor, necessari per AD)*
   * DNS secundari: deixa-ho en blanc.

---

### 🔸**3 Configurar la zona horària**

1. A SConfig → Opció **9) Date and Time**
2. Selecciona la zona:

   ```
   UTC+1 – Madrid
   ```

*(Kerberos requereix sincronització horària correcta.)*

---

### 🔸**4 Activar Escritori Remot (opcional)**

1. A SConfig → Opció **7) Remote Desktop**
2. Activa → **Enabled**
3. Selecciona → 1

*(Ajuda a gestionar el servidor des de l’host.)*

---

### 🔸**5 Actualització opcional**

Deixa l’opció **5) Update Settings** en:

```
Download only
```

I no instal·lis actualitzacions fins acabar l’AD.

---

### 🔸**6 Sortir a PowerShell**

Un cop configurat tot:

1. A SConfig → Opció **15) Exit to PowerShell**
2. Ja pots començar el punt **1. Instal·lació del AD**.


