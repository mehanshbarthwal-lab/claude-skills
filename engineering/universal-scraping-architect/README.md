# Universal Scraping Architect

> A robust, general-purpose scraping and data extraction framework designed as a reusable **Skill** for AI Agents and LLMs.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Firecrawl Ready](https://img.shields.io/badge/Firecrawl-Ready-orange.svg)](https://firecrawl.dev)

---

## What This Is

Most scraping scripts are brittle one-offs that break the moment a page layout changes, a column gets renamed, or a file format shifts slightly. This framework is built differently — it treats every extraction task as a **complete data pipeline**, with intelligent routing, validation, token tracking, and clean outputs baked in from the start.

It was built specifically to be dropped into an AI agent's skill library (like Claude's skill system), so that any scraping or data extraction task — whether it's a live public website, a local PDF, an Excel file, or a nested JSON — gets handled with the same consistent, robust approach every time.

---

## Key Features

**Intelligent Approach Routing** — Before writing a single line of code, the framework decides whether the task calls for Firecrawl (dynamic web, search-first, or bulk crawl), traditional local scraping (local files, static HTML, private data), or a hybrid of both. It states the choice and the reason.

**Firecrawl Integration (5 Paths)** — Full support for Firecrawl's CLI, REST API, and SDK across five clearly defined paths: live web data, app/product integration, finished deliverables, auth-only setup, and direct REST without installation.

**Token Budget Tracking** — Every extraction that feeds an LLM estimates token volume against a configurable context limit before processing, warning early if the output is over budget.

**Firecrawl Quota Safety** — Single-key rule enforced by default. Estimates page/credit usage before large crawl jobs and only prompts for a new key when the current one is actually exhausted or a job would genuinely exceed ~1000 requests.

**Validation at Every Step** — Required fields are checked, row counts are logged, empty outputs are caught, and duplicate rows are flagged before anything gets saved.

**Checkpointing for Large Jobs** — Progress is saved during multi-page or multi-file extractions so a failure mid-job doesn't mean starting from scratch.

**Security and Ethical Scraping** — API keys are loaded from environment variables only, never hardcoded or printed. robots.txt, rate limits, and terms of service are respected. Private files are never sent to external APIs without explicit approval.

---

## Supported Sources

| Category | Examples |
|---|---|
| **Live Web** | Public URLs, dynamic pages, SPAs, paginated sites |
| **Search-First** | Topic queries, keyword discovery, entity research |
| **Bulk Crawl** | Full site sections, documentation sites, large domains |
| **Local Files** | PDF, DOCX, Excel (.xlsx/.xls), CSV, JSON, XML, ZIP |
| **Scanned Docs** | OCR pipelines for image-based PDFs |
| **APIs** | REST APIs, paginated API responses, JSON data feeds |
| **Databases** | CSV exports, SQLite, structured data dumps |

---

## Output Formats

The framework can save clean outputs as CSV, Excel, JSON, Markdown, TXT, SQLite, Parquet, or HTML depending on what the task needs. If not specified, it defaults sensibly — CSV for structured tabular data, JSON for nested structures, Markdown for clean web page text.

---

## Project Structure

```
universal-scraping-architect/
├── SKILL.md                      # Core LLM skill definition (the agent's system prompt)
├── README.md                     # This file
├── LICENSE                       # MIT License
├── .gitignore                    # Standard Python/project ignores
├── requirements.txt              # Python dependencies
└── examples/
    ├── firecrawl_example.py      # Path C workflow: Firecrawl → clean markdown output
    └── local_bs4_example.py      # Traditional scraping: static HTML → validated CSV
```

---

## How To Use This As An AI Skill

Drop `SKILL.md` into your agent's system prompt or tool context. The agent will then adopt the Universal Scraping Architect approach for any extraction task — routing correctly, validating outputs, tracking tokens, and producing copy-paste-ready pipeline code rather than fragile one-offs.

This is how it appears in Claude's skill system:

```yaml
name: universal-scraping-architect
description: Use this skill for any scraping, crawling, extraction, parsing, web 
             research, document processing, dataset preparation, Firecrawl workflow, 
             local file extraction, API extraction, PDF/Excel/CSV/JSON/XML parsing, 
             validation-heavy data pipeline, or repeatable clean-output scraping task.
```

---

## Quick Start (For Developers Running the Examples)

**Install dependencies:**

```bash
pip install -r requirements.txt
```

**For Firecrawl workflows, set your API key:**

```bash
# Linux / macOS
export FIRECRAWL_API_KEY="fc-YOUR_API_KEY_HERE"

# Windows PowerShell
$env:FIRECRAWL_API_KEY = "fc-YOUR_API_KEY_HERE"

# Windows Command Prompt
set FIRECRAWL_API_KEY=fc-YOUR_API_KEY_HERE
```

Or add it to a `.env` file in your project root (never commit this file):

```dotenv
FIRECRAWL_API_KEY=fc-YOUR_API_KEY_HERE
```

**Run the Firecrawl example:**

```bash
python examples/firecrawl_example.py
```

**Run the local scraping example:**

```bash
python examples/local_bs4_example.py
```

---

## The 15-Step Pipeline

Every task the framework handles follows this sequence:

1. Understand the source
2. Choose the most appropriate extraction approach
3. Configure task-specific settings
4. Extract safely
5. Handle pagination, layout changes, dynamic content, or file variations
6. Clean the extracted data
7. Normalize structure and field names
8. Validate the result
9. Track token/data volume if LLM processing is involved
10. Estimate Firecrawl usage/quota before large Firecrawl jobs
11. Handle errors clearly
12. Save clean outputs
13. Save logs and checkpoints when useful
14. Print a final summary
15. Explain what was done and what can be customized

---

## Firecrawl Path Reference

| Path | When To Use |
|---|---|
| **Path A** | Need live web data right now during the current session |
| **Path B** | Building an app or product that calls Firecrawl from code |
| **Path C** | Need a finished deliverable — research brief, SEO audit, lead list, etc. |
| **Path D** | Need to set up an account or API key first |
| **Path E** | Don't want to install anything — use the REST API directly |

Install command (covers all paths):

```bash
npx -y firecrawl-cli@latest init --all --browser
```

---

## When To Use Firecrawl vs. Local Scraping

**Use Firecrawl when:**
- The source is a public URL and you want clean, reliable extraction
- The page is dynamic (JavaScript-rendered, SPA, requires interaction)
- You need search-first discovery before you know the URLs
- You're crawling many pages across a domain
- You want to wire Firecrawl into an app or agentic workflow

**Use local/traditional scraping when:**
- The source is a local file (PDF, Excel, CSV, JSON, XML)
- The data is private or sensitive and shouldn't leave your machine
- You're using official downloads/APIs where scraping isn't needed
- Simple static HTML where Firecrawl would be overkill
- Custom parsing logic with pandas, pdfplumber, openpyxl, etc. is the right fit

**Use a hybrid when:**
- Firecrawl handles web extraction, then Python cleans and structures the output
- Firecrawl discovers URLs, then local code processes and saves the dataset
- Web content gets merged with local files

---

## Security Notes

- API keys are always loaded from environment variables — never hardcoded
- `.env` files are in `.gitignore` and should never be committed
- Real keys are never printed in logs or included in output files
- Private or sensitive files are never sent to Firecrawl without explicit user approval
- One active Firecrawl API key is used at a time (no rotation by default)
- robots.txt and rate limits are respected

---

## Author

**Mehansh Barthwal**

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
