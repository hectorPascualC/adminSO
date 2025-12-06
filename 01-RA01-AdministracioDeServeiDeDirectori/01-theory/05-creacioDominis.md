# 📁 **CAPÍTOL 5 - Creació de dominis i relacions de confiança**

## 🔹**5.1. Exemple de creació d'un segon domini**

Imagineu una organització amb:

* Domini 1: `empresa.local`
* Necessitat d'un domini 2: `delegacion.local`

En crear un segon domini en un altre servidor i seleccionar:

```
Agregar un nuevo dominio en un árbol existente
```

S'afegeix al mateix **bosc** i es crea una confiança automàtica.


## 🔹**5.2. Tipus de confiança**


### 🔸**5.2.1. Confiança externa**

Per connectar **dos dominis que no pertanyen al mateix bosc**.

Exemple:
```
empresa.local  ←→  proveedores.com
```

Esquema:
```
[ Bosque A ] empresa.local
      ↑
      │ Confianza externa
      ↓
[ Bosque B ] proveedores.com
```

Configuració (pràctica real amb AD)

1. Obrir:
   **Dominios y confianzas de Active Directory**
2. Clic dret al domini → *Propiedades*
3. Pestanya **Confianzas**
4. “Nueva confianza…”
5. Introduir nom extern: `proveedores.com`
6. Seleccionar:
   * Direccionalidad:
     ↦ Entrada / Salida / Bidireccional
   * Tipo de confianza:
     ↦ Externa
7. Indicar credencials del domini extern

---

### 🔸**5.2.2. Confiança de domini**

Connecta dos dominis dins d'un **mateix arbre**.

Exemple:

```
ventas.empresa.local
soporte.empresa.local
```

Aquesta confiança es crea **automàticament**

Esquema:

```
empresa.local
├── ventas.empresa.local
└── soporte.empresa.local
```

Configuració:

No cal configurar-la manualment

---

### 🔸**5.2.3. Confiança de boscos**

Permet que **dos boscos sencers** comparteixin recursos.

Exemple:

```
Bosque A: empresa.local
Bosque B: universidad.edu
```

Esquema:

```
[ BOSQUE A ] empresa.local
      ⇅   confianza de bosques
[ BOSQUE B ] universidad.edu
```

Configuració:

1. Obrir “Dominios y confianzas de AD”
2. Clic dret → Propiedades del dominio
3. Pestanya **Confianzas**
4. Nueva confianza…
5. Introduir nom del bosc remot
6. Seleccionar:
   * Confianza bidireccional
   * Autenticación de bosque
7. Completar assistent

---

### 🔸**5.2.4. Confiança d'accés directe**

Permet que dos dominis **no adjacents** dins del mateix bosc es comuniquin directament.

Exemple:

```
madrid.empresa.local     (no està directament connectat a fabrica)
fabrica.empresa.local
```

Esquema:

```
empresa.local
├── madrid
│
└── fabrica
```

Sense confiança directa:
```
madrid → empresa → fabrica
```

Amb confiança directa:
```
madrid → fabrica
```

Configuració:

1. Dominios y confianzas
2. Propiedades
3. Crear “Confianza de acceso directo”

