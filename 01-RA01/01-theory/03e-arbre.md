# Què són arbres en AD?

**Un arbre** és un conjunt de dominis que comparteixen:

* El mateix espai de noms DNS
* Una jerarquia pare → fill
* Replicació i confiances automàtiques

Ex:

```
empresa.local        ← domini arrel (root domain)
 ├── barcelona.empresa.local
 └── madrid.empresa.local
```

Un bosc pot contenir **més d’un arbre**.