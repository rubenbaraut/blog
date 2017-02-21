---
title: Doctrine Extensions SoftDeletable amb Odm i MongoDB
date: 2016-01-27 21:30
categories:
    - programació
tags:
    - php
    - symfony2
    - mongodb
    - doctrine ODM
---

Per als que no ho coneguin, doctrine extensions és un conjunt d'utilitats o altrament dits behaviors que ens permeten 
fer tasques molt habituals dins de les nostres entitats de Symfony. Dins de les més habituals hi ha:

- Sluggable: Per a crear el típic camp slug dins de les nostres entitats.
- Translatable: Per a fer traduccions
- Timestampable : Per a afegir camps de tipus timestamp per a saber quan s'ha modificat o creat un element a la base de dades
- Loggable: Per a crear logs de les nostres taules de la base de dades i així podem tornar a un estat anterior en qualsevol 
moment o bé, podem saber exactament què és el que ha passat dins d'una taula de la base de dades.
- Tree : Per a crear estructures en forma d'arbre
- Sortable : Per a ordenar per un camp d'una entitat.

Us deixo l'enllaç del <a href="https://github.com/Atlantic18/DoctrineExtensions">repositori</a> per als que no 
els coneguin, per mi, és una de les coses bàsiques al començar qualsevol projecte ja que en un moment o altre sempre s'acaben necessitant.
La url de la documentació dins de <a href="http://symfony.com/doc/current/cookbook/doctrine/common_extensions.html">la documentació de symfony.</a>
 
Doncs bé, una de les extensions que he descobert últimament és la que s'anomena SoftDeletable. Aquesta extensió serveix per a
marcar qualsevol registre d'una taula o document de la base de dades com a esborrat. Simplement es defineix el camp que volem 
que sigui el que marca l'eliminació i l'extensió mitjançant uns listeners, fa que aquell registre de la base de dades no surti 
a cap query que es faci, per tant està esborrat però totes les dades les seguim mantenint a la base de dades, i, 
per exemple, en un futur les podem tornar a activar.

El motiu d'aquest post no és el d'explicar què fa aquesta extensió, perquè per a això està la seva documentació. 
El post està escrit perquè quan busques com instal·lar l'extensió utilitzant Doctrine ODM i Mongodb la documentació 
és un pèl confusa. En canvi, per a instal·lar-ho amb Doctrine ORM no hi ha cap problema.

Primer de tot, la documentació no està actualitzada i comenta que l'extensió SoftDeletable no soporta Doctrine ODM, 
buscant una mica per internet te n'adones que això no és així.

A continuació, explico els passos per a fer funcionar SoftDeletable sobre Doctrine ODM i MongoDb.

- Instal·lem el bundle amb composer tal i com explica el propi autor, a continuació registrem com sempre el bundle dins del Appkernel
- Després configurem el bundle dins de app/config.yml i activem l'extensió softdeleteable
~~~yml
    stof_doctrine_extensions:
        default_locale: %locale%
        mongodb:
            default:
                softdeleteable: true
~~~
- Per tal que funcioni amb mongo també hem de fer el següent dins de la configuració del doctrine. La part important d'aquest codi
és on es posen els filtres i es configura la Classe que gestionarà que cada cop que fem una query, s'activi el filtre corresponent
i permeti que els elements marcats com a "esborrats" no surtin al resultat de les cerques.
~~~yml
# Doctrine ODM Configuration
doctrine_mongodb:
    connections:
        default:
            server: "%database_driver%://%database_host%:%database_port%" #mongodb://localhost:27017
            options:
                username: "%database_user%"
                password: "%database_password%"
                db: "%database_name%"
    default_database: "%database_name%"
    document_managers:
        default:
            auto_mapping: true
            filters:
                softdeleteable:
                    class:             Gedmo\SoftDeleteable\Filter\ODM\SoftDeleteableFilter
                    enabled: true
~~~

A continuació ja tenim configurada l'extensió (és la part que no queda clara dins de la documentació de l'extensió per mongo ODM)
i podem seguir tal i com diu la documentació configurant les nostres Collections del mongo. Per a fer-ho, agafem qualsevol entitat que 
volguem implementar el SoftDeletable i li afegim un camp que ens guardarà quan s'ha esborrat l'element. 
Per a fer-ho hem de dir-li dins de la configuració de la collecció, quin camp és el que farem servir per aquest fet i després crear 
el camp dins de la classe.

~~~php
<?php

namespace xxx\ClientBundle\Document;

use Doctrine\Common\Collections\ArrayCollection;
use Doctrine\Common\Collections\Collection;
use Doctrine\ODM\MongoDB\Mapping\Annotations as MongoDB;
use Symfony\Component\Validator\Constraints as Assert;
use Gedmo\Mapping\Annotation as Gedmo;

/**
 * @MongoDB\Document(
 *   collection="Client"
 * )
 * @Gedmo\SoftDeleteable(fieldName="deletedAt", timeAware=false)
 *
 * @MongoDB\HasLifecycleCallbacks()
 */
class Client
{

    /**
     * @MongoDB\Id(strategy="auto")
     */
    protected $id;
    
    /**
     * @var \DateTime
     * @MongoDB\Date
     */
    private $deletedAt;

...
    
~~~

I li crearem els seus corresponents sets i gets, com qualsevol altre propietat d'una classe.

~~~php
    public function getDeletedAt()
    {
        return $this->deletedAt;
    }

    public function setDeletedAt($deletedAt)
    {
        $this->deletedAt = $deletedAt;
    }
~~~

A partir d'aquí i després de fer el ~~~php php app/console doctrine:schema:update ~~~ l'extensió està completament habilitada per a funcionar.

Per tal de marcar un registre com eliminat, només hem de cridar el mètode remove del CollectionManager de la mateixa 
forma que fariem per esborrar un registre completament.

~~~php
foreach ($user->getClients() as $client) {
            $this->remove($client);// soft delete!!
        }
~~~

A partir d'aquest punt, aquests clients ja no apareixeran a cap de les querys però els registres seguirant existint a la base de dades.
Si en un futur es volguessin tornar a habilitar s'hauria de posar a NULL el camp $deletedAt de la col·lecció.

~~~php
foreach ($user->getClients() as $client) {
            $client->setDeletedAt(null);
}
~~~

Hi ha casos en els que no ens interessa que s'ocultin els registres, com per exemple dins del BackEnd, en aquests casos,
l'extensió ens permet deshabitar temporalment la funcionalitat de l'extensió. Per a fer-ho, s'ha de cridar el mètode disable
de la col·lecció de filtres del doctrine odm.

~~~php
$this->getManager()->getFilterCollection()->disable('softdeleteable');
~~~

No cal dir que per a habilitar-ho s'hauria de cridar el mètode enable.  El que faig jo dins dels meus codis és, dins de qualsevol funció d'admin a la qual vulgui veure els elements esborrats
~~~php
if ($this->getManager()->getFilterCollection()->isEnabled('softdeleteable')) {
            $this->getManager()->getFilterCollection()->disable('softdeleteable');
}
~~~






  





