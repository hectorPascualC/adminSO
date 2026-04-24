# RA06 - Integració de sistemes operatius lliures i propietaris

## Índex
- [6.1 Concepte d'integració de sistemes operatius](#61-concepte-dintegració-de-sistemes-operatius)
- [6.2 Escenaris heterogenis de xarxa](#62-escenaris-heterogenis-de-xarxa)
- [6.3 Recursos compartits en xarxa](#63-recursos-compartits-en-xarxa)
- [6.4 Seguretat d'accés als recursos compartits](#64-seguretat-daccés-als-recursos-compartits)
- [6.5 Connectivitat en entorns heterogenis](#65-connectivitat-en-entorns-heterogenis)
- [6.6 Serveis de xarxa per compartir recursos](#66-serveis-de-xarxa-per-compartir-recursos)
- [6.7 Samba com a servei de compartició en entorns Linux i Windows](#67-samba-com-a-servei-de-compartició-en-entorns-linux-i-windows)
- [6.8 Configuració de recursos compartits](#68-configuració-de-recursos-compartits)
- [6.9 Proves de funcionament i ús de fitxers i impressores en una xarxa heterogènia](#69-proves-de-funcionament-i-ús-de-fitxers-i-impressores-en-una-xarxa-heterogènia)
- [6.10 Documentació de la configuració](#610-documentació-de-la-configuració)

## 6.1 Concepte d'integració de sistemes operatius

La integració de sistemes operatius consisteix a fer possible que equips diferents puguin treballar dins d'una mateixa xarxa de manera normal i funcional. En aquest context, “diferents” vol dir sobretot que no tots utilitzen el mateix sistema operatiu. Un cas molt habitual és el d'una xarxa on els equips dels usuaris funcionen amb Windows, mentre que alguns serveis centrals es troben en un servidor Linux.

Ara bé, integrar sistemes no vol dir simplement que tots els equips estiguin connectats al mateix switch o tinguin accés a internet. La integració real comença quan aquests sistemes poden compartir recursos, utilitzar serveis comuns i accedir-hi sense problemes. Per això, en aquesta RA no s'estudien Linux i Windows com dos entorns separats, sinó com dos sistemes que han de poder conviure i interoperar dins d'una mateixa xarxa.

En la pràctica, aquesta interoperabilitat es concreta sobretot en tres necessitats bàsiques:

- accedir a carpetes i fitxers compartits
- utilitzar impressores de xarxa
- controlar l'accés als recursos mitjançant usuaris i permisos

### Exemple real

En una empresa o en un centre educatiu és bastant habitual tenir ordinadors d'usuari amb Windows i un servidor Linux que actua com a servidor d'arxius. Si els usuaris poden entrar a una carpeta comuna, guardar-hi documents i, a més, imprimir sobre una impressora compartida des de diferents equips, llavors podem dir que hi ha una integració efectiva entre sistemes diferents.

## 6.2 Escenaris heterogenis de xarxa

Un escenari heterogeni de xarxa és aquell en què conviuen equips amb característiques diferents. La diferència més visible acostuma a ser el sistema operatiu, però també poden canviar els serveis instal·lats, la manera de gestionar els permisos o el paper que té cada equip dins de la xarxa.

Aquest tipus de situació és molt més freqüent que una xarxa completament homogènia. En un mateix entorn poden coincidir clients Windows, algun client Linux, un servidor Linux i una impressora de xarxa. Per això, el problema d'un entorn heterogeni no és només tècnic. També és de gestió, perquè cada sistema tracta d'una manera pròpia aspectes com els usuaris, els permisos, les rutes o la localització de recursos.

Per aquesta raó, l'administrador no pot limitar-se a connectar els equips i donar per fet que tot funcionarà correctament. Ha d'entendre quin sistema farà de servidor, quin actuarà com a client i quin servei permetrà la compartició dels recursos. En aquesta RA, el patró més habitual serà el d'un servidor Linux que ofereix recursos perquè puguin ser utilitzats des de clients Windows i Linux.

### Exemple d'escenari

```text
Clients Windows ─────┐
                     ├── Xarxa local ─── Servidor Linux ─── Recursos compartits
Client Linux ────────┘
```

En aquest cas, el servidor és el punt central. Els clients no necessiten ser iguals entre ells; el que necessiten és poder accedir correctament als recursos que el servidor posa a disposició de la xarxa.

## 6.3 Recursos compartits en xarxa

Un recurs compartit és qualsevol element d'un sistema que es posa a disposició d'altres equips a través de la xarxa. En aquesta RA, els recursos principals seran sobretot carpetes, fitxers i impressores, perquè són els casos més habituals en entorns reals d'interoperabilitat.

La compartició d'arxius és una de les necessitats més comunes. Quan una carpeta del servidor es comparteix correctament, els clients autoritzats poden accedir-hi i treballar amb el seu contingut sense que cada equip hagi de tenir una còpia local independent. Això facilita el treball comú, evita duplicacions i permet centralitzar la informació.

Amb les impressores passa una cosa semblant. En lloc de dependre d'una impressora connectada localment a cada equip, la xarxa pot utilitzar una impressora compartida o gestionada des d'un servidor. Això permet que diferents usuaris, i fins i tot diferents sistemes operatius, imprimeixin sobre el mateix recurs.

### Recursos més habituals

Dins d'una xarxa heterogènia, els recursos compartits més habituals solen ser:

- carpetes de treball o documentació comuna
- directoris de departament o d'aula
- impressores compartides
- espais de xarxa per guardar fitxers

Aquesta és la base sobre la qual després entren serveis com Samba i CUPS, que són els que permeten oferir aquests recursos de manera pràctica.

## 6.4 Seguretat d'accés als recursos compartits

Compartir recursos és útil, però no té sentit fer-ho sense control. Cada vegada que una carpeta o una impressora es posa a disposició de la xarxa, l'administrador ha de decidir qui hi pot accedir i què hi pot fer exactament.

En una carpeta compartida, per exemple, no tots els usuaris haurien de tenir necessàriament el mateix nivell d'accés. Hi haurà casos en què només cal lectura, perquè l'usuari només ha de consultar informació. En altres casos caldrà lectura i escriptura, perquè s'hi hauran de desar o modificar documents. I, finalment, hi haurà usuaris amb permisos més amplis perquè són els responsables d'administrar el recurs.

Això connecta directament amb una part molt important de la configuració de xarxa: la necessitat de crear o afegir usuaris, assignar permisos i controlar correctament l'accés als recursos compartits. La compartició de carpetes i impressores no és només una qüestió de fer visible un recurs a la xarxa, sinó de definir correctament quins comptes hi podran treballar i amb quins privilegis.

Una manera habitual d'organitzar aquest control és treballar amb usuaris i grups. Això simplifica la gestió quan hi ha moltes persones i evita haver de configurar permisos un per un.

### Idea clau de seguretat

En qualsevol recurs compartit s'hauria d'aplicar aquest criteri bàsic:

- donar només els permisos necessaris
- evitar permisos excessius
- separar els usuaris que només consulten dels que poden modificar

Això és el que en seguretat es coneix com a principi de mínim privilegi.

### Exemple senzill

Imaginem una carpeta compartida de material intern. El més lògic seria que el professorat o el personal autoritzat hi pogués llegir i escriure, mentre que altres usuaris només hi poguessin llegir o, directament, no hi tinguessin accés.

## 6.5 Connectivitat en entorns heterogenis

Abans de compartir carpetes o impressores, cal assegurar que els equips realment es poden comunicar dins de la xarxa. En entorns heterogenis, aquest punt és especialment important, perquè no n'hi ha prou amb tenir els dispositius connectats físicament. També cal que tinguin una configuració de xarxa coherent i que puguin localitzar els serveis que s'oferiran.

Quan es parla de connectivitat en aquesta RA, no s'ha de pensar només en tenir internet o en respondre a un ping. La connectivitat que interessa és la que permet que un client Windows o Linux pugui arribar al servidor, identificar-lo dins de la xarxa i intentar accedir als recursos compartits que aquest ofereix.

Per tant, abans de configurar un servei de compartició, convé comprovar aspectes bàsics com aquests:

- que el client i el servidor estiguin dins de la mateixa xarxa o tinguin ruta entre ells
- que tinguin una adreça IP correcta
- que la resolució de noms no provoqui errors
- que no hi hagi cap tallafoc bloquejant el servei

### Exemple bàsic de comprovació

Una de les primeres proves acostuma a ser verificar si el client pot arribar al servidor:

```bash id="6u6gda"
ping 192.168.1.10
```

Aquesta prova no garanteix que el servei de compartició funcioni, però sí que ajuda a confirmar que hi ha comunicació mínima entre equips.

## 6.6 Serveis de xarxa per compartir recursos

Perquè una carpeta o una impressora estiguin disponibles a la xarxa, no n'hi ha prou amb crear-les al sistema. Cal que existeixi un servei de xarxa que faci possible aquesta compartició i que actuï com a intermediari entre el servidor i els clients.

Això és precisament el que dona sentit a aquest bloc de la RA06: entendre que la interoperabilitat no s'aconsegueix sola, sinó mitjançant serveis específics que permeten compartir recursos d'una manera compatible amb diferents sistemes operatius.

En el context que treballarem, hi ha dos serveis especialment importants. El primer és Samba, que permet compartir fitxers i directoris en xarxes on conviuen Linux i Windows. El segon és CUPS, que s'utilitza per gestionar la impressió en Linux i que també pot intervenir quan es comparteixen impressores dins de la xarxa.

### Serveis principals en aquesta RA

Els dos serveis centrals seran aquests:

- **Samba**, per compartir fitxers i directoris entre Linux i Windows
- **CUPS**, per gestionar i compartir impressores en entorns Linux

És important entendre la diferència entre el servei i el recurs. El servei és el mecanisme que fa possible la compartició. El recurs és allò que realment s'ofereix a la xarxa. Dit d'una altra manera: Samba és el servei; la carpeta compartida és el recurs. CUPS és el servei; la impressora configurada és el recurs.

## 6.7 Samba com a servei de compartició en entorns Linux i Windows

Quan un servidor Linux ha de compartir carpetes amb equips Windows, el servei més habitual és Samba. Aquest servei és especialment important perquè resol un dels problemes clàssics de les xarxes heterogènies: fer que un recurs allotjat en Linux pugui ser utilitzat amb normalitat des d'un client Windows.

Això dona a Samba un paper central dins de la RA06. No és simplement un programa més, sinó un mecanisme d'interoperabilitat. Gràcies a Samba, un servidor Linux pot presentar carpetes compartides a la xarxa d'una manera que els clients Windows entenen i poden utilitzar.

El valor real de Samba és que no només comparteix directoris. També permet controlar l'accés mitjançant usuaris, definir permisos sobre els recursos i fer que la compartició s'integri dins d'un entorn mixt on hi ha diferents sistemes operatius treballant alhora.

### Què permet Samba

En termes pràctics, Samba permet:

- compartir carpetes des d'un servidor Linux
- controlar quin usuari o grup hi pot entrar
- definir si un recurs és només de lectura o també d'escriptura
- facilitar l'accés des d'equips Windows i també des d'altres equips Linux

### Exemple d'ús

Imaginem un servidor Linux amb una carpeta comuna per al departament:

```text id="hrt4wh"
/srv/compartit
```

Si aquesta carpeta es configura correctament mitjançant Samba, un equip Windows la podrà veure com un recurs de xarxa i accedir-hi amb les credencials corresponents.

## 6.8 Configuració de recursos compartits

Quan el servei ja està instal·lat, el pas següent és configurar el recurs que realment es vol oferir a la xarxa. Aquí és on la teoria es torna clarament pràctica: no n'hi ha prou amb tenir Samba o CUPS al sistema, sinó que cal decidir què es compartirà, qui hi podrà accedir i amb quins permisos.

En el cas d'un servidor d'arxius, la configuració comença normalment amb una carpeta del sistema Linux que farà de recurs compartit. Aquesta carpeta no s'ha de pensar només com un directori més, sinó com un punt d'accés per als clients de la xarxa.

### Exemple conceptual de recurs compartit amb Samba

Una configuració típica parteix d'una carpeta com aquesta:

```bash id="8cqrb3"
sudo mkdir -p /srv/compartit
```

Un cop creada, es poden ajustar els permisos i després definir el recurs dins de la configuració de Samba. A nivell conceptual, el que es fa és associar un nom visible a la xarxa amb una ruta real del sistema Linux.

```text id="24tzse"
Nom del recurs a la xarxa  →  compartit
Ruta real al servidor      →  /srv/compartit
Accés                      →  usuaris autoritzats
Permisos                   →  lectura o lectura/escriptura
```

### Usuaris i autenticació

Quan es parla d'afegir usuaris i controlar l'accés, el que es posa sobre la taula és que un recurs compartit no s'hauria de deixar obert de manera indiscriminada. El més habitual és que els usuaris que han d'accedir a una carpeta compartida estiguin definits al sistema i, a més, quedin preparats per a l'accés mitjançant Samba.

Això dona lloc a una idea molt pròpia d'aquest bloc: en una xarxa mixta, la compartició no depèn només de la carpeta, sinó també de la relació entre:

- el directori real al servidor
- l'usuari que hi accedeix
- el servei que valida aquest accés
- els permisos finals sobre el recurs

### Configuració d'impressores compartides

En el cas de la impressió, el raonament és semblant. Primer cal tenir la impressora correctament definida al sistema i gestionada pel servei corresponent, que en Linux acostuma a ser CUPS. Després s'ha de decidir si aquesta impressora quedarà disponible a la xarxa i sota quines condicions.

Per tant, la configuració d'un recurs d'impressió no és només instal·lar una impressora, sinó també:

- registrar-la al servidor
- comprovar que el sistema la reconeix
- decidir si es comparteix o no
- provar-ne l'accés des dels clients

## 6.9 Proves de funcionament i ús de fitxers i impressores en una xarxa heterogènia

Després d'instal·lar un servei i configurar un recurs, el pas imprescindible és comprovar que tot funciona de veritat. Aquest punt és especialment important a la RA06, perquè la interoperabilitat només es pot donar per bona quan el recurs és accessible des de sistemes diferents i el seu ús és realment funcional.

En el cas d'una carpeta compartida, la prova no s'hauria de limitar a veure si el recurs apareix a la xarxa. També convé verificar si l'usuari es pot autenticar correctament, si pot entrar a la carpeta, si pot llegir-ne el contingut i, quan pertoqui, si hi pot escriure o modificar fitxers.

### Exemple de comprovació sobre una carpeta compartida

Una seqüència de comprovació raonable seria aquesta:

- el client localitza el servidor a la xarxa
- intenta accedir al recurs compartit
- introdueix les credencials, si cal
- comprova si pot llegir fitxers
- prova si pot crear o modificar un document, si té permisos d'escriptura

Si algun d'aquests passos falla, el problema pot estar en la connectivitat, en l'autenticació, en la configuració del servei o en els permisos del recurs.

### Proves en impressores compartides

Amb les impressores passa exactament el mateix. No n'hi ha prou amb instal·lar la impressora al servidor i donar per fet que ja està disponible. Cal comprovar si els clients la poden veure, si la poden seleccionar i si realment poden enviar-hi treballs.

Una prova mínima d'impressió acostuma a consistir en:

- detectar la impressora compartida des del client
- afegir-la o connectar-s'hi
- enviar-hi una pàgina de prova
- verificar que la cua de treball es processa correctament

### Treball en grup en un entorn mixt

La curricular també introdueix una idea pràctica molt clara: no es tracta només que un únic usuari pugui entrar a un recurs, sinó que es pugui treballar en grup amb fitxers i impressores compartides des de diferents equips.

En aquest sentit, una bona prova no és només una connexió puntual, sinó un ús compartit i sostingut del recurs:

- diversos usuaris entrant a la mateixa carpeta
- diferents equips imprimint sobre la mateixa impressora
- comprovació que cada usuari veu només allò que li correspon

## 6.10 Documentació de la configuració

Quan un servidor de compartició ja està en funcionament, l'últim pas no és donar el treball per acabat, sinó documentar allò que s'ha configurat. Això és important perquè, si el servei s'ha d'ampliar, revisar o reparar en el futur, la documentació evita haver de reconstruir tota la configuració des de zero.

En el cas d'aquesta RA, documentar no vol dir fer un informe excessivament llarg, sinó deixar constància de les dades essencials del servei i dels recursos compartits. Si el servidor ofereix carpetes i impressores a una xarxa mixta, el més important és que quedi clar què s'ha configurat, com s'hi accedeix i quins usuaris o grups hi tenen permís.

### Informació mínima que convé documentar

En una configuració d'aquest tipus, com a mínim hauria de quedar reflectit:

- el servei instal·lat
- el nom o adreça del servidor
- el recurs compartit configurat
- la ruta real del recurs, si és una carpeta
- els usuaris o grups autoritzats
- el tipus de permisos aplicats
- les proves de funcionament realitzades

### Exemple simple de documentació

Un registre tècnic senzill podria tenir un format com aquest:

```text id="5f3vjt"
Servei: Samba
Servidor: srv-linux
Recurs compartit: compartit
Ruta real: /srv/compartit
Usuaris autoritzats: professorat, administracio
Permisos: lectura i escriptura
Clients de prova: Windows 11, Ubuntu Desktop
Resultat de les proves: accés correcte i creació de fitxers verificada
```

Per a una impressora compartida es podria seguir la mateixa lògica:

```text id="vrjyzq"
Servei: CUPS
Servidor: srv-linux
Impressora: aula1
Tipus d'accés: compartida en xarxa
Clients de prova: Windows 11, Ubuntu Desktop
Resultat: pàgina de prova impresa correctament
```

### Tancament del bloc

La interoperabilitat no s'entén només com una idea teòrica, sinó com un procés complet que passa per:

- entendre l'escenari heterogeni
- instal·lar i configurar serveis
- compartir recursos reals
- provar-los des de sistemes diferents
- documentar el resultat final
