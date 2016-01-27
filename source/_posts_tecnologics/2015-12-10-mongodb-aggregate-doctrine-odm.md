---
title: Doctrine ODM i pipeline de mongo Aggregate Framework
categories:
    - programació
tags:
    - php
    - symfony2
    - mongodb
    - doctrine ODM
---
Segurament molts de vosaltres utilitzeu MongoDb per a persistir les dades. Per a fer-ho s'ha d'utilitzar Doctrine ODM.

A continuació, us explicaré una forma senzilla de fer servir el Framework Aggregate i així podeu fer agrupacions de les 
dades ( similar al group by de SQL) o bé simplement per a fer una seqüència de querys que s'encadenen una a una i ens 
permeten utilitzar el resultat d'una query com a entrada de la query anterior.

Primer de tot agafem el document Collection que necessitem per a fer la query. En aquest cas $this és el Repository de 
qualsevol entitat.
~~~php
$dc = $this->getDocumentManager()->getDocumentCollection('PortalBundle:Portal');
~~~
A continuació, ens creem un array on anirem posant totes les operacions que volguem que es vagin executant dins del 
pipeline. En aquest cas, serà un pipeline que es composarà de tres passos (stages).

En el primer pas, fem un match per a buscar una sèrie de Identificadors de Clients
~~~php
$pipeline[] = [ '$match' => [ 'client' => $idClients ] ];
~~~
A continuació, fem un group per tal de sumar el camp totalImpresions de tots els clients trobats
~~~php
$pipeline[] = [ '$group' => [ '_id' => '', 'total' => ['$sum' => '$totalImpresions'] ] ];
~~~
Finalment fem una projecció per tal de que els resultats siguin més tractables, és a dir extraiem el _id i retornem el 
resultat final totalimpresions
~~~php
$pipeline[] = [ '$project' => [ '_id' => 0, 'totalImpresions' => '$total' ] ];
~~~
En aquest punt només ens quedarà cridar el mètode aggregate del document Collection.
~~~php
$totalGroupedByDay = $dc->aggregate($pipeline)->toArray();
~~~
Doncs bé, per a aconseguir fer això, m'he passat dos dies de feina, digueu-me negat però la documentació del Doctrine 
en aquest cas és ben poca


