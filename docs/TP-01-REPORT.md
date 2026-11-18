# TP 01 — compte rendu

## Point de départ

Le tag `checkpoint/00-starter` n'est pas publié dans ce dépôt. La branche
`tp/01-ci` a été préparée depuis le commit `ce7e9cf`, qui correspond au merge du
checkpoint de départ.

## Échec de test observé

L'assertion de `test_calculates_a_quote` a été temporairement changée de `180`
à `181`. Pytest a échoué avec la valeur réelle `180`; l'assertion a ensuite été
restaurée dans un second commit. L'échec a été reproduit localement.

Dans une exécution CI, le contrôle concerné est `Python tests`. Le rapport JUnit
est généré par Pytest et le workflow tente de le téléverser même en cas d'échec.
Le temps de retour GitHub n'a pas été mesuré; l'exécution locale ciblée a échoué
en moins d'une seconde après le démarrage de Pytest.

## Choix de CI

- Les tests et les contrôles de qualité sont des jobs séparés pour Java et
  Python; la construction Docker attend leur réussite.
- Les permissions du workflow sont en lecture seule par défaut. Les permissions
  de publication sont limitées aux jobs Docker.
- Les jobs ont des timeouts, des caches et une concurrence qui annule les
  exécutions obsolètes.
- Les workflows ne filtrent pas les chemins sur `pull_request`, afin que les
  contrôles requis soient toujours présents. Les filtres de chemins restent
  limités aux événements `push`.
- Les images ne sont publiées que lors d'un push sur `main`.
- Les rapports JUnit/Pytest et Checkstyle sont téléversés avec `if: always()`
  et conservés sept jours.

## Détection de secrets et validation locale

Le workflow Gitleaks récupère l'historique complet avec `fetch-depth: 0`.
Aucun secret réel ni valeur factice n'a été ajouté; le test de détection avec
la valeur fournie par l'enseignant reste à faire.

`python scripts/course.py verify` passe localement : tests Java, 9 tests Python,
Checkstyle et Ruff. Les exécutions GitHub et la disponibilité des artefacts
n'ont pas été vérifiées sur une pull request.

## Protection de `main`

La configuration des règles de protection et la vérification d'une PR rouge et
d'une PR verte nécessitent les droits d'administration du dépôt et la
validation de l'enseignant; elles ne sont pas déclarées effectuées ici.
