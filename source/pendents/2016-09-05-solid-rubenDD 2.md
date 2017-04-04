---
title: Instal·lació global de paquets amb composer
draft: true
---

Al principi del llibre es parla de patrons de disseny.
Resum dels principals:
- Factories
- Repositories

Principis SOLID:
- S de Single Responsability : Cada classe que creem només ha de tenir una renspobilitat. Només ha de fer una cosa. 
 Exemple:
 ~~~php
 class User {
    public function getName() {}
    public function getEmail() {}
    public function find($id) {}
    public function save() {}
 }
 ~~~
 Té dues responsabilitats. Per un costat, extreu informació de l'usuari, recupera l'estat de l'usuari.
 Per altra banda s'encarrega de la busqueda i de la persistència de l'usuari a la base de dades. Per a solucionar-ho, 
 hauríem de separar aquesta classe en dues.
  ~~~php
 class User {
    public function getName() {}
    public function getEmail() {}
 }
  ~~~
  ~~~php
 class UserRepository {
    public function find($id) {}
    public function save(User $user) {}
 }
  ~~~
  Per a aplicar aquest principi hem de tenir clar que la cerca/persistència sempre la farem des dels Repositoris, la creació
  d'objectes des de les Factories, les funcions pròpies des dels serveis.