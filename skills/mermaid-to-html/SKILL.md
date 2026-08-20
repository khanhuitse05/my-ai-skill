---
name: mermaid-to-html
description: Intelligent Mermaid diagram generator and refactorer. Converts raw Mermaid code or flow descriptions into beautifully styled, logically rearranged, and structurally optimized standalone HTML diagrams. Eliminates layout inversions, balances branches, adds rich iconography, and produces presentation-ready HTML pages.
---

# Mermaid to HTML (Architectural Refactoring & Modern Styler)

Convert raw input or flow descriptions into **professionally refactored, visually balanced, and modern standalone HTML diagrams**.

> [!IMPORTANT]
> **Do NOT simply copy raw input verbatim.** Analyze the architecture, eliminate layout defects (like inverted subgraphs or tangled back-edges), reorganize nodes logically, and apply modern design standards.

---

## 1. Core Principles: Layout Refactoring & Rearrangement

### A. Prevent Layout Inversion (The "Dagre Inversion" Bug)
When Mermaid renders flowcharts, cyclic back-edges (e.g., pointing from Step 5 back up to Step 1) force Dagre to invert the diagram rank, placing Step 5 at the top and drawing ugly lines across the entire chart.

**Refactoring Rules**:
1. **Never draw raw multi-stage back-edges**:
   - ❌ **Bad**: `FORWARD (in Step 5) -.-> DEV (in Step 1)` (Inverts the layout).
   - ✅ **Good**: Conclude the recovery path with a terminal action node (e.g. `FORWARD["🔄 Create Forward-Only Migration (New PR)"]`) or keep feedback annotations local.
2. **Explicit Stage Transitions**:
   - Order stages sequentially from top-to-bottom: `Step 1 --> Step 2 --> Step 3 --> Step 4 --> Step 5`.
3. **Use Subgraph Directions**:
   - Add `direction TB` or `direction LR` inside subgraphs to force predictable node placement.

### B. Separate Happy Path & Failure Branches
- Place the **Happy Path (Success)** in the main central/left vertical column.
- Dock **Fail-Fast / Error / Remediation** flows cleanly to the right side (e.g. in a nested `direction TB` subgraph).

### C. Balance Decision Trees
- Keep conditions symmetrical:
  - Positive / True / Pass: Left / Main stream (`linkStyle` Green).
  - Negative / False / Fail: Right / Side stream (`linkStyle` Red).

---

## 2. Visual Design System

### A. Modern Palette (`classDef`)
Always include rich, cohesive color classes:

```mermaid
classDef dev fill:#e0f2fe,stroke:#0284c7,stroke-width:2px;
classDef pipeline fill:#ede9fe,stroke:#7c3aed,stroke-width:2px;
classDef aws fill:#ffedd5,stroke:#ea580c,stroke-width:2px;
classDef flyway fill:#ecfdf5,stroke:#059669,stroke-width:2px;
classDef success fill:#dcfce7,stroke:#16a34a,stroke-width:2.5px;
classDef failure fill:#fee2e2,stroke:#dc2626,stroke-width:2px;
classDef decision fill:#fef9c3,stroke:#eab308,stroke-width:2px;
classDef note fill:#f8fafc,stroke:#94a3b8,stroke-width:1.5px;
```

### B. Contextual Iconography
Enhance node readability with clean icons:
- **Dev / Git**: `💻 Developer creates script`, `🔀 Pull Request`
- **CI/CD & Auth**: `⚡ Pipelines Triggered`, `🔑 AWS OIDC Role`, `🛡️ SSH Bastion`
- **Database & Migration**: `🔐 Secrets Manager`, `🔒 Schema Lock`, `🚀 Run Migrations`, `🔍 Validate`
- **Monitoring & Outcomes**: `💓 /health Endpoint`, `🎉 Succeeded & Passed`, `❌ Pipeline Terminated`, `📧 Email Alerts`

---

## 3. Standalone HTML Template

Every generated file must be a complete, self-contained HTML page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Architecture Flowchart</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background-color: #f8fafc;
      background-image: radial-gradient(#e2e8f0 1.2px, transparent 1.2px);
      background-size: 24px 24px;
      padding: 40px 24px;
      font-family: 'Inter', system-ui, -apple-system, sans-serif;
      color: #0f172a;
    }
    .header { text-align: center; margin-bottom: 24px; }
    .header h1 { font-size: 24px; font-weight: 700; color: #0f172a; letter-spacing: -0.02em; }
    .header p { font-size: 14px; color: #64748b; margin-top: 4px; }
    .chart-container {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      max-width: 1100px;
      background: #ffffff;
      padding: 36px 32px;
      border-radius: 16px;
      box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05), 0 8px 10px -6px rgba(0,0,0,0.03);
      border: 1px solid #e2e8f0;
    }
    .mermaid { display: flex; justify-content: center; width: 100%; }
    .mermaid svg { max-width: 100%; height: auto; }
    .node rect, .node circle, .node polygon, .node path { filter: drop-shadow(0 2px 4px rgba(0,0,0,0.04)); }
    .node rect { rx: 8px; ry: 8px; }
    .cluster rect { rx: 12px; ry: 12px; stroke-dasharray: 4, 4; }
    .edgeLabel { font-family: 'Inter', sans-serif; font-weight: 600; font-size: 12px; }
  </style>
</head>
<body>
  <div class="header">
    <h1><!-- Title --></h1>
    <p><!-- Subtitle / Description --></p>
  </div>

  <div class="chart-container">
    <pre class="mermaid">
<!-- Optimized, Reorganized Mermaid Code -->
    </pre>
  </div>

  <script type="module">
    import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
    mermaid.initialize({
      startOnLoad: true,
      theme: 'base',
      themeVariables: {
        fontFamily: "'Inter', system-ui, -apple-system, sans-serif",
        fontSize: '13px',
        primaryColor: '#ffffff',
        primaryBorderColor: '#cbd5e1',
        primaryTextColor: '#1e293b',
        lineColor: '#64748b',
        textColor: '#1e293b',
        mainBkg: '#ffffff',
        nodeBorder: '#94a3b8',
        clusterBkg: '#f8fafc',
        clusterBorder: '#cbd5e1',
        edgeLabelBackground: '#ffffff',
        tertiaryColor: '#f1f5f9'
      },
      flowchart: {
        curve: 'basis',
        htmlLabels: true,
        padding: 24,
        useMaxWidth: true
      },
      securityLevel: 'loose'
    });
  </script>
</body>
</html>
```

---

## 4. Refactoring Example (Before vs. After)

### Raw Input (Prone to Inversion & Clutter):
- Has cross-stage upward cycles (`Step 5 -> Step 1`).
- Intermixes error paths with main sequential stages.
- No subgraph direction controls.

### Refactored Output (Logical & Symmetrical):
- Linear Top-to-Bottom stage flow (`S1 --> S2 --> S3 --> S4`).
- Step 5 splits into parallel sub-containers: **Happy Path** (Left) and **Fail-Fast Recovery Flow** (Right).
- Failure transitions from Step 4 converge cleanly into the Fail-Fast container without tangling the upper stages.
- Color-coded links (`linkStyle`) make outcomes instantly distinguishable.
