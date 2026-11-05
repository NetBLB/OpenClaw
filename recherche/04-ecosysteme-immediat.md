# 04. Écosystème immédiat d'OpenClaw : skills, tableaux de bord, Paperclip, modèles locaux, Codex, outils associés

Date de la recherche : 2026-11-05. Rédigé pour le projet OpenClaw de Net BLB (voir CONTEXTE-PROJET.md).

## 1. Méthode et périmètre

Sources consultées : documentation officielle (docs.openclaw.ai, docs.paperclip.ing), registre ClawHub (clawhub.ai, pages de recherche et fiches de skills), pages GitHub des projets (lues via WebFetch), métadonnées de dépôts via l'API publique repos.ecosyste.ms (l'API GitHub directe et `gh` étaient bloqués dans cette session), articles et retours d'expérience de 2026 (WebSearch standard puis extended). Les pages `github.com/.../commits` sont interdites aux robots : les dates de dernier push viennent d'ecosyste.ms (synchronisation parfois ancienne, indiquée à chaque fois).

Conventions :
- « Signets » = le compteur affiché par ClawHub à côté de « Bookmark » (ce n'est pas un vrai système de notes; ClawHub n'affiche ni étoiles ni avis structurés). « Téléchargements » = colonne « Downloads ».
- « Audit : Pass » = mention affichée sur la fiche ClawHub du skill (section « Security audit »). Quand la fiche n'affiche que « Review » sans résultat, c'est noté « non affiché ».
- Rien ici n'est une décision. Ce document observe et signale; le choix revient à Dominic.

Périmètre couvert : A (ClawHub et skills), B (tableaux de bord), C (Paperclip), D (modèles locaux), E (Codex / abonnement ChatGPT), F (outils associés). Non couvert : tests en conditions réelles; chaque skill devrait être lu avant installation (voir risques en A.4).

## 2. Partie A : ClawHub et les skills

### A.1 Ce qu'est ClawHub aujourd'hui

- Registre public : https://clawhub.ai (dépôt https://github.com/openclaw/clawhub, 9,5 k étoiles, dernier push 2026-11-01, licence MIT). Il héberge des skills (dossier avec `SKILL.md`), des plugins de code et des bundles. Il suit « semver versions, tags such as `latest`, changelogs, files, downloads, stars, and security scan summaries » (docs.openclaw.ai/clawhub.md).
- Installation : `openclaw skills install @owner/<slug>` (dans le workspace; `--global` pour le dossier géré), `openclaw skills update --all`, `openclaw skills verify @owner/<slug>` (vérifie l'enveloppe de confiance, code de sortie non nul si échec). Les installs depuis Git ou dossier local ne sont pas suivies par `update` (docs.openclaw.ai/skills.md).
- Priorité de chargement : `<workspace>/skills` gagne sur tout le reste (puis `.agents/skills`, `~/.agents/skills`, skills gérés, skills « workshop », skills intégrés, plugins). Liste blanche par agent : `agents.entries.*.skills`.
- Publication ouverte à quiconque possède un compte GitHub assez ancien. Modération : signalement par utilisateurs connectés, masquage automatique après plusieurs signalements, modérateurs. Scan automatisé à la publication; l'état du dernier scan est affiché sur la fiche. Le README du dépôt précise que « the suspicious filter is reserved for high-impact or malicious concerns » et que les constats de niveau moyen restent visibles.
- Taille : la page d'accueil répète « 30 skills 12 plugins » dans un bandeau décoratif, chiffre non fiable. Un audit de mars 2026 cité par InsiderLLM parlait de 14 706 skills dans le registre.

### A.2 Incidents de sécurité documentés (à garder en tête avant toute installation)

| Date | Source | Constat |
|---|---|---|
| 2026-02-01/02 | Koi Security, relayé par The Hacker News (https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html) | 341 skills malveillants sur 2 857 audités; campagne « ClawHavoc » (335 skills) avec faux « Prerequisites » qui font installer un voleur d'identifiants (AMOS sur macOS, trojan avec keylogger sur Windows). Déguisements : outils crypto, utilitaires YouTube, intégrations Google Workspace, typosquats. |
| 2026-03 | RankClaw, cité par InsiderLLM (https://insiderllm.com/guides/openclaw-clawhub-security-alert/) | 1 103 skills malveillants sur 14 706 (7,5 %). Réponse d'OpenClaw : partenariat VirusTotal pour scanner les nouvelles soumissions, signalement communautaire. L'article juge la réponse partielle (pas de sandbox de test, pas de signature). |
| 2026-04-28 | même page InsiderLLM | Précautions : lire `SKILL.md`, se méfier des commandes dans « Prerequisites », des `curl` vers domaines inconnus, du base64; épingler les versions; préférer les skills intégrés. |

Mécanismes officiels côté OpenClaw : `openclaw skills verify`, `security.installPolicy` (commande de politique exécutée avant install), installs d'archives désactivés par défaut, secrets des skills injectés dans le processus hôte uniquement pour le tour d'agent (docs.openclaw.ai/skills.md).

### A.3 Skills les plus utilisés par catégorie (listes clawhub.ai/skills?q=..., consultées le 2026-11-05)

Courriel (Gmail, Google Workspace, IMAP) :

| Skill | URL | Auteur | Signets / téléch. | Fonction | Prérequis | Lecture / écriture | Audit, risques |
|---|---|---|---|---|---|---|---|
| Gog | https://clawhub.ai/steipete/gog | steipete (créateur d'OpenClaw), v1.0.0, màj il y a 1 mois | 957 / 198 k | CLI Google Workspace : Gmail, Calendar, Drive, Contacts, Sheets, Docs | binaire `gog`, client OAuth Google (`gog auth credentials client_secret.json`, `gog auth add <compte> --services ...`), `GOG_ACCOUNT` optionnel | Lecture (recherche Gmail, événements, Drive, Sheets, Docs) et écriture (envoi Gmail, Sheets update/append, copie Docs) | Pass. Notes du skill : « confirmer avec l'utilisateur avant d'envoyer un courriel ou créer un événement » |
| imap-smtp-email | https://clawhub.ai/gzlicanyi/imap-smtp-email | gzlicanyi, v0.0.19, màj il y a 3 mois | 109 / 51,2 k | Lire, chercher, télécharger pièces jointes, marquer lu/non lu, envoyer (SMTP), multi-comptes, préréglages Gmail/Outlook/QQ | Node.js, `setup.sh`, mot de passe d'application Gmail, `.env` en clair dans `~/.config/mail-skills/` (perm. 600) | Lecture et écriture (envoi, drapeaux) | Non affiché. Risques : identifiants en clair; option `*_REJECT_UNAUTHORIZED=false` suggérée pour certificats auto-signés (affaiblit TLS) |
| gmail (managed OAuth) | https://clawhub.ai/byungkyu/gmail | byungkyu | 75 / 39,8 k | API Gmail via OAuth géré par un tiers (Maton, même modèle que le skill YouTube du même auteur) | CLI `maton`, compte Maton, approbation OAuth | Lecture et écriture | Les appels transitent par la passerelle du tiers (api.maton.ai) |
| email-daily-summary | https://clawhub.ai/10e9928a/email-daily-summary | 10e9928a | 12 / 18,6 k | Résumés quotidiens multi-fournisseurs | identifiants de messagerie | Lecture surtout | Non vérifié |
| Email (Gmail, Outlook, Exchange) | https://clawhub.ai/porteden/email-gmail-outlook | porteden | 7 / 10,5 k | Gestion multi-comptes | OAuth / identifiants | Lecture et écriture | Non vérifié |
| IMAP Email Reader | https://clawhub.ai/mvarrieur/imap-email | mvarrieur | 1 / 6,9 k | IMAP (ProtonMail Bridge, Gmail) | mot de passe d'application | Lecture | Non vérifié |
| Gmail IMAP | https://clawhub.ai/scottgl9/gmail-imap | scottgl9 | 2 / 929 | Lire, envoyer, corbeille, déplacer, étiqueter | mot de passe d'application | Lecture et écriture | Non vérifié |

Alternative officielle sans skill tiers : le plugin IMAP intégré (docs.openclaw.ai/automation/imap.md) qui « does not send email, modify message flags, expose a public webhook, or backfill messages », mot de passe en SecretRef, IDLE ou sondage (`pollSeconds` min 15), liste blanche `allowedSenders`, vérification DMARC, routage vers un agent lecteur isolé (`mail_reader`, sandbox, outils minimaux). Et l'intégration Gmail Pub/Sub (docs.openclaw.ai/automation/cron-jobs/gmail.md) qui demande `gog`, un projet GCP, un topic Pub/Sub et un point d'entrée HTTPS public (Tailscale Funnel recommandé).

GitHub :

| Skill | URL | Auteur | Signets / téléch. | Fonction | Prérequis | L / É | Audit |
|---|---|---|---|---|---|---|---|
| Github | https://clawhub.ai/steipete/github | steipete, v1.0.0, màj il y a 4 mois | 674 / 204 k | Issues, PR, runs CI, requêtes `gh api` | CLI `gh` authentifié (non documenté sur la fiche) | Exemples en lecture; écriture possible via `gh api` | Pass |
| Skill Vetter | https://clawhub.ai/spclaudehome/skill-vetter | spclaudehome, v1.0.0, màj il y a 1 mois | 1,3 k / 281 k | Protocole de vérification d'un skill avant installation (source, revue de code, portée des permissions, classement du risque, approbation humaine) | `curl`, `jq` | Lecture | Pass |
| GitHub (managed OAuth) | https://clawhub.ai/byungkyu/github-api | byungkyu | 47 / 19,2 k | API GitHub via OAuth géré (Maton) | compte Maton | L et É | Passerelle tierce |
| GitHub (GraphQL) | https://clawhub.ai/hith3sh/github-repos | hith3sh | 32 / 7,6 k | Dépôts, issues, PR, commits, branches | jeton | L et É | Non vérifié |

Gestion de projets et de tâches : la recherche « todoist » ne renvoie que des skills Todoist (https://clawhub.ai/mjrussell/todoist, 50 / 22,8 k; https://clawhub.ai/byungkyu/todoist-api, 13 / 10,2 k, màj il y a 2 semaines). Aucun skill Linear, Trello, Asana ou « tâches en Markdown » n'est apparu dans les résultats consultés (constat partiel, « Load more » non exploré). Notion : https://clawhub.ai/steipete/notion (274 / 105 k), https://clawhub.ai/dimagious/notion-skill (15 / 14,5 k, « schema diffs before changes »).

Mémoire et notes :

| Skill | URL | Signets / téléch. | Fonction | Remarque |
|---|---|---|---|---|
| Obsidian | https://clawhub.ai/steipete/obsidian | 464 / 113 k | Coffres Obsidian via `obsidian-cli` | Prérequis : `obsidian-cli` sur la machine qui a le coffre |
| obsidian-markdown (kepano, source externe skills.sh) | https://clawhub.ai/skills-sh/kepano/obsidian-skills/obsidian-markdown | 94,8 k | Écrire du Markdown « Obsidian Flavored » | Auteur = fondateur d'Obsidian; libellé « External source » |
| Obsidian Official CLI | https://clawhub.ai/alexanderkinging/obsidian-cli-official | 8 / 5,9 k | CLI officiel Obsidian 1.12+ | |
| Proactive Agent Lite | https://clawhub.ai/bestrocky/proactive-agent-lite | 65 / 42 k | Mémoire pour agent « proactif » | Non vérifié |
| Memory | https://clawhub.ai/ivangdavila/memory | 18 / 20 k | Faits durables en Markdown catégorisé | Doublon fonctionnel avec MEMORY.md natif |
| memory-audit | https://clawhub.ai/broedkrummen/memory-audit | 0 / 165 | Audit en lecture seule de la mémoire OpenClaw (redondance, obsolescence) | Petit, récent |

Mémoire native (docs.openclaw.ai/concepts/memory.md) : `MEMORY.md` (faits durables, chargé au démarrage), `memory/YYYY-MM-DD.md` (notes quotidiennes indexées), `USER.md`, recherche hybride `memory_search` (embeddings OpenAI par défaut; `memory.search.provider` peut être `ollama` ou `none`), « memory flush » automatique avant compaction (`agents.defaults.compaction.memoryFlush.enabled`). Plugin intégré `memory-wiki` (docs.openclaw.ai/plugins/memory-wiki.md) : compile un wiki (entities/, concepts/, syntheses/, reports/) avec `vault.renderMode: "obsidian"`.

Recherche web : l'outil natif `web_search` couvre déjà Brave (`BRAVE_API_KEY`, seul fournisseur à offrir explicitement un palier gratuit selon la doc), DuckDuckGo (sans clé, « unofficial HTML-based »), Tavily, Exa, Perplexity, Firecrawl, Gemini, Grok, SearXNG (auto-hébergé, sans clé), Parallel Search Free (sans clé, opt-in), Codex Hosted Search (connexion ChatGPT), Ollama Web Search (docs.openclaw.ai/tools/web.md). Les fournisseurs sans clé ne sont jamais choisis automatiquement. Plugins officiels sur ClawHub : DuckDuckGo (26,6 k téléch.), Parallel (16 k), Tavily (13,3 k), Firecrawl (12,2 k). Skills tiers les plus téléchargés : Tavily Search https://clawhub.ai/jacky1n7/openclaw-tavily-search (268 / 108 k), Web Search https://clawhub.ai/billyutw/web-search (35 / 40,8 k). Les skills n'apportent rien de plus que l'outil natif pour la plupart.

Calendrier : Gog (ci-dessus) couvre Google Calendar; Caldav Calendar https://clawhub.ai/asleep123/caldav-calendar (249 / 34,2 k, v1.0.1, Pass; iCloud, Google, Fastmail, Nextcloud via `vdirsyncer` + `khal`, Linux, tmux requis pour l'édition; lecture et écriture); Google Calendar via Maton https://clawhub.ai/byungkyu/google-calendar-api (21 / 12,6 k).

YouTube :

| Skill | URL | Signets / téléch. | Fonction | Prérequis | L / É | Audit, risques |
|---|---|---|---|---|---|---|
| YouTube (managed OAuth) | https://clawhub.ai/byungkyu/youtube-api-skill | 145 / 29,2 k, v1.2.5, màj il y a 1 mois | Data API v3 : recherche, détails, « your channel », playlists, abonnements, commentaires; fonctions hébergées et déclencheurs | CLI `maton`, compte Maton, connexion YouTube approuvée | Lecture et écriture (noter, playlists, commentaires); « explicit user approval for all writes » | Pass. Tout transite par api.maton.ai; les déclencheurs peuvent réexpédier des données vers des URL externes jusqu'à suppression |
| youtube-full | https://clawhub.ai/therohitdas/youtube-full | 19 / 14,5 k | Tâches YouTube dont transcriptions | non lu | ? | Non vérifié |
| Youtube | https://clawhub.ai/grpaiva/youtube | 6 / 9,7 k, v1.0.1, màj il y a 5 mois | Recherche, infos de chaîne, vidéos récentes, détails, transcriptions horodatées | clé `YOUTUBE_API_KEY` (Google Cloud), `yt-dlp` en secours, serveur MCP npm via `mcporter` | Lecture (écrit dans /tmp) | Audit non affiché; fiche encore écrite pour « clawdbot » (ancien nom), signe d'âge |
| youtube-api | https://clawhub.ai/therohitdas/youtube-api | 1 / 5,4 k | Données YouTube « sans quota Google ni OAuth » | non lu | Lecture | Méthode non officielle, à vérifier |
| extract-youtube-transcript | https://clawhub.ai/hushenglang/extract-youtube-transcript | 0 / 2,8 k | Transcriptions en texte brut via Python local | Python | Lecture | Non vérifié |
| TinkerClaw YouTube | https://clawhub.ai/globalcaos/youtube-ultimate | 7 / 3,8 k | Transcriptions gratuites, téléchargements | non lu | Lecture | Non vérifié |

Aucun skill consulté ne couvre YouTube Analytics (rétention, revenus); la Data API v3 donne statistiques publiques et données de chaîne du propriétaire.

Patreon : un seul résultat, « Patreon Launch Coach » https://clawhub.ai/charlie-morrison/patreon-launch-coach (0 / 727, catégorie Lifestyle), un skill de conseils, pas une intégration API. Aucun skill d'accès à l'API Patreon (membres, publications) n'a été trouvé.

Réseaux sociaux (publication différée) : offre fragmentée et peu adoptée. Les plus téléchargés : Social Media Scheduler https://clawhub.ai/1kalin/social-media-scheduler (14 / 14,2 k, planification et rédaction, pas de publication documentée), Mixpost https://clawhub.ai/lao9s/mixpost (7 / 6,3 k, logiciel auto-hébergé de gestion sociale), SocialBu https://clawhub.ai/socialbu/socialbu (0 / 179, SaaS), ReplyNodes https://clawhub.ai/replynodes-ai/replynodes (0 / 533, « approval-first »). Buffer et Typefully n'apparaissent pas. Le skill « Youtube Upload » https://clawhub.ai/perufitlife/upload-to-youtube (0 / 211) n'a pas été vérifié.

Veille (RSS, Patch Tuesday, actualité) : « patch tuesday » ne renvoie aucun skill. RSS : Rss Ai Reader https://clawhub.ai/benzema216/rss-ai-reader (14 / 8,8 k, résumés LLM et diffusion multi-canaux), RSS Feed Digest https://clawhub.ai/zacjiang/rss-feed-digest (1 / 3,4 k, filtre et résumé RSS/Atom), botlearn-rss-manager https://clawhub.ai/calvinxhk/botlearn-rss-manager (0 / 1,2 k, dédoublonnage et scoring). Beaucoup de variantes « free » d'un même auteur (thcjp) orientées Feishu et chinois. Une veille Patch Tuesday se construit avec `web_fetch` plus un flux RSS Microsoft (MSRC) dans une tâche planifiée, sans skill tiers.

Validation et relecture :

| Skill | URL | Signets / téléch. | Fonction |
|---|---|---|---|
| Pre-Publish Fact Checker | https://clawhub.ai/jernejcicorbin-hub/claim-verifier | 0 / 13,6 k | Vérifie les affirmations factuelles externes avant publication |
| Fact Checker | https://clawhub.ai/nissan/fact-checker | 1 / 3,6 k | Vérifie chiffres et faits d'un brouillon Markdown contre des données sources |
| Fact Cross Check | https://clawhub.ai/marianachow0321/fact-cross-check | 1 / 1,1 k | Vérification multi-sources avec niveaux de confiance |
| Veritier | https://clawhub.ai/salomonhenao/veritier-fact-checking | 1 / 1,9 k | « Trust gate » avant appels d'outils (service tiers) |

Ce sont des skills de méthode (texte d'instructions), pas des outils. Un validateur maison dans `skills/` du workspace fait la même chose avec les règles propres à LeFiltre, et sans dépendance.

### A.4 Synthèse factuelle (A)

Le registre est vaste, peu curé, avec une longue traîne de doublons (dix skills « Runs GitHub tasks via the gh CLI » quasi identiques). Les signaux fiables sont : auteur (steipete = mainteneur d'OpenClaw), ancienneté, nombre de signets et audit « Pass ». Une famille de skills (byungkyu) repose sur un OAuth géré par un tiers (Maton) : pratique, mais toutes les données passent par ce tiers. Pour courriel, GitHub, calendrier et Obsidian, les skills de steipete (gog, github, obsidian) dominent de très loin; pour YouTube, Patreon et réseaux sociaux, l'offre est faible ou absente.

## 3. Partie B : tableaux de bord et visualisation

Métadonnées via ecosyste.ms (date « sync » = dernière synchronisation connue; le push réel peut être plus récent) et pages GitHub lues le 2026-11-05.

| Projet | URL | Étoiles | Issues ouv. | Dernier push (sync) | Ce qu'il montre | Installation | Serveur distant |
|---|---|---|---|---|---|---|---|
| Control UI (officiel, intégré) | https://docs.openclaw.ai | s.o. | s.o. | suit OpenClaw (push 2026-10-31) | chat, sessions, compteurs d'usage et de contexte, activité des outils, éditeur de config, catalogue d'UI de plugins | `openclaw dashboard`, port 18789 | Oui via Tailscale Serve, tunnel SSH ou proxy authentifié |
| Autensa, ex Mission Control (crshdn) | https://github.com/crshdn/mission-control | 2 155 | 22 | 2026-09-16 | pipeline de tâches, coût par tâche avec plafonds, flux d'événements | clone, `npm install`, `OPENCLAW_GATEWAY_URL` + jeton | URL de passerelle configurable |
| OpenClaw Studio (grp06) | https://github.com/grp06/openclaw-studio | 2 032 | 23 | 2026-06-26 | chat, approbations, cron, vue des agents | `npx -y openclaw-studio@latest` (Node 20.9+) | Oui, documenté avec Tailscale serve et `wss://` |
| OpenClaw-bot-review (xmanrui) | https://github.com/xmanrui/OpenClaw-bot-review | 2 024 | 28 | 2026-07-18 | état des bots, agents, modèles, sessions | non vérifié | non vérifié |
| LobsterBoard (Curbob) | https://github.com/Curbob/LobsterBoard | 976 | 3 | 2026-05-07 | grille de widgets (60+), coûts IA | `npm install lobsterboard` | agent `lobsterboard-agent` par VPS | 
| openclaw-nerve (daggerhashimoto) | https://github.com/daggerhashimoto/openclaw-nerve | 868 | 32 | 2026-10-06 | cockpit temps réel, voix, kanban piloté par agent, fichiers | non vérifié | non vérifié |
| AI Maestro (23blocks-OS) | https://github.com/23blocks-OS/ai-maestro | 839 | 36 | 2026-10-30 | agents sur plusieurs machines, cartes d'approbation, messagerie inter-agents | script curl, Node 18+, tmux, port 23000 | Oui (maillage de machines) |
| OpenClaw Office (WW-AI-Lab) | https://github.com/WW-AI-Lab/openclaw-office | 645 | 8 | 2026-06-12 | bureau virtuel isométrique (état des agents, appels d'outils), atelier de skills, pages Dashboard/Agents/Channels/Skills/Cron, chat; interface en chinois | `npx @ww-ai-lab/openclaw-office` (Node 22+), service optionnel | Oui : `VITE_GATEWAY_URL=ws://IP:18789`, jeton d'`openclaw.json` |
| OpenClaw Agent Dashboard (tugcantopaloglu) | https://github.com/tugcantopaloglu/openclaw-dashboard | 481 | 0 | 2026-03-17 | coûts par modèle et session, mémoire, cron, logs, gestionnaire de fichiers; MFA TOTP | `node server.js`, port 7000, zéro dépendance | intégration Tailscale |
| openclaw-dashboard (mudrii) | https://github.com/mudrii/openclaw-dashboard | 455 | 6 | 2026-09-07 (sync 2026-09-19) | CPU, RAM, disque, version, état passerelle, coûts, cron, sessions, jetons, sous-agents, logs, diagnostics, chat optionnel | binaire Go unique (Homebrew, tarball, script, Docker, Nix), service d'arrière-plan | Lit `~/.openclaw` et le CLI local : à installer SUR le serveur, lié à 127.0.0.1 (« Local Only »), puis tunnel |
| openclaw-office (wickedapp) | https://github.com/wickedapp/openclaw-office | 104 | 1 | 2026-03-24 | bureau virtuel, animations de délégation, journal d'activité | clone, `node cli/index.js init`, port 4200, plugin Notify à copier dans `~/.openclaw/extensions/` | `gateway.url` configurable; le plugin Notify doit être sur le serveur |
| openclaw-dashboard « Mission Control » (ChristianAlmurr) | https://github.com/ChristianAlmurr/openclaw-dashboard | 35 | 0 | 2026-03-21 | carte d'org, grille, flux, coûts, cron builder, kanban, santé mémoire, sécurité, « market intelligence » | `npm run dev`, port 3333, Node 20+ | Non documenté; lit `~/.openclaw/` local |
| AgentFlow (frexida) | https://github.com/frexida/agentflow | 4 | 8 | 2026-03-08 | éditeur d'organigramme par glisser-déposer qui exporte une config OpenClaw (JSON) et un script | démo hébergée (localStorage), Docker ou build npm; v2 exige Supabase | v2 : « Gateway integration (OpenClaw Protocol v3) », non détaillé |
| pixel-agents-dashboard (jaffer1979) | https://clawhub.ai/jaffer1979/openclaw-pixel-agents-dashboard | 0 signet | s.o. | « 5 mo ago » | sprites pixel-art des agents, bulles d'activité, matériel, lancement de tâches | `npm install`, `npm run build`, port 5070, `setup.sh` | URL + jeton de passerelle, cibles SSH optionnelles |
| openclaw-mission-control (abhi1693) | https://github.com/abhi1693/openclaw-mission-control | 4 103 | 85 | archivé 2026-08-06 | ancien projet le plus étoilé, archivé | ne pas installer | s.o. |
| clawsuite (David-CKS) | https://github.com/David-CKS/clawsuite | 0 | 0 | 2026-05-08 | « all-in-one command center » | non vérifié | s.o. |

Synthèse (B) : l'article de Bitdoze du 2026-10-30 (https://www.bitdoze.com/best-openclaw-dashboards/) résume l'état du marché : « For many single-agent setups [the Control UI] is enough. If this covers what you need, stop here. » Les projets « bureau virtuel » (OpenClaw Office, wickedapp, pixel-agents) sont des visualisations ludiques plus que des outils de pilotage; la moitié des tableaux de bord communautaires a cessé d'évoluer entre mars et juin 2026 (wickedapp, ChristianAlmurr, AgentFlow, tugcantopaloglu, LobsterBoard, VidClaw). Les seuls encore actifs à l'automne 2026 : Autensa (septembre), mudrii (septembre), AI Maestro (octobre), openclaw-nerve (octobre). Tous exigent un jeton de passerelle et, pour la plupart, un accès au dossier `~/.openclaw` du serveur, donc une installation sur le serveur lui-même plus un tunnel privé.

## 4. Partie C : Paperclip (paperclipai/paperclip)

### C.1 État au 2026-11-05

| Donnée | Valeur | Source |
|---|---|---|
| Dépôt | https://github.com/paperclipai/paperclip, MIT, créé 2026-03-02 | GitHub |
| Étoiles / forks | 108,9 k / 18 k (ecosyste.ms 2026-10-30); 109,7 k sur la page GitHub | ecosyste.ms, GitHub |
| Issues et PR ouvertes | 3,4 k issues + 3,2 k PR (page GitHub); 6 644 « open issues » ecosyste.ms | GitHub, ecosyste.ms |
| Dernier push | 2026-10-30 | ecosyste.ms |
| Dernière version | v2026.1023.0 (24 octobre 2026); avant : v2026.1012.0 (12 oct.), v2026.1005.0 (6 oct.), v2026.1001.0 (2 oct.), v2026.916.1 (21 sept.) | https://github.com/paperclipai/paperclip/releases |
| Rythme | une version toutes les une à deux semaines, chacune avec une section « Breaking » (rôle Operator élargi, protocole de verrou de restauration changé, budgets qui bloquent l'usage non tarifé par défaut, @-mention qui ne déclenche plus de run) | releases |
| Prérequis | Node.js 24.11+, pnpm 9.15+; `npx paperclipai@latest onboard --yes`; UI et API sur localhost:3100; PostgreSQL pour un déploiement serveur | README, xCloud |
| Stabilité rapportée | v2026.1001.0 décrite comme « mostly a reliability release »; v2026.1005.0 corrige « a server crash loop caused by a permanently rejected queued run » dans le planificateur de heartbeats | releases |

### C.2 Adaptateur `openclaw_gateway`

Source : https://docs.paperclip.ing/reference/adapters/openclaw-gateway.md (lu le 2026-11-05).

- Fonctionnement : Paperclip se connecte au Gateway OpenClaw par WebSocket (`ws://` ou `wss://`), répond au `connect.challenge`, envoie `connect` (auth + identité d'appareil), puis `agent` (message de réveil, `runId` comme clé d'idempotence, `sessionKey`), puis `agent.wait`; les événements sont relayés dans les logs et la transcription Paperclip.
- Prérequis : passerelle joignable (Docker, hôte distant, adresse Tailscale), jeton ou mot de passe de passerelle (au moins 16 caractères), `devicePrivateKeyPem` persistée (sinon une nouvelle identité d'appareil à chaque run et appairages répétés), `autoPairOnFirstConnect` (gère un défi d'appairage, sinon « pairing required » et `openclaw devices approve`), rôle `operator` et scopes `operator.admin` par défaut.
- Sessions : `issue` (une session OpenClaw par tâche, défaut suggéré), `fixed`, `run` (rien ne persiste entre heartbeats).
- Limites : « operational but not yet selectable in the UI, where it appears as Coming soon »; configuration par l'API, par import d'entreprise ou par le « OpenClaw invite prompt » (Company Settings, General, Invites : on colle le prompt dans le chat OpenClaw); `waitTimeoutMs` dépassé = `openclaw_gateway_wait_timeout`; `127.0.0.1` dans Docker pointe vers le conteneur.
- Le guide Codebridge du 2026-10-23 (https://www.codebridge.tech/articles/openclaw-paperclip-integration-how-to-connect-configure-and-test-it) confirme le statut « Coming soon » dans l'UI, décrit une variante par webhook HTTP (`/hooks/agent` d'OpenClaw), les codes d'erreur (`openclaw_gateway_unreachable`, `openclaw_auth_failed`, `openclaw_gateway_pairing_required`) et conseille cette couche « à partir d'environ trois agents ou plus ».

### C.3 Chat avec le CEO, import d'organisation, coûts

- Agent Chat : fonctionnalité expérimentale, opt-in (Settings, Experimental), « one ongoing conversation per agent »; v2026.1012.0 a fait du chat l'entrée principale de navigation. Avertissement officiel : « Experimental features may break at any time » (https://docs.paperclip.ing/experimental/overview/).
- CEO : « The first agent created in a company is always the CEO »; les nouveaux agents lui rapportent, il rapporte à l'humain; l'assignation de tâches lui est activée d'office (https://docs.paperclip.ing/guides/org/agents/).
- Import/export (« Company Portability », https://docs.paperclip.ing/guides/power/export-import.md) : paquet dossier ou `.zip` de fichiers Markdown (`COMPANY.md`, `agents/*/AGENTS.md`, `projects/`, `skills/`, `tasks/`, `.paperclip.yaml`), importable depuis un dossier, un zip ou une URL GitHub (`--ref` pour épingler), `--dry-run`, cible nouvelle ou existante. Omis : valeurs de secrets, chemins machine, approbations, historique de coûts. « After import: agent heartbeats start disabled. » Format texte, « diff cleanly in Git ».
- Heartbeats et coûts : doc officielle : entre deux heartbeats l'agent « consumes no budget, holds no context in memory, and takes no action »; « Heartbeat on interval is off for new agents »; budgets mensuels par agent avec seuil d'alerte et pause automatique. Mais la discussion #449 (https://github.com/paperclipai/paperclip/discussions/449, mars-avril 2026) documente le problème réel : un réveil par minuterie invoque le modèle complet même sans travail; un utilisateur avec 14 agents rapporte « about 90 % of heartbeats were idle »; estimation modélisée de 72 $/jour pour 10 agents à 5 min d'intervalle avec Sonnet; mesures proposées : triage avant modèle, intervalles de 15 à 30 min, pause manuelle, réutilisation de session (corrigée en commit 7d1748b), préfixes de prompt stables. La version v2026.1023.0 ajoute un « exact ledger » des coûts et le blocage de l'usage non tarifé.

### C.4 Retours d'expérience et combinaison OpenClaw + Paperclip

| Source, date | Ce qui est dit |
|---|---|
| Codebridge, 2026-10-23 | Procédure de test : deux heartbeats consécutifs réussis avec session reprise avant de passer en planifié; pièges : jetons en clair dans les logs, liste blanche d'agents, timeouts > 300 s; « makes sense at roughly three or more agents ». |
| Agent37, 2026-07-19 (https://www.agent37.com/blog/paperclip-vs-hermes-vs-openclaw) | « If you have one agent doing one job, an org chart is overhead, not leverage »; « Do not start here »; le coût des jetons « multiplies per agent » et chaque agent ajoute du sondage; fixer les budgets dès le premier jour. Vendeur d'hébergement, biais déclaré. |
| xCloud, 2026 (https://xcloud.host/openclaw-vs-paperclip-vs-hermes/) | « If OpenClaw is the employee, Paperclip is the company »; seuil : opérateurs « already running 5+ agents »; aucun exemple documenté d'entreprise autonome fonctionnant sur Paperclip seul; coûts estimés 30 à 200 $/mois en usage léger. Vendeur, chiffres non sourcés. |
| Till Freitag, 2026-04-28 màj 2026-07-01 (https://till-freitag.com/en/blog/paperclip-control-plane-for-ai-companies-en) | Modèle d'organigramme : un seul manager par agent sauf le CEO; plan stratégique initial soumis au conseil; checkout atomique des tâches. Favorable, peu de critiques techniques. |
| Aspiro AI Studio, 2026-04-09 (https://www.aspiroai.studio/insights/openclaw-implementation-reality-paperclip) | Six semaines d'agents OpenClaw sur des tâches à faible risque; hallucinations fréquentes les deux premières semaines (participants de réunion inventés, échéances inventées), dérive d'instructions, pas de mémoire inter-sessions. Chiffres internes non vérifiables. Ne décrit pas l'adaptateur. |

Aucun retour trouvé entre septembre et novembre 2026 qui décrive en détail un usage de l'adaptateur `openclaw_gateway` en production; les retours de cette période sont des guides (Codebridge) ou des comparatifs de vendeurs. Les 6 600 issues et PR ouvertes et le rythme de versions avec ruptures confirment un projet très vivant mais mouvant.

## 5. Partie D : modèles locaux avec OpenClaw

### D.1 Support officiel

- Ollama (https://docs.openclaw.ai/fr/providers/ollama.md) : clé `OLLAMA_API_KEY` factice suffisante en local; `baseUrl` canonique; trois modes (local seul, cloud seul, mixte); modèle local suggéré à l'onboarding : `gemma4`; embeddings : `nomic-embed-text`; vision : `qwen2.5vl:7b`. Avertissement central : utiliser l'API native, pas l'URL `/v1` compatible OpenAI, car « elle empêche le bon fonctionnement des appels d'outils »; `compat.supportsTools: false` pour les petits modèles qui échouent sur les schémas d'outils, « au prix de la capacité d'agent ». Aligner `contextWindow` (budget OpenClaw) et `params.num_ctx` (envoyé à Ollama). Une tâche cron qui vise un Ollama injoignable est marquée `skipped` (vérification mise en cache 5 min). Sur WSL2 avec CUDA, le service Ollama peut entrer en boucle de redémarrage et « monopoliser la mémoire de l'hôte ».
- LM Studio (https://docs.openclaw.ai/providers/lmstudio.md) : serveur `lms server start --port 1234` ou démon `llmster`; chat via l'API Messages compatible Anthropic (`/v1/messages`); préchargement par défaut; « Guided setup only auto-offers models that report tool training and at least 16K effective context »; hôtes LAN et tailnet acceptés.
- Routage par tâche (https://docs.openclaw.ai/concepts/models.md) : `agents.defaults.model.primary` et `fallbacks` (essayés dans l'ordre, cooldown 1, 5, 25 min); `agents.entries.<agent>.model` par agent; `agents.defaults.utilityModel` (« a lower-cost model for short internal tasks such as session titles, progress narration, and transcript summaries »); `decisionModel`, `imageModel`, `pdfModel`, `mediaModels`; cron : `--model` ou clé `model` du payload, `fallbacks: []` pour un run strict; `/model <alias> -s|-a|-g`; `memory.search.provider: ollama` pour des embeddings locaux; le memory flush « uses the session's model chain, so a local model with local fallbacks keeps extraction local ».
- Heartbeat : la page https://docs.openclaw.ai/gateway/heartbeat.md indique que « Heartbeat monitoring has been retired » : les anciens moniteurs deviennent des tâches d'automatisation ordinaires (`openclaw doctor --fix` convertit; `HEARTBEAT.md` est importé puis archivé). La tâche par défaut « Proactive check » tourne toutes les 30 min, en `idleOnly`, livre au DM du propriétaire; une réponse `NO_REPLY` évite l'appel final au modèle; un override de modèle doit respecter `modelPolicy.allow`.

### D.2 Retours de la communauté

| Source, date | Modèles qui marchent | Tâches « simples » conseillées | Matériel |
|---|---|---|---|
| Bitdoze, 2026-02-24 (https://www.bitdoze.com/openclaw-ollama-local-models/) | qwen3:8b (~6 Go), qwen2.5-coder:14b et deepseek-r1:14b (~10 Go), gpt-oss:20b (~14 Go), qwen3:32b (~20 Go); éviter llama2, codellama (pas d'outils) | « answering messages, scheduled jobs, lookups, and automations », estimés à 80 à 90 % de l'usage de l'auteur; cloud pour codage multi-étapes, écriture longue | règle : paramètres × 0,6 Go en Q4 + 2 à 4 Go; CPU seul 5 à 10 fois plus lent; 12 à 16 Go de VRAM pour 7B à 14B |
| dev.to (zacvibecodez), avril 2026 (https://dev.to/zacvibecodez/best-open-source-models-for-openclaw-run-locally-no-api-costs-5fff) | 8 Go : Phi-4 ou Qwen3.5:9b avec 8 à 16K de contexte; 16 Go : Qwen3.5:14b; 24 Go : Qwen3.5:27b | généraliste : Qwen3.5; code : Codestral / Qwen3-Coder | « Ollama recommends at least 64K context for agent workloads like OpenClaw »; le cache KV s'ajoute aux poids |
| BetterClaw, 2026-03-18 màj 2026-10-13 (https://www.betterclaw.io/blog/openclaw-ollama-guide; vendeur concurrent, résultats auto-déclarés) | marchent : gemma4:e4b, qwen3.5:9b, hermes3, mistral:7b, gpt-oss:20b, qwen3-coder:30b, gemma4:26b; échouent : phi3:mini, qwen2.5:3b | « simple, high-volume background tasks such as heartbeat status checks » vers un petit modèle (hermes3); rôle de « worker » sous un orchestrateur cloud ou 30B+ | ~8 Go : gemma4:e4b, hermes3, mistral:7b; ~12 Go : qwen3.5:9b; recommandé pour l'agent principal : 30B+ et 64K de contexte, soit 16 à 24 Go et plus |
| InsiderLLM, 2026-02-14 (https://insiderllm.com/guides/openclaw-model-routing/) | Ollama 8B en « free tier » | heartbeats, opérations de fichiers, analyse de logs; Haiku pour triage de courriel et Q&R; Sonnet pour code et rapports; Opus pour architecture | s.o. |

Erreurs fréquentes rapportées (BetterClaw, docs) : l'agent « raconte » l'action au lieu de l'exécuter (URL `/v1`); « model does not support tools »; timeout silencieux de découverte si le modèle n'est pas préchargé; bascule silencieuse vers un fournisseur cloud facturé quand la config Ollama n'est pas reconnue (à surveiller dans les logs).

Pour une RTX 3080 10 Go : aucun article testé ne vise cette carte précise. Par recoupement des chiffres ci-dessus, un modèle 7B à 9B en Q4 (5 à 6 Go de poids) laisse 3 à 4 Go pour le contexte, soit 8 à 16K de contexte utile; un 14B Q4 (~9 à 10 Go) ne laisse pas de place au contexte. C'est suffisant pour des tâches de classification, résumé court, extraction, tri de flux RSS et vérifications d'état, pas pour un agent principal avec outils et long contexte. Point d'architecture : la RTX 3080 est dans le PC Windows de Dominic, pas sur le serveur virtuel; un modèle local servirait donc le serveur à travers le réseau privé (Tailscale) et dépendrait de l'allumage du PC (la doc note que les crons vers un Ollama injoignable sont `skipped`).

## 6. Partie E : Codex et abonnement ChatGPT dans OpenClaw

- Doc officielle (https://docs.openclaw.ai/providers/openai/setup.md) : fournisseur `openai`, plugin Codex activé (`plugins.entries.codex.enabled: true`), connexion `openclaw onboard --auth-choice openai` ou `openclaw models auth login --provider openai` (`--device-code` en headless). Modèle par défaut à l'inscription : `openai/gpt-6-astra`; `gpt-5.6-terra`, `gpt-5.6-luna`, `gpt-5.5` selon le catalogue du compte; `gpt-5.3-codex-spark` seulement si le compte l'expose. Contexte « commonly advertises a 272000 token active window ». Différence avec la clé API : « uses your ChatGPT/Codex subscription with native Codex app-server execution, and needs no separate API key ». Le mode « Sign in with ChatGPT (Beta) » supporte « Developer function tools and web search », mais pas les plugins hébergés OpenAI ni les MCP hébergés. Les identifiants ne sont pas importés de `~/.codex`.
- Recherche web hébergée : « Codex Hosted Search » figure parmi les fournisseurs de `web_search` sans clé, via la connexion Codex/OpenAI (docs.openclaw.ai/tools/web.md).
- Contexte de politique : fin avril 2026, OpenAI a ouvert Codex à tous les paliers payants de ChatGPT et le 1er mai Sam Altman a déclaré OpenClaw « flat available under ChatGPT paid plans » (MindStudio, https://www.mindstudio.ai/blog/anthropic-vs-openai-agent-token-access-openclaw). Le même article rapporte qu'Anthropic, le même jour, a interdit l'usage des jetons OAuth Claude Pro/Max dans les outils tiers. Un fil HN (https://news.ycombinator.com/item?id=47844269) indique ensuite qu'OpenClaw considère l'usage du CLI Claude comme « sanctioned for this integration unless Anthropic publishes a new policy », avec des limites (heartbeat désactivé en mode CLI, parties du prompt système bloquées) et beaucoup de scepticisme sur la stabilité des règles.
- Limites et retours (OpenClawDC, 2026-07-29, https://openclawdc.com/blog/claude-chatgpt-subscription-agent-harness-rules/) : « Codex moved to token-based credit pricing in 2026, so plan usage draws down a metered allowance »; les utilisateurs « report hitting resets and limits when driving harnesses with subscription auth »; recommandation : garder une clé API en secours; « policies have changed more than once in 2026 ».

Synthèse (E) : l'abonnement ChatGPT fonctionne comme fournisseur secondaire ou de secours dans OpenClaw (modèles GPT récents, recherche hébergée sans clé), à condition d'accepter une allocation mesurée et un cadre de règles mouvant. Il n'apporte rien pour les ~25 projets ChatGPT existants de Dominic (pas d'import de projets ni de mémoire ChatGPT par cette voie).

## 7. Partie F : autres outils fréquemment associés

| Outil | Rôle observé | Source | Prérequis, risques |
|---|---|---|---|
| Tailscale | Exposer la Control UI et le WebSocket en HTTPS sur le tailnet (`gateway.tailscale.mode: "serve"`) tandis que la passerelle reste sur 127.0.0.1; `funnel` = public, exige mot de passe; `gateway.auth.allowTailscale: true` permet l'identité Tailscale pour la Control UI seulement | https://docs.openclaw.ai/gateway/tailscale.md | CLI `tailscale` connecté; Funnel requis par Gmail Pub/Sub; « Avoid Funnel for browser control » |
| Docker | Image officielle `ghcr.io/openclaw/openclaw` (miroir Docker Hub); volumes `/state` (SQLite, identifiants), workspace, cache; `OPENCLAW_GATEWAY_TOKEN` dans `.env`; sandbox optionnelle (`OPENCLAW_SANDBOX=1`, backend docker ou podman) | https://docs.openclaw.ai/install/docker.md | une seule réplique par volume d'état; build source = 6 Go de RAM; `127.0.0.1` dans le conteneur ne désigne pas l'hôte |
| Sauvegardes | `openclaw backup` (archive état + config + credentials + agents + workspaces, SQLite capturés proprement), `--verify`, `--to <location>` hors site (filesystem ou Cloudflare R2, chiffrement défini par la destination), `backup git` (dumps JSONL dans un dépôt privé, `--exclude-secrets`), `backup enable` (planification par cron), `backup restore` dans un dossier vide puis `openclaw doctor` | https://docs.openclaw.ai/cli/backup.md | archive locale en clair; « Restoring from a redacted backup requires re-pairing devices and re-authenticating providers » |
| Gestion de secrets | SecretRef opt-in par identifiant, fournisseurs `env`, `file`, `exec`, `store` (SQLite partagé); `openclaw secrets audit --check`, `configure`, `apply`; recettes 1Password, Bitwarden, Vault, pass, sops | https://docs.openclaw.ai/gateway/secrets.md | « Plaintext still works »; tout ce qui reste dans `openclaw.json`, `.env` ou `agents/*/agent/models.json` est lisible par l'agent |
| Obsidian | Deux voies : skill `steipete/obsidian` (CLI sur la machine du coffre) ou plugin intégré `memory-wiki` qui compile un wiki Obsidian-compatible à côté de la mémoire active | voir A.3 | memory-wiki ne remplace pas la mémoire; `vault.scope: agent` n'est pas une frontière de sécurité |
| n8n, Make | Pas d'intégration dédiée; le pont est générique : entrée par `POST /hooks/agent` (jeton `hooks.token` dédié, `hooks.allowedAgentIds`, `Idempotency-Key`, `waitForCompletion`) et `/hooks/wake`; sortie des automatisations vers un webhook | https://docs.openclaw.ai/automation/cron-jobs/webhooks.md | « Treat external event content as data »; router les entrées non fiables vers un agent restreint |
| Plugins officiels de diagnostic | Diagnostics OpenTelemetry (9,7 k téléch.), Diagnostics Prometheus (10,5 k), Memory LanceDB (20,3 k), Lossless Context Management (martian-engineering, 6,8 k) | https://clawhub.ai/plugins | Prometheus derrière l'auth de la passerelle |

## 8. Valeur pour notre projet (observations, sans décision)

| Élément | Point d'architecture touché | Ce que ça apporte | Prérequis | Risques |
|---|---|---|---|---|
| Plugin IMAP intégré | `skills/gmail-lecture-IMAP` et règle « lecture seule » | Lecture seule garantie par conception (pas d'envoi, pas de drapeaux), mot de passe en SecretRef, liste blanche d'expéditeurs, agent lecteur isolé | mot de passe d'application Gmail, backend sandbox (Docker) pour le lecteur isolé | déclencheur par courriel = entrée non fiable; ne remplace pas un skill de recherche dans la boîte (pour ça : `gog gmail search` en lecture) |
| Gog (steipete) | courriel, calendrier, Drive, Sheets | Un seul CLI maintenu par l'auteur d'OpenClaw, 198 k téléchargements, audit Pass | client OAuth Google à créer dans GCP; portées à limiter à la lecture si l'on veut respecter « aucune action externe sans GO » | le skill sait envoyer : la limite doit venir de la portée OAuth ou de `tools.deny`, pas seulement du prompt |
| Github (steipete) | `skills/github`, dépôt-mémoire du CEO | Couvre issues, PR, CI via `gh`; 204 k téléchargements | `gh auth login` avec un jeton limité aux dépôts de l'audit et du workspace | `gh api` permet des écritures; jeton à portée minimale |
| Skill Vetter | règle « aucune automatisation ajoutée sans revue » | Une procédure de revue avant toute installation de skill tiers, à intégrer au rôle du validateur | aucun | c'est un texte de méthode, pas un scanner |
| Validateur maison | `skills/validateur`, boucle d'évaluation des sous-agents | Les skills de fact-checking publics sont des textes génériques; un validateur local avec les règles LeFiltre (sources, chiffres, ton, pas de tiret cadratin) est plus pertinent et sans dépendance | rédaction du SKILL.md | aucun |
| Tâche « Proactive check » et modèle de tâche | « clôture par inactivité (heartbeat 30 min) », « filet du soir (cron) » | Le heartbeat historique n'existe plus comme mécanisme séparé : il faut décrire ces rituels comme des tâches d'automatisation (`idleOnly`, `activeHours`, `delivery.target: owner`, `NO_REPLY`) | OpenClaw à jour | le vocabulaire de l'architecture validée doit être aligné sur « automation jobs » |
| `utilityModel` et `fallbacks` | « un fournisseur principal, IA locale plus tard » | Permet de router titres de session, résumés et narration vers un modèle moins cher dès maintenant (Haiku), puis vers Ollama quand la machine locale sera prête, sans toucher à l'agent principal | aucun | une bascule silencieuse vers un fournisseur facturé est possible si une config locale est mal reconnue |
| Tailscale Serve | « interface web (accès privé) » | HTTPS privé sans ouvrir de port, identité Tailscale acceptée pour la Control UI | Tailscale sur le serveur et sur le PC | Funnel (public) à éviter sauf pour Gmail Pub/Sub |
| `openclaw backup --to` | « secrets hors dépôt », sauvegardes | Sauvegarde vérifiée de l'état, hors site, planifiable; le dépôt Git du workspace ne contient pas l'état SQLite ni les identifiants | destination configurée (R2 ou système de fichiers) | archive locale en clair; test de restauration à prévoir |
| SecretRef + `secrets audit` | « secrets hors dépôt » | Vérification outillée qu'aucun identifiant ne traîne en clair dans les fichiers lisibles par l'agent | migration de chaque identifiant | aucun |
| Control UI officielle | tableau de bord | Suffit pour un seul agent (chat, sessions, usage, config); zéro installation supplémentaire | aucun | si un suivi des coûts par projet devient nécessaire, Autensa ou mudrii sont les seuls dashboards encore actifs |
| Paperclip (plus tard) | « organigramme, tickets, budgets envisagés plus tard » | Import d'organisation en Markdown compatible Git, budgets par agent avec pause automatique, journal d'audit; l'adaptateur `openclaw_gateway` existe et fonctionne par invite prompt | Node 24, PostgreSQL, un deuxième service à maintenir, `devicePrivateKeyPem` persistée, pairing d'appareil | UI de l'adaptateur « Coming soon »; versions avec ruptures toutes les deux semaines; heartbeats par minuterie coûteux si mal réglés; toutes les sources s'accordent : inutile sous 3 à 5 agents |
| Codex / ChatGPT | « un fournisseur principal » | Fournisseur de secours sans clé API, avec recherche web hébergée | abonnement ChatGPT payant | allocation mesurée, règles changeantes; n'apporte rien à la migration des projets ChatGPT |
| Skills YouTube | fiches projet LeFiltre et chaîne anglophone | Données de chaîne et transcriptions pour la veille concurrentielle et l'alimentation des fiches | clé YouTube Data API (grpaiva) ou compte Maton (byungkyu) | passage par un tiers (Maton); pas d'Analytics; le skill grpaiva date de l'ère « clawdbot » |
| Patreon, réseaux sociaux | Patreon à 4 niveaux, Shorts, diffusion | Rien d'utilisable sur ClawHub aujourd'hui; un accès à l'API Patreon ou aux plateformes devrait être écrit sur mesure, et serait de toute façon soumis au GO | s.o. | identités publiques séparées entre marques : un skill de publication partagé devrait être cloisonné par agent |
| n8n ou Make | « aucune automatisation ajoutée à l'installation » | Si un jour nécessaire, l'entrée par `/hooks/agent` avec jeton dédié et agent restreint est la voie documentée; le workflow Make de surveillance YouTube n'existe plus (mémoire projet) | `hooks.enabled`, jeton | ouvre un point d'entrée réseau; à retarder tant que la règle tient |

## 9. Points incertains et non lus

- Les deux chiffres des listes ClawHub ne sont pas étiquetés : le premier est très probablement le compteur de signets (confirmé sur les fiches gog, github, caldav), pas des « étoiles » ni des notes.
- Les pages « Load more » de ClawHub n'ont pas été dépliées : les classements par catégorie couvrent les 25 premiers résultats de chaque requête.
- Dates de dernier push : ecosyste.ms, avec des synchronisations parfois vieilles de plusieurs mois (ChristianAlmurr sync 2026-05-05, wickedapp sync 2026-03-25, frexida sync 2026-03-08). Un push plus récent est possible.
- Le dépôt steipete/gog n'est pas indexé par ecosyste.ms; ses étoiles GitHub n'ont pas été lues.
- Paperclip : la page des releases a été lue jusqu'à 100 000 caractères sur 101 317; la liste des cinq dernières versions est complète, les notes antérieures non. Aucun retour d'expérience daté de septembre à novembre 2026 sur l'adaptateur `openclaw_gateway` en production n'a été trouvé; seul le guide Codebridge (2026-10-23) en parle.
- Modèles locaux : aucune mesure sur RTX 3080 10 Go; l'estimation de la section D.2 est un recoupement, pas un test. Le billet BetterClaw est écrit par un concurrent commercial.
- Codex : les limites exactes d'allocation de l'abonnement ChatGPT pour OpenClaw ne sont pas documentées par OpenAI dans les sources lues; les conditions d'utilisation n'ont pas été lues directement.
- Non lus : fiches ClawHub de youtube-full, therohitdas/youtube-api, email-daily-summary, porteden/email-gmail-outlook, rss-ai-reader, 1kalin/social-media-scheduler; pages de configuration détaillées de mudrii (docs/CONFIGURATION.md); README d'openclaw-nerve, OpenClaw-bot-review et AI Maestro.
- Non testé : aucune installation n'a été faite; tous les « Pass » d'audit sont ceux affichés par ClawHub, dont la méthode (ClawScan, VirusTotal) n'est pas décrite sur la page d'audit elle-même.

## 10. Sources (URL, date de consultation 2026-11-05 sauf indication)

Officiel OpenClaw : https://docs.openclaw.ai/clawhub.md ; https://docs.openclaw.ai/skills.md ; https://docs.openclaw.ai/tools/web.md ; https://docs.openclaw.ai/concepts/models.md ; https://docs.openclaw.ai/concepts/memory.md ; https://docs.openclaw.ai/plugins/memory-wiki.md ; https://docs.openclaw.ai/fr/providers/ollama.md ; https://docs.openclaw.ai/providers/lmstudio.md ; https://docs.openclaw.ai/providers/openai/setup.md ; https://docs.openclaw.ai/gateway/heartbeat.md ; https://docs.openclaw.ai/gateway/tailscale.md ; https://docs.openclaw.ai/gateway/secrets.md ; https://docs.openclaw.ai/install/docker.md ; https://docs.openclaw.ai/cli/backup.md ; https://docs.openclaw.ai/automation/cron-jobs/webhooks.md ; https://docs.openclaw.ai/automation/cron-jobs/gmail.md ; https://docs.openclaw.ai/automation/imap.md.

ClawHub : https://clawhub.ai ; https://clawhub.ai/audits ; https://github.com/openclaw/clawhub ; fiches https://clawhub.ai/steipete/gog, /steipete/github, /gzlicanyi/imap-smtp-email, /byungkyu/youtube-api-skill, /grpaiva/youtube, /spclaudehome/skill-vetter, /asleep123/caldav-calendar, /jaffer1979/openclaw-pixel-agents-dashboard ; listes `skills?q=` gmail, imap, github, youtube, obsidian, notion, todoist, memory, calendar, rss, patreon, social media schedule, fact check, web search, patch tuesday.

Sécurité ClawHub : https://thehackernews.com/2026/02/researchers-find-341-malicious-clawhub.html (2026-02-02) ; https://insiderllm.com/guides/openclaw-clawhub-security-alert/ (màj 2026-04-28).

Tableaux de bord : https://www.bitdoze.com/best-openclaw-dashboards/ (2026-10-30) ; pages GitHub mudrii/openclaw-dashboard, ChristianAlmurr/openclaw-dashboard, wickedapp/openclaw-office, WW-AI-Lab/openclaw-office, frexida/agentflow ; API https://repos.ecosyste.ms/api/v1/hosts/GitHub/repositories/<owner>/<repo> pour crshdn/mission-control, grp06/openclaw-studio, tugcantopaloglu/openclaw-dashboard, 23blocks-OS/ai-maestro, Curbob/LobsterBoard, abhi1693/openclaw-mission-control, xmanrui/OpenClaw-bot-review, daggerhashimoto/openclaw-nerve, madrzak/vidclaw, David-CKS/clawsuite, paperclipai/paperclip, openclaw/openclaw, openclaw/clawhub.

Paperclip : https://github.com/paperclipai/paperclip ; https://github.com/paperclipai/paperclip/releases ; https://github.com/paperclipai/paperclip/discussions/449 (2026-03-10 à 2026-04-19) ; https://docs.paperclip.ing/reference/adapters/openclaw-gateway.md ; https://docs.paperclip.ing/reference/adapters/overview.md ; https://docs.paperclip.ing/experimental/overview/ ; https://docs.paperclip.ing/guides/org/agents/ ; https://docs.paperclip.ing/guides/day-to-day/dashboard/ ; https://docs.paperclip.ing/guides/power/export-import.md ; https://www.codebridge.tech/articles/openclaw-paperclip-integration-how-to-connect-configure-and-test-it (2026-10-23) ; https://www.agent37.com/blog/paperclip-vs-hermes-vs-openclaw (2026-07-19) ; https://xcloud.host/openclaw-vs-paperclip-vs-hermes/ (2026) ; https://till-freitag.com/en/blog/paperclip-control-plane-for-ai-companies-en (2026-04-28, màj 2026-07-01) ; https://www.aspiroai.studio/insights/openclaw-implementation-reality-paperclip (2026-04-09).

Modèles locaux : https://www.bitdoze.com/openclaw-ollama-local-models/ (2026-02-24) ; https://dev.to/zacvibecodez/best-open-source-models-for-openclaw-run-locally-no-api-costs-5fff (avril 2026) ; https://www.betterclaw.io/blog/openclaw-ollama-guide (2026-03-18, màj 2026-10-13) ; https://insiderllm.com/guides/openclaw-model-routing/ (2026-02-14).

Codex et politiques : https://www.mindstudio.ai/blog/anthropic-vs-openai-agent-token-access-openclaw (fin avril 2026) ; https://openclawdc.com/blog/claude-chatgpt-subscription-agent-harness-rules/ (2026-07-29) ; https://news.ycombinator.com/item?id=47844269 (environ mai 2026).
