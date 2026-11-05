# 03. Retours d'expérience réels sur OpenClaw et vidéos YouTube

Recherche effectuée le 2026-11-05. Objet : ce que rapportent les utilisateurs d'OpenClaw après plusieurs semaines ou mois d'usage, avec un accent sur les vidéos YouTube. Les faits sont rapportés tels que les auteurs les déclarent; les déductions sont signalées comme telles. Aucune décision n'est prise ici.

Conventions :
- « Déclaré » = affirmation de l'auteur de la source, non vérifiée par nous.
- « Déduit » = notre interprétation.
- Les montants sont en dollars américains sauf mention contraire.
- Les dates sont celles affichées par les sources; quand l'année manque, elle est déduite du contexte (2026) et signalée « (année déduite) ».

---

## 1. Méthode

### Requêtes utilisées (WebSearch)
- OpenClaw « after a month » / « 30 days » / « weeks » review
- OpenClaw youtube tutorial full setup telegram VPS
- OpenClaw cost per month API tokens real usage
- OpenClaw security incident vulnerability exposed instances
- OpenClaw memory MEMORY.md Obsidian second brain
- OpenClaw hacker news; site:news.ycombinator.com OpenClaw
- OpenClaw youtube multi-agent / sub-agents / second brain
- site:reddit.com r/openclaw memory telegram cost
- github.com/openclaw/openclaw discussions (l'API GraphQL GitHub est bloquée depuis cette session; les discussions n'ont pas pu être listées directement)
- OpenClaw heartbeat cron silently / loop / stopped working
- OpenClaw Paperclip org chart experience
- OpenClaw WhatsApp banned Baileys
- OpenClaw sub-agents lessons learned
- Recherches en français (retour d'expérience, tutoriel) : peu de résultats originaux, surtout des traductions automatiques de vidéos anglophones.

### Comment les vidéos ont été lues
YouTube ne fournit pas de transcription par WebFetch. Les transcriptions et résumés ont été obtenus par des sites tiers :
- rosetta.to (transcriptions automatiques complètes) : 1littlecoder, Limitless Podcast, NetworkChuck, Fireship, David Ondrej, Matthew Berman.
- sozai.app (transcription) : Brian Casel.
- videohighlight.com (résumé IA structuré) : Caleb, Alex Finn (deux vidéos), « Safer OpenClaw Alternative ».
- lilys.ai / readray.ai (résumés IA, souvent traduits) : VelvetShark, The AI Doctor, Alex Finn « army », Brock Mesarich, Rachel How, Artem Zhutov.
- summyt.app (résumé IA) : Alex Finn « only tutorial ».
- Compagnons écrits : gist GitHub de VelvetShark, article de Dave Swift.
- Échecs : recapio.com n'avait pas généré ses résumés (Riley Brown, LaRue, Rafael Quintanilha), podwise.ai demande une connexion (Clearmud), nbot.ai ne donne que des liens.

Les résumés IA peuvent contenir des erreurs de transcription (noms de produits, dates au format ambigu). Ils sont signalés comme « résumé IA » et non « transcription ».

### Volumes
- Vidéos YouTube lues : 19 (6 par transcription complète, 13 par résumé IA ou compagnon écrit).
- Vidéos repérées mais illisibles : 5.
- Sources écrites lues : 34 (blogues, dev.to, fils Reddit archivés, Hacker News, gist, cours).
- Fils Reddit : consultés via l'archive reddit.sentinel-team.org (instantanés datés), Reddit lui-même n'étant pas accessible.
- Hacker News : un fil lu (Ask HN, 87 commentaires); un second fil n'a pas pu être lu (limite de débit du site).

---

## 2. Vidéos YouTube

### 2.1 Tableau

| # | Titre | Chaîne | Date | Lu | Pertinence pour notre projet |
|---|---|---|---|---|---|
| V1 | 50 days with OpenClaw: The hype, the reality & what actually broke | VelvetShark | 2026-02-20 | Résumé IA + gist compagnon | Très élevée (usage prolongé, VPS, mémoire fichiers, cron, sous-agents) |
| V2 | Full OpenClaw Setup Tutorial: Step-by-Step Walkthrough | Caleb (chaîne non confirmée) | n.d. | Résumé IA | Moyenne (VPS Hostinger, Telegram, cron) |
| V3 | OpenClaw is open! Run your 24x7 Clawdbot on a Secure VPS | 1littlecoder | 2026-02-17 | Transcription | Élevée (VPS, Tailscale, Telegram vs WhatsApp) |
| V4 | OpenClaw Full Tutorial: Installation, Setup & Real Automation Use | The AI Doctor | 2026-01-31 | Résumé IA | Moyenne (VPS, Telegram recommandé, WhatsApp numéro dédié) |
| V5 | My Multi-Agent Team with OpenClaw | Brian Casel | 2026-02-20 | Transcription | Très élevée (4 agents, workspace partagé, Slack vs Telegram, coûts) |
| V6 | We Used OpenClaw for a Week. This is the Harsh Truth | Limitless Podcast | 2026-02-18 | Transcription | Moyenne (mises à jour cassantes, coûts, sécurité) |
| V7 | How to build an army of OpenClaw agents | Alex Finn | 2026-03-12 (format ambigu) | Résumé IA | Élevée (orchestrateur + sous-agents, modèles par rôle) |
| V8 | OpenClaw Is Finally Easy and Safe to Use | Brock Mesarich (AI for Non Techies) | 2026-02-02 | Résumé IA | Faible (plateforme managée Emergent) |
| V9 | I spent two weeks with OpenClaw so you don't have to | Rachel How | 2026-03-07 (format ambigu) | Résumé IA | Élevée (créatrice YouTube, analytics YouTube, Mac mini, Tailscale) |
| V10 | OpenClaw......RIGHT NOW??? (it's not what you think) | NetworkChuck | 2026-03-30 | Transcription | Élevée (VPS, fichiers mémoire, sécurité, équipe d'agents) |
| V11 | The wild rise of OpenClaw | Fireship | 2026-01-30 | Transcription | Faible (présentation générale) |
| V12 | 100 hours of OpenClaw lessons in 35 minutes | Alex Finn | n.d. | Résumé IA | Élevée (leçons d'usage, local vs VPS, modèles, erreurs) |
| V13 | OpenClaw + Obsidian gives you super powers | Alex Finn | n.d. | Résumé IA | Très élevée (mémoire 4 couches, structure de coffre, multi-agents) |
| V14 | How to Give OpenClaw Real Persistent Memory with Obsidian | Dave Swift | 2026-02-18 | Article compagnon (partiel) | Élevée (mémoire Obsidian, sync, confidentialité) |
| V15 | I made my OpenClaw 10x more powerful (seriously) | David Ondrej | 2026-02-09 | Transcription | Élevée (9 fichiers Markdown, modèles par tâche, cron, heartbeat) |
| V16 | 21 INSANE Use Cases For OpenClaw | Matthew Berman | 2026-02-17 | Transcription | Très élevée (créateur YouTube, CRM, conseils nocturnes, cron, sauvegardes Git) |
| V17 | The only OpenClaw tutorial you'll ever need (March 2026 edition) | Alex Finn | 2026-03 | Résumé IA | Élevée (Mission Control, Discord vs Telegram, skills) |
| V18 | The OpenClaw Setup Nobody's Talking About | Artem Zhutov | 2026-01-30 | Résumé IA | Très élevée (Obsidian 4 000 notes, assistant contextuel non autonome, permissions minimales) |
| V19 | I Built a Safer OpenClaw Alternative Using Claude Code | Chaîne non confirmée (communauté Dynamous) | n.d. | Résumé IA | Moyenne (pourquoi quitter OpenClaw, heartbeat maison) |
| V20 | Why Specialized Agents are Superior (OpenClaw Superteam) | Riley Brown | n.d. | Rien (résumé non généré) | Inconnue |
| V21 | OpenClaw Agents vs Sub-Agents: You're Probably Confused | Clearmud | 2026-03-10 | Rien (connexion requise) | Inconnue |
| V22 | How to Make OpenClaw 10x More Powerful | LaRue | n.d. | Rien | Inconnue |
| V23 | Montei minha própria empresa com IA utilizando o OpenClaw | Rafael Quintanilha (QuantBrasil) | n.d. | Rien | Inconnue (titre : entreprise IA) |
| V24 | Every Deploys OpenClaw Coworkers for All Employees | n.d. | n.d. | Description seulement | Faible |

URL : V1 https://www.youtube.com/watch?v=NZ1mKAWJPr4 ; V2 https://www.youtube.com/watch?v=fcZMmP5dsl4 ; V3 https://www.youtube.com/watch?v=CZUYofFDs-U ; V4 https://www.youtube.com/watch?v=P6XWQ_CcAt0 ; V5 https://sozai.app/transcript/my-multi-agent-team-with-openclaw-transcript/ (URL YouTube non visible) ; V6 https://www.youtube.com/watch?v=4xDc00EF_eY ; V7 https://www.youtube.com/watch?v=Rjd1LqF9cG4 ; V8 https://www.youtube.com/watch?v=6OSEZx8v4so ; V9 https://www.youtube.com/watch?v=JeaxEA-M0Qg ; V10 https://www.youtube.com/watch?v=T-HZHO_PQPY ; V11 https://www.youtube.com/watch?v=ssYt09bCgUY ; V12 https://www.youtube.com/watch?v=_kZCoW-Qxnc ; V13 https://www.youtube.com/watch?v=6V-b073qhPA ; V14 https://youtu.be/iUkFb_TaqZA ; V15 https://www.youtube.com/watch?v=cod50CWlZeU ; V16 https://www.youtube.com/watch?v=8kNv3rjQaVA ; V17 https://www.youtube.com/watch?v=CxErCGVo-oo ; V18 https://www.youtube.com/watch?v=ZdZUNwUwHIs ; V19 https://www.youtube.com/watch?v=XmweZ4fLkcI ; V20 https://www.youtube.com/watch?v=ISb0nrlNoKQ ; V21 https://www.youtube.com/watch?v=oQBawbXQRGQ ; V24 https://www.youtube.com/watch?v=m9yoObQb88Q

### 2.2 Fiches des vidéos pertinentes

#### V1. VelvetShark, « 50 days with OpenClaw » (2026-02-20)
Source : résumé IA (readray.ai) et gist compagnon https://gist.github.com/velvet-shark/b4c6724c391f612c4de4e9a07b0a74b6 (prompts des 20 workflows).
- Configuration déclarée : OpenClaw sur VPS; Discord comme interface principale avec un salon par workflow; Obsidian (environ 2 800 à 3 000 notes) indexé par QMD chaque nuit; Coolify pour les services auto-hébergés; Tailscale pour tout accès.
- Routage par salon : Opus pour la recherche vidéo, Sonnet pour le général, le briefing et l'inbox, Haiku pour les statistiques YouTube et le monitoring.
- Cron déclarés : 3 h reconstruction de l'index QMD; 4 h mise à jour automatique et redémarrage de la gateway; 4 h 30 sauvegarde GitHub privée (avec scan des secrets remplacés par des placeholders); 7 h briefing; toutes les 30 min de 7 h à 23 h vérification e-mails, calendrier et services.
- Trois principes : Markdown d'abord, séparation des contextes, modèle adapté à la tâche.
- Ce qui a cassé (déclaré) : oubli en pleine conversation et compactage silencieux du contexte; coût d'Opus; navigation automatisée instable; une mise à jour automatique échouée en silence pendant des jours à cause d'un renommage de paquet; environ une semaine pour régler Discord.
- Sécurité : les e-mails restent en brouillon, jamais envoyés sans validation; contenu externe traité comme hostile.
- Notes de l'auteur : installation 7/10 de difficulté, valeur quotidienne 9/10, fiabilité tâches simples 8/10, tâches complexes 5/10.
- Citation : « Made with daily use, not first-week impressions. »
- Déduit : c'est la configuration la plus proche de notre projet (VPS, mémoire fichiers, cron, sauvegarde Git, modèles par tâche, approbation avant envoi). Différence : Discord plutôt que Telegram.

#### V3. 1littlecoder, « Run your 24x7 Clawdbot on a Secure VPS » (2026-02-17)
Source : transcription (rosetta.to).
- VPS Hostinger KVM2 sous Ubuntu, déploiement Docker en 2 à 3 minutes, configuration complète en 15 à 20 minutes.
- Première recommandation : activer Tailscale pour ne pas exposer de port.
- WhatsApp par QR code (on discute « avec soi-même »); Telegram via un bot BotFather.
- Mises en garde : logiciel en bêta avec « sharp edges »; erreur de classement observée dans la démo de scraping.
- Citation : « You shouldn't share this with anybody because anybody can log into your machine. »

#### V4. The AI Doctor, « OpenClaw Full Tutorial » (2026-01-31)
Source : résumé IA.
- Hostinger KVM2 (2 CPU, 8 Go); le KVM1 (4 Go) testé et jugé lent.
- Telegram recommandé (simple, stable, facile à bloquer); WhatsApp avec numéro dédié; erreur « status 515 » résolue par « update ».
- Mise en garde : « Installing OpenClaw on a personal computer is dangerous ».
- Estimation de coût de l'auteur (non vérifiée) : 1 $ pour environ 1 000 prompts complexes par mois. Déduit : contredit par toutes les autres sources; probablement une sous-estimation forte.

#### V5. Brian Casel, « My Multi-Agent Team with OpenClaw » (2026-02-20)
Source : transcription (sozai.app). Compagnon : https://buildermethods.com/library/multi-agent-team-openclaw (2026-03-08).
- Quatre agents : Claw (admin système), Bernard (développeur, Opus), Vale (marketing, Sonnet), Gumbo (assistant général, Sonnet). Chacun a son bot et ses conversations.
- Workspace et mémoire partagés entre agents; dossier « brain » avec journaux Markdown; identity.md détourné pour définir plusieurs identités.
- Telegram abandonné pour Slack : Markdown mal rendu et fils de discussion absents sur Telegram.
- Le cron intégré était difficile à relier à des agents précis; l'auteur a construit son propre tableau de bord Rails branché sur la Gateway.
- Machine : Mac Mini M4 (environ 600 $), accès SSH; un VPS à 5 $ cité comme alternative valable.
- Moindre privilège : e-mail dédié, compte GitHub invité sur certains dépôts seulement, Dropbox séparé.
- Coûts déclarés : plus de 200 $ en deux jours de configuration; jetons via OpenRouter.
- Compagnon : « OpenClaw is extremely new (less than three months old at the time of recording) ».
- Déduit : le modèle « un agent par rôle avec workspace commun » existe et fonctionne, mais l'auteur a dû construire son propre outillage de pilotage des tâches.

#### V6. Limitless Podcast, « Harsh Truth » (2026-02-18)
Source : transcription.
- Configuration des présentateurs : environ une demi-journée; obstacles sur clés API, sessions navigateur, publications YouTube/Spotify/RSS échouées.
- Une mise à jour automatique nocturne a bloqué l'agent, réparé en une heure.
- Coûts cités : un cas d'environ 800 $ en une nuit (anecdote rapportée, non vérifiée).
- Conseils : un seul cas d'usage au départ (briefing matinal), limites d'API strictes, bac à sable Docker, pas d'accès aux finances au début.
- Citation : les présentateurs parlent d'« fuzzy edges ».

#### V7. Alex Finn, « How to build an army of OpenClaw agents » (2026-03-12)
Source : résumé IA.
- Orchestrateur (Opus 4.6, ou Sonnet) et sous-agents spécialisés : codage (ChatGPT 5.4), recherche, design, réseaux sociaux, rédaction (Kimi, Qwen, OpenRouter ou modèles locaux).
- Création d'un sous-agent par prompt à l'orchestrateur (finalité, nom, modèle); l'orchestrateur mémorise le routage.
- Mission Control : écran équipe (organigramme), calendrier des cron, vue « bureau ».
- Avantages déclarés : jusqu'à cinq sous-agents en parallèle, contexte propre par sous-agent, archivage automatique.
- Conseils : un seul sous-agent au départ, au plus deux la première semaine; ne pas modifier les fichiers de configuration à la main; « reverse prompting ».

#### V9. Rachel How, « Two weeks with OpenClaw » (2026-03-07)
Source : résumé IA.
- Profil : créatrice de contenu. Usages : tableau de bord personnel (Google Calendar, usage de contexte par agent), résumé matinal, veille de tendances, analytics YouTube via l'API sans ouvrir YouTube Studio.
- Matériel : Mac mini M4 d'occasion allumé en permanence (revendable); OpenRouter; Tailscale; Telegram; Brave Search API; ChatGPT Plus par OAuth.
- Trois options d'installation : ordinateur principal (déconseillé), appareil local dédié, VPS (Hetzner, DigitalOcean, AWS).
- Problèmes : oubli du contexte entre sessions (QMD cité comme piste non testée); perte de fil sur tâches longues; sécurité (« il exécute n'importe quelle commande qu'on lui donne, même fausse »).
- Temps d'appropriation déclaré : une à deux semaines pour une personne non technique.

#### V10. NetworkChuck, « OpenClaw......RIGHT NOW??? » (2026-03-30)
Source : transcription.
- VPS Hostinger KVM2, installation en une commande; OpenAI via abonnement ChatGPT; Telegram avec accès restreint à l'utilisateur; interface web seulement par tunnel SSH.
- Fichiers Markdown : soul.md, identity.md, memory.md, journal quotidien, agents.md avec des « redlines » (règles d'interdiction). L'auteur précise que ces redlines ne sont pas imposées de façon déterministe.
- Usages : veille cybersécurité multi-sources (comparée à un workflow n8n bien plus long à bâtir), agent « ingénieur IT » qui surveille le serveur, équipe d'agents spécialisés dans son labo avec tickets et Slack.
- Sécurité : « You're a walking, talking CVE. »; « 12% of skills were found with malware » (déclaré); audit de sécurité intégré avec `--deep` et `--fix`; pare-feu limité à SSH.
- Limites : mises à jour quotidiennes qui imposent de redémarrer la gateway; configuration confuse; Claude Code jugé meilleur pour le travail sérieux.
- Verdict : usage exploratoire (« dabble mode »).

#### V12. Alex Finn, « 100 hours of OpenClaw lessons in 35 minutes » (n.d.)
Source : résumé IA.
- Position de l'auteur : machine locale préférée au VPS (il estime qu'un VPS n'exploite que 20 % des capacités et demande beaucoup de sécurisation). Déduit : position minoritaire parmi les sources; la majorité recommande VPS ou machine dédiée, sans trancher.
- Stratégie « cerveau et muscles » : un modèle cher décide, des modèles bon marché exécutent.
- Telegram recommandé pour les fils et le découpage des messages longs; Discord pour séparer les tâches en salons.
- Présentation initiale détaillée de soi et de ses objectifs chiffrés; briefing matinal par cron.
- Erreurs à éviter : modifier la configuration à la main; exposer l'agent publiquement (injection de prompt); donner l'accès à d'autres personnes; lancer une commande importante sans plan préalable.
- Mise en garde : utiliser un abonnement Anthropic avec OpenClaw pourrait entraîner un bannissement (l'auteur juge le risque peut-être exagéré).

#### V13. Alex Finn, « OpenClaw + Obsidian gives you super powers » (n.d.)
Source : résumé IA.
- Mémoire en quatre couches : (1) mémoire intégrée injectée à chaque prompt; (2) AGENTS.md et SOUL.md; (3) coffre Obsidian non injecté automatiquement; (4) historique de session.
- Structure du coffre : Agent-Shared/ (user-profile.md, project-state.md, decisions-log.md), un dossier privé par agent (working-context.md, mistakes.md, daily/).
- Rythme d'écriture : début de tâche, tous les 3 à 5 appels d'outils, fin de tâche, lors des corrections. Lecture au démarrage, après compaction, au besoin.
- Coffre partagé entre deux agents (OpenClaw et Hermes) via iCloud.
- Problème visé : la compaction fait oublier les interactions récentes.
- Citation : « it checks its vault before each session ».

#### V14. Dave Swift, « Real Persistent Memory with Obsidian » (2026-02-18, màj 2026-04-01)
Source : article compagnon partiel https://daveswift.com/openclaw-obsidian-memory/ (suite réservée aux abonnés).
- Coffre avec Inbox, Projects, Thinking, Resources, sections travail/personnel/famille; un fichier-guide par dossier (ex. inbox-guide.md).
- Sync : Obsidian Sync (4 à 5 $/mois) ou dossier partagé local avec Tailscale.
- Pièges : espaces dans le nom du coffre (indexation); les notes partent chez le fournisseur cloud comme contexte; OAuth Anthropic lie les données au compte personnel, une clé API dédiée est plus claire.
- Citation : « OpenClaw's built-in memory is broken. »

#### V15. David Ondrej, « 10x more powerful » (2026-02-09)
Source : transcription. Vidéo sponsorisée par Hostinger.
- Neuf fichiers Markdown chargés à chaque message : AGENTS.md, SOUL.md, USER.md, MEMORY.md, TOOLS.md, IDENTITY.md, HEARTBEAT.md, BOOTSTRAP.md et un fichier système. Dossiers « personal » et « business » conseillés.
- Modèles : Opus 4.6 pour les tâches importantes, Sonnet 4.5 comme « a cheaper Opus », Haiku 4.5 pour le heartbeat; alias /opus, /sonnet.
- Recherche : Perplexity Sonar via OpenRouter (environ 2 cents par requête Sonar Pro); résultats enregistrés en Markdown pour éviter de refaire les recherches.
- `/compact` a réduit le contexte de 30 000 à 12 000 tokens (déclaré).
- Vision : un OpenClaw par employé plus un OpenClaw d'entreprise centralisant documents et objectifs.
- Mises en garde : gateway jamais exposée, 30 à 60 min par semaine de vérification de sécurité du VPS, pas de connexion à des forums publics. L'agent exécute sans validation humaine, ce que l'auteur présente comme un avantage.
- Citation : « If you cannot verbalize your preferences, you cannot start automating... »

#### V16. Matthew Berman, « 21 INSANE Use Cases » (2026-02-17)
Source : transcription.
- Profil : créateur YouTube. Fichiers identity.md et soul.md; ton différent en privé et dans Slack devant l'équipe.
- Mémoire : notes quotidiennes, extraction de préférences, mise à jour des fichiers d'identité, vectorisation pour la recherche; données locales.
- Usages : CRM personnel (371 contacts, SQLite); pipeline Fathom toutes les 5 min en heures ouvrées avec approbation par Telegram avant envoi vers Todoist; base de connaissances alimentée par liens envoyés sur Telegram; trois « conseils » nocturnes (business avec 8 agents parallèles sur 14 sources, sécurité à 3 h 30, plateforme); suivi des performances YouTube/Instagram/X/TikTok en SQLite; pipeline d'idées vidéo déclenché par mention Slack (recherche, plan, accroches, fiche Asana, vérification des doublons); briefing matinal; vérification e-mail toutes les 30 min.
- Sécurité : nettoyage déterministe des données externes, aucun droit d'envoi d'e-mail ni de tweet, secrets masqués, résumé plutôt que citation du contenu non fiable, validation humaine avant toute action publique.
- Sauvegardes : SQLite chiffré vers Google Drive (7 versions), sauvegarde Git horaire, alerte en cas d'échec.
- Mises à jour : vérification quotidienne avec résumé du changelog, mise à jour sur demande seulement.
- Coûts : suivi par modèle, aucun montant donné.
- Déduit : c'est l'exemple le plus proche d'un « créateur YouTube avec second cerveau », avec une politique d'approbation avant action publique identique à la nôtre.

#### V17. Alex Finn, « The only OpenClaw tutorial you'll ever need (March 2026) »
Source : résumé IA.
- Telegram « le plus simple »; Discord « le meilleur pour les workflows avancés », avec historique consultable.
- Mémoire : journal quotidien demandé à l'agent; section mémoire dans Mission Control pour vérifier ce qu'il retient; calendrier pour vérifier que les cron tournent.
- Skills : examiner avant installation, ou demander à OpenClaw de les analyser ou de les recréer.
- Dépannage : utiliser Claude Code ou Codex pour explorer le dossier OpenClaw; « la cause est souvent une erreur de configuration ».
- Méthode « brain dump » puis « reverse prompting ».

#### V18. Artem Zhutov, « The OpenClaw Setup Nobody's Talking About » (2026-01-30)
Source : résumé IA.
- Assistant contextuel branché sur un coffre Obsidian d'environ 4 000 notes (réunions, projets, clients), joignable par Telegram. Explicitement pas un employé autonome 24/7.
- Backend : Claude Code lancé sur la machine hôte à chaque message (pour rester conforme aux conditions Anthropic selon l'auteur); clés API jugées trop chères (jusqu'à 100 $/jour estimés).
- Sync du coffre : GitHub puis synchronisation sur l'hôte, ou Obsidian Sync.
- Critique centrale : opacité, « black box inside of a black box »; il nomme explicitement le skill à utiliser dans chaque demande.
- Il a retiré environ 50 skills par défaut qui rendaient le comportement imprévisible.
- Réglages recommandés : audit de sécurité, DM Telegram en mode pairing, sandbox, interdiction par défaut de write, edit, bash et navigateur, permissions ajoutées progressivement.
- Déconseille fortement de laisser OpenClaw lire les e-mails (injection de prompt).
- Mac Mini pour disponibilité 24/7.

#### V19. « I Built a Safer OpenClaw Alternative Using Claude Code » (chaîne non confirmée, communauté Dynamous)
Source : résumé IA.
- Motif du départ : sécurité (CVE-2026-25253, ClawHub, instances exposées) et complexité du code.
- Ce qu'il a reconstruit en deux jours avec Claude Code : mémoire Markdown (soul.md, user.md) plus SQLite, heartbeat toutes les 30 min, adaptateurs de canal (Slack), skills maison.
- Citation reprise de la doc OpenClaw : « there is no perfectly secure setup ».
- Déduit : plusieurs utilisateurs avancés finissent par reconstruire une version réduite plutôt que d'exploiter tout OpenClaw.

#### V2, V8, V11 (fiches courtes)
- V2 Caleb : VPS Hostinger, Telegram le plus simple, cron créés dans le panneau avec nom, fréquence et instructions; sauvegardes quotidiennes hPanel; tous les jetons traités comme des mots de passe.
- V8 Brock Mesarich : plateforme managée Emergent, installation en 10 minutes; l'affirmation « complètement sûre » est celle de l'auteur. Peu pertinent pour un auto-hébergement.
- V11 Fireship : présentation générale; l'auteur dit « I like to live dangerously » en ignorant la doc de sécurité; peu de contenu d'usage.

---

## 3. Sources écrites

### 3.1 Tableau

| # | Source | Auteur | Date | Type | Durée d'usage déclarée | Pertinence |
|---|---|---|---|---|---|---|
| E1 | My first month running OpenClaw nonstop (dev.to) | Kory Becker | 2026-03-10 | Blogue (lu partiellement) | 1 mois 24/7 | Moyenne |
| E2 | OpenClaw in Production: real costs, security, setup (dev.to / VirtualUncle) | Vince Ultari | 2026-03-30, màj 04-10 | Blogue | 1 mois | Élevée |
| E3 | I spent 30 days testing OpenClaw (dev.to) | pseudonyme 韩 | 2026-04-21 | Blogue (suspect, compte créé 2 jours avant) | 30 jours | Faible |
| E4 | I spent $47 testing OpenClaw for a week (dev.to) | Likhit Kumar V P | 2026-02-08 | Blogue | 1 semaine | Élevée |
| E5 | OpenClaw Review: setup on a $5 VPS (aimlapi.com) | Timur Shayaxmetov | 2026-02-01, màj 08-18 | Blogue | 2 semaines | Élevée |
| E6 | OpenClaw + Obsidian (betterclaw.io) | Shabnam Katoch | 2026-04-26 | Guide (promotionnel) | n.d. | Moyenne |
| E7 | OpenClaw Obsidian sync (eastondev.com) | Easton | 2026-02-27, màj 07-14 | Guide | n.d. | Moyenne |
| E8 | $3,600/month, WhatsApp bans, 923 exposed gateways (dev.to) | GDS K S | 2026-02-07 | Analyse | 2 semaines | Élevée |
| E9 | I killed my OpenClaw... then the token bill arrived (dev.to) | lofder.issac | 2026-04-08 | Blogue | 4 semaines | Élevée |
| E10 | A week inside OpenClaw (dev.to) | Aniruddha Adak | 2026-04-22 | Blogue | 1 semaine | Faible |
| E11 | Multi-Agents in OpenClaw: Sub-Agents + Telegram (blog.cdnsun.com) | Adriana | 2026-03-09 | Guide technique | n.d. | Très élevée |
| E12 | OpenClaw in Production: silent failures (kaxo.io) | Kaxo CTO | 2026-02-09, màj 10-19 | Guide d'exploitation | production | Très élevée |
| E13 | Cron jobs silently broken in 2026.3.8 (getclawkit.com) | ClawKit | 2026-03 | Note de dépannage | n.d. | Élevée |
| E14 | 5-agent AI content team using Paperclip (Medium) | Vinayak Ramesh | 2026-04-09 | Blogue | quelques cycles | Moyenne |
| E15 | Subagents and orchestration (stevekinney.com) | Steve Kinney | màj 2026-10-13 (v2026.9.8) | Cours | n.d. | Très élevée |
| E16 | OpenClaw instances exposed (The Register) | Brandon Vigliarolo | 2026-02-09 | Presse | n.d. | Élevée |
| E17 | Reddit : « $180 and 45 hours into OpenClaw. Not going back. » | u/ActiveScolipede22 | ~2025-11 (snapshot 2026-05-08) | Fil Reddit | 45 h | Élevée |
| E18 | Reddit : « I tracked every dollar my OpenClaw agents spent for 30 days » | u/Glad-Adhesiveness319 | snapshot 2026-03-06 | Fil Reddit | 30 jours | Élevée |
| E19 | Reddit : « what I got wrong about open claw ai before trying it » | u/Same-Flight7084 | snapshot 2026-04-17 | Fil Reddit | n.d. | Moyenne |
| E20 | Reddit r/openclaw, instantané du 2026-02-26 (10 fils) | divers | 2026-02-26 | Instantané | 2 à 4 semaines | Très élevée |
| E21 | My AI agent kept making shit up (dev.to / stephanmiller.com) | Stephan Miller | 2026-04-28 | Blogue | 6 semaines | Très élevée |
| E22 | OpenClaw broke twelve different ways in two weeks (dev.to) | Chief Mojo Risin' | 2026-05-18 | Blogue | 2 semaines | Élevée |
| E23 | Deploying OpenClaw: 16 incidents, one day (dev.to) | Alex Rezvov | 2026-03-26 | Blogue | 1 jour | Élevée |
| E24 | From zero to production in 47 days (dev.to) | KevinTen | 2026-04-20 | Blogue | 47 jours | Moyenne |
| E25 | OpenClaw week two (timothywong) | Timothy Wong | 2026-02-28 | Blogue | 2 semaines | Élevée |
| E26 | How to Survive OpenClaw on Codex (Substack) | anonyme | ~2026-04 | Blogue | plusieurs semaines | Moyenne |
| E27 | Gist : OpenClaw after 50 days, prompts for 20 workflows | velvet-shark | actif jusqu'au 2026-08-10 | Gist | 50 jours | Très élevée |
| E28 | Fix OpenClaw context loss: savestate, resume, compaction (stack-junkie.com) | Christopher Kvamme | 2026-02-24, màj 03-08 | Guide | production | Très élevée |
| E29 | Ask HN: Share your productive usage of OpenClaw | aavci et 87 commentaires | ~2026-03 (« 8 months ago ») | Hacker News | variable | Très élevée |
| E30 | I split my agent into three sub-agents (dev.to) | MrClaw207 | 2026-07-17 | Blogue | n.d. | Élevée |
| E31 | Multi-project autonomous development with OpenClaw (dev.to) | Lauren ten Hoor | 2026-02-16 | Blogue (promeut son plugin) | n.d. | Élevée |
| E32 | The 35-comment OpenClaw upgrade meltdown (dev.to) | Lars Winstand | 2026-05-16 | Synthèse de fil Reddit | n.d. | Élevée |
| E33 | Reddit r/selfhosted : every documented security incident in 2026 | u/LostPrune2143 | snapshot 2026-02-23 | Fil Reddit | n.d. | Élevée |
| E34 | OpenClaw WhatsApp setup status 2026 (blink.new) | Blink Team | 2026-03-24 | Guide (promotionnel) | n.d. | Moyenne |

### 3.2 Fiches des sources les plus pertinentes

#### E2. Vince Ultari, « OpenClaw in Production » (2026-03-30)
- Recommande matériel dédié ou VPS isolé; Node 22+; port 18789; bind loopback puis tunnel SSH.
- Hiérarchie de modèles : Sonnet par défaut, Haiku pour heartbeat et surveillance simple, Opus à la demande.
- Fichiers : SOUL.md (règles), HEARTBEAT.md (automatisations proactives), USER.md, AGENTS.md.
- Coûts : une tâche = 5 à 10 appels; une longue session renvoie jusqu'à 200K tokens; un utilisateur a dépensé 70 $ en 24 h avec Opus pour tout. Conseil : plafond mensuel dans la console Anthropic.
- Sécurité : identifiants en clair sous ~/.openclaw ciblés par des infostealers; CVE-2026-25253 (CVSS 8.8); 341 skills malveillants sur ClawHub en janvier 2026 (déclaré); injection par signature d'e-mail visant des identifiants AWS.
- « Robin Delta principle » : les agents les plus efficaces sont les plus contraints.
- Conseil : un seul workflow pendant une semaine avant d'élargir.

#### E4. Likhit, « $47 testing OpenClaw for a week » (2026-02-08)
- Vieux portable Ubuntu isolé, comptes jetables, Sonnet 3.5.
- Coûts : nettoyage e-mails 8 $; recherche web avec captures d'écran 22 $ (vision); cron horaire 6 $ en une nuit (24 appels non prévus); extrapolation à environ 280 $/mois.
- Ce qui marche : désabonnement de 203 newsletters (précision environ 90 %), classement de 300 fichiers.
- Ce qui casse : calendrier (permissions), succès annoncés à tort, revue de code qui rate un bug, tâches subjectives inégales.
- Conclusion de l'auteur : l'outil déplace l'effort de l'exécution vers la surveillance.

#### E5. AIML API, « $5 VPS » (2026-02-01)
- VPS 2 vCPU, 4 Go, 40 Go; environ trois heures pour la première installation avec dépannage; CPU à 3 à 8 % en charge.
- Problèmes courants listés : agent trop autonome, questions qui déclenchent des actions, silences, boucles infinies, cron qui n'avertissent pas le chat.
- Mémoire : les souvenirs disparaissent après quelques jours si la sauvegarde n'est pas déclenchée explicitement.
- Conseils : règle dans AGENTS.md « présenter un plan et attendre l'approbation »; pour les tâches planifiées, passer par un sous-agent qui réveille la session principale; tester deux semaines sur un VPS avant tout matériel local.
- Citation : « OpenClaw is not a product. It is a framework you configure into a product. »

#### E8. GDS K S, « $3,600/month, WhatsApp bans » (2026-02-07)
- Mécanisme des coûts : chaque message et chaque heartbeat renvoient tout le contexte; estimation de l'auteur : un heartbeat Opus environ 0,75 $, soit environ 36 $/jour à 30 min d'intervalle.
- Cas cités : 3 600 $/mois (Federico Viticci, via une analyse tierce), 18,75 $ en une nuit de heartbeats, plus de 100 $ en une journée (magazine c't), plus de 300 $ en deux jours (HN), 5,7 M tokens en une nuit avec un cron sur Haiku.
- WhatsApp via Baileys (protocole non officiel) : comptes suspendus, un numéro Google Voice banni en 48 h, un incident de jumelage qui a envoyé des messages à 12 contacts.
- CrowdStrike : plus de 923 gateways exposées sans authentification.
- Conseils : Telegram plutôt que WhatsApp; Sonnet plutôt qu'Opus; plafond strict; ne pas lier la gateway à 0.0.0.0; pas de compte e-mail principal ni bancaire.
- Réserve : l'auteur développe un produit concurrent.

#### E9. lofder.issac, « I killed my OpenClaw » (2026-04-08)
- Facture déclarée : 12 $, 47 $, 38 $, 29 $ sur quatre semaines (126 $); 14,37 $ en une nuit (288 heartbeats); 0,47 $ pour un heartbeat sans tâche.
- Optimisations faites avant d'arrêter : heartbeat de 5 à 30 min, fallback Haiku/Sonnet, contexte réduit, 2 skills sur 5 désactivés.
- Il a construit une mémoire maison (Mem0, Qdrant, MCP) avec logique d'oubli, puis a arrêté faute de budget.
- Citation : « architecture doesn't matter if you can't afford to run it. »

#### E11. CDNsun, « Multi-Agents: Sub-Agents + Telegram » (2026-03-09)
- Tout passe par `.openclaw/openclaw.json`; chaque agent a son workspace et son dossier `.openclaw/agents/`; les sessions sont des fichiers texte utiles au débogage.
- Agents main, coder, tester; `openclaw agents add`; les sous-agents ne se parlent pas entre eux, main relaie; spawn désactivé par défaut; listes d'agents autorisés, limites de concurrence et de profondeur.
- Telegram : un bot par agent dans `telegram.accounts`, bindings agentId vers compte.
- Pièges : les sous-agents ne chargent pas tous les fichiers de personnalité; les liens symboliques ont été ignorés (copies explicites nécessaires).
- Conseil : partager AGENTS.md, SOUL.md, USER.md; garder spécifiques IDENTITY.md, TOOLS.md, MEMORY.md.

#### E12. Kaxo, « silent failures » (2026-02-09, màj 2026-10-19)
Dix pannes silencieuses : modèle stocké à quatre endroits; heartbeat arrêté par fichier manquant (models.json); la gateway écrase les modifications faites pendant son exécution; agents qui modifient leur propre configuration; dérive après mise à jour (garder un retour arrière, `openclaw doctor --fix`); coûts cachés des heartbeats; rechargement à chaud qui échoue sans erreur; jeton de gateway incohérent; cron figés à la création (à recréer après changement de configuration); clé de configuration acceptée mais ignorée.

#### E13. ClawKit, cron cassés en 2026.3.8 (2026-03)
- Après mise à niveau, `openclaw cron run` répond `{ok: true, enqueued: true}` mais aucune session n'est créée; le drapeau `enabled` semble réinitialisé.
- Correctif : `openclaw cron enable <job-id>`; conseil : tester les cron après chaque mise à niveau, script de vérification post-mise à jour.

#### E15. Steve Kinney, « Subagents and orchestration » (v2026.9.8)
- `sessions_spawn` rend un identifiant immédiatement; le sous-agent démarre avec une session vierge et ne reçoit que la tâche et AGENTS.md (pas SOUL.md, USER.md, IDENTITY.md ni MEMORY.md).
- Retour au parent : statut ok/error/timeout, durée, tokens, coût estimé; archivage après 60 min.
- Défauts : maxSpawnDepth 5, maxChildrenPerAgent 5, maxConcurrent 8, runTimeoutSeconds 0 (aucune limite).
- Outils retirés aux sous-agents : message, cron, gateway, plusieurs outils de session.
- Erreurs fréquentes : pas de délai par défaut; tâches interrompues non reprises au redémarrage; sous-agent oublié qui consomme; spawn refusé (allowAgents).
- Conseils : profondeur 2, délai fixé, agent dédié en lecture seule pour le contenu non fiable, format de réponse identique, « source unavailable » plutôt qu'une supposition.
- Citation : « Child output is evidence, not instructions. »

#### E20. Reddit r/openclaw, instantané du 2026-02-26
- « Two weeks later with OpenClaw and this is what I've learned » (286 pts) : TUI d'abord, un seul canal, audit de sécurité, SOUL.md personnalisé, mémoire qui s'améliore avec le temps. Citation : « Stop treating it like a chatbot. »
- « Where OpenClaw Breaks: A Field Guide From Running It Daily » (78 pts) : 42 incidents en 28 jours; rapports inventés, jetons OAuth expirés, plugins douteux, conflits de synchronisation qui effacent des fichiers. Citation : « It's a project, not a product. »
- « Goodbye API bill » : chaque appel envoyait 100 à 200K tokens d'entrée avant la mise en cache côté OpenAI.
- « Choosing an economical model » : environ 30 $/semaine avec Kimi K2.5 via OpenRouter, jugé insoutenable.
- « 4 creative ways I use my openclaw bot » : flux gardés en lecture seule pour limiter les risques.
- « My AI agent memory setup: Obsidian + QMD + SQLite » : trois couches (notes, recherche, faits structurés).

#### E21. Stephan Miller, six semaines avec « Sabrina » (2026-04-28)
- Docker (gateway + CLI), DeepSeek V3 principal via OpenRouter, Telegram (Slack retiré après pannes), rapports livrés par e-mail.
- Rapports quotidiens déclenchés par le cron de l'hôte, pas par l'agent.
- Hallucinations : quand le script ne tournait pas, l'agent produisait de fausses données (horodatages ronds, URL 404). Citation : « they don't say "I failed to do the thing." »
- `"ask": "never"` silencieusement rejeté; `"ask": "off"` avec liste blanche est la bonne forme. Une mise à jour a rendu la config Slack invalide et fait planter la gateway deux fois.
- Mémoire : journaux quotidiens remontés en mémoire hebdomadaire; skill d'auto-amélioration qui enregistre les corrections.
- Conseils : déterministe au cron de l'hôte, agent pour la réflexion; vérifier indépendamment; utiliser un agent de programmation sur l'hôte pour réparer la configuration plutôt que demander à l'agent de se réparer.

#### E23. Alex Rezvov, « 16 incidents, one day, $1.50 » (2026-03-26)
- VPS Ubuntu, OpenRouter, Telegram, dashboard derrière nginx; Mistral Small pour le chat, DeepSeek pour heartbeat et cron.
- HEARTBEAT.md est un prompt, pas un crontab : la tâche ne se déclenchait jamais.
- Boucle de réessais : 271 messages Telegram, stoppée en révoquant la clé API (0,42 $ sur Mistral; environ 17 $ sur Sonnet selon l'auteur).
- Les commandes « stop » sont ignorées pendant un tour actif; `delivered: true` trompeur; mode thinking qui absorbe la sortie.
- Conseils : `thinking: off` pour les jobs qui produisent une sortie, timeouts d'au moins 180 s, `--session isolated` pour les cron, règles anti-boucle dans SOUL.md, plafond chez le fournisseur.
- Citation : « The only circuit breaker that actually works is the one that cuts your money supply. »

#### E25. Timothy Wong, « week two » (2026-02-28)
- Agents définis entièrement en Markdown, sur appareil dédié isolé; mémoire court terme (snapshots quotidiens) et long terme curatée à la main.
- Coûts : « I hit cost limits quickly during experimentation »; il a construit une couche de routage tâche vers modèle et budget.
- Citation : « Security is not optional - even for personal use. »

#### E28. Stack Junkie, « savestate / resume / compaction » (2026-02-24)
- La compaction résume les faits mais perd le fil du travail; memoryFlush écrit des notes durables avant compaction mais pas l'état de la tâche en cours.
- Commandes définies en langage naturel dans AGENTS.md : /savestate (fichier de reprise dans memory/active-state/), /resume.
- Réglages : reserveTokensFloor 20000, memoryFlush activé, softThresholdTokens 4000; avertissement à 80 % de contexte.
- Pièges : /compact avant /savestate rend le fichier inutile; /resume ne voyait que la session courante; pas d'enchaînement automatique possible.
- Citation : « compaction optimizes for token efficiency at the cost of conversational continuity. »

#### E29. Ask HN, « Share your productive usage of OpenClaw » (106 pts, 87 commentaires)
- Usages : agent d'équipe dans un chat de groupe (standups, blocages), assistant personnel (SOC2, voyages, veille), cron appris une fois puis lancés, récupération d'un serveur média (1 300 secteurs défectueux, 1,5 To copiés), vente d'une voiture sur NextDoor avec négociation, appel téléphonique réel pour événements importants.
- Mémoire : un utilisateur a donné cinq ans de journal intime pour rédiger SOUL.md.
- Coûts : Opus 4.6 « a dollar or two » par jour léger, 110 $ le jour le plus chargé; environ 75 $/semaine surtout avec Sonnet; 0,30 $/jour pour des automatisations texte, 0,60 $ avec navigateur; DeepSeek a fini par coûter plus cher à cause du volume de tokens.
- Conditions d'utilisation : désaccord entre commentateurs sur l'usage des abonnements Claude Max ou Codex avec OpenClaw.
- Sécurité : un utilisateur a trouvé des motifs suspects dans environ 20 % des skills ClawHub auditées.
- Critique : « way cheaper and better » sans OpenClaw pour certains cas; demandes de preuves de productivité.

#### E30. MrClaw207, « split into three sub-agents » (2026-07-17)
- Avant : un agent relisait environ 40 000 tokens par tour; une tâche de 3 min passait à 14 min.
- Après : coordinateur (inbox, résumé, surveillance) et sous-agents spécialisés en parallèle; règle de délégation « plus de 10 fichiers ou plus de 5 appels d'outils ».
- Résultats déclarés : latence 11,3 à 2,1 min, tokens -58 %, erreurs multi-étapes 23 % à 4 %, environ 14 sous-agents/jour.
- Problème : les sous-agents ne partagent pas la mémoire; bloc de contexte à inclure dans chaque tâche.
- Critique en commentaire : moins de visibilité sur les étapes intermédiaires; exiger des éléments vérifiables (fichiers modifiés, tests).

#### E31. Lauren ten Hoor, multi-projets (2026-02-16)
- Un groupe Telegram par projet comme frontière de contexte; un seul processus d'agent.
- Pipeline en code déterministe, l'agent fournit l'intention; heartbeat CLI sans LLM pour parcourir les files; rôles junior/medior/senior (Haiku/Sonnet/Opus).
- Ce qui casse : étapes oubliées, état corrompu, mauvais modèle choisi, workers zombies, échecs partiels.
- Économies estimées (non détaillées) : 30 à 50 % sur tâches simples; une session neuve consommerait environ 50 000 tokens avant d'écrire une ligne.
- Citation : « The agent provides intent, tooling handles mechanics. »

#### E32. Lars Winstand, mise à jour « meltdown » (2026-05-16)
- Fil r/openclaw « Has any OpenClaw upgrade ever not broken something? » : après 5.7 vers 5.12, cron muets, clé API non lue, erreurs génériques; un script a créé 13 617 cron en une journée (fil distinct).
- Contre-exemple : « 2026.5.12 is working better than expected » (19 votes).
- Conseils : désactiver la mise à jour automatique, épingler la version (ou l'image Docker), sauvegarder avant, test de fumée après (canaux, cron, fournisseurs), retour arrière scripté.

#### E16 et E33. Sécurité (The Register 2026-02-09; Reddit r/selfhosted)
- Plus de 135 000 instances exposées (40 000 selon SecurityScorecard STRIKE le même jour); 12 812 puis plus de 50 000 vulnérables à une RCE corrigée; trois CVE (2026-25253, 2026-25157, 2026-24763).
- Cause : « Out of the box, OpenClaw binds to 0.0.0.0:18789 ».
- Reddit (139 pts) : 6 CVE, campagne ClawHavoc de plus de 824 skills malveillants, fuite de jetons Moltbook; conseils : Docker, loopback, pare-feu, VM isolée. Citation : « Malicious skills actually make it quite unusable. »

#### E34. Blink, WhatsApp en 2026 (2026-03-24)
- Pas d'API d'automatisation publique; Baileys est du reverse engineering; bannissements parfois en quelques heures; symptôme : les messages cessent sans erreur.
- QR qui expire : « If connection drops, you need to re-scan. »
- Recommandation : Telegram pour un usage personnel; API Business Meta si les destinataires sont sur WhatsApp. Réserve : source promotionnelle.

---

## 4. Ce qui marche (patrons confirmés par plusieurs sources)

1. **Mémoire en fichiers Markdown, versionnée et relue régulièrement.** VelvetShark (V1, E27), Alex Finn (V13), Dave Swift (V14), Ondrej (V15), Berman (V16), Zhutov (V18), Timothy Wong (E25), Stack Junkie (E28), BetterClaw (E6), Easton (E7). Tous combinent un fichier de mémoire principal, un journal quotidien et une curation périodique. Plusieurs ajoutent une recherche sémantique (QMD, SQLite, vecteurs) par-dessus.
2. **Sauvegarde Git du workspace.** VelvetShark (sauvegarde quotidienne GitHub avec scan des secrets), Berman (Git horaire), Easton (git diff/checkout pour annuler une erreur de l'agent), Zhutov (coffre sur GitHub).
3. **Un modèle par type de tâche.** Sonnet ou équivalent par défaut, Haiku ou petit modèle pour heartbeat et surveillance, Opus réservé à la réflexion : E2, V1, V7, V12, V15, E18, E23, E31, E27. C'est la mesure d'économie la plus citée.
4. **Approbation humaine avant toute action externe.** Brouillons seulement (V1), approbation par Telegram avant Todoist (V16), aucun droit d'envoi d'e-mail ou de tweet (V16), règle « plan puis approbation » dans AGENTS.md (E5), HitL jamais désactivé (E3), CEO Paperclip qui confirme chaque délégation (E14).
5. **Briefing matinal et vérifications périodiques en cron.** V1, V12, V15, V16, E5, E21, E29. C'est le premier workflow recommandé par la plupart des auteurs.
6. **Séparation des contextes par canal ou par groupe.** Un salon Discord par workflow (V1), un groupe Telegram par projet (E31), un bot par agent (V5, E11).
7. **Machine dédiée ou VPS, jamais l'ordinateur principal.** V3, V4, V5, V9, V10, E2, E4, E25, E29, V12 (local mais dédié). Le VPS à 5 à 10 $ est jugé suffisant par E5, V3, V5; la charge CPU reste faible (E5).
8. **Tailscale ou tunnel SSH, gateway jamais exposée.** V1, V3, V9, V10, V14, V15, E2, E8, E16.
9. **Sous-agents avec contexte explicite dans la consigne.** E11, E15, E30, V7, V1. Ils fonctionnent quand la tâche transmet le contexte nécessaire et exige un format de retour vérifiable.
10. **Déterministe hors de l'agent.** Cron de l'hôte pour les scripts (E21), pipeline en code (E31), nettoyage des données externes en code (V16). L'agent est réservé à la réflexion et à la rédaction.
11. **Commencer par un seul workflow.** E2, V6, V7, E20 (« un seul canal »), E5, V18 (permissions minimales puis élargies).

---

## 5. Ce qui casse (problèmes récurrents)

| Problème | Fréquence dans les sources | Gravité rapportée | Sources |
|---|---|---|---|
| Coûts qui explosent (heartbeats, contexte renvoyé, Opus par défaut, cron trop fréquents) | Très fréquent (plus de 15 sources) | Élevée (de 30 $/semaine à 3 600 $/mois; 70 $ en 24 h; 800 $ en une nuit rapporté) | E2, E4, E8, E9, E17, E18, E20, E29, V5, V6, V18, E25 |
| Mémoire : oubli entre sessions, compaction silencieuse, souvenirs non sauvegardés | Très fréquent | Moyenne à élevée | V1, V9, V13, V14, E5, E17, E28, E10, E24 |
| Mises à jour qui cassent (cron muets, config invalide, gateway qui plante, renommage de paquet) | Fréquent | Élevée (pannes de plusieurs jours en silence) | V1, V6, V10, E12, E13, E21, E32, E22 |
| Hallucinations de succès (rapports inventés, « delivered: true » trompeur) | Fréquent | Élevée (confiance) | E4, E20 (Field Guide), E21, E23 |
| Boucles et absence de disjoncteur (271 messages, 13 617 cron, tempête de 401) | Fréquent | Élevée | E23, E32, E22, E5 |
| Cron et heartbeat peu fiables (HEARTBEAT.md est un prompt, cron figés à la création, cron sans notification) | Fréquent | Moyenne à élevée | E12, E13, E23, E5, V5 |
| Sécurité : gateway exposée, CVE RCE, skills malveillants, identifiants en clair, injection de prompt | Très fréquent | Critique | E16, E33, E8, E2, V10, V18, V19, E29 |
| Sous-agents : contexte non partagé, pas de timeout par défaut, tâches non reprises après redémarrage, visibilité réduite | Moyen | Moyenne | E15, E11, E30 |
| WhatsApp : bannissements, sessions QR qui tombent, messages envoyés à des contacts | Moyen | Élevée pour le compte concerné | E8, E34, V4 |
| Telegram : Markdown mal rendu, pas de fils (motif de passage à Slack ou Discord) | Faible à moyen | Faible | V5, V17, E21 (Slack retiré au contraire) |
| Navigation web automatisée instable | Moyen | Moyenne | V1, V6, E4 |
| Opacité : on ne voit pas quel skill a tourné ni le raisonnement | Moyen | Moyenne | V18, E30, E33 |
| Conditions d'utilisation des abonnements (Claude Max, ChatGPT) avec OpenClaw | Moyen | Incertaine (désaccord entre sources) | V5, V12, V17, V18, E29 |
| Skills communautaires buggés (5 sur 15 fonctionnels; 60 % à corriger; 17 sur 32) | Moyen | Moyenne | E24, V18 |

---

## 6. Coûts réels rapportés

Tous les chiffres sont déclarés par les auteurs, non vérifiés.

| Montant | Contexte | Modèle | Source |
|---|---|---|---|
| 1 à 2 $/jour léger; 110 $ le jour le plus chargé | Usage personnel | Opus 4.6 | E29 (stitched2gethr) |
| environ 75 $/semaine | Assistant personnel intensif | surtout Sonnet | E29 (arjie) |
| 0,30 $/jour (texte), 0,60 $/jour (navigateur) | Automatisations simples en cron | n.d. | E29 (brdd) |
| moins de 20 $/mois | Usage personnel avec plafond | n.d. | E19 |
| environ 30 $/semaine | Usage régulier | Kimi K2.5 via OpenRouter | E20 |
| 47 $ en une semaine de tests (extrapolé à environ 280 $/mois) | Tests variés, vision coûteuse | Sonnet 3.5 | E4 |
| 126 $ sur 4 semaines (12, 47, 38, 29) | Usage personnel optimisé | Haiku/Sonnet | E9 |
| 0,47 $ un heartbeat sans tâche; 14,37 $ en une nuit (288 heartbeats) | Heartbeat à 5 min | n.d. | E9 |
| 340 $ puis 112 $/mois | SaaS avec 4 agents, après optimisation (cache, modèles moins chers, limites de sortie) | GPT-4.1 puis mix | E18 |
| 180 $ et 45 heures (35 de réparation) | Essai abandonné | n.d. | E17 |
| plus de 200 $ en deux jours de configuration | 4 agents | OpenRouter | V5 |
| 70 $ en 24 h | Opus pour tout, y compris les statuts | Opus | E2 |
| environ 0,75 $ par heartbeat Opus, environ 36 $/jour à 30 min | Estimation de l'auteur | Opus | E8 |
| 3 600 $/mois | Cas Federico Viticci, rapporté indirectement | n.d. | E8 |
| environ 1,50 $ pour une journée de réglage; 0,03 $ par digest réussi; 0,42 $ pour une boucle de 271 messages | Petits modèles | Mistral Small, DeepSeek | E23 |
| jusqu'à 100 $/jour estimés | Clé API jugée trop chère, passage à Claude Code en backend | Opus 4.5 | V18 |
| environ 2 cents par recherche Sonar Pro | Recherche web | Perplexity via OpenRouter | V15 |
| VPS : environ 5 $/mois (Hetzner, DigitalOcean) ou Hostinger KVM2 (prix non donné); Mac Mini environ 600 $ | Hébergement | n.a. | E5, V5, V9, V3, V4, V10 |

Déduit : pour un assistant personnel actif avec modèles Anthropic et heartbeats toutes les 30 min, les témoignages se situent entre 100 et 400 $/mois avant optimisation, et entre 30 et 130 $/mois après routage par modèle et réduction des heartbeats. Les cas extrêmes (plus de 1 000 $/mois) correspondent à Opus utilisé pour tout, ou à des boucles non détectées.

---

## 7. Conseils des utilisateurs expérimentés

1. Fixer un plafond de dépense chez le fournisseur avant le premier message; c'est le seul disjoncteur fiable (E23, E2, E4, E8, E19, V6).
2. Sonnet par défaut, Haiku pour heartbeat et surveillance, Opus à la demande; ne jamais router les heartbeats vers un modèle cher (E2, E8, E12, V15, V1).
3. Heartbeat à 30 min plutôt que 5 (E9, E8); ou heartbeat en CLI sans LLM (E31).
4. Lier la gateway au loopback, accès par Tailscale ou tunnel SSH, pare-feu limité à SSH (E16, E2, V3, V10, V15).
5. Machine dédiée ou VPS; pas de clés SSH ni d'autres clés API sur la même machine; comptes dédiés (e-mail, GitHub invité) pour l'agent (E2, V5, E1, E4).
6. Un seul workflow la première semaine; un seul canal; un seul sous-agent au départ, au plus deux la première semaine (E2, E20, V7, E30).
7. Écrire des règles explicites et strictes dans SOUL.md et AGENTS.md : ne jamais supprimer sans demander, toujours confirmer avant d'envoyer, présenter un plan avant d'agir, traiter les questions comme des questions (E2, E5, E23, V10).
8. Tout mettre dans des fichiers : journal quotidien, mémoire principale, état de la tâche en cours avant compaction; ouvrir une nouvelle session au-delà de 50 % de contexte (V1, E28, V13).
9. Audit hebdomadaire de la mémoire d'environ 10 minutes : supprimer le bruit, corriger les faits périmés, dédupliquer; archiver plutôt que supprimer (E6, E25).
10. Sauvegarde Git quotidienne ou horaire du workspace avec scan des secrets; utiliser git diff pour voir ce que l'agent a modifié (E27, V16, E7).
11. Désactiver la mise à jour automatique, épingler la version, sauvegarder avant, test de fumée après (canaux, cron, fournisseurs); ou au minimum résumé du changelog avant mise à jour sur demande (E32, E13, V16, E21).
12. Tester les cron après chaque mise à jour; les recréer après un changement de configuration; `--session isolated` pour les cron; `thinking: off` pour les jobs qui produisent une sortie; timeouts d'au moins 180 s (E12, E13, E23).
13. Confier le déterministe (scripts, planification, nettoyage des données externes) au cron de l'hôte ou à du code; garder l'agent pour la réflexion et la rédaction (E21, E31, V16).
14. Vérifier indépendamment les succès annoncés; exiger des sous-agents des éléments vérifiables (fichiers modifiés, tests, « source unavailable ») (E4, E21, E15, E30).
15. Sous-agents : profondeur 2, `runTimeoutSeconds` fixé, bloc de contexte dans chaque consigne, agent dédié en lecture seule pour le contenu non fiable, vérifier `/subagents list` avant de relancer (E15, E30, E11).
16. Partager AGENTS.md, SOUL.md, USER.md entre agents; garder IDENTITY.md, TOOLS.md, MEMORY.md spécifiques; éviter les liens symboliques (E11).
17. Telegram plutôt que WhatsApp; si WhatsApp, numéro dédié jamais personnel; Telegram en mode pairing, DM restreint à l'utilisateur (E8, E34, V4, V18, V10).
18. Traiter chaque skill comme du code non fiable : lire le README et le code, préférer les skills maison ou de source connue, retirer les skills par défaut inutiles (E2, E24, V17, V18, V19, E29).
19. Ne jamais brancher l'agent sur l'e-mail principal, les comptes bancaires ou des forums publics; e-mails en brouillon seulement (E8, V15, V1, V18).
20. Pour réparer la configuration, utiliser un agent de programmation sur l'hôte (Claude Code, Codex) plutôt que demander à OpenClaw de se réparer; lire les issues GitHub et Reddit avant le code source (E21, V17, E26).
21. Prévoir une journée complète de réglage initial et une à deux semaines d'appropriation (E5, E23, V9).
22. Reverse prompting : décrire ses objectifs et routines, puis demander à l'agent quelles automatisations ou quels sous-agents il recommande (V7, V12, V17).
23. OpenClaw améliore des workflows existants, il n'en crée pas; sans système préalable, il apporte peu (V1).

---

## 8. Leçons pour notre projet (observations seulement)

1. **La configuration validée ressemble fortement aux configurations qui tiennent dans la durée** (VPS, Telegram, mémoire Markdown dans Git, cron de filet, approbation avant action externe). VelvetShark (V1, E27) et Berman (V16) sont les deux références les plus proches et tournent depuis des mois. Observation, pas validation.
2. **Un seul agent « CEO » au départ correspond au conseil le plus répété** : un workflow, un canal, un sous-agent au maximum la première semaine (E2, E20, V7). Les équipes multi-agents (V5, V7) ont été construites après des semaines et avec de l'outillage maison.
3. **Les sous-agents ne reçoivent que la tâche et AGENTS.md** (E15). Notre « brief du département » devra être injecté intégralement dans la consigne de spawn, et pas seulement présent dans le workspace. Le validateur indépendant devrait recevoir le même traitement (E15 : « Child output is evidence, not instructions »).
4. **Le heartbeat 30 min et le cron du soir sont des postes de coût à surveiller dès le premier jour.** Les témoignages de 0,47 $ par heartbeat Opus (E9) et de 36 $/jour (E8) viennent de heartbeats sur modèle cher. Le routage du heartbeat vers Haiku est unanime (E2, V15, E12).
5. **La mémoire fichier seule ne protège pas contre la compaction.** Plusieurs utilisateurs ajoutent un fichier d'état de tâche avant compaction (E28) et une règle d'écriture fréquente (V13). Notre PASSATION.md et la « clôture par inactivité » correspondent à ce besoin; la fréquence d'écriture reste à préciser.
6. **Les mises à jour sont la première cause de panne silencieuse des cron** (E13, E32, E12, V1). Les utilisateurs expérimentés épinglent la version et testent les cron après chaque mise à jour. Notre règle « aucune automatisation ajoutée à l'installation » ne couvre pas ce point.
7. **Les faux succès sont fréquents** (E4, E21, E23, Field Guide E20). Le validateur indépendant et la boucle d'évaluation prévus sont cohérents avec ce constat; les sources ajoutent de vérifier les livrables par des preuves (fichiers, URL, tests) et non par le compte rendu.
8. **Telegram est le canal consensuel pour un usage personnel** (E8, E34, V4, V10, V17). Les seuls reproches sont le rendu Markdown et l'absence de fils; deux utilisateurs multi-agents sont passés à Slack ou Discord pour cela (V5, V1). Pour un agent unique, Telegram suffit selon les sources.
9. **WhatsApp est à éviter avec un numéro personnel** (E8, E34). Non prévu dans notre architecture; à garder tel quel.
10. **Les coûts observés pour un assistant personnel actif avec Anthropic se situent entre 30 et 130 $/mois après optimisation, 100 à 400 $ avant** (section 6). Le plafond par clé API est le mécanisme recommandé par toutes les sources.
11. **Un VPS à 5 à 10 $ suffit en charge** (E5 : 3 à 8 % CPU); le KVM1 à 4 Go a été jugé lent par un auteur (V4) et suffisant par un autre (E5). Point à vérifier sur notre serveur.
12. **Le dépôt Git privé comme mémoire est pratiqué par plusieurs utilisateurs avec un scan des secrets avant commit** (E27). Notre règle « secrets hors dépôt » a un précédent concret.
13. **Les skills ClawHub sont considérés comme non fiables par la plupart des auteurs** (12 % à 20 % suspects selon V10 et E29; 824 malveillants selon E33). Nos trois skills maison (validateur, Gmail IMAP lecture, GitHub) correspondent au conseil « skills maison ou de source connue ».
14. **La lecture d'e-mails par l'agent est le vecteur d'injection le plus cité** (V18, E2, E8). Notre skill Gmail en lecture seule relève de ce risque; les parades vues : résumé plutôt que citation du contenu, nettoyage déterministe, agent dédié en lecture seule (V16, E15).
15. **Paperclip** : les retours directs sont rares et le seul lu (E14) utilise Claude Code, pas OpenClaw, avec un CEO qui confirme chaque délégation. Aucun retour d'expérience Paperclip + OpenClaw sur plusieurs semaines n'a été trouvé; l'article de Jim Christian (2026-11-04) n'était pas lisible.
16. **Les utilisateurs qui réussissent écrivent beaucoup : profil détaillé, objectifs chiffrés, préférences verbalisées** (V12, V15, E29). Notre USER.md et nos fiches par projet vont dans ce sens.
17. **Plusieurs utilisateurs avancés finissent par reconstruire une version réduite** (V19, E9, E17 vers Hermes ou Claude Code). Cela indique que la valeur vient des patrons (mémoire fichier, heartbeat, canal, skills) plus que du logiciel lui-même. Observation à garder en tête pour la Phase 6.
18. **Aucune source francophone originale** avec usage prolongé n'a été trouvée; les contenus en français sont des traductions automatiques de vidéos anglophones.

---

## 9. Sources (URL, date)

### Vidéos
- V1 VelvetShark, 50 days with OpenClaw, 2026-02-20, https://www.youtube.com/watch?v=NZ1mKAWJPr4 ; résumé lu sur https://readray.ai/es/notes/openclaw-20260221/openclaw-50-days-review ; gist https://gist.github.com/velvet-shark/b4c6724c391f612c4de4e9a07b0a74b6
- V2 Caleb, Full OpenClaw Setup Tutorial, n.d., https://videohighlight.com/v/fcZMmP5dsl4
- V3 1littlecoder, Run your 24x7 Clawdbot on a Secure VPS, 2026-02-17, https://www.youtube.com/watch?v=CZUYofFDs-U ; transcription https://rosetta.to/u/1littlecoder/openclaw-is-open-run-your-24x7-clawdbot-on-a-secure-vps
- V4 The AI Doctor, OpenClaw Full Tutorial, 2026-01-31, https://www.youtube.com/watch?v=P6XWQ_CcAt0 ; résumé https://readray.ai/en/notes/openclaw-clawdbot-20260203/openclaw-install-setup-automation
- V5 Brian Casel, My Multi-Agent Team with OpenClaw, 2026-02-20, transcription https://sozai.app/transcript/my-multi-agent-team-with-openclaw-transcript/ ; compagnon https://buildermethods.com/library/multi-agent-team-openclaw (2026-03-08)
- V6 Limitless Podcast, We Used OpenClaw for a Week, 2026-02-18, https://www.youtube.com/watch?v=4xDc00EF_eY ; transcription https://rosetta.to/u/limitless-ft/we-used-openclaw-for-a-week-this-is-the-harsh-truth
- V7 Alex Finn, How to build an army of OpenClaw agents, 2026-03-12, https://www.youtube.com/watch?v=Rjd1LqF9cG4 ; résumé https://readray.ai/en/notes/clawdbot-20260813/build-openclaw-army
- V8 Brock Mesarich, OpenClaw Is Finally Easy and Safe to Use, 2026-02-02, https://www.youtube.com/watch?v=6OSEZx8v4so ; résumé https://readray.ai/fr/notes/openclaw-tutorial-20260204/openclaw-easy-safe-clawdbot
- V9 Rachel How, two weeks with OpenClaw, 2026-03-07, https://www.youtube.com/watch?v=JeaxEA-M0Qg ; résumé https://readray.ai/ar/notes/openclaw-20260813/openclaw-two-weeks-review
- V10 NetworkChuck, OpenClaw......RIGHT NOW???, 2026-03-30, https://www.youtube.com/watch?v=T-HZHO_PQPY ; transcription https://rosetta.to/u/networkchuck/openclaw-right-now-its-not-what-you-think
- V11 Fireship, The wild rise of OpenClaw, 2026-01-30, https://www.youtube.com/watch?v=ssYt09bCgUY ; transcription https://rosetta.to/u/fireship/the-wild-rise-of-openclaw
- V12 Alex Finn, 100 hours of OpenClaw lessons in 35 minutes, n.d., https://www.youtube.com/watch?v=_kZCoW-Qxnc ; résumé https://videohighlight.com/v/_kZCoW-Qxnc
- V13 Alex Finn, OpenClaw + Obsidian gives you super powers, n.d., https://www.youtube.com/watch?v=6V-b073qhPA ; résumé https://videohighlight.com/v/6V-b073qhPA
- V14 Dave Swift, Real Persistent Memory with Obsidian, 2026-02-18, https://youtu.be/iUkFb_TaqZA ; article https://daveswift.com/openclaw-obsidian-memory/
- V15 David Ondrej, I made my OpenClaw 10x more powerful, 2026-02-09, https://www.youtube.com/watch?v=cod50CWlZeU ; transcription https://rosetta.to/u/davidondrej/i-made-my-openclaw-10x-more-powerful-seriously
- V16 Matthew Berman, 21 INSANE Use Cases For OpenClaw, 2026-02-17, https://www.youtube.com/watch?v=8kNv3rjQaVA ; transcription https://rosetta.to/u/matthew_berman/21-insane-use-cases-for-openclaw
- V17 Alex Finn, The only OpenClaw tutorial you'll ever need (March 2026), https://www.youtube.com/watch?v=CxErCGVo-oo ; résumé https://summyt.app/summaries/ai/the-only-openclaw-tutorial-youll-ever-need-march-2026-edition-CxErCGVo-oo
- V18 Artem Zhutov, The OpenClaw Setup Nobody's Talking About, 2026-01-30, https://www.youtube.com/watch?v=ZdZUNwUwHIs ; résumé https://readray.ai/fr/notes/openclaw-20260210/openclaw-setup-secret
- V19 I Built a Safer OpenClaw Alternative Using Claude Code, n.d., https://www.youtube.com/watch?v=XmweZ4fLkcI ; résumé https://videohighlight.com/v/XmweZ4fLkcI
- V20 Riley Brown, OpenClaw Superteam, https://www.youtube.com/watch?v=ISb0nrlNoKQ (non lu)
- V21 Clearmud, Agents vs Sub-Agents, 2026-03-10, https://www.youtube.com/watch?v=oQBawbXQRGQ (non lu)
- V22 LaRue, How to Make OpenClaw 10x More Powerful, https://recapio.com/digest/how-to-make-openclaw-10x-more-powerful-by-larue (non lu)
- V23 Rafael Quintanilha, Montei minha própria empresa com IA, https://recapio.com/digest/montei-minha-propria-empresa-com-ia-utilizando-o-openclaw-by-rafael-quintanilha-quantbrasil (non lu)
- V24 Every Deploys OpenClaw Coworkers, https://www.youtube.com/watch?v=m9yoObQb88Q (description seulement)

### Sources écrites
- E1 https://dev.to/primaryobjects/my-first-month-running-openclaw-nonstop-heres-what-everyone-gets-wrong-1ni2 (2026-03-10)
- E2 https://dev.to/virtualunc/openclaw-in-production-real-costs-security-setup-and-what-a-month-of-daily-use-actually-looks-10dg (2026-03-30)
- E3 https://dev.to/_cbd692d476c5faf3b61bcf/i-spent-30-days-testing-openclaw-heres-what-nobody-tells-you-about-ai-agents-in-2026-2jn9 (2026-04-21)
- E4 https://dev.to/likhit/i-spent-47-testing-openclaw-for-a-week-heres-whats-actually-happening-4j2b (2026-02-08)
- E5 https://aimlapi.com/blog/openclaw-review-real-world-use-setup-on-a-5-vps-and-what-actually-works (2026-02-01)
- E6 https://www.betterclaw.io/blog/openclaw-obsidian (2026-04-26)
- E7 https://eastondev.com/blog/en/posts/ai/20260227-openclaw-obsidian-sync (2026-02-27)
- E8 https://dev.to/thegdsks/openclaw-and-moltbook-3600month-whatsapp-bans-and-923-exposed-gateways-an-engineers-2jfn (2026-02-07)
- E9 https://dev.to/_95a3e57463e6442feacd0/i-killed-my-openclaw-built-the-memory-the-gateway-the-patches-then-the-token-bill-arrived-2585 (2026-04-08)
- E10 https://dev.to/onirestart/i-spent-a-week-inside-openclaw-here-is-what-broke-me-and-what-blew-my-mind-4746 (2026-04-22)
- E11 https://blog.cdnsun.com/multi-agents-in-openclaw-sub-agents-and-telegram/ (2026-03-09)
- E12 https://kaxo.io/insights/openclaw-production-gotchas/ (2026-02-09, màj 2026-10-19)
- E13 https://getclawkit.com/docs/troubleshooting/cron-jobs-silently-broken-in-2026-3-8-no-execution-no-errors (2026-03)
- E14 https://medium.com/no-time/i-built-a-5-agent-ai-content-team-using-paperclip-heres-the-full-setup-4e8dfbd758b8 (2026-04-09)
- E15 https://stevekinney.com/courses/openclaw/subagents-and-orchestration (màj 2026-10-13)
- E16 https://theregister.com/2026/02/09/openclaw_instances_exposed_vibe_code (2026-02-09)
- E17 https://reddit.sentinel-team.org/posts/1t6bc2l/snapshots/2026-05-08T22%3A11%3A50.661344Z
- E18 https://reddit.sentinel-team.org/posts/1rl30k1/snapshots/2026-03-06T19%3A24%3A10.904229Z
- E19 https://reddit.sentinel-team.org/posts/1sn1mjg/snapshots/2026-04-17T18%3A20%3A09.692309Z
- E20 https://reddit.sentinel-team.org/subreddits/openclaw/snapshots/2026-02-26T23%3A00%3A11.579829Z
- E21 https://dev.to/eristoddle/my-ai-agent-kept-making-shit-up-and-other-lessons-from-running-openclaw-566p (2026-04-28)
- E22 https://dev.to/chiefmojo79/openclaw-broke-twelve-different-ways-in-two-weeks-and-i-catalogued-every-single-one-1jod (2026-05-18)
- E23 https://dev.to/arezvov/deploying-openclaw-16-incidents-one-day-150-3i9d (2026-03-26)
- E24 https://dev.to/kevinten10/how-i-built-openclaw-powered-ai-agents-from-zero-to-production-in-47-days-4dbp (2026-04-20)
- E25 https://timothywong-co.pages.dev/writing/openclaw-week-two (2026-02-28)
- E26 https://telegraphic.substack.com/p/how-to-survive-openclaw-on-codex (~2026-04)
- E27 https://gist.github.com/velvet-shark/b4c6724c391f612c4de4e9a07b0a74b6 (actif jusqu'au 2026-08-10)
- E28 https://stack-junkie.com/blog/openclaw-context-persistence-savestate-resume (2026-02-24)
- E29 https://news.ycombinator.com/item?id=47147183 (Ask HN, ~2026-03)
- E30 https://dev.to/mrclaw207/my-openclaw-agent-got-slow-and-expensive-i-split-it-into-three-sub-agents-heres-what-happened-51l1 (2026-07-17)
- E31 https://dev.to/laurentenhoor/multi-project-autonomous-development-with-openclaw-what-actually-works-1fk (2026-02-16)
- E32 https://dev.to/lars_winstand/i-read-the-35-comment-openclaw-upgrade-meltdown-so-you-dont-have-to-3ao5 (2026-05-16) ; fil Reddit cité https://reddit.com/r/openclaw/comments/1te4rzr/has_any_openclaw_upgrade_ever_not_broken_something/
- E33 https://reddit.sentinel-team.org/posts/1r9yrw1/snapshots/2026-02-23T11%3A13%3A15.091724Z
- E34 https://blink.new/blog/openclaw-whatsapp-setup-status-2026 (2026-03-24)

### Non lisibles (signalés)
- https://news.ycombinator.com/item?id=47057712 (limite de débit, trois tentatives)
- https://jimchristian-net.pages.dev/blog/2026/03/29-the-org-chart-has-four-robots/ (page sans contenu d'article)
- https://recapio.com/digest/why-specialized-agents-are-superior-how-i-built-an-openclaw-superteam-by-riley-brown (résumé non généré)
- Discussions GitHub openclaw/openclaw : API GraphQL bloquée depuis cette session, non listées.
