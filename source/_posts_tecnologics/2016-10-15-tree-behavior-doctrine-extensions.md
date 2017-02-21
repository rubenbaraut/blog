---
title: Doctrine Extensions Tree Behavior i onDelete="CASCADE" 
date: 2016-09-04 21:30
categories:
    - programació
tags:
    - doctrine extensions
    - tree
    - symfony2
    - php
---

Per a montar estructures en forma d'arbre dins dels projectes fets amb symfony, hi ha una extensió que funciona molt bé, forma part dels doctrine extensions i facilita
de forma ràpida, mètodes i repositoris per tal d'accedir a les vostres estructures en forma d'arbre.

He de dir que funciona molt bé, i en cap cap m'he vist penalitzat per lentitud de les querys. Facilita mètodes per a buscar els fills d'un node, els germans, 
reorganitzar l'arbre quan s'elimina un element, etc .. 
Si voleu donar-hi un cop d'ull us facilito <a href="https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/tree.md">l'enllaç al repositori</a>

L'entrada no és per a explicar les funcionalitats de tot aquest Bundle, que si ho voleu ho faig més endavant. Aquest post és només per a deixar clar a la gent, un detall que
apareix a la pròpia pàgina del Bundle, on explica com configurar-lo, i que si ho apliqueu com està allà, us pot portar algún mal de cap.

~~~php
 /**
     * @Gedmo\TreeRoot
     * @ORM\ManyToOne(targetEntity="Category")
     * @ORM\JoinColumn(referencedColumnName="id", onDelete="CASCADE")
     */
    private $root;

    /**
     * @Gedmo\TreeParent
     * @ORM\ManyToOne(targetEntity="Category", inversedBy="children")
     * @ORM\JoinColumn(referencedColumnName="id", onDelete="CASCADE")
     */
    private $parent;
~~~

Vull posar especial ènfasi a on diu onDelete="CASCADE", ja sé que tots sabeu llegir, però hi ha molts cops que configurem Bundles que ni llegim el que estem posant. 
Tradueixo al català: QUAN S'ESBORRI="ELIMINA EN CASCADA" !!!. I què vol dir això ? Doncs sí, just el que esteu pensant, que depèn de com tingueu montat el vostre arbre, és possible
que quan elimineu algún element de l'arbre, s'esborri tot el vostre arbre. A més a més, comentar que aquesta clàusula està posada a nivell del JOIN, i que per tant, l'esborrat el realitza
la base de dades utilitzant foreign keys, pel que amb un obrir i tancar d'ulls tindreu totes les vostres dades eliminades. 

Resum: Vigilem amb els CASCADES a tot arreu, i sobretot, vigilem amb els cascades removes i els cascades deletes. Ens poden portar molts mal de caps. Jo prefereixo no posar-los
i fer l'esborrat de forma manual. Avisats quedeu!


