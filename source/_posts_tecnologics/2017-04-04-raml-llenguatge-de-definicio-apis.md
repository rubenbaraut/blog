---
title: Raml - Llenguatge de definició d'APIS
date: 2017-03-21 22:20
categories:
    - programació
tags:
    - API
    - yml    
---
Aquesta tarda he anat a veure un meetup organitzat per la gent <a href="https://www.meetup.com/es-ES/ApiAddictsBCN/">d'ApiAddictsBCN</a> que parlàven de Raml i m'ha 
sembla prou interessant com per a treure un post al meu blog. Cal dir que cap de les coses de 
les que parlaré les he provat i que parlo des de l'absoluta ignorància.

Raml és un llenguatge open source que serveix per a definir API's gràcies a la utilització de fitxers yml. 

Ens ajuda a documentar una API i que aquesta documentació serveixi per a totes les parts
involucrades en la utilització de l'API. També es pot fer servir com a eina per a implementar metodologies com API-first i bàsicament posa els 
fonaments al que serà la nostra futura aplicació. 

El món de les API's és un món molt extens i avui dia es fan API's absolutament amb tots els llenguatges de programació existents. 
Jo personalment les desenvolupo amb php (symfony), amb el <a href="http://symfony.com/doc/current/bundles/FOSRestBundle/index.html">FosRestBundle</a> 
i fins ara generava la documentació aml el <a href="http://symfony.com/doc/current/bundles/NelmioApiDocBundle/index.html">NelmioApiDocBundle</a>. Crec que això s'ha acabat. 

Generar la documentació de les API al mateix temps que desenvolupem crec que és un error. Hem de saber abans de començar a implementar l'API, 
quins seran els seus mètodes permesos, 
la seva autenticació, els tipus d'error que retornarà, exemples d'utilització, menjar sense sucre, etc. D'aquesta manera, totes les parts poden començar a desenvolupar perquè ja saben 
què es trobaran en un futur. La gent del front, la gent de mobile, etc.. tothom té la documentació i exemples Fake per a començar a programar. Apart d'això, la pròpia eina facilita un sandbox per a que es facin proves de la pròpia API 
(evidentment sempre amb dades Fake).

Un altre problema que tenim sempre és l'actualització de la documentació de les nostres API's, aquest problema el seguirem tenint. 
Tot i així, la modificació de fitxers de text (yml) hauria de ser molt més fàcil i no hauriem de perdre tant temps com posant anotacions al 
codi de la nostra aplicació.

Raml al estar basat en fitxers yml, es fa molt ràpid d'entendre i espero que sigui molt fàcil d'aprende. Apart d'això, incorpora temes de 
programació que a qualsevol programador 
li hauria de provocar una mica de trempera. Per exemple, es poden fer includes per a no repetir el codi, es poden fer herencies de fitxers, etc.
És a dir, no és un simple fitxer de text, és un format molt orientat a desenvolupadors.

Per a començar a provar-ho es pot utilitzar directament el API manager que facilita mulesoft a la pàgina de <a href="https://anypoint.mulesoft.com">mulesoft</a>
 que és l'empresa que hi ha darrere del Raml.
Des d'aquesta pàgina es pot començar a escriure i a organitzar els yml's i es pot anar veient el resultat dinàmicament. 
Després des d'aquí podrem fer exportacions per a utilizar les crides a la nostra API des del Postman o des del gestor que volguem.

Propòsit d'any nou: la pròxima API que hagi de desenvolupar, la començaré definint-la amb RAML.