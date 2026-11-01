# Éléments et méthodes à préserver

Dernière mise à jour : 2026-11-01

Classement de chaque méthode ou élément important. Le classement est proposé par l'IA, puis confirmé ou corrigé par Dominic. Tant qu'il n'est pas confirmé, il reste une hypothèse.

| Élément / méthode | Classement proposé | Confirmé par Dominic | Raison | Notes |
|---|---|---|---|---|
| Séparation règles métier (instructions) / méthode d'exécution (Skill) / contenant (gabarit) pour les outils IA clients (CL-04, SK-01) | À préserver absolument (proposé) | Non | Méthode mise en place volontairement; facilite l'implantation chez un client et la réutilisation pour d'autres types de propositions | Classement à confirmer par Dominic |
| Claude avec accès complet au PC Windows, travaillant en arrière-plan dans DaVinci Resolve (CL-01, CL-02) | À préserver absolument (proposé) | Non | Pilier des workflows de montage | Classement à confirmer par Dominic |
| Dépôt GitHub comme source de vérité d'un système IA, avec versionnage numéroté, changelog soumis à relecture, archives, PASSATION.md réécrit à chaque séance et liste de contrôle de début de séance (CL-13 / GH-01) | À préserver absolument (proposé) | Non | Méthode la plus aboutie de l'écosystème; déjà conforme à ce que l'audit exige (observation / décision / question ouverte) | Classement à confirmer par Dominic |
| Validateur indépendant qui ne reçoit que les livrables et les règles, jamais les échanges avec le rédacteur; interdiction d'inventer une consigne (CL-13) | À préserver absolument (proposé) | Non | Exigence explicite de Dominic : rigueur réelle, pas de validation de complaisance; né d'un incident vécu | Classement à confirmer par Dominic |
| Registre unique des idées de projets avec statut, avancement et liens vers les IA concernées (ART-02) | À préserver absolument (proposé) | Non | Seul index transversal ChatGPT + Claude; tenu à jour par Dominic | Forme (artefact Claude) et relation avec l'inventaire de l'audit à discuter en phase 4 |
| Un espace isolé par client Net BLB inc., où tous les mandats du client sont traités (projets ChatGPT GPT-01 à GPT-14) | À préserver absolument (proposé) | Non | Cloisonnement des données clients; patron appliqué systématiquement | Forme (projet ChatGPT) à discuter en phase 4; le principe d'isolation est ce qui est proposé à la préservation |
| Instructions d'agent versionnées dans le dépôt du projet (`AGENTS.md` de netblb-site, briefs de studio-explainer) et exécutant qui travaille par PR validées sans fusionner ni déployer lui-même | À préserver absolument (proposé) | Non | Source de vérité unique, traçabilité, Dominic garde la main sur la mise en production | Classement à confirmer par Dominic |
| Rapport de surveillance hebdomadaire automatisé avec règles de prudence explicites (AUTO-01) | À préserver si possible (proposé) | Non | Seule automatisation planifiée; instructions de qualité procédure | La forme (tâche ChatGPT) pourra être rediscutée; le contenu des instructions est l'actif |
| Règles de sécurité du montage : timeline source jamais modifiée, nouvelle timeline de travail, prémontage de Dominic respecté, aucune décision éditoriale sur le fond (CL-01, CL-02) | À préserver absolument (proposé) | Non | Protège le travail et la voix éditoriale de Dominic; règle explicite en tête des deux jeux d'instructions | |
| Travail de l'IA en arrière-plan par API, sans prise de contrôle de l'ordinateur sans autorisation explicite (CL-01) | À préserver absolument (proposé) | Oui, comme règle générale (déclaré le 2026-11-01) | Dominic garde l'usage de sa machine pendant le montage; frontière explicite entre autonomie et contrôle | Première méthode confirmée par Dominic. Son inscription dans `decisions.md` attend la validation en bloc des autres éléments. |
| Vérification par le résultat : images exportées, listes de contrôle, seconde passe obligatoire (CL-01, CL-02); fichiers « leçons » par vidéo (CL-02) | À préserver si possible (proposé) | Non | Boucle qualité et apprentissage déjà en place | Mécanisme exact des leçons à clarifier (Q-047) |
| Doctrine éditoriale LeFiltre : sources primaires d'abord, ne jamais inventer, distinguer fait / interprétation / opinion, signaler avant de corriger un fait, liens directs vers la page exacte (CL-06, CL-08, CL-09) | À préserver absolument (proposé) | Non | Fondement de la crédibilité de la chaîne; présent dans les trois jeux d'instructions | Le vocabulaire de classement diffère entre projets (phase 3) |
| Paramètres modifiables en tête d'instructions (NIVEAU D'HUMOUR dans CL-08, SEUIL_SILENCE dans CL-02) | À préserver si possible (proposé) | Non | Réglage sans réécrire les instructions | |
| Séparation volontaire recherche (CL-06) / rédaction (GPT-22, autre IA) / édition (CL-08), avec Dominic entre chaque étape | À clarifier avec Dominic | Non | Peut être une méthode voulue (contrôle à chaque étape) ou un héritage des outils | Question Q-055 |
| Doctrine sponsor : conditions non négociables (contrôle éditorial, produit testé, avis honnête, pas de script imposé, mention de partenariat, pertinence audience), plancher tarifaire, aucun rabais sans contrepartie, catégories refusées, recherche autonome avant réponse (CL-03) | À préserver absolument (proposé) | Non | Règles métier explicites qui protègent la valeur de la chaîne; indépendantes de la plateforme | Le contenu de la doctrine est l'actif; sa duplication dans 4 jeux d'instructions est le problème (phase 3) |
| Patron « analyse d'abord, livrable ensuite » dans les outils sponsors (CL-03, CL-07, CL-11) | À préserver si possible (proposé) | Non | Dominic décide avec une analyse sous les yeux avant d'envoyer | |
| Maquette audio et GO de Dominic avant toute dépense de crédits vidéo; décisions éditoriales toujours remontées à Dominic (CL-13) | À préserver si possible (proposé) | Non | Contrôle des coûts et de la ligne éditoriale | Classement à confirmer par Dominic |

Classements possibles :
- À préserver absolument
- À préserver si possible
- Peut être repensé
- À clarifier avec Dominic

## Améliorations envisagées (format obligatoire)

Pour chaque amélioration : fonctionnement actuel, amélioration proposée, avantages, inconvénients, ce qui serait perdu, ce qui serait gagné. Décision laissée à Dominic.

- (aucune pour l'instant)
