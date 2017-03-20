---
title: Liip Image Bundle
draft: true
date: 2017-02-21 22:20
categories:
    - programació
tags:
    - bundles
    - LiipImagineBundle
    - symfony2
    - php
---
En aquesta ocasió m'agradaria presentar el Liip Imagine Bundle, una llibrería que es fa servir per a aplicar diferents filtres a les nostres imatges
per tal de mostrar-les a diferents parts de la nostra web. Aquests filtres poden ser, tant com per a crear un Thumbnail de les imatges a mida més petita, 
transformar les imatges per tal de retallar-les si no hi caben a l'espai que tenim, posar-les en blanc i negre, posar-lis un versionat, menjar sense sucre ... La veritat és que
el bundle ofereix molts tipus de filtres i una extensa configuració. Us recomano la lectura detallada de la seva configuració: 
<a href="http://symfony.com/doc/current/bundles/LiipImagineBundle/index.html">LiipImagineBundle</a>
 
La cosa per a mi més interessant en la seva utilització és que es fa tot en temps d'execució. És a dir, quan carreguem la pàgina i sense un tractament previ de la imatge
se'ns fa el thumbnail de la mateixa. El propi bundle genera una cache, per a que no es tingui de realitzar la operació cada vegada que es solicita una imatge, d'aquesta manera, 
només es penalitza el primer accés a la imatge però després la carrega es fa com la de qualsevol altre imatge de nostra web.

La forma d'utilització sempre passa pels següents passos:

Crear el filtre al fitxer de configuració de symfony

~~~php
liip_imagine:
 resolvers:
    default:
       web_path: ~

 filter_sets:
     cache: ~
     front_page_product_image:
         quality: 100
         filters:
             thumbnail: { size: [500, 400], mode: inset ,allow_upscale: true}
~~~

Aplicar el filtre dins de les nostres plantilles amb Twig de la forma
{% verbatim %}
~~~
<img src="{{asset('/relative/path/to/image.jpg') | imagine_filter('front_page_product_image')}}" />
~~~
{% endverbatim %}


M'agradaria finalitzar el post comentant dues coses que hauriem de tenir present respecte a aquest Bundle:
 
per tal de que el bundle funcioni en la seva màxima esplendor es necessiten activar les següents extensions de Php. 

~~~php
php5-gd , gmagick, imagick, php_exif, php_fileinfo
~~~

Aquest punt pot portar més d'un mal de cap, en el meu cas per exemple, el no tenir-les instal·lades feia que no se'm generes correctament el directari de la cache 
on es guarden les imatges resultants de cada un dels filtres.

crec que per estalviar-nos problemes amb les imatges sempre, i dic sempre, hauriem d'utilitzar aquest bundle per a mostrar les nostres imatges. 
És la única forma de que se'ns mostrin correctament les imatges a les nostres webs. Si ens fiem només del css i Javascript es pot donar el cas de 
que veiem imatges deformades, mal proporcionades, etc ... En canvi, utilitzant-lo, ens assegurem que nosaltres oferim sempre la imatge que toca (en quan a mida) 
i després des del Frontend hi haurà menys feina per a posar-les boniques. 