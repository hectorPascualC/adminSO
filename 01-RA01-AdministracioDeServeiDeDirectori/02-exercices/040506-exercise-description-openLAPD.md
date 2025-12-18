# 📝 PRÀCTICA 04-05-06 — OpenLDAP (GNU/Linux)

## Context

En aquesta pràctica hauràs de desplegar un servei de directori
basat en OpenLDAP en un entorn GNU/Linux, utilitzant una màquina virtual.

El servei de directori haurà de permetre la gestió centralitzada
d’usuaris i grups mitjançant una estructura jeràrquica de directori (DIT),
similar a la que s’utilitza en entorns corporatius.

## Objectius de la pràctica

Amb aquesta pràctica hauràs de ser capaç de:

- Desplegar un servidor OpenLDAP funcional.
- Definir un domini LDAP amb un administrador del directori.
- Organitzar el directori mitjançant unitats organitzatives (OU).
- Crear usuaris i grups dins del directori.
- Administrar el servei de directori mitjançant una eina gràfica.
- Garantir la seguretat de les connexions al directori.

## Tasques a realitzar

Per completar la pràctica hauràs de:

1. Instal·lar i deixar operatiu un servidor OpenLDAP.
2. Definir correctament el domini LDAP del directori.
3. Crear una estructura mínima del directori que contingui:
   - Unitats organitzatives per a usuaris.
   - Unitats organitzatives per a grups.
4. Crear almenys:
   - Un usuari del directori.
   - Un grup del directori.
5. Assignar usuaris als grups corresponents.
6. Administrar el directori utilitzant una eina web.
7. Configurar el servei perquè permeti connexions segures (TLS/LDAPS).
8. Verificar el funcionament correcte del directori i de les connexions.

## Condicions de la pràctica

- La pràctica s’ha de realitzar utilitzant màquines virtuals.
- Totes les configuracions han de ser funcionals abans del lliurament.
- No es permet utilitzar serveis externs ni directoris ja existents.
- La pràctica s’ha de poder repetir des de zero.

## Entrega

L’alumne haurà de lliurar:

- Documentació resumida de la pràctica.
- Evidències del funcionament del servei de directori.
- Captures que mostrin l’estructura del directori creada.
- Verificacions del correcte funcionament del servei i de la connexió segura.
