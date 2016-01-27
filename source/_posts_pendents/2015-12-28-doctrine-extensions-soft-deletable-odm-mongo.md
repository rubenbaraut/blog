---
title: Doctrine Extensions SoftDeletable amb Odm i MongoDB
draft: true
categories:
    - programació
tags:
    - php
    - symfony2
    - mongodb
    - doctrine ODM
---

Per als que no ho coneguin, doctrine extensions és un conjunt d'utilitats o altrament dits behaviors que ens permeten 
fer tasques molt habituals dins de les nostres entitats de Symfony. Dins de les més habituals hi han:

- Sluggable: Per a crear el típic camp slug dins de les nostres entitats.
- Translatable: Per a fer traduccions
- Timestampable : Per a afegir camps de tipus timestamp per a saber quan s'ha modificat o creat un element a la base de dades
- Loggable: Per a crear logs de les nostres taules de la base de dades i així podem tornar a un estat anterior en qualsevol 
moment o bé, podem saber exactament què és el que ha passat dins d'una taula de la base de dades.
- Tree : Per a crear estructures en forma d'arbre
- Sortable : Per a ordenar per un camp d'una entitat.

Us deixo l'enllaç del <a href="https://github.com/Atlantic18/DoctrineExtensions">repositori</a> per als que no 
els coneixin, per mi, és una de les coses bàsiques al començar qualsevol projecte ja que en un moment o altre sempre s'acaben necessitant.
La url de la documentació dins de <a href="http://symfony.com/doc/current/cookbook/doctrine/common_extensions.html">la documentació de symfony.</a>
 
Doncs bé, una de les extensions que he descobert últimament és la que s'anomena SoftDeletable. Aquesta extensió serveix per a
marcar qualsevol registre d'una taula o document de la base de dades com a esborrat. Simplement es defineix el camp que volem 
que sigui el que marca la eliminació i la extensió mitjançant uns listeners, fa que aquell registre de la base de dades no surti 
a cap query que es faci, per tant està esborrat però totes les dades les seguim mantenint a la base de dades, i, 
per exemple, en un futur les podem tornar a activar.

El motiu d'aquest post no és el d'explicar què fa aquesta extensió, perquè per a això està la seva documentació. 
El post està escrit perquè quan busques com instal·lar la extensió utiulitzant Doctrine ODM i Mongodb la documentació 
és un pèl confusa. En canvi, per a instal·lar-ho amb Doctrine ORM no hi ha cap problema.

Primer de tot, la documentació no està actualitzada i comenta que la extensió SoftDeletable no soporta Doctrine ODM, 
buscant una mica per internet te n'adones que això no és així.

A continuació, explico els passos per a fer funcionar SoftDeletable sobre Doctrine ODM i MongoDb.





~~~php
$pipeline[] = [ '$group' => [ '_id' => '', 'total' => ['$sum' => '$totalImpresions'] ] ];
~~~