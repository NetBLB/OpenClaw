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
- **Décision** : les 19 classements proposés dans `elements-a-preserver.md` sont validés tels quels par Dominic (« Oui, c'est bon »). À préserver absolument : Claude avec accès complet au PC travaillant en arrière-plan dans DaVinci Resolve; IA par API en arrière-plan sans prise de contrôle sans autorisation (règle générale); règles de sécurité du montage; dépôt GitHub comme source de vérité d'un système IA (versionnage, changelog, archives, PASSATION.md, liste de contrôle); validateur indépendant; instructions d'agent versionnées et exécutant par PR sans fusion; un espace isolé par client (principe); registre unique des idées; doctrine éditoriale LeFiltre; doctrine sponsor; passation d'une IA à une autre par document structuré avec GitHub comme mémoire durable. À préserver si possible : rapport de surveillance hebdomadaire avec règles de prudence (contenu); vérification par le résultat; paramètres en tête d'instructions; analyse d'abord, livrable ensuite; maquette audio et GO avant dépense; méthode de livraison d'un outil IA à un client. Peut être repensé : style personnel de l'Éditeur Patreon (déclaré désuet). Séparation recherche / rédaction / édition avec Dominic entre chaque étape : d'abord à clarifier, puis déclarée **voulue** par Dominic le même jour (« C'est voulu »); classée à préserver, niveau à confirmer.
- **Portée** : ce sont des contraintes de conception pour les phases 4 et 5. « Préserver » porte sur le principe ou le contenu de la méthode, pas nécessairement sur sa forme actuelle (projet ChatGPT, artefact Claude, tâche planifiée), sauf pour l'accès machine et l'API Resolve, qui sont la forme même.
- **Validée par Dominic** : oui.

## D-009 – Signalement des nouveautés pendant l'audit

- **Date** : 2026-11-04
- **Décision** : pendant l'audit, Dominic n'a pas à signaler chaque nouveau projet ou idée. Il continue d'inscrire ses idées dans le registre « Projets LeFiltre » (ART-02). Il signale ici, quand ça arrive : un nouveau client Net BLB; un nouveau système avec dépôt GitHub, connecteur, automatisation planifiée ou accès à sa machine; une nouvelle connaissance partagée entre plusieurs projets (tarifs, niveaux Patreon, chiffres d'audience); un changement à une méthode classée « à préserver » ou un projet qui en remplace un autre; un projet qu'il veut concevoir d'emblée pour la future architecture. Avant la phase 5, une passe de synchronisation sera faite (réextraction d'ART-02, capture des listes de projets Claude et ChatGPT, comparaison avec l'inventaire).
- **Raison** : minimiser le travail manuel de Dominic; l'inventaire est évolutif (D-003) et tout nouveau projet entre par défaut dans le périmètre (D-006).
- **Validée par Dominic** : oui.

## D-010 – Principe : installer d'abord, automatiser ensuite

- **Date** : 2026-11-04
- **Décision (déclarée par Dominic)** : l'architecture OpenClaw doit d'abord être montée « du mieux possible », sans chercher à automatiser tout de suite beaucoup de choses. L'automatisation viendra progressivement, une fois l'environnement installé et utilisé au quotidien, en commençant par ce qui se révélera possible et utile à l'usage.
- **Raison (telle qu'exprimée)** : Dominic a eu l'impression que l'analyse des points de répétition (recouvrements.md, sections 7 et 7b) préparait une automatisation immédiate; il préfère une mise en place solide puis une automatisation guidée par l'usage réel.
- **Conséquences** : (a) la phase 5 (architecture cible) conçoit un environnement de travail, pas un ensemble d'automatisations; la section 7b de `recouvrements.md` sert à identifier ce qui ne doit pas être touché (colonnes « Dominic seul » et « imposé par les outils »), pas à planifier des gains; (b) la phase 6 (plan de transition) doit prévoir une étape explicite « usage sans automatisation » avant toute automatisation; (c) les automatisations existantes (tâches planifiées ChatGPT, remarketing CL-05, montage en arrière-plan) sont conservées telles quelles, elles ne sont pas remises en cause par ce principe; (d) cohérent avec deux règles déjà présentes dans l'écosystème : « construire progressivement » et « ne pas écrire une automatisation sans processus réel à automatiser » (passation GPT-24), et avec la vérification par le résultat (D-008).
- **Validée par Dominic** : oui (déclaration du 2026-11-04).

## D-011 – Mode accéléré pour les phases 4 à 6

- **Date** : 2026-11-04
- **Décision (déclarée par Dominic)** : « On va accélérer un peu le processus et on affinera par la suite directement dans OpenClaw. » Les phases 4 (principes), 5 (architecture) et 6 (transition, cahier de construction) sont menées en mode accéléré : l'IA rédige des propositions complètes à partir de ce qui est consigné, Dominic valide en bloc (comme pour D-008), les points fins sont affinés à l'usage dans OpenClaw.
- **Garde-fous maintenus** : rien n'est inscrit comme validé sans accord explicite; la phase 5 ne démarre que sur le GO de Dominic (instructions du projet); les capacités réelles d'OpenClaw sont vérifiées (documentation) avant de concevoir l'architecture, pour ne pas concevoir contre des fonctions supposées; D-010 (installer d'abord) s'applique.
- **Conséquences** : `principes-de-conception.md` est rédigé d'un bloc avec des réponses proposées aux questions Q4 et Q-058; les questions ouvertes non structurantes (27) ne sont plus posées une à une mais traitées dans OpenClaw.
- **Validée par Dominic** : oui (déclaration du 2026-11-04).

## D-012 – Vision cible déclarée par Dominic (réponses aux dix questions de cadrage)

- **Date** : 2026-11-04
- **Statut** : déclarations de Dominic, consignées comme contraintes et objectifs de la phase 5. Là où Dominic dit « je ne sais pas », c'est consigné comme tel; rien n'est complété à sa place.
- **Hébergement** : OpenClaw tournera sur un **serveur virtuel**, pas sur le PC. Éventuellement, une **IA locale** pour les tâches moins complexes. Dominic ne sait pas ce qu'est concrètement une « tâche plus simple »; à expliciter en phase 5.
- **Canaux** : PC et téléphone, les deux.
- **Rituel du lundi** : « On en est où par rapport à la semaine passée? Voici ce que j'ai l'intention de faire cette semaine. » Le CEO répond : échéances, projets non regardés depuis longtemps. Il **gère les tâches planifiées** (revues d'actualité, Patch Tuesday, revues SEO des sites). Dans la journée : « On travaille sur tel projet », le CEO le fait avancer et **supervise l'avancement** de tous les projets.
- **Registre des projets** : le CEO **remplace** l'outil « Projets LeFiltre » (ART-02) : il conserve tous les projets et idées, leur état, et les liens vers les IA qui y travaillent (« un petit CRM de projets »). Réponse à P-16.
- **Un ou plusieurs interlocuteurs** : idéal = **un seul** « qui fonctionne du tonnerre »; plusieurs seulement si ça ne fonctionne pas bien. Dominic n'a jamais utilisé OpenClaw; décision à l'usage.
- **Fournisseur et coût** : idéal = **un seul fournisseur** plus une IA locale pour les tâches simples. Pas de budget chiffré donné.
- **Gmail** : Dominic **accepte** de donner ses connexions à ses comptes courriel. Lève le silo S-1 (vérité des mandats clients).
- **ChatGPT** : idéal = **tout dans OpenClaw**; réserve : retour possible vers ChatGPT si OpenClaw ne convient pas.
- **Séparation des activités** : côté **public** (auditeurs, clients, web), personne ne doit pouvoir relier la chaîne anonyme, le commerce, LeFiltre et Net BLB. Côté **interne**, l'agent traite le tout « comme une grosse compagnie avec différents départements ». Reformule P-1 et P-3 : le cloisonnement porte sur les identités et comptes publics, pas sur la connaissance interne.
- **Jour 1** : (1) parler à **un seul agent qui comprend tous les projets et les distingue** sans les mélanger (implications, fonctionnement, état); (2) une fois cette compréhension acquise, qu'il **aide à améliorer, automatiser, penser à ce que Dominic ne pense pas**; (3) **déléguer pour réduire la charge mentale** (« beaucoup de projets en même temps, mon cerveau veut exploser »). Critère d'acceptation implicite du prototype : le CEO distingue correctement les projets.
- **Ce que Dominic garde pour lui** : ce que l'agent ne saura pas faire de façon adéquate selon ses critères; rien de défini a priori.
- **Validée par Dominic** : oui (déclarations du 2026-11-04).

## D-013 – Vision de long terme : une entreprise d'agents (déclarée par Dominic)

- **Date** : 2026-11-04
- **Vision déclarée** : « Au lieu d'engager des humains dans mon entreprise, j'engage des agents IA. » Structure visée : Dominic en haut (idées, coordination, décisions); un **agent CEO** qui coordonne; des **chefs d'équipe** par département; sous eux des **agents employés** qui font la main-d'œuvre. Dominic garde une vision globale, décide, et reste impliqué au minimum dans l'exécution; il conserve ce qui exige sa personne (tourner des vidéos, mettre en ligne, etc.). Dominic ne sait pas si OpenClaw permet cela et demande s'il est « dans les nuages ».
- **Lecture de l'audit (R, pas une décision)** : (1) la structure est exprimable dans OpenClaw tel que documenté (plusieurs agents sur un Gateway, sous-agents en arrière-plan, profondeur de délégation configurable, compétences partagées, tâches planifiées); le système studio-explainer (Maître + Scripteur + Validateur) en est déjà une instance à un niveau; (2) la main-d'œuvre est réaliste pour la recherche, les premiers jets, l'analyse, la veille, le suivi d'état, le fact-checking, le code et le montage par règles; non réaliste sans Dominic pour le jugement de qualité final, la négociation réelle, l'argent et le public; (3) le point « dans les nuages » n'est pas la structure mais le rythme : l'implication de Dominic baisse département par département, à mesure que chaque équipe reçoit règles, exemples et critère de bon travail; la coordination multi-niveaux a un coût (dégradation des passations, consommation); l'organigramme est le cadre, à remplir une équipe à la fois (cohérent avec D-010).
- **Conséquences pour la phase 5** : l'architecture est conçue comme un organigramme à trois niveaux (CEO, chefs d'équipe par département, agents spécialisés), mais le plan de transition (phase 6) ne construit qu'une équipe à la fois; chaque équipe a ses règles, ses exemples, son validateur et son critère d'acceptation avant qu'on lui délègue.
- **Validée par Dominic** : la vision, oui (déclaration du 2026-11-04); la lecture de l'audit reste une recommandation.

## D-014 – Choix de la plateforme : OpenClaw d'abord, conçu pour Paperclip, Paperclip plus tard (et GO pour la phase 5)

- **Date** : 2026-11-04
- **Décision** : Dominic accepte la recommandation de l'audit (Q-061, détail dans `openclaw-capacites.md`) : (1) installation d'un agent CEO unique dans OpenClaw sur un serveur virtuel, conversationnel, avec mémoire en fichiers versionnés, rituel du lundi, réception des revues planifiées, délégation par sous-agents et compétences par département, sans automatisation ajoutée (D-010); (2) organisation écrite dès le départ au format d'une entreprise (fichier d'organisation, un `AGENTS.md` par équipe avec règles, validateur et critère d'acceptation), compatible avec un branchement ultérieur à Paperclip; (3) visualisation de l'organigramme par un tableau de bord communautaire ou une page générée depuis le fichier d'organisation; (4) bascule vers Paperclip seulement quand au moins deux équipes produisent un travail jugé bon, que le besoin de tickets, budgets et approbations est réel, et que Paperclip est stabilisé; (5) fournisseur principal Anthropic par clé API avec plafond mensuel, forfait Claude conservé pour le montage dans Claude Desktop, abonnement ChatGPT éventuellement réutilisé via Codex pour des tâches secondaires, IA locale plus tard après mesure.
- **Alternatives envisagées** : Paperclip dès le départ avec OpenClaw comme main-d'œuvre (écartée : interaction centrée sur les tickets alors que l'exigence du jour 1 est conversationnelle; projet jeune et instable; failles récentes; coût par agent; contraire à D-010); OpenClaw seul sans préparation à Paperclip (écartée : perdrait la trajectoire vers l'organigramme voulu en D-013).
- **Perdu** : vue d'organigramme officielle et budgets par agent dès le jour 1. **Gagné** : CEO conversationnel dès la semaine 1, coût maîtrisé, pas de dépendance à un projet instable, migration vers Paperclip sans refonte.
- **Conséquence** : la phase 5 (architecture cible) est autorisée et conçue sur cette base.
- **Validée par Dominic** : oui (« OK j'accepte ta recommandation », 2026-11-04).

## D-015 – Architecture cible validée (v1.2)

- **Date** : 2026-11-04
- **Décision** : Dominic accepte `architecture-cible.md` telle que corrigée (« Tu as raison, j'accepte tes propositions ») : CEO unique sur serveur virtuel; dépôt « siege » comme mémoire; organigramme à dix départements activés un à la fois; développement travaillé avec le CEO dès le jour 1, machines de production en place; commerce autonome repris par le CEO via sa passation; Black Friday terminée dans ChatGPT puis importée; composants, coûts et risques de la section 5 à 9. Restent ouvertes les cinq questions de la section 10 (nom du dépôt, canal mobile, hébergeur, première équipe, canal des rappels).
- **Scénario ajouté par Dominic** : perte du fil entre deux séances espacées (trois projets travaillés un soir, retour une semaine plus tard). Le CEO fait la clôture de séance (décisions, suspens, prochaine action, « reprendre ici ») et la reprise; c'est le test d'acceptation n° 1 du prototype.
- **Conséquence** : la phase 6 (plan de transition et cahier de construction) peut être rédigée.
- **Validée par Dominic** : oui.

## D-016 – Choix de détail pour la construction

- **Date** : 2026-11-05
- **Décisions de Dominic** : dépôt de mémoire du CEO = `NetBLB/ceo`; canal mobile = Telegram.
- **Non décidés, consignés comme tels** : hébergeur du serveur virtuel (Dominic n'a pas encore choisi; critères et préférence dans `cahier-de-construction.md`, section 2); première équipe à activer (Dominic n'a pas encore décidé et n'avait pas le répertoire des équipes en tête; répertoire fourni; choix reporté au bilan du premier mois, plan de transition étape 5).
- **Validée par Dominic** : oui pour les deux premiers points.

## D-017 – Plan de transition et cahier de construction validés, avec deux exigences ajoutées

- **Date** : 2026-11-05
- **Décision** : Dominic valide le plan de transition (« on va y aller comme ça ») et, par là, le cahier de construction, avec deux exigences explicites : (1) dès le jour 1, pouvoir ajouter de nouveaux projets et que le CEO les classe au bon endroit selon la structure établie (règle « Nouveau projet » dans AGENTS.md du CEO; test T-6); (2) pouvoir évaluer tout sous-agent développé, dire quand le travail n'est pas bon, et l'optimiser jusqu'à ce qu'il soit convenable (règle « Évaluation et amélioration d'un sous-agent »; `evaluations.md`, brief versionné, validateur, boucle de correction; principe P-32).
- **Conséquence** : les documents de phase 6 passent en version 1.1; l'audit a produit ses huit livrables. Reste la remise à l'IA technique et la préparation par Dominic (étape 0).
- **Validée par Dominic** : oui.

## D-002 – Structure minimale du dépôt

- **Date** : 2026-11-01
- **Décision** : six fichiers maîtres au départ (`README.md`, `inventaire.md`, `workflows-actuels.md`, `elements-a-preserver.md`, `questions-ouvertes.md`, `decisions.md`). Tout ajout de fichier ou dossier doit répondre à un besoin réel et être expliqué avant.
- **Raison** : éviter la surarchitecture; faire évoluer le dépôt avec l'audit.
- **Validée par Dominic** : oui (instructions du projet).
