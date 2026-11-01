# Questions ouvertes

Dernière mise à jour : 2026-11-01

Questions accumulées pendant l'audit, à poser au moment où elles deviennent nécessaires. Une question résolue est déplacée dans la section « Résolues » avec la réponse et la date.

## En attente

| # | Question | Élément concerné | Phase | Priorité | Ajoutée le |
|---|---|---|---|---|---|
| Q-001b | Détail technique restant : le « connecteur DaVinci Resolve » est-il un MCP local, un script, ou autre? CL-01 utilise-t-il exactement le même? (à préciser en phase 2, non bloquant) | CL-01, CL-02 | 2 | Moyenne | 2026-11-01 |
| Q-005 | Le projet a-t-il des fichiers de connaissances (grille tarifaire, historique des ententes, modèles de courriels) ou tout est-il dans les instructions? Où est conservé l'historique des négociations conclues ou refusées? | CL-03 | 2 | Moyenne | 2026-11-01 |
| Q-007 | Le dossier « claude » contenant les 5 fichiers de CL-05 est-il un dossier téléversé dans le projet ou un dossier lié depuis le PC (synchronisé)? Qui met à jour `historique thinkific.md` et `historique thinkific suivi` : Dominic ou l'agent lui-même? La déduplication mensuelle repose-t-elle sur ces fichiers, sur la mémoire du projet, ou sur les contacts/segments Resend? | CL-05 | 2 | Moyenne (conservation de l'état entre sessions) | 2026-11-01 |
| Q-008b | Vers quelle(s) IA la sortie de RECHERCHISTE est-elle copiée-collée (projet Claude de scénarisation, ChatGPT, GPT personnalisé)? À identifier quand ces projets apparaîtront dans l'inventaire. | CL-06 | 2 | Moyenne | 2026-11-01 |
| Q-009 | Les conditions du « programme de test hardware » (troc, distribution) sont-elles documentées ailleurs que dans les instructions de CL-07 (page web, document, Patreon)? Qu'est-ce qui décide qu'une demande va vers CL-03 plutôt que CL-07 : Dominic choisit manuellement selon le type de demande? | CL-07, CL-03 | 2 | Moyenne | 2026-11-01 |
| Q-010 | Les instructions de CL-08 parlent des « articles du blog LeFiltre »; Dominic parle d'articles Patreon. S'agit-il du même support (ex. Le Magazine sur Patreon), ou existe-t-il aussi un blog distinct (lefiltre.com ou autre)? | CL-08 | 2 | Moyenne | 2026-11-01 |
| Q-004 | Les instructions de CL-02 se désignent comme « ce Skill » : ont-elles été rédigées comme un Skill (fichier SKILL.md) puis collées dans les instructions du projet? Existe-t-il un Skill séparé en plus du projet? | CL-02 | 2 | Moyenne (méthode de création d'IA spécialisées) | 2026-11-01 |
| Q-002 | Que contiennent les 7 fichiers de contexte du projet? | CL-01 | 2 | Moyenne | 2026-11-01 |
| Q-003 | Une session récente s'intitule « ChatGPT 6… » : y a-t-il un lien avec ChatGPT dans ce workflow (import, comparaison, transfert)? | CL-01 | 2 | Basse | 2026-11-01 |

## Résolues

| # | Question | Réponse | Date |
|---|---|---|---|
| Q-001 | Par quel mécanisme Claude travaille-t-il dans DaVinci Resolve? | Réponse de Dominic : Claude a un accès complet à sa machine et travaille en arrière-plan directement dans DaVinci Resolve (via un connecteur DaVinci Resolve observé dans CL-02). Reliquat technique déplacé en Q-001b. | 2026-11-01 |
| Q-006 | Relation entre le projet CL-04 et le Skill `fp-conseils-proposition-coaching`? | Trois couches distinctes et complémentaires : instructions du projet = règles métier; Skill = méthode d'exécution (manipulation du gabarit et génération du .pptx); gabarit PowerPoint = contenant. Voir inventaire CL-04 et SK-01. | 2026-11-01 |
| Q-008 | RECHERCHISTE sert-il uniquement LeFiltre? Où va sa sortie? | Sert la création de vidéos YouTube, de vidéos Patreon et d'articles écrits Patreon (LeFiltre). Sortie copiée-collée vers une autre IA ou utilisée pour une feuille de notes de vidéo. L'IA aval reste à identifier (Q-008b). | 2026-11-01 |
