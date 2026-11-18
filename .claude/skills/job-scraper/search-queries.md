# Search Queries for Job Scraper

<!-- Populated by /setup on 2026-11-18 for Luis Enrique Garcia Moreno (Berlin, MSc AI student,
working-student roles now, full-time after Nov 2027). -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. None ship with AI Job Search: copy the ones for your market from [community-portals](https://github.com/ai-job-search-dev/community-portals), or build one with `/add-portal`. You do **not** need a matching `site:` line below for those CLIs to run.

For this candidate: of the boards he uses, only **LinkedIn** has a community portal (`portals/global/linkedin-search`, checked 2026-11-18). StepStone, Indeed, JobTeaser and Berlin Startup Jobs have no portal yet, so they run through the WebSearch `site:` fallback below (or build one with `/add-portal`).

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** queries are written in English (working language, C1) and German (Berlin market; many working-student titles are German, e.g. "Werkstudent"). Spanish (native) and French (A2) are declared but not searched by default, since the target market is Berlin/Germany; add Spanish queries if you want Spanish-speaking-market roles. Per `04-job-evaluation.md`, a posting requiring German at C1 or above is a FAIL for this candidate (his explicit deal-breaker).

## Search Sites

Primary:
- **linkedin.com/jobs** - LinkedIn (filter: Berlin, Germany / Remote in Germany); community `linkedin-search` CLI once installed
- **stepstone.de** - StepStone, Germany's largest general job board
- **de.indeed.com** - Indeed Germany
- **jobteaser.com** - JobTeaser (BSBI student career portal; may require login, so WebSearch coverage is partial)
- **berlinstartupjobs.com** - Berlin Startup Jobs

Secondary (company career pages via Google):
- SAP, Siemens, Garmin, Google, Amazon / AWS / Audible, TikTok, Netflix, Spotify, Strava, GitHub, Delivery Hero, Wolt, GetYourGuide, eBay, Kleinanzeigen, Zalando, Apple, Databricks, Bolt, SumUp

## Query Categories

Each priority has two kinds of block. **Plain terms** blocks are what the portal CLIs consume: one term per line, one CLI call per term, no `OR`, no `site:` prefix, and no location (the CLI's location flag carries it). **WebSearch-only** blocks are Google syntax for the fallback path and are never passed to a CLI.

### Priority 1: Product management and agile delivery (working student now, PM later)

Owning or supporting digital products: backlog, user stories, release and go-live coordination.

#### Plain terms (portal CLIs)

```
Working Student Product Management
Werkstudent Product Management
Working Student Product Owner
Werkstudent Produktmanagement
Working Student Agile Project Support
Werkstudent Projektmanagement
Associate Product Manager
Junior Product Manager
AI Product Manager
Product Owner
```

#### WebSearch-only (`site:` fallback)

```
site:linkedin.com/jobs "Working Student" "Product" Berlin
site:stepstone.de "Werkstudent" "Product Management" Berlin
site:stepstone.de "Werkstudent" "Produktmanagement" Berlin
site:de.indeed.com "Werkstudent" "Product Owner" Berlin
site:berlinstartupjobs.com "working student" product
site:jobteaser.com "Working Student" "Product" Berlin
site:linkedin.com/jobs "Working Student" "Product" Remote Germany
```

### Priority 2: Digital project management, product operations and e-commerce

Digital transformation, B2B e-commerce and platform operations (direct domain experience).

#### Plain terms (portal CLIs)

```
Working Student Product Operations
Werkstudent Product Operations
Working Student Digital Project Management
Werkstudent Digitalisierung
Working Student E-Commerce
Werkstudent E-Commerce
E-Commerce Manager
Product Operations
Digital Project Manager
```

#### WebSearch-only (`site:` fallback)

```
site:stepstone.de "Werkstudent" "E-Commerce" Berlin
site:stepstone.de "Werkstudent" "Digitalisierung" Berlin
site:linkedin.com/jobs "Product Operations" "Working Student" Berlin
site:berlinstartupjobs.com "product operations"
site:de.indeed.com "Werkstudent" "Projektmanagement" Remote
```

### Priority 3: Business analysis, AI and solutions consulting (adjacent)

Roles that use the business-to-tech translation and the MSc AI.

#### Plain terms (portal CLIs)

```
Working Student Business Analyst
Werkstudent Business Analyst
Working Student AI
Werkstudent Künstliche Intelligenz
Working Student Solutions Consultant
Werkstudent Data Analytics
Business Analyst
Solutions Consultant
```

#### WebSearch-only (`site:` fallback)

```
site:linkedin.com/jobs "Working Student" "Business Analyst" Berlin
site:stepstone.de "Werkstudent" "Business Analyst" Berlin
site:linkedin.com/jobs "Working Student" "AI" Berlin
site:berlinstartupjobs.com "working student" AI
site:linkedin.com/jobs "Solutions Consultant" "Working Student" Germany
```

### Priority 4: Target companies and sector focus (sports tech, fintech)

Wider net through target employers' own career pages.

#### Plain terms (portal CLIs)

```
Working Student Sports Tech
Working Student Fintech
Werkstudent Fintech
```

#### WebSearch-only (`site:` fallback)

```
site:jobs.sap.com "Working Student" Berlin
site:jobs.siemens.com "Working Student" Berlin
site:careers.garmin.com Germany
site:jobs.zalando.com "Working Student"
site:careers.deliveryhero.com "Working Student" Berlin
site:getyourguide.careers "Working Student"
site:sumup.com/careers Berlin
site:boards.greenhouse.io "Working Student" Berlin (Product OR "Business Analyst")
"Working Student" (Strava OR Spotify OR Wolt OR Bolt OR Databricks) Berlin
```

## Location Filter

Remote roles are preferred. Accept:
- Berlin, on-site or hybrid: ideal
- Fully remote within Germany: ideal (preferred)
- Hybrid with occasional travel to another German city: borderline, flag it
- On-site outside Berlin: too far (fail)
- Outside Germany, unless fully remote from Germany: too far (fail)

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). **Candidate override:** German required at C1 or above ("fluent", "fließend", "verhandlungssicher") is excluded. Postings simply *written* in German that don't require German on the job are fine.

## Hours Filter

Student visa: up to 20 h/week during the semester until Nov 2027. Prefer working-student / part-time postings; flag full-time postings that start before graduation.

## Date Filter

Only include jobs posted within the last 14 days, matching `/scrape` Step 1b.3: use a supported recency filter or filter the returned publication dates client-side. A future application deadline does not bypass this discovery window; `/rank` handles deadline expiry and urgency separately after discovery. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape fintech" -> Priority 1-3 terms combined with "fintech" + SumUp / Bolt career pages
- "/scrape sports tech" -> Priority 1-3 terms combined with "sports" + Strava / Garmin / Zalando career pages
