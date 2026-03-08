# Manual detallat - Pràctica RA04

# 1. Escenari

Treballarem amb dues màquines Linux.

CLIENT

```
192.168.1.10
```

SERVIDOR

```
192.168.1.20
```

Al servidor existeix la carpeta:

```
/data/backups
```

L’objectiu és copiar-la periòdicament al client.

---

# 2. Comprovar que SSH està instal·lat

Al servidor comprovar el servei.

```bash
systemctl status ssh
```

Resultat correcte:

```
active (running)
```

Si no està instal·lat:

```bash
sudo apt install openssh-server
```

Després iniciar-lo:

```bash
sudo systemctl start ssh
```

---

# 3. Crear el grup de confiança

Al servidor crear el grup que podrà accedir per SSH.

```bash
sudo groupadd sshusers
```

Comprovar que s’ha creat.

```bash
cat /etc/group | grep sshusers
```

Resultat esperat:

```
sshusers:x:1002:
```

---

# 4. Crear usuari de prova

Crear l’usuari que utilitzarem per connectar-nos.

```bash
sudo adduser backupuser
```

Afegir-lo al grup de confiança.

```bash
sudo usermod -aG sshusers backupuser
```

Comprovar que està al grup.

```bash
groups backupuser
```

Resultat esperat:

```
backupuser : backupuser sshusers
```

---

# 5. Configurar restricció d'accés SSH

Editar el fitxer de configuració.

```bash
sudo nano /etc/ssh/sshd_config
```

Afegir la línia:

```
AllowGroups sshusers
```

Guardar el fitxer i reiniciar SSH.

```bash
sudo systemctl restart ssh
```

Comprovar que continua funcionant.

```bash
systemctl status ssh
```

---

# 6. Provar connexió SSH

Des del client executar:

```bash
ssh backupuser@192.168.1.20
```

El sistema demanarà la contrasenya.

Si tot és correcte apareixerà:

```
backupuser@server:~$
```

---

# 7. Crear claus SSH

Al client generar les claus.

```bash
ssh-keygen
```

Acceptar la ubicació per defecte.

Es crearan:

```
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

---

# 8. Copiar la clau al servidor

Executar:

```bash
ssh-copy-id backupuser@192.168.1.20
```

A partir d’aquest moment es podrà accedir sense contrasenya.

Prova:

```bash
ssh backupuser@192.168.1.20
```

---

# 9. Crear carpeta de destinació

Al client:

```bash
mkdir ~/copia_remota
```

Comprovar:

```bash
ls
```

---

# 10. Provar còpia amb rsync

Executar:

```bash
rsync -av backupuser@192.168.1.20:/data/backups ~/copia_remota
```

Resultat esperat:

```
sending incremental file list
file1.txt
file2.txt
```

Comprovar:

```bash
ls ~/copia_remota
```

---

# 11. Crear carpeta de logs

Al client:

```bash
mkdir ~/logs
```

---

# 12. Crear script de còpia

Crear l’script.

```bash
nano ~/backup_remot.sh
```

Contingut:

```bash
#!/bin/bash

echo "===== Execució $(date) =====" >> ~/logs/backup.log

rsync -av backupuser@192.168.1.20:/data/backups ~/copia_remota >> ~/logs/backup.log 2>&1

echo "" >> ~/logs/backup.log
```

Guardar el fitxer.

---

# 13. Fer executable l’script

```bash
chmod +x ~/backup_remot.sh
```

Provar-lo manualment.

```bash
./backup_remot.sh
```

Comprovar el log.

```bash
cat ~/logs/backup.log
```

---

# 14. Automatitzar amb cron

Editar cron.

```bash
crontab -e
```

Afegir:

```
*/5 * * * * /home/usuari/backup_remot.sh
```

Comprovar:

```bash
crontab -l
```

---

# 15. Verificar execució automàtica

Esperar uns minuts i consultar el log.

```bash
cat ~/logs/backup.log
```

Ha d’aparèixer una nova execució.

---

# Problemes habituals i solucions

## Error: connection refused

```
ssh: connect to host 192.168.1.20 port 22: Connection refused
```

Possible causa:

* el servei SSH no està iniciat

Solució:

```bash
sudo systemctl start ssh
```

---

## Error: permission denied

```
Permission denied (publickey,password)
```

Possible causa:

* usuari no autoritzat
* no està al grup sshusers

Solució:

```bash
sudo usermod -aG sshusers backupuser
```

Reiniciar SSH.

---

## Error en rsync

```
rsync: command not found
```

Solució:

```bash
sudo apt install rsync
```

---

## Cron no executa la tasca

Comprovar cron.

```bash
systemctl status cron
```

Si no està actiu:

```bash
sudo systemctl start cron
```

---

## Error de permisos a l'script

```
permission denied
```

Solució:

```bash
chmod +x ~/backup_remot.sh
```


