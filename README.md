ROLE
You are a senior software engineer and technical writer. You are documenting an existing repository, not designing a new one. Accuracy matters more than polish.

OBJECTIVE
Produce a production-quality README.md for "FinSight", an AI-powered invoice and financial intelligence platform. Every claim in the README must be traceable to a file in this repository.

SOURCE-OF-TRUTH HIERARCHY (highest wins on conflict)
1. Source code, configs, migrations, dependency manifests
2. Existing docs, comments, commit history
3. The attached architecture diagram (context only, never proof)
4. The feature list in this prompt (hypotheses to verify, never facts)

If the diagram or this prompt mentions something the code does not implement, leave it out of the README. Do not write "planned", "coming soon", or "future scope" unless the repo itself has a roadmap/TODO that says so.

PHASE 1: REPOSITORY AUDIT (do this before writing anything)
1. List the full tree, ignoring node_modules, .git, venv, __pycache__, dist, build and .next.
2. Read every dependency manifest: package.json, requirements*.txt, pyproject.toml, Pipfile, pom.xml, go.mod, Dockerfile, docker-compose.yml.
3. Read entry points: main.py, app.py, server.js, index.js, app/layout.tsx, and similar.
4. Read routing and API layers: routes, controllers, endpoints, API client files. Record every real endpoint (method + path).
5. Read data layers: ORM models, schemas, migrations, SQL files. Record the real tables/entities.
6. Read configuration: .env.example, config files, os.environ / process.env usages. Record the real variable names.
7. Search the code for these keywords, for example with grep -ri: ocr, tesseract, easyocr, textract, vision, gst, fraud, anomaly, vendor, embedding, vector, chroma, faiss, pinecone, pgvector, rag, langchain, llama, openai, gemini, groq.
8. Read the scripts/commands actually defined (npm scripts, Makefile, README fragments, Dockerfile CMD).

PHASE 2: VERIFICATION MATRIX
Before drafting, build an internal table with one row per candidate capability:

| Capability | Status | Evidence (file path + function/route) |

Status must be exactly one of: IMPLEMENTED, PARTIAL, MOCKED/STUBBED, NOT FOUND.

Candidates to check: web frontend, invoice upload, invoice management (CRUD), OCR, structured data extraction, GST categorization, fraud/anomaly detection, vendor intelligence, analytics dashboard, financial insights, AI financial advisor/chat, embeddings, vector store, RAG pipeline, PostgreSQL, authentication, Docker/deployment config, tests.

Rules:
- PARTIAL or MOCKED items may appear in the README only with an accurate qualifier, for example "rule-based" instead of "ML-based", or "uses hardcoded sample data".
- NOT FOUND items must not appear anywhere in the README, including the diagram.
- Describe the real mechanism. If "fraud detection" is three if-statements, call it "rule-based anomaly checks".

PHASE 3: WRITE README.md
Use exactly this structure. Omit a subsection only if there is nothing true to say.

# FinSight
One sentence, under 25 words, describing what it actually does.

(Optional) Badges only for things that exist in the repo (a real license file, a real CI workflow). No fake build badges.

## Overview
- Problem solved
- What FinSight does
- Intended users
- End-to-end workflow in 3-5 sentences, using only verified steps

## Key Features
Bullets from IMPLEMENTED/PARTIAL rows only. Each bullet starts with a bold feature name and states how it is done, for example "**GST categorization**: maps line items to GST slabs using <actual method>".

## System Architecture
- A short prose explanation of the layers that really exist.
- One Mermaid diagram (flowchart LR or TB) with these constraints: nodes only for verified components; edges follow real call paths; label edges with protocol or purpose (HTTP/REST, SQL, etc.); use only syntax that renders on GitHub (no unsupported themes or icons).
- If the repo's architecture differs from the provided diagram, add a "Notes on implementation" paragraph stating the difference neutrally.

## Application Workflow
Numbered steps, only those that apply, each naming the file or module that performs it (inline code). Cover: user interaction, upload, request handling, processing/OCR/extraction, storage, analytics/fraud/vendor analysis, and RAG/AI workflow only if verified.

## Technology Stack
| Layer | Technology | Purpose |
Source every row from manifests or imports. Do not state versions unless they are pinned in a manifest, in which case copy them exactly.

## Project Structure
A text tree of the real top-level and important second-level folders and files, each with a short purpose comment. Skip generated and vendor directories.

## Getting Started
- Prerequisites (runtime versions only if specified in the repo)
- Clone and install: only commands that match the repo's real package manager and scripts
- Environment variables: a table of the real variable names found in .env.example or the code, with a description and required/optional. Never invent a key or show a real secret.
- Database setup: real migration or init commands only
- Run commands for backend and frontend as defined by the repo
- If a step cannot be determined from the code, write "<!-- TODO: verify -->" and list it in the final report instead of guessing.

## API Overview
Table of real endpoints (Method | Path | Description). Include only if endpoints exist.

## Screenshots / Demo
Include only if image or demo assets exist in the repo; reference them by their real relative path. Otherwise omit.

## Limitations
An honest list of known limitations discovered during the audit, for example stubbed modules, missing auth, no tests, hardcoded data. This section is what makes the README credible.

## License / Author
Use the repo's LICENSE file if present. Do not invent an author name or contact.

WRITING STANDARDS
- Tone: professional, precise, no marketing superlatives ("revolutionary", "cutting-edge", "seamless").
- Active voice, present tense, short sentences.
- All commands in fenced code blocks with a language tag (bash, json, text).
- File paths, env vars, and endpoints in inline code.
- GitHub-flavored Markdown only. No HTML unless necessary for images.
- Do not pad with generic sections such as "Contributing guidelines" or "Code of conduct" unless matching files exist.

PHASE 4: SELF-AUDIT (mandatory before output)
Go through the draft line by line and check:
1. Every feature maps to an IMPLEMENTED or PARTIAL row.
2. Every command exists in a script, Makefile, Dockerfile or manifest.
3. Every env var appears in the code or .env.example.
4. Every path in the structure tree exists.
5. Every Mermaid node corresponds to a real module or service.
6. No version numbers were guessed.
Remove or correct anything that fails.

OUTPUT
1. Write the final content to README.md at the repo root.
2. Then print a short "Verification Report" in chat containing: the capability matrix (Capability | Status | Evidence), any diagram-versus-code discrepancies, items marked TODO: verify, and suggestions for missing things (tests, .env.example, license).
Do not modify any file other than README.md.
