# Audit de l'écosystème IA – Conception OpenClaw

Dépôt privé servant de **source de vérité documentaire persistante** pour l'audit de l'écosystème IA de Dominic Lessard (Net BLB inc.), réalisé en préparation d'une éventuelle architecture OpenClaw.

La mémoire du projet Claude sert au travail conversationnel; ce dépôt conserve les éléments structurants.

## Phases

| Phase | Nom | Statut |
|---|---|---|
| 1 | Inventaire | Première passe terminée le 2026-11-01 (voir `inventaire.md`, sections 7b et 7c) |
| 2 | Compréhension | Close le 2026-11-01 (D-005). Fiches A1 à A4 et B1 dans `inventaire.md` section 10 |
| 3 | Relations et recouvrements | Faite le 2026-11-04 (`recouvrements.md` v1.3, validée par une IA indépendante); corrections de Dominic sur la section 7b possibles à tout moment |
| 4 | Principes de conception | Proposés le 2026-11-04 en mode accéléré (D-011), `principes-de-conception.md`, en attente de validation en bloc |
| 5 | Architecture cible | Validée le 2026-11-04 (D-015), `architecture-cible.md` v1.2; cinq questions de détail ouvertes |
| 6 | Plan de transition | Proposés le 2026-11-05 : `plan-de-transition.md` et `cahier-de-construction.md`; en attente de validation |

## Documents maîtres

| Fichier | Rôle |
|---|---|
| `inventaire.md` | Inventaire maître évolutif de tous les éléments (projets, IA, dépôts, outils, automatisations) |
| `workflows-actuels.md` | Méthodes de travail observées, telles qu'elles existent aujourd'hui |
| `elements-a-preserver.md` | Méthodes et éléments classés : à préserver absolument / si possible / peut être repensé / à clarifier |
| `recouvrements.md` | Phase 3 : analyse transversale des doublons, silos, dépendances et points de répétition de contexte (observations et hypothèses, aucune décision) |
| `principes-de-conception.md` | Phase 4 : règles que l'architecture cible doit respecter (proposées, puis validées une à une) |
| `architecture-cible.md` | Phase 5 : architecture OpenClaw proposée, chaque choix justifié; organigramme, dépôt « siege », composants, coûts, risques |
| `plan-de-transition.md` | Phase 6 : étapes, critères de sortie, tests d'acceptation, sort de chaque élément actuel |
| `cahier-de-construction.md` | Phase 6 : document remis à l'IA technique pour construire l'environnement sans réinventer l'architecture |
| `openclaw-capacites.md` | Ce qu'OpenClaw permet réellement, d'après sa documentation (lecture partielle, à compléter avant la phase 5) |
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
