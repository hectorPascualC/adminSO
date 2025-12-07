# Upgrade de Windows 11 Home a Windows 11 Pro per unir-lo al Domini

## 🔹 1. Versió inicial del Windows 11 utilitzat

Si no tens la versió de Windows:
* Windows 10/11 Pro
* Windows 10/11 Enterprise
* Windows 10/11 Education


Hauràs de fer un upgrade a la PRO

La versió no PRO:
* **No permet unir l'equip a un domini** d'Active Directory.
* **cal convertir-la a Windows 11 Pro**.

En els següents pasos epxlicarem com fer un upgrade de Windows 11 **Home** → **Pro**

## 🔹 2. Com actualitzar Windows 11 Home → Windows 11 Pro

L'actualització és interna i **no necessita connexió a Internet**, especialment perquè la MV està en **Red Interna** de VB

### **Passos:**

1. Obre:
   **Configuración → Sistema → Activación**
2. Fes clic a:
   **Cambiar la clave de producto**
3. Introdueix aquesta clau genèrica oficial de Microsoft per convertir a Pro:

    ```
    VK7JG-NPHTM-C97JM-9MPGT-3V66T
    ```

4. Accepta l'actualització. L'equip es reiniciarà
5. En encendre's, ja apareixerà com:
    ```
    Windows 11 Pro
    Estado de activación: No activado
    ```

> ⚠️ **Important**: És normal que no estigui activat
> La pràctica no requereix activació real. Només necessitem les funcionalitats de Pro


## 🔹 3. Per què s'ha d'activar com a Windows 11 Pro?

Windows 11 Home **no inclou** les funcionalitats necessàries per integrar-se en un entorn corporatiu amb AD

### Funcions que **NO té** Windows 11 Home:

* Unir-se a un **domini AD**
* Aplicar **GPOs (Group Policies)**
* Suport complet de **Kerberos**
* Integració amb **DNS corporatiu**
* Gestió de **cuentas de dominio**
* Remote Server Administration Tools (RSAT)

### Funcions que **sí té** Windows 11 Pro:

* Unir-se a un domini (`domain join`)
* Client Kerberos complet
* Aplicació de polítiques de grup (GPO)
* Gestió centralitzada d'usuaris i permisos
* Compatibilitat amb AD DS i DNS de l'empresa

**Per això és obligatori fer l'upgrade a Pro abans de fer el pas d'unió al domini.**


## 🔹 4. Per què l'upgrade funciona sense Internet?

* La clau facilitada és una **clau genèrica oficial** per activar components interns de Pro
* El sistema **no descarrega res de Microsoft**
* L'actualització és només un *canvi d'edició*. Els fitxers Pro ja formen part de la instal·lació base
* En una xarxa **Internal Network**, Windows **no pot sortir a Internet**, però això **no afecta** el procés

-