<!-- Titre : format Conventional Commits, il devient le message du commit de squash et fait la version.
     Rupture de compatibilité : `!` après le type (`feat!:`) et un pied `BREAKING CHANGE:`.
     Guide de rédaction : CONTRIBUTING.md de l'organisation, section « Rédiger la PR ». -->

**Issue** : <!-- `Closes #42`, ou « sans issue » -->

## Impact utilisateur

<!-- Ce que ça change pour la personne qui utilise l'application, sans jargon. « Aucun » si rien de visible.
     Cette section est reprise telle quelle dans les notes de version : ne pas renommer ce titre. -->

## Pourquoi

<!-- Le problème à l'origine du changement. -->

## Changements

<!-- Ce que fait la PR, et ce qu'elle ne fait pas encore. -->

## Vérifications faites

- Tests ajoutés : <!-- fichier › scénario ; « aucun » avec la raison -->
- Contrôles locaux : <!-- `npm run check` ou les contrôles du README, et tout ce qui a été joué au-delà : `npm run build`, `docker build`,
     migration appliquée, tests SQL ou de bout en bout. Ce qui n'a pas pu être joué, et pourquoi. -->
- Vérification manuelle : <!-- parcours joué, où, avec quel compte et quelles données ; « aucune » sinon -->

## Preuves

<!-- Captures avant / après dès qu'un écran change. Sinon la preuve finale que ça marche : sortie de commande,
     extrait de log, réponse d'API — dans un bloc <details> si elle est longue. Sans donnée réelle ni secret. -->

## Revues

<!-- Une ligne par revue : remarques remontées · corrigées · écartées (faux positifs). -->

- [ ] `/simplify` : <!-- 4 remontées · 3 corrigées · 1 écartée -->
- [ ] `/code-review high` :
- [ ] `/security-review` : <!-- bilan, ou « non lancée : » et la raison -->
- [ ] `/full-review` :
- [ ] CodeRabbit : <!-- tours (3 au plus) · remontées · corrigées · écartées · Merge Risk final -->

## Impacts

<!-- Ce qui ne se rattrape pas en annulant la PR, ou qui demande une action hors du dépôt.
     Cocher ce qui s'applique et préciser en une ligne. Rien de coché = la PR s'annule sans suite.
     Une case cochée, ou un Merge Risk « Moderate » ou au-dessus, demande une relecture humaine :
     CONTRIBUTING.md, section « Relecture selon le risque ». -->

- [ ] Contrat d'API ou de webhook modifié, ou rupture de compatibilité (API, données, configuration) :
- [ ] Migration de base : réversible ? données existantes reprises ?
- [ ] Accès aux données (connexion, droits, cloisonnement entre clients ou organisations) : quel test le prouve ?
- [ ] Action au déploiement (variable d'environnement, secret, configuration de l'hôte) :
- [ ] Donnée personnelle nouvelle : durée de conservation, prestataire qui la traite ?
- [ ] Dépendance ajoutée, ou montée de version majeure :
- [ ] Autre effet que l'annulation ne défait pas (e-mail ou notification envoyés, appel à un service externe, donnée effacée) :

## Avant le merge

- [ ] CI au vert.
- [ ] Relecture humaine faite si le risque est important (`CODEOWNERS`, à défaut la personne qui a ouvert la PR).
- [ ] La documentation du dépôt est à jour si une règle ou un comportement change.
- [ ] Aucun secret ni donnée réelle, y compris dans les tests, les captures et les preuves.
