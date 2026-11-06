# .github

Fichiers par défaut des dépôts de l'organisation : GitHub les applique à tout dépôt de
l'organisation qui n'a pas le sien.

| Fichier                                                | Rôle                            |
| ------------------------------------------------------ | ------------------------------- |
| [`CONTRIBUTING.md`](CONTRIBUTING.md)                   | Règles communes pour contribuer |
| [`pull_request_template.md`](pull_request_template.md) | Modèle de description des PR    |

- **Source unique** : aucun dépôt de l'organisation ne garde de copie de ces fichiers. Une règle qui
  change se change ici.
- **Contrat avec les notes de version** : le workflow commun de notes de version lit
  la section `## Impact utilisateur` du modèle de PR. Ce titre ne se renomme pas sans modifier le
  workflow dans la même foulée.
- **Remplacement, pas fusion** : un dépôt qui a son propre fichier (à la racine, dans `docs/` ou dans
  `.github/`) ignore celui-ci en entier.
- **Absents des clones** : ces fichiers n'existent que dans l'interface GitHub. Un outil ou un agent
  qui travaille sur un checkout ne les voit pas, et `gh pr create --body` ne les applique pas.
- **Dépôt public** : GitHub l'exige pour appliquer ces fichiers. Rien de confidentiel ici : ni nom de
  client, ni adresse d'infrastructure.
