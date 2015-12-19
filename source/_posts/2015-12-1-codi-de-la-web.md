---
title: Codi d'aquesta web - Sculpin
categories:
    - blog-tecnologic
tags:
    - php
    - sculpin
    - markdown
---

Si algú té curiositat per veure com està feta aquesta web, us passo l'enllaç al github, qualsevol errada / millora serà
benvinguda. <a target='_blank' href='https://github.com/sitobcn82/ruben.baraut'>https://github.com/sitobcn82/ruben.baraut</a>

Tot i ser programador de Symfony, he decidit fer aquesta web amb un generador PHP de contingut estàtic que funciona amb
fitxers de tipus Markdown i que utilitza Twig per a renderitzar els templates.

El generador en qüestió és  <a target='_blank' href="https://sculpin.io/">Sculpin</a>.

El que m'ha fet decidir per aquesta eina són els següents punts:

- El temps de desenvolupament de la web ha estat de 4 hores, i lo que més feina a portat ha estat buscar un template i aplicar-lo.
- Sculpin genera el contingut mitjançant fitxers Markdown, com sóc programador, no és un problema per mi generar el
contingut utilitzant marcatge. Apart d'això, fet d'aquesta forma no necessito Back-End  amb tot el que això suposa
(usuaris, seguretat, pujada de fitxers, etc...)
- Es poden afegir llibreries i bundles de symfony utilitzant composer, per tant puc afegir-li totes les funcionalitats que vulgui.
- La web incrementa la velocitat de carrega d'una forma increïble.

El template utilitzat està extret de  <a target='_blank' href='http://pozhilov.com'> **Sergey Pozhilov**</a>

Apart d'això, el codi base d'esculpin està extret de <a target='_blank' href="https://github.com/acelaya/blog">Alejandro Celaya </a>. 
Bàsicament he agafat aquest projecte perquè els css per defecte ja em semblaven bastant correctes, i apart, ja tenia montada
una estructura que m'anava perfecte per al que jo volia fer. Per altra banda, he vist que a la seva web feia servir un cercador
i que funciona molt bé i que a mi també m'agradaria fer-lo servir

Per tal de generar els comentaris dels posts, utilitzo la plataforma Disqus, que s'integra perfectament amb sculpin només
afegint-li el identificador d'aquesta plataforma a dins dels fitxers de configuració de Sculpin.