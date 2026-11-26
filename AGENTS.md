Ce codebase est le vôtre : à vous de décider à quoi il sert et comment il est construit, et vous continuerez à le développer.

# Instructions pour les agents

## Mission et sources faisant autorité

Evaly aide à choisir un modèle pour une tâche précise à partir de preuves lisibles (configuration observée sous des conditions communes, jamais le nom du modèle seul). Les documents ci-dessous font foi chacun dans leur domaine :

- `GLOSSARY.md` (vocabulaire), `docs/roadmap.md` (jalons), `docs/task-template.md` (contrat minimal d'une future tâche de benchmark), `DESIGN.md` (langage visuel web, à lire avant tout travail UI).
- Le `README`, les releases GitHub et leurs artefacts portent les faits opérationnels ; la demande courante prévaut et tout conflit doit être signalé.
- Ne jamais créer de copie, variante datée ou dossier d'archives, ni requalifier les campagnes et prototypes historiques pour les faire coller à la spécification courante.

## Autorité, sécurité et GitHub

- Travailler sur une branche typée (`feat`, `fix`, `docs`, `chore`, `refactor`), jamais sur `main` ni `master` ; préserver les changements non liés du worktree.
- Exiger une autorisation explicite avant : opération Git destructive, fusion, push/PR, suppression de branche, fermeture d'Issue/PR, release, appel modèle (facturé ou non), campagne, publication ou action externe. Ne jamais exposer de secret.
- GitHub Issues et le champ `Status` du [Project #5](https://github.com/users/eliasprunaire/projects/5) portent l'état de travail (relations natives `Parent issue` et `Sub-issues progress` ; aucune création/clôture sans preuve ou décision explicite). L'artefact sous `reports/` est local et ignoré par Git.

## Écrire des tests

Appliquer les règles détaillées dans [`.agents/rules/testing.md`](.agents/rules/testing.md) : étendre les parcours E2E existants sans TU redondants ni nouveau harnais, et ne jamais désactiver un test avec `skip`. Le navigateur de test passe par le serveur Node de fixture existant, jamais par `benchmark_web/server.py`.

## Validation

Exécuter les tests pertinents puis les quatre étapes de [`.github/workflows/ci.yml`](.github/workflows/ci.yml) (précédées de la vérification statique Python/JS et incluant la double construction reproductible). Gotcha local : sans `BENCHMARK_TEST_CI_SPLIT=1`, la découverte Python locale inclut aussi `tests.test_private_comparison`, `tests.test_provider_crypto` et `tests.test_privacy`. Pour la documentation, vérifier les liens, les chemins canoniques et l'absence de source concurrente sans recopier les commandes de CI. Voir aussi [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Livraison, arrêt et revue

- Conclure avec les fichiers modifiés/supprimés, les validations exécutées et les limites ouvertes ; s'arrêter en `HOLD` sans fallback implicite si une autorité ou preuve manque.
- En revue, bloquer toute autorité documentaire concurrente, requalification historique, architecture affirmée sans preuve, secret versionné ou contournement de preuve.
