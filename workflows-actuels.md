# Méthodes de travail actuelles

Dernière mise à jour : 2026-11-01

Objectif : documenter COMMENT Dominic travaille aujourd'hui, pas seulement où se trouvent ses données. Chaque énoncé porte un statut (Observation / Hypothèse).

## Interfaces utilisées

- Observation (2026-11-01) : Claude Desktop sur Windows, avec dossiers liés (ex. dossier « production » dans CL-02).
- Déclaré par Dominic (2026-11-01) : Claude dispose d'un accès complet à sa machine Windows et travaille en arrière-plan directement dans DaVinci Resolve (projets CL-01 et CL-02) via un connecteur DaVinci Resolve. Cette méthode est un pilier des workflows de montage; à classer dans `elements-a-preserver.md` lorsque Dominic le confirmera.

## Démarrage d'un projet

- Déclaré par Dominic (2026-11-01) : il a beaucoup d'idées de projets en développement. Lorsqu'il a une idée, il démarre **toujours** un projet Claude ou un projet ChatGPT pour brainstormer et développer l'idée (ex. CL-10, en idéation). Conséquence : les deux plateformes contiennent vraisemblablement des projets d'idéation à divers stades de maturité, certains peut-être abandonnés (H).
- Déclaré par Dominic (2026-11-01) : **souvent, les idées démarrent dans ChatGPT**. Dominic demande ensuite à ChatGPT de créer une instruction personnalisée pour une tâche spécifique, puis crée un projet Claude spécialisé avec cette instruction. Exemple : projet ChatGPT « Application Retraite » (conseiller, idéation, remise en question) + projet Claude CL-10 (spécialiste développement de produit). Rôles observés : ChatGPT = idéation, conseil, rédaction d'instructions; Claude = exécution spécialisée. Hypothèse : d'autres projets Claude de l'inventaire ont aussi des instructions rédigées par ChatGPT (Q-014).

## Passation entre conversations Claude (CL-13)

- Déclaré par Dominic (2026-11-01) : pour la chaîne sans visage, le travail s'est fait en chats Claude sans projet. Quand le chat « chef d'orchestre » a dû passer le relais, il a produit un fichier zip + un fichier .md de passation, et un nouveau chat a repris le travail à partir de ces fichiers. C'est le seul mécanisme formel de passation de contexte observé jusqu'ici; il est né du besoin, pas d'une méthode établie (H).
- Déclaré par Dominic (2026-11-01) : expérience multi-agents : 1 chat → 3 Claude spécialisés (chef d'orchestre, scripteur, éditeur) → fusion en un seul Claude après le premier test. Les raisons de la fusion ne sont pas encore documentées (Q-017). Cette expérience vécue est un intrant majeur pour la phase 4 (principes de conception), à ne pas interpréter avant d'avoir la réponse de Dominic.

## Transmission du contexte à une IA (complément)

- Déclaré par Dominic (2026-11-01) : les instructions personnalisées d'un projet Claude peuvent être produites par ChatGPT à partir de la discussion d'idéation. Le contexte passe donc de ChatGPT à Claude sous forme d'instructions, par copier-coller (H sur le mécanisme exact).

## Transmission du contexte à une IA

- (à documenter)

## Conservation des décisions

- (à documenter)

## Versionnage des instructions

- Déclaré par Dominic (2026-11-01) : le suffixe « V1 » dans le nom d'un projet (CL-09) désigne la version des instructions personnalisées; une amélioration significative fera passer à « V2 ». Observation : CL-09 est le seul projet vu jusqu'ici avec un numéro de version; les autres sont modifiés sans trace de version visible (H, voir Q-011b).

## Utilisation de GitHub

- Observation (2026-11-01) : un dépôt privé dédié sert de source de vérité documentaire pour cet audit; la mémoire du projet Claude est considérée comme non suffisante à elle seule.

## Utilisation de Claude

- (à documenter)

## Utilisation de ChatGPT

- (à documenter)

## Création d'IA spécialisées

- Déclaré par Dominic (2026-11-01), à partir de CL-04 : pour un client de Net BLB inc. (FP Conseils), Dominic prototype l'IA dans un projet Claude de son propre compte, affine les instructions personnalisées en phase de test, puis implante l'outil dans le Claude du client une fois au point. Plusieurs outils du même type sont prévus pour ce client (un par type de proposition). Conséquence : son compte Claude héberge des prototypes destinés à quitter son environnement.
- Déclaré par Dominic (2026-11-01) : séparation volontaire en trois couches pour un outil IA : **règles métier** dans les instructions du projet, **méthode d'exécution** dans un Skill, **contenant** dans un fichier gabarit. Observé sur CL-04 + SK-01 + gabarit PowerPoint. Hypothèse : cette séparation pourrait être un patron réutilisé ailleurs (à vérifier sur les autres projets, notamment CL-02 dont les instructions se désignent comme « ce Skill »).

## Workflows récurrents

- Déclaré par Dominic (2026-11-01) : remarketing mensuel des formations LeFiltre. Export manuel depuis Thinkific de l'ensemble des inscrits non acheteurs (liste complète, pas seulement les nouveaux), fourni au projet CL-05; l'agent détecte ceux qui ont déjà reçu une campagne, n'ajoute que les nouveaux, puis crée et exécute la campagne via le connecteur Resend. Principe inscrit dans les instructions : l'agent exécute lui-même ce que le connecteur permet, Dominic approuve seulement quand nécessaire.
- Observation : les fichiers de contexte de CL-05 incluent des « historiques » (`historique thinkific.md`, `historique thinkific suivi`), ce qui suggère que l'état entre sessions est conservé dans des fichiers Markdown du projet plutôt que dans la seule mémoire du projet (H, voir Q-007).

## Passage d'un outil à un autre

- Déclaré par Dominic (2026-11-01) : la sortie du projet RECHERCHISTE (CL-06) est transférée par copier-coller vers une autre IA, ou sert à créer une feuille de notes de vidéo. Premier cas documenté de transfert manuel de contexte entre outils; à surveiller comme point de friction potentiel (observation, pas recommandation).
- Déclaré par Dominic (2026-11-01) : chaîne inter-plateformes pour les articles Patreon : un GPT sur ChatGPT rédige l'article → copier-coller (parfois) dans le projet Claude « Éditeur de textes Patreon » (CL-08) pour fact-check et réécriture plus humaine → copier-coller du résultat vers la publication. Deuxième cas de transfert manuel. Hypothèse : chaîne complète RECHERCHISTE (CL-06) → GPT rédacteur → CL-08 → Patreon, à confirmer.

## Compétences transversales observées

- RECHERCHISTE (CL-06) : une même IA sert plusieurs types de contenus (YouTube, vidéos Patreon, articles Patreon). Candidat à une compétence partagée dans l'analyse de phase 3 (H).

## Ce qui fonctionne bien (selon Dominic)

- (à documenter)

## Ce qui fait perdre du temps (selon Dominic)

- (à documenter)
