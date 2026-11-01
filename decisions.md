# Journal des décisions

Dernière mise à jour : 2026-11-01

Seules les décisions **validées explicitement par Dominic** figurent ici. Les recommandations restent dans les autres documents, identifiées comme telles.

## D-001 – Dépôt GitHub privé comme source de vérité de l'audit

- **Date** : 2026-11-01
- **Décision** : le dépôt `NetBLB/OpenClaw` sert de source de vérité documentaire persistante pour l'audit. La mémoire du projet Claude « OPEN CLAW - conception » sert au travail conversationnel.
- **Raison** : la mémoire interne d'un projet Claude n'est pas considérée comme une mémoire fiable à elle seule pour un audit de longue durée.
- **Alternatives envisagées** : garder l'inventaire uniquement dans la conversation et les documents du projet Claude.
- **Avantages** : persistance, historique, versionnage, réutilisable par une IA technique ultérieurement.
- **Inconvénients** : nécessite de maintenir la synchronisation entre conversation et dépôt.
- **Conséquences** : toute information structurante doit être reflétée dans le dépôt.
- **Validée par Dominic** : oui (création du dépôt le 2026-11-01).

## D-002 – Structure minimale du dépôt

- **Date** : 2026-11-01
- **Décision** : six fichiers maîtres au départ (`README.md`, `inventaire.md`, `workflows-actuels.md`, `elements-a-preserver.md`, `questions-ouvertes.md`, `decisions.md`). Tout ajout de fichier ou dossier doit répondre à un besoin réel et être expliqué avant.
- **Raison** : éviter la surarchitecture; faire évoluer le dépôt avec l'audit.
- **Validée par Dominic** : oui (instructions du projet).
