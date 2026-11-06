# Contribuer aux dépôts de l'organisation

Ces règles valent pour tout dépôt de l'organisation qui n'a pas son propre guide, pour toute personne
qui contribue, interne ou externe, et pour les agents IA. Les règles de code d'un dépôt (stack,
commandes, conventions) sont dans son `CLAUDE.md` ; en cas d'écart, le dépôt fait foi.

## Avant de commencer

- Un besoin flou se clarifie avant d'écrire du code : qui s'en sert, ce qu'il doit pouvoir faire, et
  comment on saura que ça marche. Le résultat va dans une issue GitHub.
- Installation locale : voir le README du dépôt. `npm install` installe aussi les hooks pre-commit
  (secretlint, puis formatage, lint, typage et tests liés).

## Branches et commits

- `main` ne reçoit que des PR : aucun push direct.
- Une branche courte par changement, préfixée par son type : `feat/recherche-fournisseurs`,
  `fix/connexion-google`, `docs/readme`.
- Commits au format [Conventional Commits](https://www.conventionalcommits.org/fr/) (`feat:`, `fix:`,
  `chore:`…). La version en dépend.
- L'historique d'une PR poussée ne se réécrit pas : ni `--amend`, ni `--force`. Une correction est un
  nouveau commit, pour que les fils de revue et la CI restent attachés.

## Pull requests

- Un sujet par PR, liée à son issue (`Closes #42`) quand il y en a une. Un travail sans issue
  l'indique : « sans issue ».
- Titre au format Conventional Commits : il devient le message du commit de squash, et la
  description en devient le corps.
- Avant d'ouvrir la PR :
  1. `npm run check` au vert (formatage, lint, typage, tests), en entier et pas seulement sur les
     fichiers touchés.
  2. Jouer en local le plus possible de ce que la CI et le déploiement feront : `npm run build`,
     image Docker construite, migration appliquée, tests SQL ou de bout en bout. Ce qui n'a pas pu
     être joué en local se dit dans la PR.
  3. Revues IA, dans cet ordre, pour que chacune voie le code corrigé par la précédente :
     `/simplify`, `/code-review high`, `/security-review`, puis `/full-review`.
     Chaque remarque est corrigée, ou écartée comme faux positif avec sa raison.
- `/security-review` se lance dès que la PR touche un sujet complexe ou exposé à internet : API,
  webhook, authentification, droits, accès aux données, fichiers reçus, secrets, toute entrée venue
  de l'extérieur. L'agent en juge ; s'il ne la lance pas, il le justifie dans la PR.
- Après l'ouverture, CodeRabbit relit la PR. Chaque remarque reçoit une réponse dans son fil : le
  commit qui la corrige, ou la raison de l'écarter. Trois tours au plus ; ce qui reste est tranché
  par un humain.
- Avant le merge : CI au vert, revues traitées, relecture humaine si le risque l'exige.
- Merge en squash uniquement ; la branche est supprimée au merge. Un agent IA ne fusionne jamais une
  PR : un humain le fait.
- Une règle ou un comportement qui change met à jour, dans la même PR, la documentation du dépôt
  (`README`, `docs/`, `CLAUDE.md`).

## Relecture selon le risque

Le risque se lit après le dernier tour de CodeRabbit, dans son « Merge Risk », et dans la section
« Impacts » du modèle de PR. En cas de doute, le niveau supérieur s'applique.

| Risque    | PR                                                                         | Relecture                                                                                                                                                                                                               |
| --------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Faible    | Merge Risk « Minimal » ou « Low », et aucune case d'impact cochée          | Revues traitées ; un humain fusionne. Renovate fusionne seul les mises à jour que son preset autorise.                                                                                                                  |
| Important | Merge Risk « Moderate » ou au-dessus, ou au moins une case d'impact cochée | Un relecteur humain est ajouté à la PR : le `CODEOWNERS` de la partie touchée, à défaut la personne qui a ouvert la PR (pour une PR d'agent, celle qui l'a lancé). Il vérifie chaque ligne d'impact avant de fusionner. |

## Rédiger la PR

Le [modèle de PR](pull_request_template.md) sert deux lecteurs : la personne qui relit le code, puis
celle qui lira les notes de version. Une section sans objet le dit (« aucun », « sans objet ») au
lieu d'être supprimée.

- **Impact utilisateur** : ce que le changement change pour la personne qui utilise l'application,
  sans jargon. Le workflow de notes de version la reprend comme source prioritaire : le titre de la
  section ne change pas. « Aucun » si rien ne se voit.
- **Pourquoi** : le problème à l'origine du changement.
- **Changements** : ce que fait la PR, et ce qu'elle ne fait pas encore.
- **Vérifications faites** : les tests ajoutés, les contrôles joués en local (build, image Docker,
  migration…), ce qui a été joué à la main, où et avec quelles données.
- **Preuves** : jointes au corps de la PR. Captures avant et après dès qu'un écran change ; sinon la
  preuve finale que ça marche (sortie de commande, extrait de log, réponse d'API), dans un bloc
  `<details>` si elle est longue. Sans donnée réelle ni secret.
- **Revues** : une ligne par revue, avec son bilan : nombre de remarques, combien corrigées, combien
  écartées comme faux positifs. Pour CodeRabbit, le nombre de tours et le Merge Risk final. Une
  `/security-review` non lancée dit pourquoi.
- **Impacts** : ce qui ne se rattrape pas en annulant la PR, ou qui demande une action hors du dépôt.
  Chaque case cochée porte une ligne d'explication. Une rupture de compatibilité se déclare aussi
  dans le titre (`feat!:`) et le commit (`BREAKING CHANGE:`), ce qui fixe la version majeure.

Les commentaires `<!-- … -->` du modèle disparaissent des notes de version ; ce qui est écrit en
dehors y part.

## Versions et publication

- Semver calculé à partir des commits : `fix` → patch, `feat` → mineure, rupture → majeure. Tags
  sans préfixe `v` (`2.15.1`).
- release-please tient la PR de release et le changelog ; sa fusion crée le tag, qui déclenche la
  publication et les notes de version. Aucun tag à la main.

## Dépendances, secrets et données

- Actions GitHub verrouillées par le lockfile `.github/workflows/actions.lock`
  ([format GitHub](https://github.com/github/actions-lockfile)), qui fige chaque version résolue,
  dépendances transitives comprises.
- Renovate propose les mises à jour des dépendances, avec le preset commun de l'organisation ;
  une montée de version majeure se lit avant d'être fusionnée.
- Jamais de secret dans le dépôt, les tests, les captures ou les issues. Une nouvelle variable va
  dans `.env.example`, sans valeur. Un secret partagé par plusieurs dépôts se déclare une fois, au
  niveau de l'organisation.
- Jamais de donnée réelle (nom, e-mail, téléphone d'un client ou d'un contact) dans le code, les
  tests, les fixtures, les captures ou les issues. Aucun outil ne la repère : c'est à l'auteur et au
  relecteur d'y veiller.
- Une donnée personnelle nouvelle se signale dans la PR, avec sa durée de conservation et, s'il y a
  lieu, le prestataire qui la traite.
