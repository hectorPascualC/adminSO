# Què és exactament un bosc?

En Active Directory, un **bosc (forest)** és la **màxima unitat administrativa**

Un bosc conté:

* **Un o més arbres (trees)**
* Aquests contenen **un o més dominis (domains)**
* Tots ells comparteixen:

  * **L’esquema** (definició dels objectes)
  * **El Global Catalog**: servei especialitzat que s'executa dins d’un controlador de domini i que conté una còpia parcial però indexada de tots els objectes de tot el bosc. És com l'índex general del bosc AD
  * **Una relació de confiança automàtica**

**Un bosc és com el món AD d’una organització**

Ex:

```
empresa.local   ← arbre1
 ├── madrid.empresa.local
 └── barcelona.empresa.local

empresa.org     ← arbre2
 ├── it.empresa.org
 └── hr.empresa.org
```

Si aquests dos arbres comparteixen:

* Esquema
* GC
* Confiances internes

vol dir que  formen un únic **bosc**