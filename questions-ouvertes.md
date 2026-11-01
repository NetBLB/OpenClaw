# Questions ouvertes

Dernière mise à jour : 2026-11-01

Questions accumulées pendant l'audit, à poser au moment où elles deviennent nécessaires. Une question résolue est déplacée dans la section « Résolues » avec la réponse et la date.

## En attente

| # | Question | Élément concerné | Phase | Priorité | Ajoutée le |
|---|---|---|---|---|---|
| Q-001b | Détail technique restant : le « connecteur DaVinci Resolve » est-il un MCP local, un script, ou autre? CL-01 utilise-t-il exactement le même? (à préciser en phase 2, non bloquant) | CL-01, CL-02 | 2 | Moyenne | 2026-11-01 |
| Q-005 | Le projet a-t-il des fichiers de connaissances (grille tarifaire, historique des ententes, modèles de courriels) ou tout est-il dans les instructions? Où sont conservés l'historique des négociations conclues ou refusées, les contrats reçus et les analyses produites par CL-11? Comment le contexte d'un sponsor passe-t-il de CL-03 à CL-11 (copier-coller du contrat seulement, ou aussi l'historique de négociation)? | CL-03, CL-11 | 2 | Moyenne-haute | 2026-11-01 |
| Q-007 | Le dossier « claude » contenant les 5 fichiers de CL-05 est-il un dossier téléversé dans le projet ou un dossier lié depuis le PC (synchronisé)? Qui met à jour `historique thinkific.md` et `historique thinkific suivi` : Dominic ou l'agent lui-même? La déduplication mensuelle repose-t-elle sur ces fichiers, sur la mémoire du projet, ou sur les contacts/segments Resend? | CL-05 | 2 | Moyenne (conservation de l'état entre sessions) | 2026-11-01 |
| Q-008b | Vers quelle(s) IA la sortie de RECHERCHISTE est-elle copiée-collée (projet Claude de scénarisation, ChatGPT, GPT personnalisé)? À identifier quand ces projets apparaîtront dans l'inventaire. | CL-06 | 2 | Moyenne | 2026-11-01 |
| Q-009 | Les conditions du « programme de test hardware » (troc, distribution) sont-elles documentées ailleurs que dans les instructions de CL-07 (page web, document, Patreon)? Qu'est-ce qui décide qu'une demande va vers CL-03 plutôt que CL-07 : Dominic choisit manuellement selon le type de demande? | CL-07, CL-03 | 2 | Moyenne | 2026-11-01 |
| Q-011b | Lors du passage à « V2 » des instructions, le projet est-il renommé en place, dupliqué, ou l'ancienne version des instructions est-elle conservée quelque part (fichier, GitHub)? Pourquoi seul CL-09 porte un numéro de version alors que les autres projets n'en ont pas? | CL-09 et tous les projets | 2 | Moyenne (conservation de l'historique des instructions) | 2026-11-01 |
| Q-013 | Les idées abandonnées sont-elles supprimées, archivées ou laissées en place (dans ChatGPT comme dans Claude)? | Méthode générale | 2 | Moyenne | 2026-11-01 |
| Q-014 | Parmi les projets Claude inventoriés (CL-01 à CL-09), lesquels ont des instructions rédigées par ChatGPT, lesquels ont été rédigés directement par Dominic ou par Claude? Les instructions sont-elles conservées ailleurs que dans le projet (fichier, ChatGPT, GitHub)? | Tous les projets Claude | 2 | Haute (source de vérité des instructions) | 2026-11-01 |
| Q-015b | Le Skill `video-making` présent sur le compte est-il lié à CL-13 ou à autre chose? | CL-13 | 2 | Basse | 2026-11-01 |
| Q-021 | Projet « Site Web NetBLB » (PRJ-08) : que fait la tâche planifiée ChatGPT « Rapport » (contenu, fréquence)? Quel est le rôle de Codex (développement du site en local?)? | AUTO-01, OUT-05 | 1 (inventaire ChatGPT) | Moyenne | 2026-11-01 |
| Q-023 | Clients Cité-Joie et Julie Savoie (cités par ChatGPT) : existent-ils comme projets, et où (ChatGPT? Claude? ailleurs)? Y a-t-il d'autres clients Net BLB avec un espace IA dédié? | Inventaire ChatGPT | 1 | Haute (séparation entre clients) | 2026-11-01 |
| Q-024 | « Détecteur d'arnaque », « Décrypteur de contrats », « Diagnostic PC » (cités par ChatGPT) : sont-ce des GPT personnalisés? Lesquels existent encore? | Inventaire GPT | 1 | Moyenne | 2026-11-01 |
| Q-025 | Dépôt GitHub du site NetBLB (Astro) : existe-t-il, sous quel nom, et est-il accessible à cette session? Autres dépôts GitHub non encore listés (seuls studio-explainer et OpenClaw sont visibles)? | Inventaire GitHub | 1 | Moyenne | 2026-11-01 |
| Q-019b | Horaire éphémère (ART-01) : où l'état d'une semaine à l'autre est-il sauvegardé (stockage du navigateur ou base de données d'artefact)? À vérifier dans le code en phase 2. | CL-14 | 2 | Moyenne | 2026-11-01 |
| Q-004 | Les instructions de CL-02 se désignent comme « ce Skill » : ont-elles été rédigées comme un Skill (fichier SKILL.md) puis collées dans les instructions du projet? Existe-t-il un Skill séparé en plus du projet? | CL-02 | 2 | Moyenne (méthode de création d'IA spécialisées) | 2026-11-01 |
| Q-002 | Que contiennent les 7 fichiers de contexte du projet? | CL-01 | 2 | Moyenne | 2026-11-01 |
| Q-003 | Une session récente s'intitule « ChatGPT 6… » : y a-t-il un lien avec ChatGPT dans ce workflow (import, comparaison, transfert)? | CL-01 | 2 | Basse | 2026-11-01 |

## Résolues

| # | Question | Réponse | Date |
|---|---|---|---|
| Q-001 | Par quel mécanisme Claude travaille-t-il dans DaVinci Resolve? | Réponse de Dominic : Claude a un accès complet à sa machine et travaille en arrière-plan directement dans DaVinci Resolve (via un connecteur DaVinci Resolve observé dans CL-02). Reliquat technique déplacé en Q-001b. | 2026-11-01 |
| Q-006 | Relation entre le projet CL-04 et le Skill `fp-conseils-proposition-coaching`? | Trois couches distinctes et complémentaires : instructions du projet = règles métier; Skill = méthode d'exécution (manipulation du gabarit et génération du .pptx); gabarit PowerPoint = contenant. Voir inventaire CL-04 et SK-01. | 2026-11-01 |
| Q-008 | RECHERCHISTE sert-il uniquement LeFiltre? Où va sa sortie? | Sert la création de vidéos YouTube, de vidéos Patreon et d'articles écrits Patreon (LeFiltre). Sortie copiée-collée vers une autre IA ou utilisée pour une feuille de notes de vidéo. L'IA aval reste à identifier (Q-008b). | 2026-11-01 |
| Q-022b | Contenu de la conversation ChatGPT d'origine : intentions ou décisions non reflétées dans l'audit? | PDF de 63 pages lu intégralement. Les instructions du projet en sont issues telles quelles. Les déclarations de Dominic, les éléments d'environnement cités par ChatGPT (à vérifier) et les pistes de ChatGPT (non validées) sont consignés dans `inventaire.md` section 6c. Aucune décision d'architecture n'y a été prise; trois choix de méthode d'audit y figurent (projet Claude, GitHub dès le départ, deux passes), déjà appliqués. | 2026-11-01 |
| Q-018 | Les projets Claude « Scripteur » et « Éditeur » (désuets) existent-ils encore? | Oui, encore présents (2026-11-01). Ajoutés à l'inventaire comme CL-15 et CL-16. | 2026-11-01 |
| Q-022 | Les instructions du projet Claude de l'audit viennent-elles de ChatGPT? | Oui : la conversation ChatGPT « IA de départ » (lien dans ART-02) a permis de créer le projet Claude « OPEN CLAW - conception ». Contenu à lire (Q-022b). | 2026-11-01 |
| Q-020 | Fonction de l'artefact « Projets LeFiltre » (ART-02)? | Outil de suivi de toutes les idées de projets (statut, avancement, liens vers les IA ayant servi), codé par Claude, données en base d'artefact; 15 projets extraits dans l'inventaire (section 6b). | 2026-11-01 |
| Q-020a | Fonction de « Conducteur AGI Astra » (ART-04)? | Conducteur d'une vidéo éditoriale; obsolète selon Dominic. ART-03 (grille Keychron) : grille d'évaluation d'un produit sponsor, pas encore utilisée, usage incertain. | 2026-11-01 |
| Q-019 | Où vit l'horaire éphémère? | Artefact Claude publié (ART-01, adresse propre, code HTML récupérable), confirmé par extraction. Aucun fichier sur le PC ni sur GitHub (déclaré). | 2026-11-01 |
| Q-017 | Verdict de Dominic sur le mode Maître + sous-agents vs trois projets séparés? | Le mode « un seul agent avec sous-agents » a donné de meilleurs résultats que l'ancien mode; Dominic le conserve pour l'instant. Le projet reste toutefois en développement, amélioration et tests : la vidéo test n° 2 n'a pas donné de résultats assez satisfaisants à son goût. | 2026-11-01 |
| Q-015 | NetBLB/studio-explainer est-il le dépôt de la chaîne sans visage? Le Claude fusionné est-il un chat ou un projet? | Oui, confirmé par capture GitHub; le dépôt est la source de vérité du système v1.6/1.7. Le Maître est une conversation Claude hors projet (« Projet YouTube et compte Higgsfield »), pas un projet. | 2026-11-01 |
| Q-016 | Pourquoi deux connecteurs Higgsfield? | PASSATION.md : `Higgsfield_MCP` = connecteur complet utilisé pour produire; `Higgsfield_AI` = lecture seule. | 2026-11-01 |
| Q-012 | Nature du « conseiller stratégique indépendant » dans CL-10? | Un projet ChatGPT. Dominic démarre souvent ses idées dans ChatGPT, lui demande de créer une instruction personnalisée pour une tâche spécifique, puis crée le projet Claude spécialisé (ici : développement de produit). | 2026-11-01 |
| Q-011 | Signification du suffixe « V1 » dans CL-09? | Version 1 des instructions personnalisées. Pas de V2 pour l'instant; lors d'une amélioration des instructions, Dominic passera à V2. Reliquat en Q-011b. | 2026-11-01 |
| Q-010 | « Articles du blog LeFiltre » (CL-08) = articles Patreon? | Oui, exactement la même chose : les publications écrites Patreon. Pas de blog distinct. | 2026-11-01 |
