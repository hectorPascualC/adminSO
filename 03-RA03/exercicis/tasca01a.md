# Cercar automatització repetitiva a Linux

## 1. Llistem tasques de cron del sistema

```bash
    ls -ld /etc/cron.*
```

```bash
    drwxr-xr-x 2 root root 4096 feb 27 10:54 /etc/cron.d/
    drwxr-xr-x 2 root root 4096 dec 27 10:55 /etc/cron.daily/
    drwxr-xr-x 2 root root 4096 dec  7 23:04 /etc/cron.hourly/
    drwxr-xr-x 2 root root 4096 dec  7 23:04 /etc/cron.monthly/
    drwxr-xr-x 2 root root 4096 dec  7 23:04 /etc/cron.weekly/
```

## 2. Investigo un dels arxius automatitzats

```bash
    ls /etc/cron.daily
```

Resultat
```bash
    0anacron
    apt-compat # Tasques relacionades amb gestió de paquets
    dpkg # Comprovacions del gestor de paquets
    logrotate # Evita que els fitxers de log creixin sense control
    brave-browser
    slack
```

## 3. Mostrem el fitxer relacionat amb brave
```bash
    sudo cat /etc/cron.daily/brave-browser
```

Resultat
```bash
    ...
```