# La comanda netdom query fsmo

Aquest comandament serveix per **consultar quin DC ostenta cadascun dels 5 rols FSMO**

```
netdom query fsmo
```

Sortida:

```
Schema master               DC1
Domain naming master        DC1
RID master                  DC2
PDC emulator                DC2
Infrastructure master       DC1
```

**Per què és útil?**

* Quan tens diversos DC i no saps quin és el que fa determinats rols
* Quan hi ha problemes de replicació o caiguda d’un DC, i vols saber quin rol s’ha de transferir
* És fonamental per diagnosticar problemes avançats d’AD