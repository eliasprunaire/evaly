---
trigger: glob
globs: "tests/**, benchmark/**, benchmark_web/**, .github/workflows/ci.yml"
description: "Règles d'écriture, harnais et répartition CI des tests dans Evaly."
---

# Écrire et valider des tests (Evaly)

- Étendre d'abord un parcours E2E existant depuis une entrée publique ; compléter le scénario responsable avant d'ajouter un test.
- Ne créer ni TU isolé, ni doublon d'un comportement déjà vérifié, ni nouveau harnais : réutiliser les fixtures, le serveur et les transports fictifs existants.
- Préserver les contrôles de sécurité, cryptographie, intégrité et architecture ; avant toute suppression, exécuter le scénario de remplacement.
- Pour une correction, ajouter l'assertion manquante au parcours existant avant de modifier le code produit.
- Conserver un résultat reproductible (commande, scénario, résultat, artefact dans `reports/`) et ne jamais désactiver un test avec `skip`, sauf `tests/test_private_comparison.py`, qui saute localement quand Pi et Node sont absents (la CI installe Pi).

| Preuve | Harnais existant |
|---|---|
| Parcours complet, HTTP réel, exécuteur, stockage et résultat observable | `tests/test_parcours_complet.py` |
| Statuts, en-têtes, routage et négociation du vrai serveur HTTP | `tests/test_web_access.py` |
| Frontière moteur/présentation et projections | `tests/test_web_boundary.py` |
| DOM, JavaScript, accessibilité, CSP et affichage mobile dans Chromium | `tests/privacy_browser.test.cjs` |

- Le navigateur reçoit le HTML produit par `views.render` derrière le serveur Node de fixture existant ; il ne contacte pas `benchmark_web/server.py`.

## Répartition CI (`.github/workflows/ci.yml`)

Exécuter d'abord les tests pertinents, puis les quatre étapes précédées de la vérification statique Python/JS avant toute livraison de comportement :

| Étape | Répartition |
|---|---|
| Harnais Pi sans appel modèle | `tests.test_private_comparison` |
| Suite Python générale | Découverte Python hors des trois modules dédiés |
| Runtime cryptographique | `tests.test_provider_crypto` et `tests.test_privacy` |
| Navigateur | `tests/privacy_browser.test.cjs` |

- Sans `BENCHMARK_TEST_CI_SPLIT=1`, la découverte locale inclut tous les tests Python ; distinguer résultat local macOS et résultat Linux CI.
- La CI vérifie aussi la double construction reproductible du runtime ; aucun appel modèle ne compense une preuve absente.
