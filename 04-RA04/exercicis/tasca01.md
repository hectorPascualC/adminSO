# Pràctica RA04

# 1. Context

En entorns d’administració de sistemes és habitual gestionar servidors de forma remota mitjançant **SSH**. També és habitual automatitzar tasques periòdiques com còpies de seguretat o sincronització de dades.

En aquesta pràctica es configurarà un sistema Linux perquè permeti accés remot controlat i realitzi còpies periòdiques d’una carpeta remota.

---

# 2. Objectius de la pràctica

L’alumne haurà de ser capaç de:

* configurar accés remot amb **SSH**
* restringir l’accés SSH a un **grup de confiança**
* utilitzar **claus SSH** per autenticar-se
* copiar fitxers entre sistemes amb **rsync**
* automatitzar tasques amb **cron**
* registrar l’activitat de la còpia en un **fitxer de log**

---

# 3. Escenari

Treballarem amb dues màquines Linux:

**Servidor**

* conté la informació original

**Client**

* rebrà les còpies periòdiques

Al servidor existeix una carpeta amb dades que haurà de copiar-se periòdicament al client.

---

# 4. Tasques a realitzar

L’alumne haurà de completar les tasques següents.

---

## Tasca 1 - Crear un grup de confiança

Al servidor s’ha de crear un **grup d’usuaris autoritzats per accedir per SSH**.

Aquest grup serà el grup de confiança del sistema.

---

## Tasca 2 - Crear un usuari per a la pràctica

S’ha de crear un usuari que s’utilitzarà per realitzar la connexió remota.

Aquest usuari ha de formar part del grup de confiança creat anteriorment.

---

## Tasca 3 - Restringir l’accés SSH

El servidor SSH s’ha de configurar perquè **només els usuaris del grup de confiança puguin accedir al sistema mitjançant SSH**.

---

## Tasca 4 - Verificar la connexió SSH

Des del client s’ha de provar la connexió amb l’usuari creat.

La connexió ha de funcionar correctament.

---

## Tasca 5 - Configurar autenticació amb claus SSH

S’ha de configurar l’autenticació amb **claus SSH** perquè la connexió entre el client i el servidor es pugui fer **sense utilitzar contrasenya**.

---

## Tasca 6 - Preparar la carpeta de destinació

Al client s’ha de crear una carpeta on es guardaran les còpies procedents del servidor.

---

## Tasca 7 - Copiar dades remotament

S’ha d’utilitzar una eina de sincronització per copiar la carpeta del servidor al client.

---

## Tasca 8 - Crear un script d’automatització

S’ha de crear un script que executi la sincronització de dades entre el servidor i el client.

Aquest script també haurà de registrar la seva execució en un fitxer de log.

---

## Tasca 9 - Automatitzar l’execució

S’ha de configurar el planificador de tasques del sistema perquè l’script s’executi **periòdicament**.

---

## Tasca 10 - Verificar el funcionament

S’ha de comprovar que:

* la còpia de dades es realitza correctament
* l’execució queda registrada al fitxer de log

---

# 5. Evidència a entregar

L’alumne haurà d’entregar captures o documentació que mostrin:

* creació del grup de confiança
* creació de l’usuari
* configuració del servidor SSH
* connexió SSH entre client i servidor
* configuració de claus SSH
* sincronització de fitxers
* configuració del planificador de tasques
* contingut del fitxer de log

---

# 6. Preguntes finals

Respon breument:

1. Quin protocol permet administrar sistemes Linux remotament de forma segura?
2. Quin port utilitza aquest protocol per defecte?
3. Per què és recomanable utilitzar autenticació amb claus en lloc de contrasenyes?
4. Quin avantatge té la sincronització de fitxers respecte una còpia simple?
5. Per què és important registrar l’execució de tasques automàtiques?
