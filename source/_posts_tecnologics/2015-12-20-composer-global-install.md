---
title: Instal·lació global de paquets amb composer
draft: true
categories:
    - programació
tags:
    - composer
    - symfony2
---

Composer permet la instal·lació de paquets/vendors de forma global a tot l'ordinador i no depenent del projecte.
Això és molt útil quan vols instal·lar llibreries externes, com per exemple PhpUnit o bé eines per a debugar, i saps que
les necessitaràs per a tots els projectes amb els que treballaràs. 

També seria molt útil en el cas de servidors on estiguin allotjats varis projectes i que es fecin servir els mateixos vendors 
a un projecte que a l'altre, si s'instal·len de forma global, només s'han d'instal·lar un cop,
mentres que posant les dependències a cada projecte es necessitarà més espai i es crea una duplicació innecessària de codi
de les llibreries externes.

Us deixo la comanda que instal·laria el phpunit de forma global:

~~~
composer global require phpunit/phpunit
~~~
L'anterior comanda instal·la la dependència a la carpeta ~/.composer/vendor i els executables a  ~/.composer/vendor/bin
Per a actualitzar el paquet utilitzaríem:
~~~
composer global update
~~~

Per a esborrar-lo simplement esborraríem la linea corresponent del fitxer ~/.composer/composer.json i tornaríem a executar 
~~~
composer global update
~~~

Finalment, per tal de poder utilitzar la llibreria, s'hauria d'afegir al PATH del sistema. D'aquesta manera sempre tindrem 
disponible des de línea de comandes el paquet acabat d'instal·lar.


La informació d'aquest post, ha estat extreta <a href="https://akrabat.com/global-installation-of-php-tools-with-composer/">d'aquí</a>