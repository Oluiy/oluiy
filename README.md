## Hi there 👋 Israel here!

**Backend-heavy Software Engineer** · C#/.NET first, plus TypeScript and Python · Lagos, Nigeria 🇳🇬

I build the parts of software that aren't glamorous but have to work: APIs that don't fall over, backups that actually restore when it matters. I try to write code that's still easy to follow six months later, and I'm just as comfortable in React and Tailwind when something needs a face, not just an engine.

But I don't just write or debug code. **I design the system.** What actually matters to me is the decision behind it: why a boundary sits where it does, what happens at 3am when a provider goes down, whether the next person to touch this curses my name or not. A lot of what I've shipped lately came from hitting the same failure twice. A bill nobody caught in time. A restore nobody had actually tried. Boilerplate I'd typed out by hand once too often. Each one turned into something I only had to build once.

---

### 🔭 Right now

C#/.NET is home base, the one I'm strongest in. TypeScript and Python come right after, I reach for them without thinking about it. I'm also **picking up Rust and Go**, and the only way that's ever actually stuck for me is building something real in it, something unforgiving enough that I can't fake my way through. That's what db-guard did for Rust. FitGuard did the same for Go. Both are below, and both do work I'd trust with someone else's data.

Beyond that, I'm going deeper on **distributed systems** and the practical side of running LLMs in production.

---

### 🚀 Open source I've shipped

#### 🛡️ [FitGuard](https://github.com/Oluiy/ai-cost-guard) · Go

[![npm](https://img.shields.io/npm/v/fitguard.svg?label=npm)](https://www.npmjs.com/package/fitguard)
[![Downloads](https://img.shields.io/npm/dm/fitguard.svg)](https://www.npmjs.com/package/fitguard)
[![License: Apache](https://img.shields.io/badge/License-Apache-yellow.svg)](https://github.com/Oluiy/ai-cost-guard/blob/main/LICENSE)

A **self-hostable, OpenAI-compatible AI gateway** that stops runaway LLM bills *before* they happen: the kind of incident where a stuck loop or an unbounded `max_tokens` turns into an $8K bill. It's a single Go binary with no required dependencies: change one `baseURL` and your existing OpenAI/Anthropic/Gemini/Groq/Together SDK calls keep working, now with guardrails.

- **Per-user budgets, enforced properly.** Worst-case cost gets reserved before the request goes upstream, so two requests firing at once can't both squeak under the limit. It's tied to the API key, not to whatever the caller claims about itself.
- **Caching that only fires on exact matches.** No semantic fuzziness, no surprises: same prompt inside the TTL costs $0.
- Flags a `finish_reason` of `length`, usually the first sign something's stuck in a loop.
- Falls back automatically if a provider errors out or rate-limits you, before the client ever notices.
- Every request lands in SQLite: user, cost, tokens, cache hit, never the prompt itself. Dashboard shows it live.
- Streaming, embeddings, vision, tool calls: all of it works through the cache and the fallback path too, not just plain chat.

📖 [Documentation](https://ai-cost-guard-ruddy.vercel.app/) · `npm install -g fitguard` or `go install github.com/Oluiy/ai-cost-guard/cmd/fitguard@latest`

#### 🏗️ [BuildQuickPkg](https://github.com/Oluiy/build-quick-aspnet) · C# / .NET

[![NuGet](https://img.shields.io/nuget/v/BuildQuickPkg.svg)](https://www.nuget.org/packages/BuildQuickPkg)
[![Downloads](https://img.shields.io/nuget/dt/BuildQuickPkg.svg)](https://www.nuget.org/packages/BuildQuickPkg)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/Oluiy/build-quick-aspnet/blob/main/LICENSE)

An interactive .NET CLI that scaffolds a complete **Clean Architecture** ASP.NET Core solution (API, Application, Domain, and optionally Infrastructure), already wired up, testable, and building in seconds. `dotnet new` gives you an empty folder. This gives you something that actually runs.

- 3-layer or 4-layer architecture, Minimal API or Controllers + Services: all real interactive choices, not one baked-in opinion.
- **Monolith or microservices**, with as many named services as you want.
- Optional EF Core (with a generated `DbContext` and repository/unit-of-work templates for transactional work), JWT auth, Docker + `docker-compose`, Serilog structured logging, and xUnit + `WebApplicationFactory` integration tests.
- Per-environment `appsettings` done properly: a committable dev config, and a production config with secrets deliberately left blank for env vars or a secret manager.

📖 [Documentation](https://oluiy.github.io/build-quick-aspnet/) · `dotnet tool install --global BuildQuickPkg`

#### 🦀 db-guard · Rust *(source private pending release; happy to walk through it on a call)*

**A backup nobody has ever restored is a rumour.** db-guard turns it into a checked fact: it pulls your latest dump, restores it into a throwaway Docker container, counts the tables and rows that came back, and tells you — on your terminal, in Slack, in Telegram, and in a local audit log — whether it actually worked.

- Supports **PostgreSQL, MySQL, SQLite, MongoDB, and Redis**, each through a shared `Verifier` trait.
- A restore only passes if *all* of it holds: the restore tool exits cleanly, the database actually has tables, those tables actually have rows, and the backup is newer than your `max_age`. A pristine three-week-old dump is still a three-week-old dump.
- SQLite additionally runs `PRAGMA integrity_check`, since there's no container to check for it. Redis is the odd one out: there's no restore command, an RDB file just loads once, automatically, the moment `redis-server` starts, so a corrupted one shows up as the container crashing on that first load. Early on I had that crash lumped in with "Docker itself isn't working," which is a different problem with a different fix. Caught it and split the two apart. Every engine here got tested against real containers with dumps I deliberately corrupted myself, not mocked ones.
- `verify --json` exits non-zero on a real failure, so it drops straight into CI to fail a deploy the moment backups stop being restorable. Plus `init`, `doctor`, `history`, cron scheduling, and multi-database watching via `--config-dir`.

#### 🧩 buildquick · TypeScript *(the cross-stack successor to BuildQuickPkg; private for now)*

Same idea as BuildQuickPkg above, generalised beyond .NET: **one interactive scaffolder, any backend stack**. Pick ASP.NET Core or Spring Boot (Rust/Axum planned), pick Multi-Module, Package-by-Layer, or Package-by-Feature, pick your add-ons, and get a solution that builds and runs immediately, Maven Wrapper and all.

Each stack is a self-contained `StackAdapter`, and the interactive engine knows nothing about C#, Java, or Rust specifically. Adding a language just means writing a new adapter. I did it that way on purpose, I didn't want the engine tied to any one language's assumptions.

---

### 💼 Where I've worked

**Backend Engineer, BLYKN** · *April 2026 – Present · Remote*
Leading the modernization of core legacy systems: upgrading outdated frameworks to current standards, introducing containerization for faster and safer deployments, and rewriting complex legacy code into readable, efficient logic that the team can actually maintain and extend. Built a cloud-based messaging layer connecting independent services, keeping the platform stable under heavy load.

**Product Manager & Software Engineer, Imaginarium Marketing Communications** · *March 2026 – Present · Hybrid, Lagos*
Applying Jobs-to-Be-Done and MoSCoW prioritization alongside user interviews to map real user friction into a product roadmap. Engineered an internal analytics tool that automated client campaign tracking, replacing manual monitoring with real-time performance insights.

**Independent engineering & open source** · *2024 – Present*
Alongside my degree, I spent this stretch building backend systems for early-stage products, and turning recurring headaches into tools I could stop rebuilding: a scaffolder, once I'd retyped the same folder structure one too many times. A gateway, after an LLM bill got away from me. A backup verifier, once I realised nobody on any team I'd worked with had ever actually tried restoring one. Everything above came out of this stretch.

**Game Research Analyst Intern, Extern (Mobalytics)** · *April 2024 – May 2024*
Data analytics and reporting on player engagement trends; research and findings delivered to program leadership, contributing to a measurable shift toward more action/combat-oriented content.

---

### 🏆 Things I'm proud of

- **Cut projected infrastructure spend by 60% at BLYKN**: architected a hybrid-cloud microservice setup on DigitalOcean Kubernetes plus Azure Service Bus rather than a full Azure deployment, and held a production-grade SLA doing it. The 60% is measured against the costed-out all-Azure alternative, not against a previous bill.
- **Designed a budget system that survives concurrency**: FitGuard reserves each request's worst-case cost *before* the upstream call, so simultaneous requests can't jointly blow through a limit. Getting that right is the difference between a spending cap and a suggestion.
- **Published tools people can actually install**: [BuildQuickPkg](https://www.nuget.org/packages/BuildQuickPkg) on NuGet (v1.1.0, seven releases) and [FitGuard](https://www.npmjs.com/package/fitguard) on npm, each with a documentation site instead of a lonely README.

---

### 🧑🏾‍💻 Tech stack & skills

**Languages:** C# (primary), TypeScript, Python, JavaScript, SQL, C · *currently learning:* Rust, Go

**Backend:** ASP.NET Core / .NET 8–9, Node.js, NestJS, Express, Fastify, FastAPI

**Data:** PostgreSQL, MySQL, MongoDB, Redis, SQLite, EF Core, Prisma ORM

**Cloud & DevOps:** Docker, Kubernetes, Azure (Service Bus, App Services), DigitalOcean (Kubernetes, Spaces), AWS, Cloudflare, GitHub Actions, cert-manager, Git/GitHub

**Architecture:** Distributed systems, event-driven messaging, Clean Architecture, systems design, database design, API design & documentation

**Frontend:** React.js, Tailwind CSS, Motion, HTML5, CSS3, Bootstrap

**AI/ML:** LLM gateways & cost control, NLP/LLM integration, emotion detection, prompt engineering

---

### 📌 Earlier projects

Where I cut my teeth. Smaller, but each one taught me something I still use.

| Project | Description | Tech |
|---|---|---|
| [prayerforce_bot](https://github.com/Oluiy/prayerforce_bot) | Telegram bot automating a prayer group's daily rhythm | Python |
| [MiniAssetManagementProj](https://github.com/Oluiy/MiniAssetManagementProj) | Asset management system | TypeScript |
| [ECommerceAPI](https://github.com/Oluiy/ECommerceAPI) | E-commerce REST API covering the full order lifecycle | C#/.NET |
| [ECommerceMVC](https://github.com/Oluiy/ECommerceMVC) | E-commerce app built on the MVC pattern | C#/.NET |
| [TASKMGM](https://github.com/Oluiy/TASKMGM) | Task management APIs | C#/.NET |
| [Emotion_detection_Application](https://github.com/Oluiy/Emotion_detection_Application) | AI-powered emotion detection from images | Python, AI |
| [Books-store](https://github.com/Oluiy/Books-store) | Full-stack bookstore app | MERN Stack |
| [VendorInfoSys](https://github.com/Oluiy/VendorInfoSys) | Vendor directory for a university campus | JavaScript |
| [mindease-ai](https://github.com/Oluiy/mindease-ai) | AI mental health companion frontend | JavaScript |
| [Library_And_TodoConsoleApp](https://github.com/Oluiy/Library_And_TodoConsoleApp) | Library & Todo console apps, OOP principles end to end | C#/.NET |
| [fastapi-book-project](https://github.com/Oluiy/fastapi-book-project) | HNG12 DevOps × Backend stage project | Python, FastAPI |

---

### 🎓 Education & certifications

**B.Sc. Computer Science**, Covenant University, Ota, Nigeria · *September 2023 – Present*

- Docker Foundations Professional Certificate
- Ubuntu Linux Professional Certificate
- Prompt Engineering for Large Language Models (LLMs) · OBTranslate

---

### ✍️ Writing

I write up what I learn along the way, mostly the decisions behind the stuff above, and the handful of things I only really understood once I had to explain them to someone else.

[![Substack](https://img.shields.io/badge/Substack-iyanuoluwaubuntu-FF6719?logo=substack&logoColor=white)](https://substack.com/@iyanuoluwaubuntu)
[![Medium](https://img.shields.io/badge/Medium-@akinboyewaiyanuoluwa15-000000?logo=medium&logoColor=white)](https://medium.com/@akinboyewaiyanuoluwa15)

---

### 📫 Let's talk

If you're building something backend-heavy, fighting an LLM bill, or wondering whether your backups actually restore, I'd genuinely like to hear about it.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-israel--akinboyewa-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/israel-akinboyewa)
[![Email](https://img.shields.io/badge/Email-akinboyewaiyanuoluwa15@gmail.com-EA4335?logo=gmail&logoColor=white)](mailto:akinboyewaiyanuoluwa15@gmail.com)

😇 Always learning, always building 🚀

---

![Oluiy's Stats](https://github-readme-stats.vercel.app/api?username=Oluiy&theme=vue-dark&show_icons=true&hide_border=true)
![Oluiy's Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=Oluiy&theme=vue-dark&show_icons=true&hide_border=true&layout=compact)