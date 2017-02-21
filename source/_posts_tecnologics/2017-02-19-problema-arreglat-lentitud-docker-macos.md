---
title: Problema arreglat: Lentitud de Docker amb MacOs
date: 2017-02-19 20:20
categories:
    - programació
    - sistemes
tags:
    - docker
    - macos
    - dev environment
    - docker-osx-dev
    - symfony
    - php
---
Per fi, i després de molts intents, he aconseguit tenir un entorn de desenvolupament per Symfony a la meva màquina en MacOs. 
Feia molt temps que li donava voltes i encara no ho havia vist clar en cap moment.
He provat desenvolupar en local, vagrant, desenvolupar en remot, menjar sense sucre, ...

Una cosa tan simple com un NGINX + PHP + MYSQL/MONGO, m'ha portat molts mals de cap.

Des de fa un temps estic provant Docker, i la veritat és que em sento "moderno" i això que no gasto ulleres de pasta. Tothom està parlant de docker i jo no podia ser menys.
A la feina, fa un temps que treballo amb docker i realment estic molt content. A casa, l'objectiu era treballar amb la mateixa agilitat que ho faig a la feina 
però amb una màquina que no té linux i que té MacOS.

Fer funcionar docker en MacOs no és cap misteri, de fet, és el mateix que fer-ho sobre Linux.
El problema apareix quan intentes treballar amb projectes que tenen 10 milions de vendors i que el sistema no sap gestionar el mapeig de tants fitxers cap a dins del contenidor.

Sé que no és cap proesa, però us ben asseguro que estava per comprar-me el portàtil més barat del mercat, posar-hi qualsevol distribució de Linux i que aquest fos el meu ordinador per treballar des del sofà.

Doncs bé, tot el problema de lentitud que tenia s'ha sol·lucionat gràcies a aquesta llibreria: <a href="https://github.com/brikis98/docker-osx-dev">docker-osx-dev</a>.

La forma d'utilització és molt simple. Just abans d'arrancar els contenidors amb docker-compose o bé amb el run, es fa la crida a docker-osx-dev. A continuació, 
ell sol s'encarrega de gestionar els volums que tens definits al docker-compose i fer el rsync per a que el contenidor vegi en tot moment els fitxers mapejats.
L'únic problema d'aquesta llibreria, com el seu propi autor indica, és que la comunicació es fa de forma unidireccional. No hi ha rsync entre els 
fitxers de dins del contenidor cap a la màquina host.
Què vol dir això ? Per exemple, no podreu mapejar els fitxers de logs del nginx o del propi symfony, cap a fora del contenidor i que per tant, seran fitxers volàtils. En principi, no hauria 
de ser cap problema ja que estem en un entorn de desenvolupament. Sigui com sigui, prefereixo que l'entorn em vagi ràpid i no em suposa cap problema perdre els 
fitxers de logs cada cop que arranco el contenidor.

Aprofito per compartir la configuració que utilitzo per a fer funcionar projectes en symfony amb docker.

Els contenidors que arranca el docker-compose són els següents:

- web: Imatge oficial de nginx amb el port 80 mapejat cap al 8080 de la màquina host i amb un volum pel documentRoot i un altre pel fitxer de configuració del host.
- mysql: Imatge oficial de Mysql executant-se mapejat al port 3306 i amb un volum per a persistir la base de dades un cop el contenidor es destrueix. Al arrancar el contenidor crea la base de dades amb l'usuari i password que hi ha a les variables d'entorn.
- php: Imatge personalitzada de Php5.6 afegint-li el xdebug, llibreries de php com el intl, soap, ..., el composer, ruby i el compass
- mongo: Imatge oficial de Mongo mapejat al port 27017 i igual que en el cas del mysql amb un volum per a persistir la base de dades. 

El link al repositori per si el voleu descarregar és aquest: <a href="https://github.com/sitobcn82/docker_lamp_symfony.git">docker_lamp_symfony</a>
 
Au! A programar!