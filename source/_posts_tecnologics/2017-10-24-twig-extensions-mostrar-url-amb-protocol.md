---
title: Twig Extensions - Filtre de twig per a afegir el protocol a una URL en el cas de que no hi sigui
draft: true
date: 2017-10-24 22:22
categories:
    - programació
tags:
    - symfony
    - php
    - twig
---

Avui us poso el codi d'una twig extension que he creat i que és molt útil per a quan es volen mostrar adresses URL que no han estat validades al introduir-les al sistema i que per tant alguns usuaris te la introdueixen amb el protocol i d'altres no.
Si al final es vol fer un enllaç a aquesta url, necessitarem saber el protocol i per tant si no està el http el posarem per a crear el link, igual que amb el https, o ftp, i si ja està el protocol no farem res.

Al utilitzar Twig Extensions, passem la nostra llògica a fitxers de php, i no als fitxers de Twig. Ja sabem que Twig té un número limitat de funcions. En canvi, en php tenim més possibilitats per a solucionar el mateix problema.

Primer de tot hem de crear la nostra classe que extendrà de TwigExtension

~~~php
class UrlProtocol extends \Twig_Extension
{
    public function getFilters()
    {
        return array(
            new \Twig_SimpleFilter('urlprotocol', array($this, 'urlFilter')),
        );
    }

    public function urlFilter($url)
    {
        if (!preg_match("~^(?:f|ht)tps?://~i", $url)) {
            $url = "http://" . $url;
        }
        return $url;
    }

    public function getName()
    {
        return 'urlprotocol';
    }
}
~~~

Hem creat un filtre que s'anomerà urlprotocol i que busca si un string té o no té http, ftp o https, si ho té no remplaça res, sinó posa http

A continuació registrem la extensió de Twig dins els serveis de Symfony


~~~php
services:
    app.twig.urlprotocol_extension:
        class: AppBundle\Twig\UrlProtocol
        tags:
                - { name: twig.extension }
~~~

Finalment només haurem d'utilitzar el filtre dins dels fitxers de twig, cal a dir que d'aquesta forma podem reaprofitar codi entre diferents templates.

{% verbatim %}
~~~
{% if entity.facebook is not null %}
    <a target="_blank" href="{{ entity.facebook | urlprotocol }}"><i class="fa fa-facebook"></i></a>
{% endif %}
~~~
{% endverbatim %}



