# Replicació multimàster

## 1. Explicació

Active Directory utilitza un model de **replicació multimàster**:

* **Tots els DC poden acceptar canvis** (usuaris, grups, OUs…)
* Aquests canvis es **repliquen automàticament** a la resta de controladors


## 2. Restriccions

**No totes les operacions poden ser multimàster**

Si dues màquines editen l'esquema alhora, podria ser catastròfic

Per això existeixen els rols **FSMO**, on *només un* DC pot fer certes operacions sensibles (crear dominis, modificar l’esquema, assignar blocs RID…)

Per tant:

> AD és multimàster per quasi tot, però té restringides 5 funcions protegides
