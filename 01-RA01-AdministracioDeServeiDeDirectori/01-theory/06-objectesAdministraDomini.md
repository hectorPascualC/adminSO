# 📁 **CAPÍTOL 6 - Objectes administrats pel domini: usuaris, grups i equips**

Ampliació amb exemples i esquemes

## 🔹**6.1. Usuaris - Exemple bàsic**

### Crear usuari:

1. Dins OU → clic dret → **Nuevo → Usuario**
2. Introduir dades:

```
Nombre: Ana Torres
Usuario: atorres
Contraseña: PrimerDia123
```

### Esquema d’usuari:

```
CN=Ana Torres,OU=Profesores,DC=priserver,DC=local
```

---

## 🔹**6.2. Grups - Exemple**

Crear grup de seguretat:

```
Nombre: DepartamentoInformatica
Ámbito: Global
Tipo: Seguridad
```

Assignar usuaris:

```
DepartamentoInformatica
 ├── atorres
 └── jperez
```

---

## 🔹**6.3. Equips - Exemple**

Quan un equip entra al domini:

```
EQUIPO23.priserver.local
```

Apareix a:

```
CN=EQUIPO23,OU=Equipos,DC=priserver,DC=local
```

---

## 🔹**6.4. Exemple complet d’infraestructura**

```
priserver.local
│
├── OU=Profesores
│    ├── Ana Torres
│    └── Juan Pérez
│
├── OU=Alumnos
│    ├── Marta Ruiz
│    └── Carlos León
│
├── OU=Grupos
│    ├── Profesores
│    └── Informatica
│
└── OU=Equipos
     ├── EQUIPO01
     └── EQUIPO02
```
