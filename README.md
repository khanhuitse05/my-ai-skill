# my-ai-skill

Simple collection of custom Cursor AI skills that I use frequently.

## Skills

- **code-quality-report**  
  Produces a structured code quality assessment report for Frontend (Mobile & Web) or Backend codebases using shared templates.
- **mermaid-to-html**  
  Converts raw Mermaid chart syntax or flow descriptions into modern, presentation-ready standalone HTML files featuring curated pastel color palettes, iconography, subtle drop shadows, custom fonts, and zero UI clutter.

### How to use

#### 1. Code Quality Report

**Installation:**

```bash
npx skills add https://github.com/khanhuitse05/my-ai-skill --skill code-quality-report
```

**Usage:**

Prompt Cursor AI with any of the following:

- `Generate a code quality report for this frontend project`
- `Create a backend code quality assessment`
- `Review code quality for mobile app`
- `Generate quality assessment report`
- `Codebase review report`
- `Frontend vs backend quality report`
- `Export report as markdown file`

The skill will automatically detect the platform (Frontend/Backend) and generate a comprehensive assessment report following IEEE 730/1016/829 standards, Clean Code principles, and OWASP security guidelines.

---

#### 2. Mermaid to HTML

**Installation:**

```bash
npx skills add https://github.com/khanhuitse05/my-ai-skill --skill mermaid-to-html
```

**Usage:**

Prompt Cursor AI with any of the following:

- `Convert this raw mermaid chart into an HTML file: <paste mermaid code>`
- `Generate a modern, beautiful HTML architecture flowchart for this AWS S3/EFS flow`
- `Render this sequence diagram as a clean HTML file with soft shadows and custom icons`
- `Create a simple html file to draw this mermaid flow, no header or footer, just show chart`

The skill produces a self-contained, responsive HTML file with modern Inter typography, soft drop shadows, curated color palettes, icon support, and color-coded branch labels (Green/Red).
