# Journal des décisions

Dernière mise à jour : 2026-11-04

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

## D-006 – Périmètre de l'architecture cible : l'ensemble des activités de Dominic

- **Date** : 2026-11-04
- **Décision (déclarée par Dominic)** : l'environnement OpenClaw en préparation « va gérer l'entièreté de toutes mes activités : NetBLB, LeFiltre et tous les projets ». Le périmètre de l'architecture cible couvre donc Net BLB inc. (clients, site, outils), LeFiltre (production vidéo, Patreon, sponsors, formations, chaîne éditoriale), la chaîne anglophone anonyme, les outils personnels, les projets en développement et le nouveau projet de commerce autonome IA (GPT-24), dont la passation prévoit déjà l'éventualité OpenClaw.
- **Exceptions déjà décidées** : l'outil FP Conseils (D-004, effacé à la fin du mandat) et le projet personnel « Donjon et Dragon » (D-003, hors périmètre).
- **Ce que cette décision ne tranche pas** : la forme de l'architecture (un ou plusieurs environnements, agents, espaces de travail), ce qui sera partagé et ce qui restera cloisonné entre activités, le rôle de GitHub par activité. Ces points relèvent des phases 4 et 5. En particulier, le principe « aucune mémoire commune implicite » et « tenants, comptes et secrets séparés » formulé dans la passation de GPT-24 n'est pas contradictoire avec ce périmètre : un périmètre global n'implique pas un partage global. Il sera traité en phase 4 comme un principe de conception candidat.
- **Conséquences pour l'audit** : tout nouveau projet ou activité entre par défaut dans le périmètre; la règle de signalement des nouveautés pendant l'audit (recommandation du 2026-11-04, en attente de validation) s'applique à l'ensemble des activités.
- **Validée par Dominic** : oui (déclaration du 2026-11-04).

## D-007 – Exigence pour l'architecture cible : un agent central de type « CEO »

- **Date** : 2026-11-04
- **Exigence déclarée par Dominic** : « Je veux avoir un agent dans OpenClaw qui agit un peu comme mon CEO. Je lui parle à lui, et lui, il gère avec moi l'ensemble des activités. » Un interlocuteur unique, au-dessus de l'ensemble des activités couvertes par D-006, avec lequel Dominic pilote tout.
- **Précisions de Dominic (même jour)** : l'agent est « le fil conducteur de tous mes projets, c'est lui qui relie tout ensemble »; « le grand chef d'orchestre de tout ce qui est en développement, tout ce qui est en maintenance »; « c'est aussi lui qui devra me rappeler plusieurs éléments pour ne pas que j'oublie certaines choses ». Trois dimensions distinctes : (1) **mémoire transversale** entre projets (état, décisions, blocages de chacun; aujourd'hui éclaté entre le registre ART-02, les passations par projet et la tête de Dominic); (2) **développement et maintenance** (ce qui se construit et ce qui tourne déjà; aujourd'hui aucun chef d'orchestre ne couvre la maintenance de façon transversale); (3) **proactivité** (rappels à l'initiative de l'agent; aujourd'hui assurée partiellement par les tâches planifiées ChatGPT et la règle « prochaine action unique »).
- **Nature** : exigence de conception (besoin exprimé), pas une décision d'architecture. Elle fixe ce que la future architecture doit offrir; elle ne décide ni comment l'agent est construit, ni ce qu'il voit, ni comment il délègue aux autres composants. Ces points relèvent des phases 4 et 5.
- **Lien avec l'existant (observation)** : c'est la généralisation du patron « chef d'orchestre » observé cinq fois dans l'écosystème (studio-explainer, site NetBLB, campagne Black Friday, commerce autonome, présent audit), chaque fois réécrit de zéro et limité à un projet. L'exigence revient à vouloir un chef d'orchestre unique et permanent au lieu d'un par projet.
- **Points à clarifier en phase 4 (Q-058)** : (a) le CEO est-il le point d'entrée exclusif, ou Dominic continuera-t-il aussi à parler directement à des outils spécialisés (monteur DaVinci, recherchiste)? (b) le CEO remplace-t-il les chefs d'orchestre par projet, ou les coiffe-t-il? (c) que doit-il savoir en permanence, et que doit-il ignorer (cloisonnement entre activités, clients, commerce)? (d) quelle gouvernance (analyse → recommandation → validation → exécution, déjà présente dans tous les chefs d'orchestre existants) lui applique-t-on? (e) la mémoire transversale (dimension 1) face à l'exigence d'isolation de la passation GPT-24 (« aucune mémoire commune implicite ») : ce que le CEO sait de chaque activité et ce qu'il ignore; (f) la proactivité (dimension 3) suppose une exécution autonome planifiée ou déclenchée par événement : fréquence, canal de rappel, coûts, garde-fous.
- **Validée par Dominic** : oui, comme exigence (déclaration du 2026-11-04).

## D-008 – Validation en bloc des éléments et méthodes à préserver

- **Date** : 2026-11-04
- **Décision** : les 19 classements proposés dans `elements-a-preserver.md` sont validés tels quels par Dominic (« Oui, c'est bon »). À préserver absolument : Claude avec accès complet au PC travaillant en arrière-plan dans DaVinci Resolve; IA par API en arrière-plan sans prise de contrôle sans autorisation (règle générale); règles de sécurité du montage; dépôt GitHub comme source de vérité d'un système IA (versionnage, changelog, archives, PASSATION.md, liste de contrôle); validateur indépendant; instructions d'agent versionnées et exécutant par PR sans fusion; un espace isolé par client (principe); registre unique des idées; doctrine éditoriale LeFiltre; doctrine sponsor; passation d'une IA à une autre par document structuré avec GitHub comme mémoire durable. À préserver si possible : rapport de surveillance hebdomadaire avec règles de prudence (contenu); vérification par le résultat; paramètres en tête d'instructions; analyse d'abord, livrable ensuite; maquette audio et GO avant dépense; méthode de livraison d'un outil IA à un client. Peut être repensé : style personnel de l'Éditeur Patreon (déclaré désuet). À clarifier (non tranché, à reposer en phase 4) : séparation recherche / rédaction / édition avec Dominic entre chaque étape.
- **Portée** : ce sont des contraintes de conception pour les phases 4 et 5. « Préserver » porte sur le principe ou le contenu de la méthode, pas nécessairement sur sa forme actuelle (projet ChatGPT, artefact Claude, tâche planifiée), sauf pour l'accès machine et l'API Resolve, qui sont la forme même.
- **Validée par Dominic** : oui.

## D-009 – Signalement des nouveautés pendant l'audit

- **Date** : 2026-11-04
- **Décision** : pendant l'audit, Dominic n'a pas à signaler chaque nouveau projet ou idée. Il continue d'inscrire ses idées dans le registre « Projets LeFiltre » (ART-02). Il signale ici, quand ça arrive : un nouveau client Net BLB; un nouveau système avec dépôt GitHub, connecteur, automatisation planifiée ou accès à sa machine; une nouvelle connaissance partagée entre plusieurs projets (tarifs, niveaux Patreon, chiffres d'audience); un changement à une méthode classée « à préserver » ou un projet qui en remplace un autre; un projet qu'il veut concevoir d'emblée pour la future architecture. Avant la phase 5, une passe de synchronisation sera faite (réextraction d'ART-02, capture des listes de projets Claude et ChatGPT, comparaison avec l'inventaire).
- **Raison** : minimiser le travail manuel de Dominic; l'inventaire est évolutif (D-003) et tout nouveau projet entre par défaut dans le périmètre (D-006).
- **Validée par Dominic** : oui.

## D-002 – Structure minimale du dépôt

- **Date** : 2026-11-01
- **Décision** : six fichiers maîtres au départ (`README.md`, `inventaire.md`, `workflows-actuels.md`, `elements-a-preserver.md`, `questions-ouvertes.md`, `decisions.md`). Tout ajout de fichier ou dossier doit répondre à un besoin réel et être expliqué avant.
- **Raison** : éviter la surarchitecture; faire évoluer le dépôt avec l'audit.
- **Validée par Dominic** : oui (instructions du projet).
