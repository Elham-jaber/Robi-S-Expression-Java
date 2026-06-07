# Robi : Interpreteur de S-Expressions et Application Distribuee

Projet de Synthese : Licence 3 Informatique : Universite de Bretagne Occidentale : 2025/2026

Equipe : Elham Jaber / Mohamad El Chamaa / Akram Alnezami / Lenaic Hounleba

Encadrants : Alain Plantec / Yann Glemarec


## Idee logique du projet

Le projet part d'une idee simple : construire pas a pas un langage de script capable de piloter des elements graphiques 2D, puis transformer ce langage en systeme distribue multi-clients. Chaque exercice etend le precedent sans jamais casser ce qui existe. Le resultat est un interpreteur de S-Expressions inspire de Lisp/Scheme, connecte a un serveur TCP, avec une interface Swing et des agents autonomes appeles Robibots.

Chaque commande suit la forme (receiver commande arg1 arg2). Le parseur transforme ce texte en arbre de noeuds SNode exploitables par l'interpreteur. L'idee directrice est qu'ajouter une fonctionnalite ne doit jamais modifier ce qui fonctionne deja : c'est le patron Command qui garantit cela. Chaque commande est une classe Java independante, testable isolement. L'architecture Environment/Reference elimine tout conditionnel dans la boucle principale : Environment associe un nom a une Reference via une HashMap, Reference encapsule un objet graphique et son dictionnaire de commandes. La seconde partie transforme cet interpreteur en application distribuee : le serveur TCP ecoute sur le port 4242, un thread par client, un interpreteur partage. Les Robibots sont synchronises cote serveur et diffuses a tous les clients connectes.


## Pipeline CI/CD

Un fichier .gitlab-ci.yml declenche automatiquement la chaine qualite a chaque push ou merge request. Le pipeline comprend 5 etapes sequentielles. Si une etape echoue, les suivantes ne s'executent pas. Les 127 tests passent en 1 minute 5 secondes.

Le pipeline est court car toute la configuration reelle vit dans le pom.xml. Le fichier .gitlab-ci.yml appelle 5 commandes Maven. Maven connait les regles, les plugins et les seuils. Le pipeline orchestre, Maven execute.

```
build : test : coverage : analyze : report
  OK      OK      OK          OK       OK
```

### Etape 1 : build

Outil : Maven

Role : compilation complete des 3 modules du projet : 2DGraphicCore / SParser / Robi

### Etape 2 : test

Outil : JUnit

Role : execution des 127 tests unitaires automatises

### Etape 3 : coverage

Outil : JaCoCo

Role : mesure du taux de couverture du code par les tests

### Etape 4 : analyze

Outil : Checkstyle / SpotBugs

Role : analyse statique du code : detection des mauvaises pratiques et des bugs potentiels

### Etape 5 : report

Outil : Maven Site

Role : generation du rapport qualite consolide


## Strategie de branches

main : code stable : pipeline complet requis avant merge

release : versions livrables

dev : integration continue des developpements en cours

feat/* : une branche par fonctionnalite


## Modules du projet

2DGraphicCore : couche graphique bas niveau : GSpace / GRect / GOval / GImage / GString

SParser : parseur de S-Expressions : compilation d'une chaine de caracteres en arbre de SNode

Robi : projet principal : exercices / client Swing / serveur TCP / Robibots


## Progression des exercices

Exercice 1 : animation graphique de base

Exercice 2 : fondations de l'interpreteur

Exercice 3 : patron de conception Command

Exercice 4 : architecture Environment / Reference

Exercice 5 : conteneurs imbriques et notation pointee

Exercice 6 : scripts utilisateur via addScript et le mot-cle self

Exercice 7 : expressions / conditionnelles / boucles


## Application distribuee

Serveur TCP : port 4242 : un thread par client : un interpreteur RobiInterpreter partage

Protocole : le client envoie une S-Expression texte : le serveur repond avec un objet Java serialise

Selon la requete la reponse est une liste de SNode / un tableau d'octets PNG / une chaine JSON

Persistance : sauvegarde et rechargement de l'etat complet en JSON

Capture d'ecran : transmission du rendu serveur au client sous forme PNG

Robibots : machine a etats finie implementee from scratch : synchronisation centralisee cote serveur via BotUpdateListener


## Stack technique

Langage : Java : environnement Eclipse

Build et gestion de dependances : Maven : pom.xml multi-module

Tests unitaires : JUnit : 127 tests automatises

Couverture de code : JaCoCo

Analyse statique : Checkstyle / SpotBugs

Integration continue : GitLab CI/CD

Interface graphique : Java Swing / AWT

Communication reseau : TCP : sockets Java : port 4242

Persistance : serialisation JSON
