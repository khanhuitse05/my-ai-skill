---
name: code-quality-report
description: Produces structured code quality assessment reports for Frontend (Mobile & Web) or Backend codebases (IEEE 730/1016/829, Clean Code, OWASP). Use when the user asks for a code quality report, quality assessment, codebase review, or wants a report for frontend vs backend or mobile vs web vs backend.
---

# Code Quality Report

Produce a **Code Quality Assessment Report**. Reports are **platform-specific**: **Frontend** (Mobile & Web) or **Backend**. Use the matching template and criteria below.

## When to Use

- User asks for "code quality report", "quality assessment", or "codebase review report".
- User asks by platform: "frontend report", "backend report", "mobile report", "web report", "FE vs BE", or "mobile & web & backend".
- User mentions migrating or updating code quality rules/skills.

## Step 1: Determine Platform

- **Frontend (Mobile & Web)**: Client-side apps — Mobile (Flutter, React Native, etc.) and/or Web (React, Vue, Next, Angular, etc.). Use [report_template_frontend.md](report_template_frontend.md) and Frontend criteria below. In the report header set **Scope** to: _Mobile_ | _Web_ | _Mobile & Web_.
- **Backend (BE)**: APIs, services, databases, auth, jobs, workers. Use [report_template_backend.md](report_template_backend.md) and Backend criteria below.
- **Both (Frontend + Backend)**: Produce **two reports** (one Frontend, one Backend). Optionally add a short "Cross-cutting" section if they share repos or contracts.
- **Three-way (Mobile + Web + Backend)**: For maximum detail, produce **three reports**: one Frontend-Mobile, one Frontend-Web, one Backend. Use the same Frontend template twice with Scope _Mobile_ and _Web_ respectively, and the Backend template once.

## Standards Applied

- **IEEE 730** (Software Quality Assurance)
- **IEEE 1016** (Software Design Description)
- **IEEE 829** (Software Test Documentation)
- **Clean Code** (readable, small functions, naming, DRY)
- **OWASP** (injection, auth, sensitive data, logging)

---

## Scoring Guide (1–10)

Use for every criterion so scores are consistent:

| Score | Meaning           | Example                                                  |
| ----- | ----------------- | -------------------------------------------------------- |
| 1–3   | Needs improvement | No tests, no structure, many security issues             |
| 4–6   | Fair              | Some tests, acceptable structure, partial best practices |
| 7–8   | Good              | Good structure, tests present, minor gaps                |
| 9–10  | Very good         | Consistent, well-tested, documented, production-ready    |

**Total score:** Average of 7 criteria. Round to 1 decimal. Map to: <4 Needs improvement, 4–6.9 Fair, 7–8.4 Good, ≥8.5 Very good.

---

## Common Criteria (All Platforms)

These 7 criteria apply; **what you look at** differs by Frontend vs Backend.

| Criterion             | Backend focus                              | Frontend focus (Mobile & Web)                     |
| --------------------- | ------------------------------------------ | ------------------------------------------------- |
| **Code Organization** | Layers (API/service/repo), modules, naming | Features/pages, components, state layer, naming   |
| **Type Safety**       | Typed APIs, DTOs, DB types                 | Null safety / TypeScript, types, lint, no dynamic |
| **Error Handling**    | Global handler, status codes, logging      | Error boundaries, user messages, logging          |
| **Documentation**     | README, API docs, runbooks                 | README, doc comments, setup                       |
| **Testing**           | Unit, integration, e2e, coverage           | Unit, component/widget, e2e, coverage             |
| **Maintainability**   | DI, no giant files, duplication            | DI/state, component size, reuse                   |
| **Security**          | Auth, secrets, validation, OWASP           | Storage, CSP, tokens, input, OWASP                |

**Optional criterion (include when relevant):**

- **Performance**: Backend — response time, N+1, caching, connection pooling. Frontend — bundle size, Core Web Vitals, lazy load, image optimization.

**Frontend-only consideration:**

- **Accessibility (a11y)**: WCAG, screen readers, contrast, focus — include under Strengths / Areas for improvement or as a short subsection when assessable.

**Cross-cutting (mention in report when assessable):**

- **CI/CD**: Build, test in pipeline, deploy, env management — can be under Testing or Documentation.
- **Observability**: Logging, metrics, tracing, alerting — can be under Error Handling or Documentation.

---

## Dependencies, Libraries & Framework Review (Required Section)

Every report **must** include the **Dependencies, Libraries & Framework Review** section. It covers:

### What to include

1. **Framework(s) and current version**
   - Primary framework/runtime (e.g. Flutter 3.24, Node 20, React 18, Go 1.21) and version in use.
   - SDK/language version if relevant (e.g. Dart 3.x, ECMAScript target).

2. **Libraries and dependencies**
   - Total direct dependency count (from `package.json`, `pubspec.yaml`, `go.mod`, etc.).
   - Key libraries: name, **current version**, **latest version** (or “up to date”).
   - Optional: table of top 10–20 dependencies with Current | Latest | Status (Up to date / Minor behind / Major behind / Outdated).

3. **Outdated status**
   - How many packages are outdated (use `npm outdated`, `dart pub outdated`, `go list -u -m all`, etc.).
   - Which are **minor/patch behind** vs **major behind**.
   - Note if the project pins versions (e.g. exact vs range) and impact on upgrades.

4. **Upgrade suggestions**
   - **Safe to upgrade**: Patch/minor updates, low risk — list or summarize.
   - **Suggest upgrade**: Minor/major updates recommended; note breaking-change risk.
   - **Major upgrade recommended**: Framework or critical lib (e.g. React 17→18, Node 18→20); note effort and migration path.

5. **Technical risks**
   - Deprecated APIs or packages in use.
   - End-of-life (EOL) or end-of-support soon for framework/runtime or key libs.
   - Incompatibilities or known issues between current versions.
   - Lockfile or dependency resolution issues (e.g. peer dependency warnings).

6. **Security and vulnerabilities**
   - Known vulnerabilities: count and severity (critical/high/medium/low) from `npm audit`, `dart pub audit` (if available), `go list -m -u -json`, Snyk, Dependabot, etc.
   - List critical/high CVEs or advisories with package name and recommended fix (e.g. “upgrade to X.Y.Z”).
   - License risks if relevant (e.g. GPL in a commercial product).

### How to gather data (examples)

- **npm/yarn/pnpm**: `npm outdated`, `npm audit`, `package-lock.json` / `yarn.lock`.
- **Flutter/Dart**: `flutter pub outdated`, `dart pub get`; check pub.dev for current versions.
- **Go**: `go list -u -m all`, `govulncheck` or similar.
- **Other**: Use lockfiles, `pip list --outdated`, etc., and cross-check with official advisories or Snyk/OSV.

### Where it goes in the report

- After **Codebase statistics** (section 2).
- Section title: **3. Dependencies, Libraries & Framework Review**.
- Subsequent sections renumber: 4 Strengths, 5 Areas for improvement, 6 Recommendations, 7 Conclusion, 8 Action items.

---

## Backend-Specific Detail

Use for **Codebase statistics** (typical folders):

| Directory / concept       | Description               |
| ------------------------- | ------------------------- |
| api/ controllers/ routes/ | HTTP handlers, routing    |
| services/ usecases/       | Business logic            |
| repositories/ dao/        | Data access               |
| models/ entities/ dto/    | Data structures           |
| config/ env/              | Configuration, env vars   |
| middleware/               | Auth, logging, rate limit |
| utils/ helpers/           | Shared utilities          |
| migrations/               | DB schema changes         |
| tests/                    | Unit, integration tests   |

**Backend checklist (evidence for scores):**

- **Organization**: Layered or modular; no business logic in controllers/handlers.
- **Type Safety**: Request/response types; no raw `any`/`interface{}` where avoidable.
- **Error Handling**: Consistent error type; HTTP status mapping; structured logging (no print-only).
- **Documentation**: How to run, env vars, main endpoints or link to API docs.
- **Testing**: Unit for services; integration for API or DB; coverage %.
- **Maintainability**: DI or factory; single responsibility; no 500+ line files without reason.
- **Security**: No secrets in code; auth on protected routes; input validation; rate limiting considered.

---

## Frontend-Specific Detail (Mobile & Web)

Use for **Codebase statistics**:

- **Mobile** (Flutter, React Native, etc.): features/, widgets|components/, package|lib/, utils/, config/, localization/, common/ — see [report_template_frontend.md](report_template_frontend.md) “Mobile” table.
- **Web** (React, Vue, Next, etc.): components/, pages|views/, hooks|store/, services|api/, config/, utils/, styles|assets/ — see “Web” table in same template.

**Frontend checklist (evidence for scores):**

- **Organization**: Feature/page-based; state separated from UI; clear naming.
- **Type Safety**: Null safety / TypeScript; return types; lint rules; no unnecessary dynamic.
- **Error Handling**: User-visible messages; error boundaries where applicable; no debug print in prod.
- **Documentation**: README, run instructions, env; key flows documented.
- **Testing**: Unit for state/logic; component/widget tests for critical UI; e2e for main flows.
- **Maintainability**: DI or state management; small components; shared components; no huge files.
- **Security**: No secrets in repo; secure storage for tokens (mobile); CSP/safe headers (web); input validation.

---

## Report Structure (Same for Frontend and Backend)

1. **Header**: **Project / Repo name**, **Tech stack** (language, framework, main version), Assessment date, Report version, Assessed by, Standards, **Platform** (Frontend / Backend), and for Frontend **Scope** (Mobile / Web / Mobile & Web).
2. **1. Executive summary**: Table 7 criteria (+ optional Performance), score X/10, rating; total score and overall rating.
3. **1b. Risks / Critical findings** (optional): 3–5 bullets summarizing critical risks or issues (security, data loss, compliance) — include when applicable.
4. **2. Codebase statistics**: Table(s) directory + file count + description.
5. **3. Dependencies, Libraries & Framework Review** (required): Framework version; key libraries and versions; outdated count/table; upgrade suggestions (safe / suggest / major); technical risks (deprecated, EOL, incompatibilities); security and vulnerabilities (audit results, CVEs, fix recommendations).
6. **4. Strengths**: Bullet list, evidence-based; optional (Criterion: content). Frontend: include **Accessibility** (a11y) when assessable; both: **Performance** and **CI/CD** when applicable.
7. **5. Areas for improvement**: Bullet list, specific issues.
8. **6. Recommendations**: 🔴 High / 🟡 Medium / 🟢 Low with concrete actions.
9. **7. Conclusion**: Summarize score, main strengths, improvement priorities.
10. **8. Action items** (optional): Immediate, Short-term, Long-term.

---

## Output Format

- Use [report_template_backend.md](report_template_backend.md) or [report_template_frontend.md](report_template_frontend.md) for exact headings and tables.
- Section titles and content in English.
- Replace all placeholders with real data; cite paths, files, and metrics.
- For "Frontend vs Backend" or "mobile & web & backend": deliver two or three reports as above; optionally add a short comparison.

---

## Checklist Before Delivering

- [ ] Platform chosen (Frontend vs Backend; if Frontend: Mobile / Web / Both) and correct template used
- [ ] Header includes project/repo name and tech stack
- [ ] All 7 criteria scored with Scoring Guide; total and overall rating computed; add Performance and/or Risks section if relevant
- [ ] Codebase stats completed; **Dependencies, Libraries & Framework Review** section filled (framework version, key libs, outdated, upgrade suggestions, technical risks, security/vulnerabilities)
- [ ] Frontend: Accessibility considered when assessable; Backend/Frontend: Performance, CI/CD, observability mentioned when assessable
- [ ] Strengths and improvements are evidence-based (file/code references)
- [ ] Recommendations are prioritized and actionable
- [ ] Conclusion matches scores and states next steps
