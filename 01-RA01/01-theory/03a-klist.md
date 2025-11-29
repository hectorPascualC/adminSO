# 🔵 Klist i autenticació Kerberos

Quan un usuari inicia sessió en un domini Active Directory, **Kerberos** li assigna un **TGT (Ticket-Granting Ticket)** que és:

* Una “prova criptogràfica” que confirma que l’usuari ha estat verificat
* Un passaport digital que permet demanar tickets per accedir a serveis

Per veure els tickets que tens, s’utilitza:

```
klist
```

Una sortida típica és:

```
Client: joan@EMPRESA.LOCAL
Server: krbtgt/EMPRESA.LOCAL
Start Time: 23/11/2025 10:05
End Time:   23/11/2025 20:05
```

**Com interpretar això?**

* **Client**     → usuari autenticat (joan@domini)
* **Server**     → el servei KDC que va emetre el ticket (`krbtgt`)
* **Start Time** → moment en què s’emet el ticket
* **End Time**   → caducitat del ticket (normalment 10 hores)

**Per què és important?**

* Permet comprovar si l’autenticació Kerberos funciona
* Ajuda a diagnosticar problemes de temps, replicació o rellotges desfasats
* És una eina clau per a tècnics que administren AD