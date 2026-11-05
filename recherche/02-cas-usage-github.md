# 02. Cas d'usage réels et publiés d'OpenClaw (code, configurations, workspaces)

Date de la recherche : 2026-11-05. Agent de recherche : Claude (sous-agent). Langue : français, sans tiret cadratin.

Convention de lecture :
- « Déclaré » = ce que l'auteur affirme dans sa source.
- « Déduit » = ce que je conclus à partir de la source, sans que l'auteur l'ait dit.
- Les citations sont courtes (moins de 15 mots) et entre guillemets.
- La pertinence est évaluée par rapport à notre projet : un solo entrepreneur (LeFiltre, clients PME, chaîne anonyme, commerce autonome) piloté par un agent central « CEO » avec mémoire dans un dépôt Git privé, Telegram et interface web, rituels planifiés, sous-agents et validateur.

---

## 1. Méthode et périmètre

### Requêtes exécutées (WebSearch, modes standard et étendu)
- « openclaw workspace github AGENTS.md SOUL.md example »
- « awesome openclaw github »
- « openclaw chief of staff agent setup »
- « openclaw multi-agent config example sub-agents orchestrator »
- « openclaw solo founder workspace memory git weekly review cron »
- « openclaw content creator youtube workflow agent »
- « "openclaw" "SOUL.md" github repo my workspace »
- « openclaw CRM clients agent telegram small business »
- « "awesome-openclaw-usecases" github »
- « github openclaw workspace "my agent" AGENTS.md MEMORY.md repo public personal assistant setup »
- « Nat Eliason openclaw zero human company agents setup »
- « openclaw agent "Larry" Oliver Henry results »
- « openclaw "sessions_spawn" sub-agent reviewer validator pattern workspace »
- « clawhub most popular skills openclaw 2026 »
- « openclaw weekly review cron telegram "MEMORY.md" git backup my setup blog »
- « openclaw "one agent" company departments org chart CEO agent markdown projects folder »
- « github "openclaw-workspace" repo template memory projects AGENTS.md "USER.md" »
- « openclaw Paperclip org chart agents company tickets budget integration »
- « openclaw second brain obsidian git memory solopreneur "AGENTS.md" lessons learned months »
- « openclaw agency multiple clients separate agents isolation per client workspace »
- « openclaw retour d'expérience configuration agent francophone mémoire cron Telegram »
- « openclaw "projects" folder "status" stale nudge follow-up agent markdown project files tracker »

### Fetches réalisés
Environ 45 pages lues (dépôts GitHub, gists, articles de blog, documentation officielle, pages de synthèse). Les échecs d'accès sont notés en section 7.

### Nombre de cas retenus
31 cas distincts, dont 9 de pertinence haute, 15 de pertinence moyenne et 7 de pertinence basse.

### Limites
- Peu de dépôts GitHub publient un workspace OpenClaw complet et réel : la plupart sont des gabarits (templates) nettoyés, ou des récits de blog sans code. Les fichiers AGENTS.md et SOUL.md réels des utilisateurs sont rarement visibles (vie privée).
- Plusieurs sources (Remote OpenClaw, fast.io, CrewClaw, Hostinger) sont des contenus commerciaux ; je les ai conservés seulement quand ils décrivaient un patron concret.
- Les publications X (Twitter) ne sont pas lisibles par fetch (robots.txt). Les cas Trebuh et Nat Eliason sont documentés par des sources secondaires.
- Les dates GitHub ne sont souvent pas visibles sur la page ; j'indique « 2026, date non affichée » dans ce cas.
- Le résumé des fichiers de certains dépôts n'a pas pu être lu en détail (raw.githubusercontent.com a refusé l'accès).

---

## 2. Tableau de synthèse

| # | Cas | Type d'usage | Agents | Mémoire | Proactivité | Canal | Pertinence |
|---|-----|--------------|--------|---------|-------------|-------|------------|
| 1 | Dithilli/openclaw-starter | Gabarit d'assistant quotidien | 1 | memory/AAAA-MM-JJ.md + MEMORY.md, sauvegarde Git nocturne | cron (9 créneaux), heartbeat, revue mémoire hebdo dimanche | Discord, WhatsApp, Slack, iMessage | Haute |
| 2 | ucsandman/OpenClaw-Setup | Workspace démo complet | 1 + spécialistes (recherche, code) | memory/people, projects, decisions, journaux; index MEMORY.md ~1-2k tokens; SQLite FTS5 | HEARTBEAT.md, prompts dédiés | non précisé | Haute |
| 3 | kindomLee/openclaw-workspace-template | Gabarit « 4+ mois en production » | 1 + sous-agents | 3 couches (journal, MEMORY.md, notes PARA), priorités P0/P1/P2, .learnings/ | 6 crons (curate, dreaming, rumination, forgetting, janitor, compress) | Telegram (alertes), Claude Code, OpenClaw | Haute |
| 4 | Ryan Sarver, « chief of staff » Stella | VC, suivi de levée de fonds, 100+ contacts | 1 | memory/AAAA-MM-JJ.md + MEMORY.md curé, Git | cron vendredi (recherche kaizen), revue dimanche | non précisé | Haute |
| 5 | Trebuh, équipe multi-agents solo founder | Startup solo | 4 (Milo, Josh, Marketing, Dev) | GOALS.md, DECISIONS.md, PROJECT_STATUS.md partagés + dossiers privés | HEARTBEAT.md : standup 8 h, recap 18 h, priorités lundi, métriques vendredi | 1 groupe Telegram, tags @nom | Haute |
| 6 | Tommaso Nervegna, organigramme et COO | Créateur Substack, contenu, veille | 8 (hub Aerith) | mémoire isolée par agent, fichier intel/ partagé lecture seule, vault Obsidian 9 000 notes | crons nocturnes, « War Room » 7 h 30, wrap vendredi, tidy dimanche | Telegram unique | Haute |
| 7 | NLW (AI Daily Brief), équipe de 10 agents | Média, podcast, formations | ~10 (3 PM, chief of staff, recherche, tâches) | fichiers markdown par agent | heartbeat 30 min, PM 8 h et 17 h | Telegram + tableau Mission Control | Haute |
| 8 | Rahul Subramaniam (Trilogy), DevBot | Dev sur 50+ dépôts, agents work/personal/family | parent + 5 sous-agents déclarés dans AGENTS.md | journaux + MEMORY.md, compaction QMD hebdo, Git + Stow | cron (standup 6 h, compaction dimanche 2 h, archivage mensuel), heartbeat jugé peu fiable | Telegram, Google Chat, Slack | Haute |
| 9 | Ming Tang, agent Helix (créateur de contenu scientifique) | Threads X, blog, infolettre, plans YouTube | 1 + délégation OpenSpace | OpenViking (L0/L1/L2), skills/content-creation/SKILL.md | heartbeat ramené à 4 h, cron recherche virale 2x/semaine | TUI, Telegram | Haute |
| 10 | Radek Sienkiewicz (VelvetShark), 50 jours | Créateur YouTube, infra, recherche | 1 + sous-agents recherche | SOUL.md, MEMORY.md, Obsidian 2 800 notes, index QMD, Git nocturne | cron 4 h/4 h 30/5 h 30/7 h, heartbeat 30 min 7 h-23 h | Discord par salons (#briefing, #youtube-stats...) | Moyenne |
| 11 | Reed Whetstone (Purveyors), 2 semaines solo dev | Code, blog, prospection | 1 | MEMORY.md, journaux, brain/ zettelkasten (people, projects, ideas) | cron évoqué sans détail | Discord par salons | Moyenne |
| 12 | Aman Khan, agent sécurisé | Assistant personnel | 1 | memory/ + MEMORY.md | watchdog 2 min, pas de cron décrit | WhatsApp | Moyenne |
| 13 | Oliver Henry, agent Larry | Création TikTok pour 2 apps | 1 | fichiers mémoire de performance par post, journal d'échecs | non précisé | WhatsApp, Postiz, X | Moyenne |
| 14 | hesamsheikh, Autonomous Project Management | Coordination par fichiers | principal + PM par projet + sous-sous-agents | STATE.yaml par projet, PROJECT_REGISTRY.md, Git optionnel | « checks in periodically » | non précisé | Moyenne |
| 15 | hesamsheikh, Project State Management | Suivi de projets par événements | 1 + sous-agents (sprint) | base SQL (projects, events, blockers) | cron 9 h : standup Discord + scan commits | Discord #project-state | Moyenne |
| 16 | hesamsheikh, YouTube Content Pipeline | Veille d'idées vidéo | 1 | SQLite pitches + embeddings, catalogue 90 jours | cron horaire | Telegram (sujet « video ideas »), Slack, Asana | Moyenne |
| 17 | Reza Rezvani, gist 25 prompts | Flux exécutifs (fondateur, dev, marketing) | 1 | dossiers daily, projects, decisions, people, lessons + MEMORY.md index | auto-audit lundi, revue config mensuelle, heartbeat 30 min | non précisé | Moyenne |
| 18 | ishwarjha, guide 11 étapes | Guide d'installation à production | 2 (home, work) | memory/, Git, backups | heartbeat 30 min 8 h-22 h, cron briefing 7 h, revue hebdo lundi, résumé Telegram nocturne | WhatsApp, Telegram, Discord, Slack | Moyenne |
| 19 | Zach Hajjaj, solo founder | Inbox, briefs, suivi relances | 1 + skill coding-agent | MEMORY.md + memory/*.md | cron : brief 9 h, update investisseurs hebdo, rétro mensuelle, relance 7 jours | Telegram | Moyenne |
| 20 | Atlas (Remote OpenClaw), chief of staff commercial | Persona vendue en 8 fichiers | 1, coordonné avec Scout et Muse | MEMORY.md « self-cleaning », budgets tokens | briefing 7 h, résumé 18 h, relance après 48 h de silence | Email, Telegram, Slack, WhatsApp | Moyenne |
| 21 | zac (Remote OpenClaw), équipes d'agents | 3 agents par rôle | 3 | soul/user/identity/memory par agent, Google Sheet partagé | cron par rôle (7 h, 9 h, 14 h, lundi, vendredi) | WhatsApp, Telegram, Slack | Moyenne |
| 22 | Droptica, sauvegarde Git | Méthode de versionnage | n/a | workspace, skills, crons, config sans secrets | cron horaire commit/push | n/a | Moyenne |
| 23 | ClawHub skill subagent-architecture | Patron orchestrateur / worker / reviewer | orchestrateur + spécialistes + ReviewerAgent | subagents/[nom]/SPECIALIST.md, knowledge-base/ | n/a | n/a | Moyenne |
| 24 | Reddit r/AI_Agents, isolation clients | Agence multi-clients | 1 ou plusieurs | problème : contenus clients mélangés | n/a | n/a | Moyenne |
| 25 | Paperclip (paperclipai/paperclip) | Orchestration « entreprise » d'agents | n agents avec organigramme | tâches, documents, pas de mémoire dédiée (en cours) | heartbeats BD, routines cron | UI web | Moyenne |
| 26 | Stanza, isolation de workspaces | Cours production | 1 agent par client | MEMORY.md privé par agent | n/a | bindings par compte | Basse |
| 27 | Easton (FR), guide pratique | Guide francophone | 1 | MEMORY.md | cron 8 h, rapport vendredi, sauvegarde dimanche | Telegram | Basse |
| 28 | openclaw.ai/showcase | Vitrine de 20 utilisateurs | variable | variable | crons quotidiens, SEO hebdo | Telegram, WhatsApp, Discord | Basse |
| 29 | mergisi/awesome-openclaw-agents | 196 gabarits SOUL.md | 1 par gabarit | memory-wiki/ | n/a | n/a | Basse |
| 30 | garrytan/gbrain | Couche mémoire externe | n/a | Postgres/PGLite, markdown canonique Git | 66 crons déclarés | MCP, CLI | Basse |
| 31 | Stack Junkie, architecture de workspace | Guide de référence | 1 | limites 20 000 car./fichier, 150 000 total | HEARTBEAT.md < 50 lignes | n/a | Basse |

---

## 3. Fiches détaillées (pertinence haute et moyenne)

### Cas 1. Dithilli/openclaw-starter
- URL : https://github.com/Dithilli/openclaw-starter
- Auteur : David Szarzynski (Dithilli), crédité avec son agent « A Linea ». Date : 2026, date non affichée. Licence MIT, 1 étoile.
- Usage déclaré : « a battle-tested workspace template », extrait d'un agent utilisé quotidiennement.
- Structure : AGENTS.md, BOOTSTRAP.md, HEARTBEAT.md, IDENTITY.md, MEMORY.md, SOUL.md (vide, « this is yours to write »), TOOLS.md, USER.md, openclaw.example.json (secrets retirés).
- Agents : 1. Sous-agents : non décrits.
- Mémoire : journaux memory/AAAA-MM-JJ.md ; MEMORY.md curé, chargé seulement en session privée. Règle : « If you want to remember it, write it to a file. »
- Proactivité : exemple de cron à 9 créneaux (8 h briefing, 9 h exploration du code, 10 h 25 et 14 h check-in, 15 h 47 message volontairement hors heure ronde, 20 h, 22 h 15 bonne nuit, 23 h sauvegarde Git silencieuse, dimanche 20 h revue hebdo de la mémoire). Heartbeat utilisé pour « suivre des curiosités », réfléchir, faire du travail de fond (commits Git).
- Canaux : Discord, WhatsApp, Slack, iMessage. Modèles : non précisés.
- Points forts déclarés : continuité, jugement (savoir se taire), proactivité, sécurité (« asking before external actions »).
- Patrons notables : « Self-healing » (jusqu'à 10 tentatives avant de demander de l'aide), « Pattern recognition » (proposer d'automatiser les demandes répétées), « The Sugar Problem » (auto-audit contre la complaisance du modèle).
- Problèmes déclarés : aucun.
- Ressemble à notre projet : agent unique, mémoire en fichiers, sauvegarde Git nocturne, revue hebdomadaire de la mémoire, demande d'accord avant action externe.
- Diffère : pas de fiches de projets ni de départements ; pas de validateur.
- Idées réutilisables : revue mémoire hebdo planifiée ; auto-audit anti-complaisance ; cron « sauvegarde Git silencieuse ».

### Cas 2. ucsandman/OpenClaw-Setup
- URL : https://github.com/ucsandman/OpenClaw-Setup (docs : https://raw.githubusercontent.com/ucsandman/OpenClaw-Setup/main/docs/agent-architecture.md)
- Auteur : Wes Sander (avec « MoltFire »). Date : 2026, non affichée. Licence MIT. Données fictives uniquement (déclaré).
- Usage déclaré : « A complete demo workspace for running an AI assistant with OpenClaw. »
- Structure : racine (README, BOOTSTRAP, SOUL, IDENTITY, USER, AGENTS, HEARTBEAT, MEMORY, meditations.md, Hierarchical-Memory-System.txt) ; docs/ (agent-architecture, decision-template, morning-brief-template, silent-replies, task-routing) ; memory/ (people/, projects/, decisions/, journaux quotidiens) ; reflections/ ; prompts/ (heartbeat, meditation, memory-search, session-start) ; tools/ (example-tool, memory-db SQLite FTS5, security).
- Agents : un agent principal « single point of contact », scope « Everything » ; deux spécialistes (Research Specialist, Coding Specialist) créés seulement quand la tâche est bornée, longue ou parallélisable. Les actions externes (courriel) passent par le principal.
- Routage déclaré : reste au principal si simple, personnel ou nécessite le contexte complet ; délègue si borné, complexe, long ou parallèle.
- Gabarits de délégation : recherche (question, contexte, livrable, échéance, budget) ; code (tâche, fichiers, exigences, tests, contraintes).
- Mémoire : MEMORY.md « Lightweight index (~1-2k tokens) loaded every session » ; sous-dossiers people, projects, decisions ; journaux append-only ; recherche FTS5.
- Proactivité : HEARTBEAT.md + prompt dédié ; règles de silence (23 h-8 h sauf urgence, rien de neuf, « All systems nominal »).
- Canaux et modèles : non précisés.
- Problèmes déclarés : aucun.
- Ressemble à notre projet : un agent central qui délègue à des spécialistes avec un brief structuré ; dossiers projects/ et decisions/ ; index mémoire court chargé à chaque session ; règles de silence.
- Diffère : pas de départements ; spécialistes limités à recherche et code.
- Idées réutilisables : gabarit de délégation à 5 champs ; critères explicites « garder / déléguer » ; index mémoire plafonné à ~2k tokens.

### Cas 3. kindomLee/openclaw-workspace-template
- URL : https://github.com/kindomLee/openclaw-workspace-template
- Auteur : kindomLee (blog.mklee.org). Date : 2026, 91 commits. Licence MIT.
- Usage déclaré : « Production-tested », agent « 4+ months in production » ; fonctionne avec Claude Code, OpenClaw et Codex.
- Structure : CLAUDE.md, AGENTS.md, SOUL.md, IDENTITY.md, USER.md, TOOLS.md, MEMORY.md, MEMORY_COMPACT.md (~200 tokens chargés à chaque session), HEARTBEAT.md, BOOTSTRAP.md ; memory/ (journaux, dreams.md, reflections.md, archives) ; notes/ (PARA 00-Inbox à 04-Archive) ; .learnings/ (ERRORS.md, LEARNINGS.md, FEATURE_REQUESTS.md) ; cron/ ; scripts/ ; guides/ ; .gitleaks.toml ; .pre-commit-config.yaml.
- Mémoire : 3 couches (journal quotidien, MEMORY.md curé, base de notes) ; priorités P0 permanent, P1 daté et revu, P2 expire après 30 jours ; recherche hybride BM25 + récence ; hook qui force une recherche mémoire sur certains mots-clés.
- Boucle d'amélioration : 3 niveaux (repair, optimize, innovate) ; ce qui revient 3 fois ou plus est promu en mémoire longue ; après 3 corrections similaires, l'agent propose une mise à jour de SOUL.md soumise à approbation.
- Délégation : le principal reste focalisé, les sous-agents font le travail lourd ; « Four defense lines » : create, execute, deliver, alert ; « I did it » ne suffit pas, l'utilisateur doit recevoir le résultat.
- Proactivité : cron curate (horaire), dreaming (dimanche 3 h), rumination (mercredi 21 h, détecte les contradictions), forgetting (quotidien), janitor (quotidien), compress (mensuel). Les crons déposent des drapeaux dans .claude/flags/ sans réveiller le LLM ; un hook de début de session les remonte.
- Canaux : alertes Telegram. Modèles : non précisés.
- Problèmes déclarés : Codex sans cron ; mises à jour des hooks à fusionner à la main ; certains scripts dépréciés.
- Ressemble à notre projet : boucle d'évaluation et brief versionné (notre evaluations.md), vérification de livraison par le principal, mémoire en Git avec protection des secrets (gitleaks).
- Diffère : système cron orienté Claude Code, pas OpenClaw natif ; sophistication élevée (recherche hybride, taxonomie).
- Idées réutilisables : « règle des 3 » pour promouvoir une leçon ou proposer un changement de SOUL.md ; chaîne de vérification de livraison ; cron de détection de contradictions entre mémoire récente et longue ; pre-commit gitleaks.

### Cas 4. Ryan Sarver, « chief of staff » Stella
- URL : https://x.com/rsarver/status/2041148425366843500 (miroir lu : https://weekly-digest.pages.dev/content/2041148425366843500)
- Auteur : Ryan Sarver, investisseur. Date : 6 avril 2026.
- Usage déclaré : suivi d'une levée de fonds avec « 100+ LP contacts », recherche sur chaque nouveau contact avant la première rencontre (règle fixée par l'auteur).
- Structure : memory/AAAA-MM-JJ.md (journal brut : réunions, décisions, tâches) ; MEMORY.md curé par l'agent à partir des journaux, lu au démarrage ; sauvegarde Git. « There's no abstraction layer » entre lui et la connaissance de l'agent.
- Agents : 1. Sous-agents : non décrits.
- Proactivité : un script alimente les notes du jour depuis les sessions ; cron du vendredi qui recherche les nouveautés de la communauté OpenClaw et écrit memory/kaizen-research-AAAA-MM-JJ.md ; revue conjointe le dimanche matin.
- Canaux, modèles : non visibles.
- Points forts déclarés : « never forgets a commitment » ; fait les petites tâches sans qu'on le demande ; s'améliore par la boucle kaizen hebdo.
- Problèmes déclarés : « There are still some bumps, but less and less each week. »
- Ressemble à notre projet : agent unique de type chef de cabinet, mémoire markdown en Git, rituel hebdo d'amélioration.
- Diffère : pas de départements ni de sous-agents visibles.
- Idées réutilisables : cron hebdo « veille sur OpenClaw » suivie d'une revue à deux ; script d'extraction des sessions vers le journal du jour.

### Cas 5. Trebuh, équipe multi-agents pour solo founder
- URL : https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/multi-agent-team.md (source originale : https://x.com/iamtrebuh/status/2011260468975771862, non lisible par fetch ; résumé aussi sur https://openclaw.ai/showcase)
- Auteur : Trebuh, « solo founder ». Date : janvier-février 2026 (déduit du numéro de post et du showcase).
- Agents : Milo (stratégie, Claude Opus, défaut), Josh (business, Claude Sonnet), Marketing (Gemini), Dev (Claude Opus / Codex). Chacun a son SOUL.md.
- Structure : team/GOALS.md (OKR, lus par tous), DECISIONS.md (journal append-only), PROJECT_STATUS.md (mis à jour par tous), agents/milo, josh, marketing, dev (contexte privé).
- Mémoire : partagée (3 fichiers) + privée par agent.
- Proactivité (HEARTBEAT.md) : standup 8 h et recap 18 h (Milo), métriques 9 h (Josh), idées 10 h (Marketing), veille Reddit/X toutes les 2 h, prix concurrents hebdo, priorités du lundi, rapport métriques du vendredi.
- Canal : un seul groupe Telegram « Team », réponse seulement si tagué, @all pour diffusion.
- Points forts déclarés : spécialisation, mémoire partagée + privée, coût du modèle adapté à la tâche, « Start with 2, not 4. »
- Problèmes : la fiche ne signale pas de conflits d'écriture sur les fichiers partagés (déduit : risque réel avec 4 écrivains).
- Ressemble à notre projet : fichiers de décisions et d'état partagés, rituels du lundi et du vendredi, Telegram.
- Diffère : 4 agents permanents au lieu d'un CEO et de départements activés un à la fois.
- Idées réutilisables : trio GOALS / DECISIONS / PROJECT_STATUS ; convention « ne répond que si tagué » dans un groupe.

### Cas 6. Tommaso Nervegna, organigramme et promotion au poste de COO
- URL : https://nervegna.substack.com/p/i-gave-my-openclaw-agent-an-org-chart
- Auteur : Tommaso Nervegna. Date : 18 février 2026.
- Usage déclaré : 8 agents Claude sur Mac mini M4, pilotés par Telegram ; contenu Substack, veille, vault Obsidian.
- Organigramme : Aerith (lead, seule à déléguer, voit tous les autres) ; Barret (opérations, routage, « Friday wrap-up ») ; Cait Sith (veille X/Reddit/HN nocturne) ; Cloud, Yuffie, Cid (contenu : textes, posts, LinkedIn) ; Red XIII (vault Obsidian 9 000+ notes) ; Behemoth (Claude Code, PC avec 5090).
- Structure : workspace isolé par agent ; un fichier de veille partagé en lecture seule écrit chaque nuit (intel/AAAA-MM-JJ.md) ; vault Obsidian comme base de connaissances. Les agents ne s'appellent pas directement : tout passe par Aerith et le fichier intel.
- Proactivité : 0 h 30 veille, 2 h extraction de citations, 7 h briefing, 7 h 30 « War Room », toutes les 4 h brouillons de tweets, lun/mer/ven LinkedIn, vendredi bilan, dimanche soir rangement du vault. Règle « stay silent if nothing's wrong ».
- Canaux : Telegram unique ; Twilio (SMS, voix), gog (Gmail, Calendar), ElevenLabs.
- Modèles : Claude Sonnet et Opus.
- Points forts déclarés : la spécialisation bat un généraliste ; l'isolation évite la pollution de contexte ; Telegram unique réduit la friction.
- Problèmes déclarés : prompts à ajuster sans cesse ; hallucinations ; brouillons nocturnes parfois non publiables ; « Not set and forget. »
- Leçons déclarées : « Start with one agent and one job » ; rôle étroit et contexte serré ; notifications rares ; humain dans la boucle pour publier.
- Ressemble à notre projet : hub unique qui délègue ; départements ; bilan du vendredi ; Telegram ; aucune publication sans humain.
- Diffère : 8 agents permanents, chacun avec sa mémoire ; pas de Git mentionné.
- Idées réutilisables : fichier de veille partagé en lecture seule comme seul canal inter-agents ; départ à un agent et un travail.

### Cas 7. NLW (The AI Daily Brief), équipe de 10 agents
- URL : https://readray.ai/en/notes/unreal-engine-5-20260221/building-10-agent-openclaw-team (résumé de la vidéo YouTube HzVYgpMxMLE)
- Auteur : animateur de The AI Daily Brief (« NLW »). Date : 15 février 2026.
- Usage : Mac mini toujours allumé, accès Tailscale ; un projet Claude a servi de « coach » pour l'installation.
- Agents : builder (peu utilisé), 2 agents de recherche permanents, 3 project managers (surtout des rappels de tâches), un chief of staff « built but idle », un agent « NLW Tasks » (le plus utilisé).
- Orchestrateur : « None. » Le chief of staff peut lire les fichiers des autres mais ne coordonne pas. Un tableau « Mission Control » maison affiche interactions planifiées, résultats, coûts, décisions en attente.
- Fichiers par agent : identity, soul, agents.md, user.md (dont le style de « pushback » souhaité), tools.md, memory.md, heartbeat.md.
- Proactivité : heartbeat 30 min ; PM à 8 h et 17 h ; recherche continue.
- Canaux : Telegram principal ; Slack prévu. Modèles : Claude (Opus 4.6 tenté).
- Points forts déclarés : capture mobile, recherche persistante, PM qui relancent sur les décisions en retard.
- Problèmes déclarés : heartbeats « flaky », agents qui décrochent ; Mission Control très coûteux à construire ; pas de coordination inter-agents ; pas d'accès courriel ; skills évités par crainte de malware.
- Ressemble à notre projet : PM par projet qui relancent les décisions ; chief of staff envisagé comme triage.
- Diffère : beaucoup d'agents permanents sans orchestrateur.
- Idées réutilisables : lister « décisions en attente » comme sortie standard ; prudence sur un tableau de bord maison.

### Cas 8. Rahul Subramaniam (Trilogy), « OpenClaw in the real world »
- URL : https://trilogyai.substack.com/p/openclaw-in-the-real-world
- Auteur : Rahul Subramaniam. Date : 3 mars 2026.
- Usage déclaré : agents work-agent, personal-agent, family-agent sous ~/.openclaw/agents/, versionnés dans Git et liés par GNU Stow ; DevBot comme agent parent pour 50+ dépôts GitHub.
- Sous-agents : pr-reviewer, issue-triager, codemod, dependency-auditor, ci-monitor ; « Sub-agents are not peer agents » ; leur configuration vit dans l'AGENTS.md du parent ; niveaux d'autorité attribués (non définis).
- Mémoire : journaux + MEMORY.md ; compaction hebdomadaire QMD d'une semaine de journaux en un digest ; journaux de transactions JSONL séparés et archivés ; index de recherche reconstruit chaque nuit.
- Proactivité : cron compaction dimanche 2 h, « dream » 3 h, standup DevBot 6 h, archivage mensuel ; vérification courriel toutes les 15 min par script shell qui n'appelle l'agent que si urgent. Heartbeat jugé peu fiable et coûteux : « Cron doesn't skip. »
- Canaux : Telegram, Google Chat, Slack. Modèles : non nommés.
- Points forts déclarés : les scripts font le déterministe, le LLM l'intention ; config en Git, « Recovery time: 10 minutes » ; TOOLS.md avec valeurs par défaut pour réduire les erreurs d'outils.
- Problèmes déclarés : gonflement de la mémoire ; travail perdu après redémarrage sans Git ; interférence entre agents sur les mêmes boîtes ; heartbeat irrégulier ; PII dans les logs.
- Ressemble à notre projet : agent parent + sous-agents définis dans AGENTS.md (proche de nos departements/AGENTS.md) ; dépôt Git comme source de vérité ; filet planifié par cron.
- Diffère : contexte développement logiciel ; plusieurs agents permanents (work, personal, family).
- Idées réutilisables : préférer cron au heartbeat pour le filet du soir ; scripts déterministes avant le LLM ; archivage mensuel des journaux.

### Cas 9. Ming « Tommy » Tang, agent Helix (créateur de contenu)
- URL : https://divingintogeneticsandgenomics.com/post/openclaw-ai-assistant-mac-mini-setup/
- Auteur : Ming Tang. Date : 6-7 avril 2026.
- Usage déclaré : « I wanted an AI assistant that knew me » ; lit l'agenda et les brouillons, aide à écrire threads X, tutoriels, infolettre, plans YouTube.
- Structure : SOUL.md, USER.md, IDENTITY.md, TOOLS.md (ID du calendrier) ; workspace/skills/content-creation/SKILL.md avec 5 flux de rédaction ; skills OpenSpace (delegate-task, skill-discovery).
- Mémoire : OpenViking à 3 niveaux (L0 résumés, L1 aperçus, L2 contenu), rappel automatique avant réponse et capture après ; embeddings Ollama.
- Proactivité : heartbeat par défaut 30 min 24/7 jugé trop coûteux, ramené à toutes les 4 h entre 8 h et 23 h (« 90 % » d'économie déclarée) ; cron de recherche de contenus viraux 2 fois par semaine sur un modèle bon marché.
- Canaux : TUI et Telegram (appairage approuvé) ; WhatsApp abandonné (déconnexions). Compte Google séparé pour l'agent.
- Modèles : ClawRouter (Gemini 2.5 Flash pour le simple, Claude Sonnet 4 et Gemini 2.5 Pro pour le complexe) ; coût mesuré ~8 $ pour 335 requêtes contre ~153 $ tout Opus.
- Problèmes déclarés : les modèles bon marché ont halluciné et la mémoire a stocké les fabrications, « Helix reported these hallucinations as fact », 35+ souvenirs empoisonnés retirés ; l'agent a modifié sa propre config et cassé la recherche ; 36 % des skills communautaires contiendraient des injections (chiffre cité).
- Ressemble à notre projet : créateur de contenu avec un agent unique ; skill maison de création de contenu ; Telegram ; fournisseur principal + routage.
- Diffère : mémoire vectorielle externe plutôt que Git.
- Idées réutilisables : skill « création de contenu » avec flux nommés ; compte Google dédié à l'agent ; heartbeat réduit à quelques fois par jour ; vigilance sur la mémoire empoisonnée par des modèles faibles.

### Cas 10. Radek Sienkiewicz (VelvetShark), 50 jours + masterclass mémoire + 20 workflows
- URL : https://velvetshark.com/50-days-with-openclaw ; https://velvetshark.com/openclaw-memory-masterclass ; https://gist.github.com/velvet-shark/b4c6724c391f612c4de4e9a07b0a74b6
- Auteur : Radek Sienkiewicz (se dit mainteneur du code OpenClaw). Dates : 26 février 2026, 5 mars 2026, gist actif jusqu'au 10 août 2026.
- Usage déclaré : créateur YouTube et infra ; « markdown-first » ; Obsidian 2 800 notes avec index QMD nocturne ; sauvegarde nocturne de config, crons, skills, workflows vers un dépôt GitHub privé avec secrets remplacés par des espaces réservés.
- Proactivité : 4 h mise à jour, 4 h 30 sauvegarde GitHub, 5 h 30 art, 7 h briefing X, 3 h index QMD ; heartbeat 30 min 7 h-23 h (courriel en brouillon seulement, agenda, santé des services) ; rappel standup lundi 9 h 45.
- Sous-agents : recherche parallèle (X, Reddit, HN, YouTube, web) puis synthèse dans Obsidian.
- Canaux : WhatsApp puis Telegram puis Discord par salons (#general, #youtube-stats, #video-research, #inbox, #monitoring, #briefing) avec modèle par salon (Opus pour la recherche, modèles bon marché pour le routinier).
- Problèmes déclarés : un cron appelant l'ancienne commande a échoué en silence 7 jours ; perte de mémoire par compaction silencieuse « mid-research » ; navigateur instable ; tâches multi-étapes à relancer ; migration Discord d'une semaine.
- Masterclass : « if it's not written to a file, it doesn't exist » ; flush pré-compaction (memoryFlush) comme filet « best-effort » ; MEMORY.md sous 100 lignes ; revue hebdomadaire qui promeut les journaux vers MEMORY.md, à automatiser par cron ; incident d'un directeur chez Meta dont l'instruction « ne fais rien » donnée en chat a été perdue à la compaction, puis l'agent a supprimé des courriels.
- Ressemble à notre projet : Git nocturne, revue hebdo, rituel du lundi, heartbeat borné aux heures de veille, YouTube.
- Diffère : Discord multi-salons plutôt que Telegram ; mémoire Obsidian + QMD.
- Idées réutilisables : règles durables dans les fichiers et jamais seulement en chat ; cron de vérification que les crons tournent (déduit de l'échec silencieux de 7 jours) ; MEMORY.md < 100 lignes.

### Cas 11. Reed Whetstone (Purveyors), deux semaines en solo dev
- URL : https://www.purveyors.io/blog/two-weeks-with-ai-co-developer
- Auteur : Reed Whetstone. Date : 22 février 2026.
- Usage déclaré : 43 PR sur 3 dépôts, 16 journaux quotidiens ; données, tests, scraper, contenu de blog, brouillons LinkedIn.
- Structure : MEMORY.md ; memory/AAAA-MM-JJ.md ; brain/ zettelkasten (people/, projects/, ideas/, references/, tâches) ; AGENTS.md et SOUL.md « grew reactively ».
- Mémoire : revue hebdo de MEMORY.md ; fichiers de moins de « ~100 lines » ; règle « no mental notes » ; « memory does not self-correct », audits humains périodiques nécessaires.
- Canaux : Discord par salons (#scraper, #career, #blog, #purveyors) pour cloisonner le contexte.
- Problèmes déclarés : confabulation (« inventing a company name that sounds plausible »), erreurs spatiales, corrige les symptômes, sortie d'outils verbeuse qui chasse l'historique, mémoire périmée propagée dans les contenus, résultats faibles sur les messages humains, « output can outpace the human reviewer ».
- Ressemble : fiches projets et personnes en markdown ; revue hebdo.
- Diffère : orientation code.
- Idées réutilisables : audit humain périodique de la mémoire ; séparation des contextes par domaine.

### Cas 12. Aman Khan, agent utile et sécurisé
- URL : https://amankhan1.substack.com/p/how-to-make-your-openclaw-agent-useful
- Auteur : Aman Khan. Date : 17 février 2026.
- Structure (~/clawd/) : SOUL.md (« Be genuinely helpful, not performatively helpful »), AGENTS.md (mémoire, sécurité, groupes, workflow : planifier avant de construire, sous-agents pour le complexe, vérifier avant de dire « fait »), USER.md, MEMORY.md, BOOTSTRAP.md (n'a jamais tourné parce que le premier message était une vraie question).
- Mémoire : journaux puis curation ; « Remember what we just talked about for next time ».
- Proactivité : pas de cron ; watchdog toutes les 2 min sur la passerelle.
- Canaux : WhatsApp (déconnexions tous les quelques jours), Telegram en bêta.
- Modèles et coûts : Opus 4.6, ~10-15 $/jour avec pics > 30 $ ; Sonnet recommandé (2-5 $/jour).
- Sécurité : passerelle sur loopback, jeton, Tailscale Serve mais jamais Funnel, requireMention en groupe, défenses anti-injection dans AGENTS.md, ne pas installer sur la machine de travail.
- Problèmes déclarés : l'agent « fails silently » quand le plafond de dépense Anthropic est atteint.
- Idées réutilisables : prévoir une alerte explicite quand le plafond API est atteint (notre règle de plafond) ; règle « vérifier avant de marquer fait ».

### Cas 13. Oliver Henry, agent Larry (TikTok)
- URL : https://gameplaydev.substack.com/p/how-his-openclaw-agent-larry-got
- Auteur de l'article : anul agarwal ; développeur profilé : Oliver Henry. Date : 24 février 2026.
- Usage déclaré : diaporamas TikTok pour deux apps iOS ; Larry fait « 95% of the work » ; l'humain ajoute le son et publie (brouillons « SELF_ONLY » via Postiz).
- Mémoire : fichiers qui journalisent chaque post et sa performance, réutilisés pour les accroches ; journal d'échecs recommandé. « The agent is only as good as its memory. »
- Skill : fichier TikTok de 500+ lignes.
- Canaux : WhatsApp ; X (compte propre de l'agent). Modèles : gpt-image-1.5 pour les images, Claude pour l'agent.
- Résultats déclarés : 500K+ vues, 108 abonnés payants, ~0,50 $/post ; MRR annoncé de façon incohérente (714 $ puis 588 $).
- Problèmes déclarés : images locales « uncanny », cadrage, textes illisibles, format d'accroche à revoir.
- Ressemble : créateur de contenu, publication gardée par l'humain.
- Idées réutilisables : journal de performance par contenu ; journal d'échecs ; publication en brouillon privé puis validation humaine.

### Cas 14. hesamsheikh, Autonomous Project Management (STATE.yaml)
- URL : https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/autonomous-project-management.md (dépôt ~31,7k étoiles)
- Auteur : collection communautaire, inspirée de Nicholas Carlini. Date : 2026.
- Patron : un STATE.yaml par projet « serves as the single source of truth » (tâches avec id, status, owner, notes, blocked_by, next_actions) ; PROJECT_REGISTRY.md ; Git optionnel mais recommandé. Le principal est « coordinator ONLY » (0 à 2 appels d'outils), spawne des PM nommés pm-{projet}-{scope} qui peuvent spawner à leur tour.
- Déclaré : « File-based coordination scales better than message-passing. »
- Non traité : conflits d'écriture, état périmé, gestion des échecs (déduit).
- Ressemble : fiche d'état par projet, principal qui coordonne seulement.
- Idées réutilisables : champ next_actions et blocked_by dans nos fiches projets ; registre des projets.

### Cas 15. hesamsheikh, Project State Management (événements)
- URL : https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/project-state-management.md
- Patron : l'utilisateur raconte le travail en chat ; l'agent journalise des événements (progress, blocker, decision, pivot) avec contexte, dans une base SQL ; sous-agents pour la planification de sprint.
- Proactivité : cron 9 h qui scanne les commits Git des 24 h, les relie aux projets et poste un standup Discord (événements d'hier, plan du jour, bloqueurs).
- Déduit : dépend d'un signalement régulier par l'humain ; sinon l'état se périme comme un Kanban.
- Idées réutilisables : lier automatiquement les commits aux fiches projets ; typer les événements (décision, pivot, bloqueur).

### Cas 16. hesamsheikh, YouTube Content Pipeline
- URL : https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/youtube-content-pipeline.md
- Patron : cron horaire de veille (web, X) ; comparaison au catalogue des 90 derniers jours (YouTube Analytics via gog) ; dédoublonnage sémantique des pitches en SQLite ; pitch dans un sujet Telegram « video ideas » ; recherche déclenchée par un lien partagé sur Slack, carte Asana avec plan.
- Non précisé : modèle, seuil de similarité, coûts.
- Ressemble : chaîne YouTube avec veille d'idées et mémoire des sujets déjà couverts.
- Idées réutilisables : mémoire des sujets déjà traités pour éviter les doublons ; sujet Telegram dédié aux idées.

### Cas 17. Reza Rezvani, gist « 25 prompts » pour fondateurs
- URL : https://gist.github.com/alirezarezvani/bb68f4f7444fcab00beb6ded31eeb028
- Auteur : Reza Rezvani (CTO, fondateur). Date : mars 2026, actif jusqu'en septembre 2026.
- Contenu : prompts pour SOUL.md, IDENTITY.md, USER.md ; mémoire en dossiers daily, projects, decisions, people, lessons avec MEMORY.md index (« Files are your continuity ») ; moteur d'auto-amélioration (leçons après conversation, auto-audit du lundi, revue config mensuelle) ; 22 workflows dont journal des décisions automatique (avec options rejetées), digest hebdo du lundi (terminé, en cours, bloqué, « stalled »), suivi OKR du vendredi, pipeline de contenu avec détection de doublons ; heartbeat 30 min silencieux sauf action requise ; sécurité (courriel en lecture seule, portes d'approbation).
- Recommandation déclarée pour fondateurs : brief du matin + triage courriel + journal des décisions.
- Ressemble : journal/decisions.md, rituel du lundi, relance des projets « stalled ».
- Idées réutilisables : digest du lundi avec catégorie « stalled » ; décision consignée avec les options rejetées ; auto-audit mensuel de la configuration.

### Cas 18. ishwarjha, guide d'installation en 11 étapes
- URL : https://github.com/ishwarjha/openclaw-setup-guide-i-wish-i-had (37 étoiles)
- Date : vers mars 2026 (version 2026.3.13 citée).
- Structure : ~/.openclaw/workspace/ avec SOUL, USER, IDENTITY, AGENTS, TOOLS, HEARTBEAT ; memory/, samples/ (exemples d'écriture), templates/ ; secrets dans ~/.openclaw/.env.
- Mémoire : memorySearch expérimental, instantanés Git, openclaw backup create.
- Proactivité : heartbeat 30 min limité 8 h-22 h lisant HEARTBeat.md comme checklist ; crons : briefing 7 h, revue hebdo du lundi, rappel ponctuel, résumé Telegram nocturne.
- Multi-agent : agents « home » et « work » séparés, avec bindings.
- Modèles : Anthropic Sonnet principal, GPT-4o-mini puis Haiku en repli ; cache de prompt.
- Problèmes déclarés : passerelle, ports, jetons, boucles d'agent, configs corrompues qui « burns time and money ».
- Idées réutilisables : dossier samples/ d'exemples de style ; modèle de repli déclaré.

### Cas 19. Zach Hajjaj, « The Solo Founder's Secret Weapon »
- URL : https://dev.to/zach_hajjaj_260cfed0ef4b4/the-solo-founders-secret-weapon-how-openclaw-runs-the-business-you-dont-have-time-to-2lg3
- Date : 15 mars 2026.
- Usage : bot Telegram qui répond et escalade ; brief 9 h (issues GitHub, métriques) ; update investisseurs hebdo ; rétrospective mensuelle ; recherche pré-réunion ; revue de PR ; cron qui signale les contacts sans réponse depuis 7 jours et prépare des relances à approuver.
- Structure : SOUL.md (ton, escalade, « hard noes »), USER.md, MEMORY.md + memory/*.md.
- Problèmes déclarés : « doesn't replace judgment » ; besoin d'un réglage de 4 semaines.
- Idées réutilisables : relance automatique des contacts silencieux avec approbation (proche de notre relance des projets dormants).

### Cas 20. Atlas, « chief of staff » commercial (Remote OpenClaw)
- URL : https://dev.to/zacvibecodez/openclaw-atlas-the-ai-chief-of-staff-that-runs-your-35gl ; https://remoteopenclaw.com/blog/atlas-ai-chief-of-staff-openclaw
- Auteur : Remote OpenClaw (vendeur). Date : 2026, non précisée.
- Structure vendue : 8 fichiers (SOUL, IDENTITY, AGENTS avec règles de coordination avec Scout et Muse, HEARTBEAT, MEMORY avec budgets de tokens, TOOLS, BOOTSTRAP, README) ; 4 skills (outreach, lead gen, content, inbox triage).
- Proactivité : briefing 7 h, résumé 18 h avec cycle mémoire, relance après 48 h de silence selon réglages d'approbation.
- Mémoire : « self-cleaning » (archive les fils résolus, consolide en fin de journée).
- Caveat : page promotionnelle, chiffres non sourcés ; dépend du bon réglage des approbations.
- Idées réutilisables : seuil de relance par défaut (48 h) paramétrable ; portes de validation avant envoi.

### Cas 21. zac (Remote OpenClaw), équipes d'agents par rôle
- URL : https://dev.to/zacvibecodez/multi-agent-openclaw-how-to-set-up-agent-teams-for-32ik
- Date : février 2026.
- Patron : 3 agents (fondateur, ventes, opérations) avec soul/user/identity/memory chacun ; pas de mémoire partagée native, contournement par Google Sheet ; crons par rôle ; sandbox « restricted » et outils activés par rôle ; coût estimé 55-180 $/mois.
- Problèmes déclarés : un seul bot avec tout l'accès = compromission totale ; « context pollution » si mémoire partagée ; conflits de permissions.
- Idées réutilisables : permissions par département ; estimation de coût.

### Cas 22. Droptica, sauvegarde Git de la configuration
- URL : https://www.droptica.ai/blog/how-to-back-up-openclaw-configuration-with-git/
- Date : 18 février 2026.
- Versionné : SOUL, AGENTS, MEMORY, TOOLS, USER, config passerelle sans secrets, définitions de cron, skills/, scripts, README ; layout config/, workspace/, skills/, scripts/, crons/.
- Exclu : sessions/, logs/, browser-data/, media/, subagents/, node_modules/, .env, *.key, *.pem. « Credentials are the one thing you absolutely cannot commit to Git. »
- Automatisation : cron horaire rsync + commit + push si changements ; alternative via le cron OpenClaw.
- Problèmes déclarés : restauration manuelle des credentials, appairages, crons ; conflits sur MEMORY.md entre machines, « designate one machine as the primary for memory writes ».
- Idées réutilisables : liste d'exclusions .gitignore ; une seule machine écrit la mémoire.

### Cas 23. ClawHub, skill « subagent-architecture »
- URL : https://clawhub.ai/subagent-architecture (v2.3.0, 24 février 2026)
- Patron : orchestrateur qui estime le coût et demande approbation pour les spawns coûteux ; workers « leaf node » qui ne spawnent jamais ; ReviewerAgent dans un pipeline architecte, codeur, reviewer avec documents de passation ; rubrique qualité (score 7+) ; détection d'injection quelle que soit la confiance.
- Fichiers : subagents/[nom]/SPECIALIST.md, knowledge-base/, research/, _archived/ ; memory/intent-manifest.json ; notes/cost-tracking.md.
- Limites déclarées : communication « One-way only », pas de clarification en cours de tâche ; fin par timeout seulement ; journalisation des succès seulement.
- Idées réutilisables : fichier SPECIALIST.md par sous-agent (proche de nos briefs de département) ; rubrique de notation pour le validateur ; suivi de coût par spawn.

### Cas 24. Reddit r/AI_Agents, isolation des contenus clients
- URL : https://reddit.sentinel-team.org/posts/1s9dmqv/snapshots/2026-04-04T01%3A38%3A01.387914Z
- Auteur : u/ExoticYesterday8282. Date : capture avril 2026.
- Problème déclaré : contenus de plusieurs clients qui « cannot be isolated » et se mélangent. Commentaires : fichiers de workspace séparés par client (manuel) ; un commentateur soutient qu'un dossier n'isole rien car une instance « shares a context window » et recommande une instance d'agent par client.
- Ressemble : nos clients PME et nos marques à identités séparées.
- Idées réutilisables : décider explicitement du niveau d'isolation (dossier, session ou agent) par client.

### Cas 25. Paperclip (paperclipai/paperclip)
- URL : https://github.com/paperclipai/paperclip (~109,7k étoiles, MIT)
- Positionnement déclaré : « If OpenClaw is an employee, Paperclip is the company. » « Not an agent framework. »
- Fonctions : organigramme (rôles, lignes hiérarchiques, permissions, budgets), tâches liées à projets et objectifs avec verrou d'exécution, budgets par entreprise, agent et projet avec arrêt à la limite, heartbeats par file BD, routines cron ou webhook, étapes d'approbation, config versionnée et réversible.
- OpenClaw : traité comme agent HTTP/webhook, « uses those agents ».
- Limites déclarées : mémoire / connaissances en cours ; connecteurs chat « Experimental » ; télémétrie active par défaut ; Node 24.11+.
- Ressemble : notre idée d'ajouter Paperclip plus tard pour organigramme, tickets, budgets.
- Déduit : utile seulement quand plusieurs agents tournent en parallèle ; prématuré avec un seul CEO.

### Cas 26 à 31 (pertinence basse, résumés)
- Stanza, isolation de workspace (https://www.stanza.dev/courses/openclaw-production/multi-agent/openclaw-production-workspace-isolation) : un agent par client dans un cabinet de conseil ; mémoire privée par défaut ; « the first match wins » sur les bindings en conflit.
- Easton, guide pratique en français (https://eastondev.com/blog/fr/posts/ai/20260318-openclaw-practice-guide/, 18 mars 2026) : crons 8 h, vendredi, dimanche ; « Ce n'est pas magique du jour au lendemain » ; abandon fréquent dans les 1 à 3 premiers mois ; SELinux et appairage Telegram comme pièges.
- openclaw.ai/showcase (https://openclaw.ai/showcase) : @jdrhyne (flotte Discord, 10 000 courriels), @danpeguine (« Personal Operating System » sur WhatsApp), @avi_press (boucle WhatsApp ayant envoyé des milliers de messages à un contact : avertissement sur l'envoi automatisé), @xz3dev (SEO hebdo), @chrisrodz35 (résumés YouTube quotidiens).
- mergisi/awesome-openclaw-agents (https://github.com/mergisi/awesome-openclaw-agents) : 196 gabarits SOUL.md, dont Orion (coordination), Inbox, Minutes, Echo (contenu), Personal CRM ; memory-wiki/ « Karpathy-style ».
- garrytan/gbrain (https://github.com/garrytan/gbrain) : couche mémoire Postgres avec markdown Git canonique ; 66 crons déclarés ; « Markdown export is not a full database backup. »
- Stack Junkie (https://www.stack-junkie.com/blog/openclaw-workspace-architecture, 23 mars 2026) : limites bootstrapMaxChars 20 000 par fichier et 150 000 au total, fichiers trop longs « silently truncated » ; HEARTBEAT.md « Keep it under 50 lines » ; erreurs fréquentes (credentials dans le workspace, ton dans AGENTS.md, MEMORY.md chargé en groupe).

Références officielles consultées pour cadrer les cas :
- Multi-agent (https://docs.openclaw.ai/concepts/multi-agent) : un agent = workspace + profils d'auth + registre de modèles + sessions ; « Never reuse agentDir across agents » ; bindings canal vers agent ; skills par agent remplacent la liste par défaut au lieu de fusionner.
- AGENTS.default.md (https://docs.openclaw.ai/reference/AGENTS.default.md) : USER.md pour les préférences datées, MEMORY.md pour décisions et faits durables, journaux quotidiens ; « Backup tip: make the workspace a git repo » ; confirmer les actions destructrices non demandées.
- Guide sous-agents fast.io (https://fast.io/resources/openclaw-subagent-guide/, mis à jour 27 mai 2026) : maxSpawnDepth 1 à 5, maxConcurrent 8 par défaut, runTimeoutSeconds 0 par défaut (à fixer) ; les sous-agents reçoivent seulement AGENTS.md et TOOLS.md, pas SOUL.md ni MEMORY.md ; « Announce delivery is best-effort » ; fork « multiplies token consumption ».

---

## 4. Patrons récurrents observés

### 4.1 Structure du workspace
- Le noyau est presque toujours le même : AGENTS.md (règles d'exploitation), SOUL.md (ton et valeurs), USER.md (profil), IDENTITY.md, TOOLS.md (notes d'environnement), HEARTBEAT.md, MEMORY.md, BOOTSTRAP.md à supprimer après le premier démarrage (cas 1, 2, 3, 12, 18, 31, docs officielles).
- Séparation des rôles entre fichiers : règles dans AGENTS.md, ton dans SOUL.md, faits sur l'utilisateur dans USER.md ; mélanger est cité comme erreur (cas 31).
- Dossiers thématiques ajoutés par les usages avancés : memory/people, projects, decisions (cas 2, 11, 17) ; notes PARA ou zettelkasten (cas 3, 11) ; samples/ et templates/ (cas 18) ; skills/ maison (cas 9, 22).
- Taille : fichiers courts (< 100 lignes pour MEMORY.md, < 50 lignes pour HEARTBEAT.md) à cause des plafonds de chargement et du coût par heartbeat (cas 10, 11, 31).

### 4.2 Mémoire
- Modèle à deux niveaux quasi universel : journaux memory/AAAA-MM-JJ.md (brut, append-only) puis MEMORY.md curé (court, chargé au démarrage) (cas 1, 3, 4, 8, 10, 11, 12, 17, 18).
- Règle répétée partout : « no mental notes », tout ce qui doit survivre est écrit dans un fichier (cas 1, 10, 11, 17, jangwook).
- Promotion périodique : revue hebdomadaire ou cron qui monte les faits durables des journaux vers MEMORY.md (cas 1, 3, 8, 10, 17).
- Git comme sauvegarde ou source de vérité : commit nocturne ou horaire, secrets exclus (cas 1, 3, 4, 8, 10, 22, docs officielles). Deux auteurs vont plus loin : dépôt + Stow pour reconstruire une machine (cas 8), gitleaks en pre-commit (cas 3).
- Bases externes (SQLite FTS5, QMD, OpenViking, GBrain) apparaissent chez les utilisateurs intensifs, mais toujours en complément des fichiers markdown (cas 2, 8, 9, 10, 30).
- Index court chargé à chaque session (~200 tokens à 2k tokens) plutôt que la mémoire entière (cas 2, 3).

### 4.3 Projets et décisions
- Fichier d'état par projet avec propriétaire, statut, bloqueurs et prochaines actions (STATE.yaml, PROJECT_STATUS.md, memory/projects/) (cas 2, 5, 14).
- Journal des décisions append-only, parfois avec options rejetées (cas 5, 17, docs officielles).
- Catégorie « stalled » ou « bloqué » dans les bilans hebdomadaires (cas 17) et relance des contacts ou tâches silencieux après un seuil (7 jours cas 19, 48 h cas 20).

### 4.4 Délégation et sous-agents
- Le principal garde le contexte complet et le contact avec l'humain ; les spécialistes reçoivent un brief borné (cas 2, 3, 8, 23). Critère commun : déléguer si borné, long, parallélisable ; garder si personnel ou nécessitant tout le contexte (cas 2).
- Brief de délégation structuré : question, contexte, livrable, échéance, budget (cas 2) ; SPECIALIST.md par sous-agent (cas 23) ; sous-agents décrits dans l'AGENTS.md du parent (cas 8).
- Vérification de livraison par le principal : « I did it » ne suffit pas (cas 3) ; vérifier avant de marquer fait (cas 12).
- Reviewer ou validateur : présent comme rôle explicite seulement dans le cas 23 (ReviewerAgent, rubrique 7+) et le cas 8 (pr-reviewer) ; ailleurs, c'est l'humain qui valide avant publication (cas 6, 13, 10).
- Contrainte technique : les sous-agents ne reçoivent que AGENTS.md et TOOLS.md (fast.io) ; d'où l'intérêt de mettre le brief dans la tâche elle-même.
- Les équipes de plusieurs agents permanents utilisent soit un hub unique (cas 6), soit des fichiers partagés (cas 5, 14), soit rien du tout (cas 7, 21). La coordination par fichiers est la plus citée.

### 4.5 Rituels planifiés
- Briefing du matin (7 h à 9 h) et résumé du soir (17 h à 18 h) presque partout (cas 1, 5, 6, 7, 8, 18, 20, 21).
- Rituel du lundi (priorités, digest, auto-audit) et du vendredi (métriques, wrap-up) (cas 5, 6, 17, 18, 21, 27).
- Maintenance nocturne : mise à jour, sauvegarde Git, reconstruction d'index, compaction (cas 1, 3, 8, 10, 22).
- Heartbeat : borné aux heures de veille (8 h-22 h ou 7 h-23 h), silencieux si rien à signaler ; plusieurs auteurs le réduisent ou le remplacent par cron pour la fiabilité et le coût (cas 8, 9, 10, 18).

### 4.6 Canaux et modèles
- Telegram est le canal mobile dominant (cas 5, 6, 7, 8, 9, 16, 18, 19, 27) ; WhatsApp est abandonné par plusieurs à cause des déconnexions (cas 9, 10, 12).
- Discord par salons sert à cloisonner les contextes et à router un modèle par salon (cas 10, 11).
- Routage de modèles : modèle fort pour le principal et la recherche, modèle bon marché pour le routinier et les sous-agents (cas 5, 9, 10, 18, 21, jangwook).

### 4.7 Sécurité et contrôle
- Aucune action externe sans approbation (envoi de courriel, publication, actions destructrices) (cas 1, 10, 13, 17, 20, docs officielles).
- Courriel en lecture ou brouillon seulement (cas 10, 17).
- Compte Google ou machine dédiés à l'agent (cas 9, showcase @localghost).
- Méfiance envers les skills communautaires (cas 7, 9, fast.io).

---

## 5. Anti-patrons et problèmes rapportés

1. Instruction donnée seulement en chat puis perdue à la compaction ; l'agent a ensuite supprimé des courriels (cas 10, incident cité).
2. Cron qui échoue en silence pendant 7 jours après un renommage de commande (cas 10) ; agent qui se tait quand le plafond de dépense est atteint (cas 12).
3. Mémoire empoisonnée : un modèle bon marché hallucine, la mémoire enregistre, l'agent répète comme un fait ; 35+ entrées retirées à la main (cas 9). La mémoire « does not self-correct » (cas 11).
4. Heartbeat 30 min 24/7 : coût et exécutions irrégulières ; plusieurs auteurs le réduisent ou préfèrent cron (cas 7, 8, 9).
5. Trop d'agents permanents trop tôt : chief of staff « built but idle », builder inutilisé, tableau de bord coûteux à maintenir (cas 7) ; conseil répété « Start with one agent and one job » (cas 6) ou « Start with 2, not 4 » (cas 5).
6. Fichiers partagés écrits par plusieurs agents sans règle de verrou (cas 5, 14, déduit) ; conflits Git sur MEMORY.md entre machines (cas 22).
7. Contenus de clients différents mélangés dans un même contexte (cas 24) ; « context pollution » quand la mémoire est partagée entre rôles (cas 21).
8. L'agent modifie sa propre configuration et casse une fonction en silence (cas 9).
9. Boucle d'envoi automatisée qui a envoyé des milliers de messages WhatsApp à un contact (showcase @avi_press).
10. Fichiers de bootstrap trop longs tronqués en silence ; BOOTSTRAP.md laissé en place ; MEMORY.md chargé dans un groupe (cas 31).
11. Sortie d'outils verbeuse qui chasse l'historique utile ; relecture humaine dépassée par le volume produit (cas 11).
12. Période de calibrage de 2 à 4 semaines avec corrections lourdes ; abandon fréquent dans les 1 à 3 premiers mois (cas 19, 20, 27).
13. Brouillons nocturnes non publiables ; hallucinations à vérifier ; « Not set and forget » (cas 6).
14. Sous-agents : communication à sens unique, pas de clarification possible, timeout seul mécanisme d'arrêt, sous-agent bloqué qui garde un créneau de concurrence (cas 23, fast.io).

---

## 6. Idées réutilisables pour notre projet (numérotées, sans décision)

1. Garder MEMORY.md court (< 100 lignes) comme index, et promouvoir les faits durables depuis les journaux lors du rituel du lundi. Source : cas 10 (masterclass), cas 1, cas 3.
2. Écrire toute règle durable (dont « aucune action externe sans GO ») dans AGENTS.md et jamais seulement dans une conversation ; la compaction peut l'effacer. Source : cas 10, incident cité.
3. Ajouter dans chaque fiche projet les champs owner, status, blocked_by, next_actions et un seuil de dormance, puis une catégorie « stalled » dans le bilan du lundi. Source : cas 14, cas 17, cas 19.
4. Journal des décisions append-only avec les options rejetées et la raison. Source : cas 5 (DECISIONS.md), cas 17 (Workflow 5).
5. Gabarit de brief de délégation à cinq champs (question, contexte, livrable, échéance, budget) et critères explicites « garder au CEO / déléguer ». Source : cas 2.
6. Mettre le brief complet dans la tâche du sous-agent, car il ne reçoit que AGENTS.md et TOOLS.md, pas SOUL.md ni MEMORY.md. Source : fast.io (guide sous-agents).
7. Chaîne de vérification de livraison par le CEO : créer, exécuter, livrer, alerter ; « fait » n'est accepté que si Dominic a reçu le résultat. Source : cas 3, cas 12.
8. Validateur avec rubrique de notation (seuil 7 sur 10) et rapport de revue comme document de passation. Source : cas 23.
9. « Règle des 3 » : une correction répétée trois fois devient une leçon en mémoire longue ou une proposition de modification de SOUL.md soumise à approbation. Source : cas 3.
10. Cron hebdo de détection de contradictions entre mémoire récente et mémoire longue (« rumination »). Source : cas 3.
11. Préférer cron au heartbeat pour le filet du soir ; borner le heartbeat aux heures de veille et le réduire si le coût monte. Source : cas 8, cas 9, cas 18.
12. Cron de contrôle qui vérifie que les autres crons ont bien tourné et alerte sinon (déduit de l'échec silencieux de 7 jours). Source : cas 10.
13. Alerte explicite quand le plafond API Anthropic est atteint, sinon l'agent se tait. Source : cas 12.
14. Sauvegarde Git automatique (horaire ou nocturne) avec .gitignore strict (sessions, logs, .env, *.key) et gitleaks en pre-commit ; une seule machine écrit la mémoire. Source : cas 22, cas 3, cas 10.
15. Fichier de veille quotidien en lecture seule comme seul canal entre équipes, plutôt que des appels directs entre agents. Source : cas 6.
16. Trio de fichiers partagés GOALS / DECISIONS / PROJECT_STATUS si plusieurs sous-agents coexistent un jour. Source : cas 5.
17. Journal de performance par contenu publié (vues, accroche, format) et journal d'échecs, pour LeFiltre et la chaîne anonyme. Source : cas 13.
18. Mémoire des sujets déjà couverts avec dédoublonnage avant de pitcher une idée de vidéo ; sujet Telegram dédié aux idées. Source : cas 16.
19. Skill maison « création de contenu » avec flux nommés (thread, article, plan vidéo) et dossier samples/ d'exemples de style. Source : cas 9, cas 18.
20. Décider explicitement du niveau d'isolation par client et par marque (dossier, session ou agent séparé) ; un dossier seul n'isole pas le contexte. Source : cas 24, cas 26, docs multi-agent.
21. Compte Google et identités dédiés à l'agent plutôt que les comptes personnels. Source : cas 9, showcase @localghost.
22. Publication en brouillon privé (SELF_ONLY) puis validation humaine, pour toute sortie publique. Source : cas 13, cas 6.
23. Cron du vendredi « veille sur l'écosystème OpenClaw » et revue conjointe le dimanche ou le lundi (boucle kaizen). Source : cas 4.
24. Auto-audit anti-complaisance (« Sugar Problem ») dans le heartbeat ou le rituel du lundi. Source : cas 1.
25. Sortie standard « décisions en attente de Dominic » dans chaque bilan. Source : cas 7.
26. Démarrer avec un seul agent et un seul travail ; n'ajouter un département qu'après preuve d'usage. Source : cas 6, cas 5, cas 7.
27. Suivi de coût par délégation (notes/cost-tracking.md) et approbation requise au-delà d'un seuil. Source : cas 23.
28. Interdire à l'agent de modifier sa propre configuration sans GO. Source : cas 9.
29. Considérer Paperclip seulement quand plusieurs agents tournent en parallèle avec budgets ; prématuré avec un seul CEO (déduit). Source : cas 25.
30. Limiter les fichiers chargés au démarrage sous les plafonds (20 000 caractères par fichier, 150 000 au total) et garder HEARTBEAT.md sous 50 lignes. Source : cas 31.

---

## 7. Sources

Dépôts et gists
- https://github.com/Dithilli/openclaw-starter (2026)
- https://github.com/ucsandman/OpenClaw-Setup (2026) et https://raw.githubusercontent.com/ucsandman/OpenClaw-Setup/main/docs/agent-architecture.md
- https://github.com/kindomLee/openclaw-workspace-template (2026)
- https://github.com/ngcdan/openclaw-workspace (2026, structure seulement, sans README)
- https://github.com/ishwarjha/openclaw-setup-guide-i-wish-i-had (mars 2026)
- https://github.com/hesamsheikh/awesome-openclaw-usecases (2026) : usecases/autonomous-project-management.md, project-state-management.md, youtube-content-pipeline.md, multi-agent-team.md
- https://github.com/rohitg00/awesome-openclaw (2026)
- https://github.com/SebConejo/awesome-openclaw-resources (2026)
- https://github.com/mergisi/awesome-openclaw-agents (2026)
- https://github.com/garrytan/gbrain (2026)
- https://github.com/paperclipai/paperclip (2026)
- https://github.com/abhi1693/openclaw-mission-control (archivé le 6 août 2026, successeur wardn-ai)
- https://gist.github.com/velvet-shark/b4c6724c391f612c4de4e9a07b0a74b6 (actif jusqu'au 10 août 2026)
- https://gist.github.com/alirezarezvani/bb68f4f7444fcab00beb6ded31eeb028 (mars à septembre 2026)
- https://clawhub.ai/subagent-architecture (v2.3.0, 24 février 2026)

Articles et récits
- https://x.com/rsarver/status/2041148425366843500 via https://weekly-digest.pages.dev/content/2041148425366843500 (6 avril 2026)
- https://nervegna.substack.com/p/i-gave-my-openclaw-agent-an-org-chart (18 février 2026)
- https://readray.ai/en/notes/unreal-engine-5-20260221/building-10-agent-openclaw-team (15 février 2026)
- https://trilogyai.substack.com/p/openclaw-in-the-real-world (3 mars 2026)
- https://divingintogeneticsandgenomics.com/post/openclaw-ai-assistant-mac-mini-setup/ (6 avril 2026)
- https://velvetshark.com/50-days-with-openclaw (26 février 2026)
- https://velvetshark.com/openclaw-memory-masterclass (5 mars 2026)
- https://www.purveyors.io/blog/two-weeks-with-ai-co-developer (22 février 2026)
- https://amankhan1.substack.com/p/how-to-make-your-openclaw-agent-useful (17 février 2026)
- https://gameplaydev.substack.com/p/how-his-openclaw-agent-larry-got (24 février 2026)
- https://dev.to/zach_hajjaj_260cfed0ef4b4/the-solo-founders-secret-weapon-how-openclaw-runs-the-business-you-dont-have-time-to-2lg3 (15 mars 2026)
- https://dev.to/zacvibecodez/openclaw-atlas-the-ai-chief-of-staff-that-runs-your-35gl (2026, commercial)
- https://dev.to/zacvibecodez/ai-chief-of-staff-what-it-does-and-how-to-deploy-one-2e6p (avril 2026, commercial)
- https://dev.to/zacvibecodez/multi-agent-openclaw-how-to-set-up-agent-teams-for-32ik (février 2026, commercial)
- https://www.droptica.ai/blog/how-to-back-up-openclaw-configuration-with-git/ (18 février 2026)
- https://www.stack-junkie.com/blog/openclaw-workspace-architecture (23 mars 2026)
- https://jangwook.net/en/blog/en/openclaw-advanced-usage/ (5 février 2026)
- https://eastondev.com/blog/fr/posts/ai/20260318-openclaw-practice-guide/ (18 mars 2026)
- https://www.stanza.dev/courses/openclaw-production/multi-agent/openclaw-production-workspace-isolation (2026)
- https://reddit.sentinel-team.org/posts/1s9dmqv/snapshots/2026-04-04T01%3A38%3A01.387914Z (capture avril 2026)
- https://www.datacamp.com/blog/best-clawhub-skills (5 mars 2026)
- https://fast.io/resources/openclaw-subagent-guide/ (mis à jour 27 mai 2026, commercial)
- https://openclaw.ai/showcase (2026)
- https://podwise.ai/episodes/7376786 (Bankless, Nat Eliason, 4 mars 2026 ; résumé seulement)

Documentation officielle
- https://docs.openclaw.ai/concepts/multi-agent
- https://docs.openclaw.ai/reference/AGENTS.default.md
- https://docs.openclaw.ai/concepts/agent-workspace

Accès refusés ou vides (notés, non exploités)
- https://x.com/iamtrebuh/status/2011260468975771862 (robots.txt)
- https://raw.githubusercontent.com/Dithilli/openclaw-starter/main/AGENTS.md (erreur client)
- https://github.com/alvinreal/awesome-openclaw-tips (erreur client)
- https://lobehub.com/skills/openclaw-skills-project-tracker (erreur client)
- https://www.moltbook.com/post/eb8688c2-22f4-41f9-8fe8-08ff531732de (page sans contenu)
