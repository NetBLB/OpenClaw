# Contexte du projet (résumé pour les agents de recherche)

Dominic Lessard (Net BLB inc., Québec) : créateur YouTube francophone (LeFiltre, 170 k abonnés, Patreon à 4 niveaux), services IT pour PME (clients), une chaîne YouTube anglophone anonyme produite avec Claude + Higgsfield, un projet de commerce autonome (dropshipping piloté par l'IA), une vingtaine d'idées et projets en développement. Il travaille aujourd'hui avec ~15 projets Claude, ~25 projets ChatGPT, 4 dépôts GitHub, des tâches planifiées ChatGPT, Claude Desktop avec accès complet à son PC Windows (montage DaVinci Resolve par API de scripting).

Architecture validée (à vérifier par la recherche) :
- OpenClaw sur un serveur virtuel Linux, toujours allumé. Un seul agent au départ : le « CEO ».
- Dominic lui parle par l'interface web (accès privé) et Telegram.
- Mémoire du CEO = un dépôt Git privé (workspace) : AGENTS.md, SOUL.md, USER.md, MEMORY.md, organisation.md (organigramme), projets/ (une fiche par projet), departements/ (dix départements, un AGENTS.md par équipe, activés un à la fois), commun/ (sept fichiers de connaissances partagées), skills/ (validateur, gmail lecture IMAP, github), journal/decisions.md, PASSATION.md.
- Le CEO : rituel du lundi (bilan), mise à jour des fiches au fil de l'eau, clôture par inactivité (heartbeat 30 min), filet du soir (cron), relance des projets dormants (seuil par fiche), création de fiches pour les nouveaux projets, délégation à des sous-agents avec le brief du département, validateur indépendant, boucle d'évaluation des sous-agents (brief versionné, evaluations.md).
- Règles : aucune action externe (envoi, publication, dépense) sans GO; aucune automatisation ajoutée à l'installation; secrets hors dépôt; identités publiques séparées entre marques; un fournisseur principal (Anthropic par clé API, plafond), IA locale plus tard; Paperclip envisagé plus tard pour l'organigramme, les tickets et les budgets.
