# AGENTS.md — Cinéma Notebook

Ce fichier définit les règles de travail pour toute interaction avec le projet **Cinéma Notebook**.

## Règle principale

**Au début de chaque nouveau chat consacré à ce projet, lire ce fichier avant toute modification du projet.**

Le dépôt GitHub est la source de vérité du projet.

## Objectif

Construire progressivement un carnet personnel de films permettant de reconstruire les films vus et les films à voir.

L'utilisateur souhaite que **ChatGPT soit l'interface principale de saisie et de gestion**. Il n'est donc pas nécessaire de multiplier les mécanismes d'édition dans le site.

## Modèle de données actuel

Le fichier canonique est :

`_data/films.yml`

Chaque film utilise actuellement :

- `id` : identifiant stable
- `titre`
- `année`
- `auteur` : réalisateur
- `statut`
- `date_vue`

### Statut

Les valeurs autorisées actuellement sont :

- `seen`
- `to_watch`

Ne pas ajouter d'autres statuts sans décision explicite de l'utilisateur.

### Date de visionnage

`date_vue` vaut :

- une date ISO `YYYY-MM-DD` lorsque l'utilisateur indique ou permet explicitement de déterminer la date ;
- `inconnue` lorsque la date n'est pas connue.

Ne **jamais déduire** une date de visionnage à partir de l'année du film, de sa sortie, d'une conversation ou d'un contexte approximatif.

Exemples :

- « Je viens de voir ce film » → date du jour.
- « Je l'ai vu hier » → date correspondante.
- « Je l'ai vu en 2022 » → conserver l'information de période selon le modèle approprié ; ne pas inventer un jour.
- « Je l'ai vu » → `date_vue: inconnue`.

Pour l'instant, `date_vue` représente la date de la dernière vision connue. Ne pas introduire un historique de plusieurs visionnages sans besoin réel.

## Principe de minimalité

Ne pas enrichir le modèle de données spontanément.

Les métadonnées d'affichage et les sources externes peuvent être ajoutées lorsqu'elles répondent à un besoin du site. Elles restent simples et ne doivent pas devenir une base de données externe.

Ne pas ajouter sans demande explicite :

- genres
- acteurs
- pays
- durée
- notes
- commentaires
- tags
- listes thématiques
- certitude
- autres métadonnées

On ajoute une donnée uniquement lorsqu'elle répond à un besoin réel identifié dans les conversations.

## Données initiales

Le catalogue initial contient notamment :

- Blade Runner (1982), Ridley Scott — vu
- Blade Runner 2049 (2017), Denis Villeneuve — vu

**Important :** le fait que ces films soient vus ne signifie pas que leur date de visionnage est connue.

## Site

`index.html` est une interface très simple permettant actuellement :

- d'afficher les films ;
- de filtrer par auteur ;
- de filtrer par année ;
- de filtrer par statut.

Le site est déployé via GitHub Pages.

Le site est pour l'instant principalement une **interface de consultation**.

### Vues

Le site propose trois vues :

- `Liste` : vue textuelle existante, avec défilement infini ;
- `Vignettes` : affichage par affiches/vignettes, avec défilement infini ;
- `Frise` : années organisées en colonnes horizontales, avec défilement horizontal.

Les vues Liste et Vignettes utilisent un chargement progressif côté interface. La Frise n'utilise pas le défilement infini.

Ne pas introduire une authentification GitHub ou une API d'écriture simplement pour ajouter des boutons d'édition. ChatGPT peut modifier le dépôt directement lorsque l'utilisateur lui demande de modifier le catalogue.

## Workflow avec l'utilisateur

Lorsqu'une conversation permet d'identifier ou de qualifier un film :

1. discuter avec l'utilisateur ;
2. ne transformer en donnée que ce qui est explicitement établi ;
3. modifier `_data/films.yml` si une modification est confirmée ;
4. conserver les informations inconnues comme inconnues plutôt que les deviner ;
5. faire un commit GitHub avec un message clair.

L'utilisateur peut travailler dans plusieurs conversations thématiques (par exemple science-fiction, Nouvelle Vague, remise en mémoire, réalisateur, etc.). Toutes ces conversations alimentent le même catalogue canonique.

## Tâche de consolidation

La **consolidation** sert à compléter ou vérifier une métadonnée précise du catalogue, et non à enrichir automatiquement toutes les fiches.

Elle est ciblée par métadonnée, par exemple :

- **vignettes** : vérifier les `thumb_url`, rechercher une vignette lorsqu'elle manque ou est invalide ;
- **acteurs principaux** : vérifier ou compléter la liste des acteurs principaux ;
- toute autre métadonnée explicitement demandée par l'utilisateur.

### Règles de consolidation

1. L'utilisateur indique ce qu'il veut consolider : par exemple « consolide les vignettes » ou « consolide les acteurs principaux ».
2. La consolidation porte uniquement sur cette métadonnée.
3. Si la métadonnée demandée **n'existe pas encore dans le modèle**, ne pas inventer son nom, sa structure ou ses valeurs : demander d'abord à l'utilisateur comment il souhaite la nommer et la structurer.
4. Si la métadonnée existe déjà, compléter les fiches où elle manque et corriger les valeurs manifestement invalides, en s'appuyant sur des sources externes fiables.
5. Ne pas ajouter automatiquement d'autres métadonnées découvertes pendant la consolidation.
6. Ne pas considérer une information externe comme certaine lorsqu'elle est ambiguë : demander à l'utilisateur lorsque le choix ne peut pas être établi proprement.
7. Après une consolidation, vérifier les modifications effectuées et résumer précisément ce qui a été ajouté ou corrigé.

Exemples :

- « consolide les vignettes » → travailler uniquement sur `thumb_url`.
- « consolide les acteurs principaux » → si le champ n'existe pas encore, demander d'abord le nom et la structure souhaités.
- « consolide les sources » → travailler uniquement sur `url_sources`.


## Historique Git

Git fournit déjà l'historique technique des modifications, mais **ce n'est pas un historique de visionnage**.

Ne pas présenter l'historique des commits comme un journal des films vus.

## Philosophie du projet

Commencer très simple.

Ne pas construire une application complexe avant que l'usage ne le justifie.

Le modèle, le site et les fonctionnalités doivent évoluer à partir des besoins réellement rencontrés dans les conversations.
