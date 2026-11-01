# Audit de l'écosystème IA – Conception OpenClaw

Dépôt privé servant de **source de vérité documentaire persistante** pour l'audit de l'écosystème IA de Dominic Lessard (Net BLB inc.), réalisé en préparation d'une éventuelle architecture OpenClaw.

La mémoire du projet Claude sert au travail conversationnel; ce dépôt conserve les éléments structurants.

## Phases

| Phase | Nom | Statut |
|---|---|---|
| 1 | Inventaire | Première passe terminée le 2026-11-01 (voir `inventaire.md`, sections 7b et 7c) |
| 2 | Compréhension | En cours depuis le 2026-11-01 (ordre : `inventaire.md` section 7c; fiches : section 10). Lot A1 sponsors : fiche rédigée, réponses reçues. Lot A2 montage DaVinci : fiche rédigée, réponses reçues. Lot A3 clients : fiche rédigée, réponses reçues. Lot A4 chaîne éditoriale : fiche rédigée |
| 3 | Relations et recouvrements | À venir |
| 4 | Principes de conception | À venir |
| 5 | Architecture cible | À venir (nécessite accord explicite) |
| 6 | Plan de transition | À venir |

## Documents maîtres

| Fichier | Rôle |
|---|---|
| `inventaire.md` | Inventaire maître évolutif de tous les éléments (projets, IA, dépôts, outils, automatisations) |
| `workflows-actuels.md` | Méthodes de travail observées, telles qu'elles existent aujourd'hui |
| `elements-a-preserver.md` | Méthodes et éléments classés : à préserver absolument / si possible / peut être repensé / à clarifier |
| `questions-ouvertes.md` | Questions accumulées, à poser au bon moment |
| `decisions.md` | Journal des décisions validées explicitement par Dominic |

## Convention de statut des énoncés

Chaque énoncé documenté porte l'un de ces statuts :

- **Observation** : constaté directement (capture d'écran, fichier, instruction fournie).
- **Hypothèse** : déduction non confirmée. Ne doit jamais devenir un fait sans confirmation.
- **Recommandation** : proposition de l'IA. N'est jamais une décision.
- **Décision validée** : approuvée explicitement par Dominic, consignée dans `decisions.md`.

## Règles du dépôt

- Structure minimale; nouveaux fichiers ou dossiers uniquement sur besoin réel.
- Peu de documents maîtres bien maintenus plutôt qu'une fragmentation.
- Commits regroupés logiquement, messages décrivant ce qui a changé dans l'audit.
- OpenClaw ne doit pas biaiser l'analyse de l'existant.
