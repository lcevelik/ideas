# Utopai Studios — Company & Process Research (2026)
*Compiled from multi-source research — 2026-11-01 · Category: research · Trigger: user shared 2 screenshots (AI-first studios thread; $1B banner claim)*

## Part 1: What the company is
- **Legal name:** Utopai Studios Inc. — AKA **Cybever** (former name). AI-native film + TV studio. [CONFIRMED: Crunchbase, Wikipedia]
- **HQ:** 215 Castro Street, 3rd floor, Mountain View, CA 94041 [CONFIRMED: official site]
- **Founded:** 2022 as **Cybever** (AI research: procedural content generation, high-fidelity 3D environments for gaming & previsualization) → rebranded/repositioned as **Utopai Studios** in 2025 as a film/TV studio built around AI-enabled production. [Wikipedia says 2025 rebrand; company "history" page ties evolution to same era; Forbes May 2026 confirms she rebranded Cybever. Slight date ambiguity 2025 vs 2026 — marked AMBIGUOUS]
- **Founders:** **Cecilia Shen** (CEO, ex-Google engineer; Forbes May 21 2026: "Meet the 25-Year-Old Vying to Become Hollywood's First AI Movie Mogul") and **Jie Yang** (CTO; ex-Google Research/Google X → founding team + Head of Research at **Mineral**, the Alphabet moonshot; met Shen while building Mineral). [CONFIRMED: official bios, Forbes, Crunchbase]
- **Scale:** ~73 employees, distributed across 9 countries (US, Spain, Canada, Andorra, Chile, + Korea studio, Japan activity, South America). Crunchbase: 51–100 band, 68 employee profiles. [CONFIRMED: LinkedIn/Crunchbase]
- **Positioning:** "AI-native, full-stack studio — our own tech, our own production, our own IP development" (COO Kevin Chong, KVIFF Industry fireside, July 2026, moderated by Deadline). "Silicon Valley DNA and Hollywood DNA." [CONFIRMED: Cineuropa]

### Leadership
- Cecilia Shen — Co-founder & CEO (vision, strategy, studio)
- Jie Yang — Co-founder & CTO (technical vision, research, engineering)
- Zijian He — Chief Scientific Officer (AI research, foundation models)
- Kevin Chong — COO (business ops, strategy, partnerships, global expansion)
- Manuel Calero — VP Applied AI & Generative Systems; Javier Moreno — Sr Product Design Lead; Linjia Wu — Head of Strategic IPs; Phil Sawyer — Chief of Staff [CONFIRMED: official About page]

## Part 2: Funding
- **Oct 2026:** $60M raised at **$1B valuation** (The Information / Dealroom reported, Oct 22–25 2026). Follow-on interest reported at **$2–3B**. [CONFIRMED as press-reported; NOT from filings — treat as reported, not audited]
- Crunchbase shows 2 rounds (Venture–Series Unknown obfuscated + Series A obfuscated). Earlier round amounts not publicly clear — UNVERIFIED.
- $1B = "highest independently reported valuation for a pure-play AI-native film/TV studio" (Variety). Trade framing: as a tool vendor $1B would be hard to defend; as an entertainment company owning model + planning system + theatrical distribution + IP library, the logic changes (QbitAI/company-supplied feature).

## Part 3: Technology stack
### PAI — Production Assistive Intelligence (platform, launched all-new 2026-09-30)
- Human-directed **production engine**: workspace connecting screenplay ↔ assets ↔ shots ↔ takes ↔ versions. [CONFIRMED: official docs]
- **Production Agents (PAs / "PA"):** AI agents that execute defined production tasks under filmmaker direction — script analysis (characters, locations, props, beats), structured scene breakdowns, multi-view character sheets, shot lists, keyframe options, dialogue-timing preview, multi-shot assembly.
- **Human checkpoints (governance):** PA **cannot authorize a paid render** (every costed job needs explicit approval); PA **cannot alter an existing asset** without a specific edit instruction; PA does not determine story meaning — humans own intent. Comments/approvals attach to specific versions; generation **provenance record** for IP tracing (enterprise). [CONFIRMED: official + SoaplandTV analysis]
- **Reference system:** approved character sheets (front/profile/back/close-up) + location images become locked continuity references reused in later shots; variants (injured version, costume change) build on approved base; "smart reference routing" to scenes. Drift is still human-reviewed.
- **PAI Code:** developer/agent interface — CLI workflows exposing PAI's script/image/video/voice skills into coding agents the team already uses (**Claude Code, Codex** named), local canvas + timeline, usage-based billing. [CONFIRMED: official About — notable: agentic/CLI-native pipeline]
- **Exit path:** selected footage leaves PAI for **Adobe Premiere Pro / DaVinci Resolve** — finishing stays a human-run stage outside the platform (kinovela analysis: "the project file as last human gate").
- Enterprise: private deployment, custom models, workflows built around a client's production (B2B channel).

### Utopai X (video model, launched 2026-09-30)
- **Post-trained on MiniMax H3** — disclosed in official FAQ. [CONFIRMED]
- Debuted **#2 globally** on Artificial Analysis Text-to-Video Leaderboard **With Audio** (Elo 1,150, Sep 29 2026) — 9 pts behind **Wan 3.0**, ahead of Dreamina Seedance 2.5; **highest-ranked US-company model**; also highest of the three MiniMax H3-based models (ahead of H3 itself and fal's H3 Max). [CONFIRMED: Artificial Analysis official post]
- Category ranks (AA launch analysis Sep 30): **#1 Physics, #1 Audio Synchronization**; #2 Lighting & Materials, #2 Camera Control. Stated strengths: reflections, caustics, world understanding, high-level spatial consistency. [CONFIRMED]
- Consumed **inside PAI only**, 93 PAI credits/sec of video, **no public API**; PAI plans from $15/mo (1,000 credits). Blind human-preference evaluation method. [CONFIRMED: AA + official pricing FAQ]
- Proprietary **script-analysis model** inside PAI's agentic layer (2026). [CONFIRMED: official about]

## Part 4: THE PROCESS (workflow, end to end)
Evidence: official "What is PAI" blog, PAI product page, Matias Collado "Super Powers" case study, kinovela analysis.
1. **Script intake** — screenplay written **outside** the tool by the filmmaker, then loaded; PA analyzes → identifies characters/locations/props/story beats → proposes **structured scene breakdown**. Human reviews structure + visual direction first.
2. **World/character locking** — PA generates multi-view **character sheets** (front/profile/back/close-up) from description; filmmaker approves anatomy/face/costume; approval makes it the reusable reference for all later shots. Same for locations (architecture, lighting, color, props).
3. **Shot planning** — PA breaks approved scenes into shots, maintains shot list + notes; script edits propagate everywhere automatically ("every time I changed something, it updated it everywhere" — Collado); camera/performance rules persist as project context.
4. **Generation** — filmmaker directs in plain language; PA produces variations (camera angles, staging, performance options); **Utopai X** (or other selected models — model choice stays with filmmaker) generates takes from text prompts; takes remain **associated with the shot** for comparison.
5. **Review & versioning** — compare new takes vs earlier versions vs references; previous takes always retrievable; approvals/comments pinned to versions; provenance log of generations.
6. **Edit** — assemble on PAI's internal timeline for review; **selected footage exports to DaVinci Resolve / Premiere Pro** for the real edit (Collado's rough first cut: ~4 days working alone).
7. **Loop** — revisions preserve earlier approved decisions (no rebuild-from-scratch); paid renders explicitly authorized per job.

**Case proof:** *Super Powers* (Matias Collado, superhero comedy) — one person, screenplay → PAI breakdown/refs/generation → Resolve edit, first cut in ~4 days. In use on *The Most Serious Fart* (animated feature, Mike Bender).

**Compounding asset thesis:** developing the model **inside** an operating studio gives research/product teams the decisions behind the footage — references established, takes selected, changes requested → production data + expert feedback flywheel (Variety + kinovela framing).

## Part 5: Studio business (slate, distribution, money)
- **Model:** develops + finances + produces original projects AND co-productions; also sells tech (PAI SaaS credits + enterprise deployments). Three revenue layers: IP/slate, technology subscriptions, production services.
- **Slate:** ~25 projects; 2027 releases: **Cortés** (Banderas as Hernán Cortés; dir. Paul Mignot; screenplay Nicholas Kazan; prod. with Ex Machina Studios/Marco Weber; Paris shoot + Spain), **Half Moon** (Korean-German arthouse, dir. Hyo-joo Yang, Berlin Silver Bear winner — first release, expected Feb 2027), **The Most Serious Fart** (AI-assisted animated, Mike Bender); series **Space Nation** (sold to Globoplay + German broadcaster at prices benchmarked to non-AI content — "first AI series to strike deals with traditional rights buyers") and **Journey to the West: The Lost Five Hundred Years** (Huace co-pro, fully AI-generated).
- **Distribution:** ~3,000 North American screens target; Utopai leads sales US/Canada/Asia/MEA; Ex Machina leads Europe/LatAm/AUS/NZ (Cortés). Forbes-cited: Cortés + Space Nation presales potential **up to $110M** (subject to delivery). [press-reported]
- **Footprint:** Mountain View HQ, LA team, **production team in Spain**, acquired **Korea** studio (IP), **Japan** co-pro/anime activity, South America team.
- **Strategy quote (COO Chong, KVIFF):** "We didn't start by asking 'How can we make AI movies?' … 'How can we help storytellers make the best possible movies, and bigger ones?'" — restore the scale of mid-to-high-budget filmmaking at much lower cost.

## Part 6: Competitive context (from user's screenshots, verified)
- **Promise** (LA): AI-native studio, **MUSE** studio OS; backers Peter Chernin's North Road + a16z + Google AI Futures Fund + Crossbeam (Michael Ovitz) etc.; **Disney 2026 Accelerator** member (official, Jul 31 2026); runs pavilion by Sony Culver City lot; founders George Strompolos (CEO) + Clark (CCO) + Caleb Ward (Curious Refuge, 170+ countries AI film school); first feature NinjaPunk. [CONFIRMED]
- **Primordial Soup:** Darren Aronofsky + Dylan Golden (CEOs), founded 2025, **Google DeepMind** partner shorts program; SEC filing seeking $15M, **$11M+ equity sold** (Oct 2026). [CONFIRMED: Variety/SEC]
- **Asteria:** Natasha Lyonne, directing debut *Uncanny Valley* on a "clean" AI model — NOT independently verified this pass. [UNVERIFIED]
- **80–90% CGI/scene-planning cost cut** = Utopai self-reported + author anecdote; no independent audit. [MARKETING CLAIM]

## Part 7: Relevance to Prague Studios AI proposal (VP hybrid unit)
- The post author's profile ("hybrid AI production unit inside an established producer") = the proposal's own positioning — external validation of Route A/B framing.
- Utopai proves market cap: $1B in ~4 years for AI-native full-stack; Promise shows incumbents (Disney) funneling AI studios through accelerator; Aronofsky shows talent-led $11M entrants — the tier map frames a €880K–1.56M **hybrid VP capability layer** as the missing middle.
- PAI's design principles map onto HIVE/VP: locked references = their continuity solution ↔ proposal's "locked world" (Route B); PAI Code/Claude Code integration validates agentic pipeline approach; "finishing stays human" aligns with VP supervised model.
- 80-90% cost claim + "price both ways" post = ammunition for §2/§4 budget comparison.

## Sources
- Official: utopaistudios.com/en/pai, /en/blog/what-is-pai, /en/blog/introducing-the-all-new-pai-and-utopai-x, /en/pai/utopai-x, /en/blog/utopai-x-image-to-video-audio-ranking, /en/about, /en/faq, /en/blog/super-powers-matias-collado-pai-case-study
- Variety: worlds-largest-independent-ai-native-film-tv-studio (…-1236844803), antonio-banderas-ai-powered-feature-cortes (…-1236908962), darren-aronofsky-ai-primordial-soup-raising-15-million (…-1236818544)
- Artificial Analysis: x.com/ArtificialAnlys/status/2105343643251032304 (MiniMax H3 base, Elo, pricing)
- Cineuropa: /en/newsdetail/493268 (KVIFF full-stack talk)
- kinovela.com/insights/utopai-pai-production-intelligence-2026 (PAI launch analysis, data flywheel, Premiere/Resolve exit)
- Disney: sites.disney.com/accelerator/announcement (2026 participants incl. Promise)
- TheWrap: promise-ai-studio-investors-google-michael-ovitz; Guardian: directors-embracing-ai-film-making
- Forbes: meet-the-25-year-old-vying-to-become-hollywoods-first-ai-movie-mogul (2026-05-21)
- Crunchbase: organization/utopai-studios (Cybever AKA, 2 rounds, people); Wikipedia: Utopai_Studios
- Dealroom: primordial-soup; digitaltoday.co.kr ($60M @ $1B round report)
- User screenshots (2026-11-01): LinkedIn post thread — "price both ways / AI into development first / train your team" + article (Utopai/Promise/Primordial Soup/Asteria, 80-90% cost, closing "edge is narrowing")