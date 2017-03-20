---
title: Twig Extensions
draft: true
date: 2017-02-19 20:20
categories:
    - programació
tags:
    - symfony
    - php
    - twig
---

El principal motiu per tal d'utilitzar Twig Extensions és el fet de reutilitzar troços de codi dins de twig a tot arreu on ho necessitem. D'aquesta manera aconseguirem no duplicar codi i ja podrem dir que fem clean code.

En el moments que veiem que estem posant massa llògica als nostres fitxers de Twig és quan hem de crear una extensió.

Per altra banda, al utilitzar Twig Extensions, passem la nostra llògica a fitxers de php, i no als fitxers de Twig. Ja sabem que Twig té un número limitat de funcions. En canvi, en php tenim més possibilitats per a solucionar el mateix problema.

Expliquem el procés per a la creació d'una Extensió de Twig a partir d'un exemple.



