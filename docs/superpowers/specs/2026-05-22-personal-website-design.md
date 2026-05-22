# Personal Website Design Spec
**Date:** 2026-05-22  
**Owner:** Nidhi Mathihalli  
**Deploy target:** nidhimath.github.io (GitHub Pages)

---

## Overview

A minimal, sleek personal website combining:
- Anjan Bharadwaj's two-column layout structure
- Oliver Ye / Nikhil Mathihalli's clean page-per-tab style
- Legora.com's overall simplicity and restraint

Static HTML/CSS/JS — no framework, no build step. Deployed via GitHub Pages.

---

## Visual Design

| Token | Value |
|---|---|
| Background | `#e2ebdd` (light muted green) |
| Text | `#000000` |
| Muted text | `rgba(0,0,0,0.5)` |
| Border / dividers | `rgba(0,0,0,0.08–0.12)` |
| Font | DM Sans (Google Fonts), weights 300/400/500/600 |
| Max content width | 860px, centered |
| Page padding | 64px top, 60px horizontal |

No accent colors. No gradients. No images (no photo). Borders and opacity carry all visual hierarchy.

---

## Architecture

Single HTML file (`index.html`) with four tab sections. JavaScript toggles `display` on tab switch — no routing, no page loads. CSS is inline in a `<style>` block.

```
nidhimath.github.io/
├── index.html          ← entire site
├── Nidhi Mathihalli Resume.pdf
└── .gitignore
```

---

## Navigation

Sticky top bar, `border-bottom: 1px solid rgba(0,0,0,0.1)`.

- **Left:** "Nidhi Mathihalli" — 15px, weight 500
- **Right:** About · Experience · Projects · CV — 14px, weight 400, opacity 0.5; active tab opacity 1 weight 500

Tab switching: JS `switchTab()` toggles `.visible` class on `.tab-content` divs, `.active` class on nav links.

---

## Two-Column Grid

Used on About and as the base grid for Experience/Projects entries.

```css
grid-template-columns: 190px 1fr;
gap: 56px;
```

Left column: label, date, or identity info.  
Right column: content.

---

## Pages

### About
- **Left col:** Name (large, weight 600), tagline (MIT '27 / CS & Math / AI/ML), contact links (email, LinkedIn, GitHub)
- **Right col:** 3-paragraph bio (pre-written below), skill chips

**Bio text:**
> Hi, I'm Nidhi — a junior at MIT studying Computer Science and Mathematics, concentrating in AI and Machine Learning.
>
> I'm interested in the intersection of machine learning and real-world systems — from building AI agents that automate complex engineering workflows to applying deep learning to medical imaging and space reconstruction.
>
> Outside of research and internships, I'm a math competition enthusiast (USAMO qualifier), a science fair competitor (4× ISEF 1st place), and a first author at IAC '24.

**Skill chips:** Python, PyTorch, React.js, TypeScript, C/C++, Go, SQL, Docker

---

### Experience
Two labelled sub-sections, each with a section header row (`section-label` left, empty right, `border-bottom`).

#### Work (chronological, newest first)
| Date | Company | Role |
|---|---|---|
| Jun–Aug 2025 | BNY (Bank of New York Mellon), Pittsburgh PA | Software Engineer Intern — Gen AI |
| May–Aug 2024 | Martini.ai, Palo Alto CA | Software Engineer Intern — Fintech |

#### Research (chronological, newest first)
| Date | Project | Institution |
|---|---|---|
| Jan 2025–Present | 3D X-Ray Reconstruction from Ultrasound Imaging | MIT CSAIL & MIT Lincoln Labs |
| Nov 2023–May 2024 | DreamSat: Novel View Synthesis of Space Objects | MIT CSAIL |

Each entry uses the two-column grid: date/location left, `h4` title + italic role + description paragraph + tech tag chips right.

---

### Projects
Same two-column entry format. Date left, title + description + tags right. Link shown where URL exists. Exactly 4 projects.

| Date | Title | Link |
|---|---|---|
| Oct 2024 (HackMIT) | Coursemate | — |
| Fall 2024 | RISC-V Processor | — |
| 2022 (WSCG) | Currency Detector | — |
| Fall 2025 | Multi-Processor xv6 OS | — |

DreamSat lives in the Research section of the Experience tab only. The paper link (https://spacenvs.github.io/) appears as a `proj-link` on the DreamSat Research entry.

---

### CV
The "CV" nav item does **not** switch tabs. It opens the PDF directly:
```js
window.open('Nidhi Mathihalli Resume.pdf', '_blank');
```
The `switchTab` function is not called for CV. No tab-content div needed for CV.

---

## Entry Component Structure

```html
<div class="entry">
  <div class="entry-date">Jun–Aug 2025<br>Pittsburgh, PA</div>
  <div class="entry-body">
    <h4>Company Name</h4>
    <div class="role">Role Title</div>
    <p>Description...</p>
    <div class="tags"><span>Tag</span></div>
    <!-- optional: <a class="proj-link" href="...">↗ link text</a> -->
  </div>
</div>
```

---

## Section Header Component

Used to label Work / Research / Projects sub-sections.

```html
<div class="section-header">
  <div class="section-label">Work</div>
  <div></div>
</div>
```

`section-label`: 11px, weight 500, 0.09em letter-spacing, uppercase, opacity 0.35.

---

## GitHub Pages Setup

- Repo: `nidhimath.github.io`
- Branch: `main`
- Pages source: root `/`
- PDF served from root alongside `index.html`
- Add `.superpowers/` to `.gitignore`

---

## Content Reference (from CV)

### BNY bullets
- Developed an AI SRE Agent and MCP server to process ServiceNow incident tickets, auto-generate code fixes, create test cases, and trigger production deployments
- Built an AI meeting assistant for Microsoft Teams to capture notes and auto-update ServiceNow tickets
- Presented to CTO & senior leadership and Engineering Townhall

**Tags:** MCP Architecture, OpenAI GPT-4o, Microsoft Graph API, ServiceNow API, OAuth2

### Martini.ai bullets
- Developed financial market signals using graph embeddings on bonds data (nearest neighbors, employee count, rank, company age) to predict Z-Volatility Spreads
- Improved z-spread prediction accuracy; Spearman coefficient +23.7% all companies, +10.5% highest-data companies

**Tags:** PyTorch Big Graph, GGVec, ProNE, k-NN, PostgreSQL, Python

### 3D X-Ray Reconstruction
- End-to-end pipeline for 3D point clouds from ultrasound images of arms for tumor detection
- Spatial Temporal Attention Heads + CNNs improved accuracy by 20%

**Tags:** 3D Gaussian Splatting, CNN, Multi-Attention Head

### DreamSat
- 3D point cloud prototypes from single-image input using DreamGaussian + modified Zero123XL
- PSNR +2.53%, SSIM +2.38% on 30 images; first author IAC 2024

**Tags:** 3D Gaussian Splatting, DreamGaussian, Zero123XL, Stable Diffusion

### Coursemate
HackMIT project. AI agent that personalizes course curriculum based on past class structures a student succeeded in.

### RISC-V Processor
MIT Computer Architecture (Fall 2024). RISC-V processor in Minispec with cache and parallel processing.

### Currency Detector
Transfer learning model on Raspberry Pi to predict INR/USD currency denominations. Tested with 3 blind schools, presented at WSCG 2022.

### xv6 OS
MIT OS course (Fall 2025). Multi-processor xv6 OS for RISC-V with VM, networking, kernel subsystems.
