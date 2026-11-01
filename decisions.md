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

## D-000 – Choix de méthode antérieurs à l'audit (consignés a posteriori)

- **Date** : fin octobre 2026 (conversation ChatGPT « OPEN CLAW », lue le 2026-11-01).
- **Décisions de Dominic** : (a) mener l'audit dans un projet Claude dédié avant toute installation d'OpenClaw; (b) relier GitHub dès le départ comme mémoire documentaire; (c) procéder en deux passes (captures d'écran, puis instructions des projets prioritaires); (d) aucune migration directe : analyse puis stratégie avant de créer OpenClaw; (e) Dominic reste décideur final, les méthodes de travail existantes sont des contraintes de conception.
- **Alternatives envisagées** : projet ChatGPT d'audit (écarté au profit de Claude); remplir manuellement 25 fiches (écarté au profit des captures).
- **Validée par Dominic** : oui (appliquées depuis le début de l'audit).

## D-003 – Clôture de la première passe d'inventaire (phase 1)

- **Date** : 2026-11-01
- **Décision** : la première passe de la phase 1 est considérée comme suffisamment complète (« ça fait pas mal le tour »). L'inventaire reste évolutif : tout élément découvert plus tard y sera ajouté.
- **Périmètre couvert** : projets Claude, chats et artefacts Claude, Skill, projets et conversations ChatGPT, dépôts GitHub, automatisations, outils, registre des idées, structure Patreon, système site NetBLB, conversation ChatGPT d'origine de l'audit.
- **Hors périmètre, à la demande de Dominic** : projet personnel « Donjon et Dragon ».
- **Validée par Dominic** : oui.

## D-004 – L'outil FP Conseils est hors périmètre de l'architecture cible

- **Date** : 2026-11-01
- **Décision** : le projet Claude « FPconseils, proposition Coaching » (CL-04), le Skill `fp-conseils-proposition-coaching` (SK-01) et la copie ChatGPT « Proposition de Coaching » (GPT-04) relèvent d'un mandat unique pour le client FP Conseils. À la fin du mandat, Dominic effacera tout. Ces éléments ne doivent pas être intégrés à l'environnement LeFiltre / Net BLB ni à la future architecture.
- **Raison** : livrable destiné au client, pas un actif de Dominic.
- **Conséquences** : l'audit documente ces éléments pour la *méthode* (livraison d'outils IA à des clients, deux autres outils prévus pour ce client), pas pour les intégrer. Le projet client « FP CONSEILS » (GPT-05, mandats) reste, lui, dans le périmètre comme les autres clients.
- **Validée par Dominic** : oui (déclaration du 2026-11-01).

## D-005 – Clôture de la phase 2 (compréhension)

- **Date** : 2026-11-01
- **Décision** : la phase 2 est close. Fiches rédigées pour les lots A1 (sponsors), A2 (montage DaVinci), A3 (clients Net BLB), A4 (chaîne éditoriale) et B1 (FP Conseils). Les vérifications B2 (fichiers du remarketing) et B3 (`AGENTS.md` de netblb-site) sont laissées de côté; elles pourront être faites plus tard si la phase 3 en a besoin.
- **Suite** : validation en bloc des éléments à préserver, puis phase 3 (relations et recouvrements).
- **Validée par Dominic** : oui.

## D-002 – Structure minimale du dépôt

- **Date** : 2026-11-01
- **Décision** : six fichiers maîtres au départ (`README.md`, `inventaire.md`, `workflows-actuels.md`, `elements-a-preserver.md`, `questions-ouvertes.md`, `decisions.md`). Tout ajout de fichier ou dossier doit répondre à un besoin réel et être expliqué avant.
- **Raison** : éviter la surarchitecture; faire évoluer le dépôt avec l'audit.
- **Validée par Dominic** : oui (instructions du projet).
