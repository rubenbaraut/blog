---
title: Deploy automàtic d'aplicació Symfony utilitzant git
draft: true
categories:
    - programació
tags:
    - php
    - symfony2
    - git
---
Una de les tasques més faragoses i que més mal de caps sol portar a un desenvolupador és fer el deploy d'una aplicació al servidor de 
producció. Recordo fa uns anys, en els meus inicis amb Php, que per a pujar una aplicació al servidor ho feia utilitzant FTP.

Avui en dia, poques persones haurien de quedar que encara féssin anar el FTP per a pujar tot el codi i llibreries d'una aplicació.

~~~php
$pipeline[] = [ '$group' => [ '_id' => '', 'total' => ['$sum' => '$totalImpresions'] ] ];
~~~