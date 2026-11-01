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

- Observé (PASSATION.md, capture du chat successeur, 2026-11-01) : mécanisme de passation en trois pièces : (1) un fichier `PASSATION.md` rédigé par l'IA sortante « pour l'IA qui prend le relais » (projet en une minute, où on en est, diagnostic validé, décisions prises, plan convenu, recette technique testée, questions ouvertes à poser à Dominic, notes); (2) une archive zip des fichiers du système et de la production; (3) un prompt court de Dominic dans la nouvelle conversation : « Tu prends le relais d'une autre IA… Lis d'abord PASSATION.md en entier, puis ouvre l'archive… pose-moi les questions de la section 8. » L'IA successeur a posé ces questions via un formulaire à choix et consigné les réponses. Depuis la v1.6, `PASSATION.md` vit dans le dépôt GitHub et est réécrit à chaque fin de séance importante; la passation entrante est archivée (`archive/PASSATION-2026-10-29-entrante.md`).
- Observé : la passation sépare explicitement « décisions déjà prises », « diagnostic validé avec Dominic », « changements convenus, pas encore rédigés » et « questions ouvertes à poser » : même distinction que celle exigée par le présent audit.

## Expérience multi-agents et consolidation (CL-13)

- Observé (archive v1.5, CHANGELOG v1.6, orchestration-maitre.md) : v1.5 = 3 projets Claude séparés (Scripteur, Éditeur-validateur, Producteur), Dominic faisant le copier-coller des paquets entre projets; la bible et le gabarit étaient recopiés à l'identique dans chaque projet. v1.6 = une seule conversation « Maître » (chef d'orchestre + Producteur) qui lance le Scripteur et le Validateur comme sous-agents avec l'outil Agent, chacun avec son brief et uniquement les fichiers listés; le Validateur est lancé dans un agent neuf et ne reçoit jamais les échanges du Maître avec le Scripteur (indépendance). Règles d'arbitrage : le Maître vérifie chaque correction, le factuel se corrige automatiquement, l'éditorial remonte à Dominic, 3 cycles maximum, résumé final à Dominic. Première vidéo en « mode transparent » (paquet, rapport, arbitrages montrés). Exigence de Dominic consignée : les sous-agents ne doivent perdre ni puissance ni rigueur, « un vrai aller-retour, pas une validation de complaisance ».
- Observé : les briefs v1.6 gardent un « mode projet interactif » de repli pour fonctionner aussi comme instructions d'un projet Claude classique.
- Raisons de la consolidation telles que documentées (O) : friction du copier-coller manuel entre trois projets; besoin de garder un Validateur indépendant tout en centralisant l'arbitrage; capacité de la conversation à lancer des sous-agents. Déclaré par Dominic (2026-11-01) : le mode « un seul agent avec sous-agents » a donné de meilleurs résultats que les trois projets séparés; il le conserve pour l'instant. Le projet reste en développement, amélioration et tests, la vidéo test n° 2 n'étant pas encore assez satisfaisante. (Verdict sur la méthode de production de cette chaîne; ce n'est pas une décision d'architecture pour l'audit.)

## Utilisation de GitHub (complément CL-13)

- Observé : `NetBLB/studio-explainer` est explicitement désigné « source de vérité » du système. Pratiques en place : versionnage numéroté de tous les fichiers de rôles avec historique dans la bible; `CHANGELOG` par version « pour relecture par Dominic avant adoption »; archivage des versions précédentes et de la vidéo refusée; un dossier par univers visuel et par vidéo; banque de sujets avec grille de sélection et statuts; outils Python de contrôle (`lint_paquet.py`) et de production; liste de contrôle de début de séance (lire bible, orchestration, PASSATION, dossier de production; vérifier version et solde Higgsfield; annoncer en trois lignes où on en est). Tous les commits sont faits par l'IA (auteur « Claude »), 14 commits le 2026-10-29.
- Observation : c'est le seul endroit de l'écosystème où instructions, connaissances, décisions, historique et outils sont réunis dans une structure versionnée. Les 12 projets Claude n'ont pas d'équivalent observé.

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
