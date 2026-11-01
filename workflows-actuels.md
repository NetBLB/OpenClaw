# Méthodes de travail actuelles

Dernière mise à jour : 2026-11-01

Objectif : documenter COMMENT Dominic travaille aujourd'hui, pas seulement où se trouvent ses données. Chaque énoncé porte un statut (Observation / Hypothèse).

## Interfaces utilisées

- Observation (2026-11-01) : Claude Desktop sur Windows, avec dossiers liés (ex. dossier « production » dans CL-02).
- Déclaré par Dominic (2026-11-01) : Claude dispose d'un accès complet à sa machine Windows et travaille en arrière-plan directement dans DaVinci Resolve (projets CL-01 et CL-02) via un connecteur DaVinci Resolve. Cette méthode est un pilier des workflows de montage; à classer dans `elements-a-preserver.md` lorsque Dominic le confirmera.

## Démarrage d'un projet

- Déclaré par Dominic (2026-11-01) : il a beaucoup d'idées de projets en développement. Lorsqu'il a une idée, il démarre **toujours** un projet Claude ou un projet ChatGPT pour brainstormer et développer l'idée (ex. CL-10, en idéation). Conséquence : les deux plateformes contiennent vraisemblablement des projets d'idéation à divers stades de maturité, certains peut-être abandonnés (H).
- Déclaré par Dominic (2026-11-01) : **souvent, les idées démarrent dans ChatGPT**. Dominic demande ensuite à ChatGPT de créer une instruction personnalisée pour une tâche spécifique, puis crée un projet Claude spécialisé avec cette instruction. Exemple : projet ChatGPT « Application Retraite » (conseiller, idéation, remise en question) + projet Claude CL-10 (spécialiste développement de produit). Rôles observés : ChatGPT = idéation, conseil, rédaction d'instructions; Claude = exécution spécialisée. Hypothèse : d'autres projets Claude de l'inventaire ont aussi des instructions rédigées par ChatGPT (Q-014).
- Déclaré par Dominic (2026-11-01), deuxième exemple du même patron : l'outil « horaire éphémère » (CL-14) a été conçu sur ChatGPT; ChatGPT a aidé à créer le Claude qui a codé l'application; l'application vit dans Claude (artefact) et y est utilisée chaque semaine; ChatGPT reste le lieu d'exploration stratégique (rendre public, commercialiser). Observation : un outil d'usage hebdomadaire vit dans une seule conversation Claude, sans projet ni dépôt observé (Q-019).

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

## Conversations épinglées comme outils (ChatGPT)

- Observé et déclaré (2026-11-01) : au-delà des projets, Dominic utilise des conversations ChatGPT épinglées comme outils récurrents, certaines avec des instructions personnalisées données directement dans la conversation (ex. « Info de description YouTube »). Les ateliers qui ont produit les instructions d'un projet Claude sont conservés comme conversations épinglées pour continuer à raffiner ces instructions (ex. « Créer un prompt Short » pour CL-01, « Affiner instruction montage » pour CL-02). Conséquence : pour ces projets, l'historique d'évolution des instructions vit dans une conversation ChatGPT (O).

## Chaîne de publication LeFiltre (observée)

- Vidéo longue : montage par Claude dans DaVinci (CL-02) → transcription collée dans la conversation ChatGPT « Info de description YouTube » (GPTC-03) → titre, description, tags, liens → description YouTube. Transfert par copier-coller.
- Shorts : découpage par Claude dans DaVinci (CL-01) → 1 à 10 Shorts envoyés à la conversation ChatGPT « Titres et descriptions vidéos » (GPTC-02) → titres, liens, descriptions → collés dans Metricool, qui publie automatiquement sur toutes les plateformes. Transfert par copier-coller.

## Développement du site NetBLB (ChatGPT chef d'orchestre + Codex + GitHub)

- Déclaré et observé (2026-11-01) : un agent chef d'orchestre ChatGPT a organisé le projet du nouveau site netblb.com et rédigé les instructions de Codex; Codex a construit le site en entier, par branches et pull requests sur `NetBLB/netblb-site` (au moins 13 PR), chacune validée par build et audits, sans fusion ni déploiement de sa part. Le dépôt est déclaré source de vérité du code et des contenus; `AGENTS.md` y conserve les orientations commerciales, éditoriales et techniques. Déploiement manuel sur Plesk selon un guide du dépôt.
- Déclaré : surveillance automatisée par une tâche planifiée ChatGPT chaque lundi (rapport SEO/GEO et santé du site, sources GSC et GA4 via Windsor.ai), avec des instructions détaillées de type procédure (sources, seuils, format, règles de prudence, action « agir maintenant » ou « attendre »). Seule automatisation planifiée observée dans l'écosystème.
- Observation transversale : le patron « chef d'orchestre ChatGPT → exécutant » s'applique avec Claude (projets Claude, outils codés) comme avec Codex (site). Quand l'exécutant travaille dans un dépôt (studio-explainer, netblb-site), ses instructions sont versionnées dans GitHub (`AGENTS.md`, briefs); quand l'exécutant est un projet Claude, elles ne vivent que dans le projet et dans la conversation ChatGPT d'origine.

## Rôles respectifs de Claude et ChatGPT (synthèse provisoire au 2026-11-01)

- Déclaré (conversation ChatGPT d'origine) : tous les projets de développement sont gérés depuis les interfaces Claude et ChatGPT; GitHub sert déjà d'entrepôt des informations importantes.
- Observé à travers l'inventaire : **ChatGPT** = point de départ des idées, conseil, remise en question, rédaction des instructions personnalisées des projets Claude, exploration stratégique (commercialisation, notoriété); **Claude** = exécution spécialisée (montage DaVinci, campagnes Resend, codage d'outils, production vidéo avec sous-agents, propositions clients), avec forfait à gros volume justifié par le montage vidéo. Hypothèse à confirmer en phase 2 : cette répartition est-elle voulue ou le fruit des circonstances?

## Transmission du contexte à une IA (complément)

- Déclaré par Dominic (2026-11-01) : les instructions personnalisées d'un projet Claude peuvent être produites par ChatGPT à partir de la discussion d'idéation. Le contexte passe donc de ChatGPT à Claude sous forme d'instructions, par copier-coller (H sur le mécanisme exact).

## Transmission du contexte à une IA

- (à documenter)

## Conservation des décisions

- (à documenter)

## Suivi des idées et conservation des liens (ART-02)

- Déclaré par Dominic et observé (2026-11-01) : un outil codé par Claude (« Projets LeFiltre », artefact avec base de données) sert de registre de toutes les idées de projets, de leur statut et de leur avancement. Chaque fiche contient des liens directs vers les conversations ChatGPT, projets Claude, chats Claude et artefacts ayant servi au projet. C'est le seul index transversal ChatGPT + Claude observé dans l'écosystème; il est tenu à jour (modifications le jour de l'audit). Méthode candidate « à préserver » (classement à confirmer).
- Observation : ce registre et l'inventaire du présent audit se recoupent partiellement (idées de projets). Relation à définir en phase 4 (lequel est la référence pour quoi), sans recommandation à ce stade.

## Versionnage des instructions

- Déclaré par Dominic (2026-11-01) : le suffixe « V1 » dans le nom d'un projet (CL-09) désigne la version des instructions personnalisées; une amélioration significative fera passer à « V2 ». Observation : CL-09 est le seul projet vu jusqu'ici avec un numéro de version; les autres sont modifiés sans trace de version visible (H, voir Q-011b).

## Utilisation de GitHub

- Observation (2026-11-01) : un dépôt privé dédié sert de source de vérité documentaire pour cet audit; la mémoire du projet Claude est considérée comme non suffisante à elle seule.

## Utilisation de Claude

- (à documenter)

## Utilisation de ChatGPT

- Déclaré par Dominic (2026-11-01) : **chaque client de Net BLB inc. a un projet ChatGPT, et c'est dans ce projet que chaque requête, demande ou mandat du client est traité.** Neuf projets clients observés (Québec D'Abord, Evoluo, FP CONSEILS (H), Portée Sociale, Valérie Comptabilité, PointComm, Julie Comptabilité, Cité Joie, Molloy). Les clients sont donc cloisonnés par projet, et entièrement du côté ChatGPT. Méthode candidate « à préserver » (classement à confirmer).
- Déclaré : ChatGPT héberge aussi des « machines » de production répétitive pour LeFiltre (extraction d'anciens articles Patreon vers des fiches coffre-fort, articles Patreon sur l'IA, aides-mémoire pour vidéos de nouvelles), des ateliers d'idéation (machine à idées, conseiller app retraite) et des ateliers d'instructions pour Claude (video éditoriale V1.2).
- Déclaré : pour un même outil (proposition de coaching FP Conseils), Dominic a fait tourner en parallèle un projet Claude et un projet ChatGPT identiques et a laissé le client comparer les résultats. Méthode de choix de plateforme par test comparatif (O).
- Observé : un projet en pause (LeFiltre Italie) et un projet inutilisé (SHORTS) sont conservés dans la liste plutôt que supprimés; cohérent avec les projets Claude désuets conservés (CL-15, CL-16). Hypothèse : Dominic ne supprime pas, il laisse en place (Q-013).

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
