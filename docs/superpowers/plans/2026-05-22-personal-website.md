# Personal Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a static personal website at nidhimath.github.io with About, Experience, Projects, and CV tabs.

**Architecture:** Single `index.html` file containing all HTML, inline CSS in a `<style>` block, and inline JS for tab switching. No build step, no framework — served directly by GitHub Pages from the repo root.

**Tech Stack:** HTML5, CSS (Grid, custom properties), vanilla JS, DM Sans via Google Fonts, GitHub Pages.

---

## File Map

| File | Action | Responsibility |
|---|---|---|
| `index.html` | Create | Entire site — structure, styles, content, tab logic |
| `Nidhi Mathihalli Resume.pdf` | Existing | Served at root, opened by CV nav click |
| `.gitignore` | Existing | Already has `.superpowers/` |

---

### Task 1: HTML skeleton + tab switching

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create `index.html` with nav, four tab shells, and `switchTab` JS**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nidhi Mathihalli</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,400&display=swap" rel="stylesheet">
  <style>
    /* styles go here in Task 2 */
  </style>
</head>
<body>

  <nav>
    <div class="nav-name">Nidhi Mathihalli</div>
    <ul class="nav-links">
      <li><a href="#" class="active" data-tab="about">About</a></li>
      <li><a href="#" data-tab="experience">Experience</a></li>
      <li><a href="#" data-tab="projects">Projects</a></li>
      <li><a href="#" id="cv-link">CV</a></li>
    </ul>
  </nav>

  <div class="page">
    <div id="tab-about"      class="tab-content visible"></div>
    <div id="tab-experience" class="tab-content"></div>
    <div id="tab-projects"   class="tab-content"></div>
  </div>

  <script>
    document.querySelectorAll('.nav-links a[data-tab]').forEach(link => {
      link.addEventListener('click', e => {
        e.preventDefault();
        document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('visible'));
        document.querySelectorAll('.nav-links a').forEach(a => a.classList.remove('active'));
        document.getElementById('tab-' + link.dataset.tab).classList.add('visible');
        link.classList.add('active');
      });
    });

    document.getElementById('cv-link').addEventListener('click', e => {
      e.preventDefault();
      window.open('Nidhi Mathihalli Resume.pdf', '_blank');
    });
  </script>
</body>
</html>
```

- [ ] **Step 2: Open `index.html` in browser and verify**

Open `index.html` directly in a browser (file://). You should see:
- A blank page with "Nidhi Mathihalli" on the left and four nav links on the right
- Clicking About/Experience/Projects switches which tab div is visible (all empty for now)
- Clicking CV opens the PDF in a new tab

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add HTML skeleton with tab switching and CV link"
```

---

### Task 2: Base CSS — colors, typography, layout

**Files:**
- Modify: `index.html` (fill in the `<style>` block)

- [ ] **Step 1: Add base CSS inside the `<style>` block**

Replace the `/* styles go here in Task 2 */` comment with:

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'DM Sans', sans-serif;
  background: #e2ebdd;
  color: #000;
  min-height: 100vh;
  font-size: 15px;
  line-height: 1.6;
}

/* ── NAV ── */
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 22px 60px;
  border-bottom: 1px solid rgba(0,0,0,0.1);
  position: sticky;
  top: 0;
  background: #e2ebdd;
  z-index: 100;
}
.nav-name {
  font-size: 15px;
  font-weight: 500;
  letter-spacing: -0.2px;
  text-decoration: none;
  color: #000;
}
.nav-links {
  display: flex;
  gap: 32px;
  list-style: none;
}
.nav-links li a {
  text-decoration: none;
  color: #000;
  font-size: 14px;
  font-weight: 400;
  opacity: 0.5;
  transition: opacity 0.15s;
}
.nav-links li a.active { opacity: 1; font-weight: 500; }
.nav-links li a:hover  { opacity: 0.85; }

/* ── PAGE ── */
.page {
  max-width: 860px;
  margin: 0 auto;
  padding: 64px 60px 100px;
}
.tab-content          { display: none; }
.tab-content.visible  { display: block; }

/* ── TWO-COLUMN GRID ── */
.two-col {
  display: grid;
  grid-template-columns: 190px 1fr;
  gap: 56px;
  align-items: start;
}

/* ── SECTION HEADER (Work / Research / Projects label row) ── */
.section-header {
  display: grid;
  grid-template-columns: 190px 1fr;
  gap: 56px;
  padding-bottom: 4px;
  border-bottom: 1px solid rgba(0,0,0,0.12);
}
.section-label {
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.09em;
  text-transform: uppercase;
  opacity: 0.35;
  padding-top: 2px;
}
.section-gap { margin-top: 56px; }

/* ── ENTRY (experience / project rows) ── */
.entry {
  display: grid;
  grid-template-columns: 190px 1fr;
  gap: 56px;
  padding: 22px 0;
  border-top: 1px solid rgba(0,0,0,0.07);
}
.entry-date {
  font-size: 12px;
  opacity: 0.4;
  padding-top: 3px;
  line-height: 1.55;
}
.entry-body h4 {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 2px;
  letter-spacing: -0.1px;
}
.entry-body .role {
  font-size: 13px;
  opacity: 0.5;
  margin-bottom: 9px;
  font-style: italic;
}
.entry-body p {
  font-size: 13px;
  opacity: 0.72;
  line-height: 1.65;
}
.entry-body .tags {
  margin-top: 10px;
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}
.entry-body .tags span {
  font-size: 11px;
  opacity: 0.5;
  border: 1px solid rgba(0,0,0,0.15);
  padding: 2px 9px;
  border-radius: 999px;
}
.entry-body .proj-link {
  display: inline-block;
  margin-top: 8px;
  font-size: 12px;
  color: #000;
  opacity: 0.55;
  text-decoration: underline;
  text-underline-offset: 3px;
}
.entry-body .proj-link:hover { opacity: 1; }
```

- [ ] **Step 2: Open in browser and verify**

Open `index.html`. You should see:
- Green (#e2ebdd) background, DM Sans font
- Sticky nav with "Nidhi Mathihalli" left, links right at 50% opacity
- Active tab link is fully opaque and medium weight
- No content yet — that comes in the next tasks

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add base CSS — colors, typography, two-column grid, entry components"
```

---

### Task 3: About tab content

**Files:**
- Modify: `index.html` — fill `#tab-about`

- [ ] **Step 1: Add CSS for About-specific elements at the bottom of the `<style>` block**

```css
/* ── ABOUT ── */
.col-left .name-big {
  font-size: 21px;
  font-weight: 600;
  line-height: 1.25;
  letter-spacing: -0.4px;
  margin-bottom: 8px;
}
.col-left .tagline {
  font-size: 13px;
  opacity: 0.5;
  line-height: 1.65;
  margin-bottom: 22px;
}
.contact-links {
  display: flex;
  flex-direction: column;
  gap: 7px;
}
.contact-links a {
  font-size: 13px;
  color: #000;
  text-decoration: none;
  opacity: 0.55;
}
.contact-links a:hover { opacity: 1; text-decoration: underline; }

.about-text {
  font-size: 15px;
  line-height: 1.78;
  opacity: 0.82;
}
.about-text p + p { margin-top: 15px; }

.skills-row {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
  margin-top: 28px;
}
.chip {
  border: 1px solid rgba(0,0,0,0.18);
  padding: 3px 11px;
  border-radius: 999px;
  font-size: 12px;
  opacity: 0.65;
}
```

- [ ] **Step 2: Fill in `#tab-about` with HTML**

Replace `<div id="tab-about" class="tab-content visible"></div>` with:

```html
<div id="tab-about" class="tab-content visible">
  <div class="two-col">

    <div class="col-left">
      <div class="name-big">Nidhi<br>Mathihalli</div>
      <div class="tagline">MIT '27<br>CS &amp; Math<br>AI / ML</div>
      <div class="contact-links">
        <a href="mailto:nidhim27@mit.edu">nidhim27@mit.edu</a>
        <a href="https://www.linkedin.com/in/nidhi-mathihalli/" target="_blank" rel="noopener">LinkedIn ↗</a>
        <a href="https://github.com/nidhimath" target="_blank" rel="noopener">GitHub ↗</a>
      </div>
    </div>

    <div>
      <div class="about-text">
        <p>Hi, I'm Nidhi — a junior at MIT studying Computer Science and Mathematics, concentrating in AI and Machine Learning.</p>
        <p>I'm interested in the intersection of machine learning and real-world systems — from building AI agents that automate complex engineering workflows to applying deep learning to medical imaging and space reconstruction.</p>
        <p>Outside of research and internships, I'm a math competition enthusiast (USAMO qualifier), a science fair competitor (4× ISEF 1st place), and a first author at IAC '24.</p>
      </div>
      <div class="skills-row">
        <span class="chip">Python</span>
        <span class="chip">PyTorch</span>
        <span class="chip">React.js</span>
        <span class="chip">TypeScript</span>
        <span class="chip">C / C++</span>
        <span class="chip">Go</span>
        <span class="chip">SQL</span>
        <span class="chip">Docker</span>
      </div>
    </div>

  </div>
</div>
```

- [ ] **Step 3: Open in browser and verify**

Open `index.html`, click About. You should see:
- Left col: large name, tagline, three contact links
- Right col: three paragraphs of bio text, row of skill chips below
- Two-column layout holds at normal browser width

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add About tab content"
```

---

### Task 4: Experience tab — Work section

**Files:**
- Modify: `index.html` — fill `#tab-experience` with Work entries

- [ ] **Step 1: Fill in the Work section inside `#tab-experience`**

Replace `<div id="tab-experience" class="tab-content"></div>` with:

```html
<div id="tab-experience" class="tab-content">

  <!-- WORK -->
  <div class="section-header">
    <div class="section-label">Work</div>
    <div></div>
  </div>

  <div class="entry">
    <div class="entry-date">Jun – Aug 2025<br>Pittsburgh, PA</div>
    <div class="entry-body">
      <h4>BNY (Bank of New York Mellon)</h4>
      <div class="role">Software Engineer Intern — Gen AI</div>
      <p>Developed an AI SRE Agent and MCP server to process ServiceNow incident tickets — automatically generating code fixes, creating test cases, and triggering production deployments by leveraging similar issues and solutions from the past six months. Also built an AI meeting assistant for Microsoft Teams that captures notes and automatically updates ServiceNow tickets. Presented work to the CTO, senior leadership, and Engineering Townhall.</p>
      <div class="tags">
        <span>MCP Architecture</span>
        <span>OpenAI GPT-4o</span>
        <span>Microsoft Graph API</span>
        <span>ServiceNow API</span>
        <span>OAuth2</span>
      </div>
    </div>
  </div>

  <div class="entry">
    <div class="entry-date">May – Aug 2024<br>Palo Alto, CA</div>
    <div class="entry-body">
      <h4>Martini.ai</h4>
      <div class="role">Software Engineer Intern — Fintech</div>
      <p>Developed financial market signals using graph embeddings on bonds data — incorporating nearest neighbors, employee count, rank, and company age — to predict Zero-Volatility Spreads for assessing company default risk. Improved Spearman coefficient by +23.7% across all companies and by +10.5% for companies with the most data.</p>
      <div class="tags">
        <span>PyTorch Big Graph</span>
        <span>GGVec</span>
        <span>ProNE</span>
        <span>k-NN</span>
        <span>PostgreSQL</span>
        <span>Python</span>
      </div>
    </div>
  </div>

  <!-- RESEARCH and EDUCATION placeholders — filled in Task 5 -->

</div>
```

- [ ] **Step 2: Open in browser and verify**

Click Experience. You should see:
- "WORK" section label row with bottom border
- Two entries: BNY then Martini.ai
- Date/location in left col, title + italic role + paragraph + tag chips in right col

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Experience tab — Work section"
```

---

### Task 5: Experience tab — Research section + Education

**Files:**
- Modify: `index.html` — append Research + Education inside `#tab-experience`

- [ ] **Step 1: Replace the `<!-- RESEARCH and EDUCATION placeholders -->` comment with**

```html
  <!-- RESEARCH -->
  <div class="section-gap"></div>
  <div class="section-header">
    <div class="section-label">Research</div>
    <div></div>
  </div>

  <div class="entry">
    <div class="entry-date">Jan 2025 – Present<br>MIT CSAIL &amp;<br>MIT Lincoln Labs</div>
    <div class="entry-body">
      <h4>3D X-Ray Reconstruction from Ultrasound Imaging</h4>
      <div class="role">Research Assistant</div>
      <p>Developed an end-to-end pipeline for generating 3D point clouds from ultrasound images of arms for tumor detection. Leveraged Spatial Temporal Attention Heads with CNNs to improve training and validation accuracy by 20%.</p>
      <div class="tags">
        <span>3D Gaussian Splatting</span>
        <span>CNN</span>
        <span>Multi-Attention Head</span>
      </div>
    </div>
  </div>

  <div class="entry">
    <div class="entry-date">Nov 2023 – May 2024<br>MIT CSAIL</div>
    <div class="entry-body">
      <h4>DreamSat: Novel View Synthesis of Space Objects</h4>
      <div class="role">Research Assistant</div>
      <p>Created 3D point cloud prototypes from single-image input using DreamGaussian and a modified Zero123XL for spacecraft reconstruction. Increased PSNR (+2.53%) and SSIM (+2.38%) on 30 images. Accepted as first author at IAC 2024.</p>
      <div class="tags">
        <span>3D Gaussian Splatting</span>
        <span>DreamGaussian</span>
        <span>Zero123XL</span>
        <span>Stable Diffusion</span>
      </div>
      <a class="proj-link" href="https://spacenvs.github.io/" target="_blank" rel="noopener">↗ Paper &amp; code</a>
    </div>
  </div>

  <!-- EDUCATION -->
  <div class="section-gap"></div>
  <div class="section-header">
    <div class="section-label">Education</div>
    <div></div>
  </div>

  <div class="entry">
    <div class="entry-date">Aug 2023 – May 2027<br>Cambridge, MA</div>
    <div class="entry-body">
      <h4>Massachusetts Institute of Technology</h4>
      <div class="role">B.S. Computer Science &amp; Mathematics</div>
      <p>Concentration in AI and Machine Learning. Relevant coursework: Intro to ML, Deep Learning, NLP, Design &amp; Analysis of Algorithms, Data Structures, Linear Algebra, Discrete Math, Probability and Random Variables.</p>
      <div class="tags">
        <span>USAMO Qualifier</span>
        <span>4× AIME Qualifier</span>
        <span>1st Place NJSHS 2022</span>
        <span>4× 1st Place ISEF</span>
      </div>
    </div>
  </div>
```

- [ ] **Step 2: Open in browser and verify**

Click Experience. Scroll down. You should see three sections separated by gaps:
- WORK (2 entries)
- RESEARCH (2 entries, DreamSat has a "↗ Paper & code" link)
- EDUCATION (1 entry with awards as chips)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Experience tab — Research and Education sections"
```

---

### Task 6: Projects tab

**Files:**
- Modify: `index.html` — fill `#tab-projects`

- [ ] **Step 1: Replace `<div id="tab-projects" class="tab-content"></div>` with**

```html
<div id="tab-projects" class="tab-content">

  <div class="section-header">
    <div class="section-label">Projects</div>
    <div></div>
  </div>

  <div class="entry">
    <div class="entry-date">Oct 2024<br>HackMIT</div>
    <div class="entry-body">
      <h4>Coursemate</h4>
      <p>AI agent that personalizes course curriculum based on past class structures a student has succeeded in. Built at HackMIT to help students navigate MIT's course catalog more effectively.</p>
      <div class="tags">
        <span>AI Agents</span>
        <span>Python</span>
        <span>React.js</span>
      </div>
    </div>
  </div>

  <div class="entry">
    <div class="entry-date">Fall 2024</div>
    <div class="entry-body">
      <h4>RISC-V Processor</h4>
      <p>Designed and implemented a RISC-V processor in Minispec with cache hierarchies and parallel processing pipelines as part of MIT's Computer Architecture course.</p>
      <div class="tags">
        <span>Minispec</span>
        <span>Computer Architecture</span>
        <span>RISC-V</span>
        <span>Cache Design</span>
      </div>
    </div>
  </div>

  <div class="entry">
    <div class="entry-date">2022<br>WSCG Conference</div>
    <div class="entry-body">
      <h4>Currency Detector</h4>
      <p>Transfer learning model on Raspberry Pi to predict currency denominations in INR and USD. Validated through testing with three blind schools and presented at WSCG 2022.</p>
      <div class="tags">
        <span>Transfer Learning</span>
        <span>Raspberry Pi</span>
        <span>Python</span>
        <span>TensorFlow</span>
      </div>
    </div>
  </div>

  <div class="entry">
    <div class="entry-date">Fall 2025</div>
    <div class="entry-body">
      <h4>Multi-Processor xv6 OS</h4>
      <p>Designed a multi-processor xv6 operating system for RISC-V with virtual memory management, networking, and kernel subsystems as part of MIT's Operating Systems course.</p>
      <div class="tags">
        <span>C</span>
        <span>RISC-V</span>
        <span>Operating Systems</span>
        <span>Networking</span>
        <span>Virtual Memory</span>
      </div>
    </div>
  </div>

</div>
```

- [ ] **Step 2: Open in browser and verify**

Click Projects. You should see:
- "PROJECTS" label row with border
- 4 entries: Coursemate, RISC-V Processor, Currency Detector, xv6 OS
- Same date-left / content-right grid as Experience
- No "role" line (projects don't have a role)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Projects tab with 4 entries"
```

---

### Task 7: Responsive layout + polish

**Files:**
- Modify: `index.html` — add responsive CSS at bottom of `<style>` block, minor visual polish

- [ ] **Step 1: Add responsive CSS at the bottom of the `<style>` block**

```css
/* ── RESPONSIVE ── */
@media (max-width: 680px) {
  nav {
    padding: 18px 24px;
    flex-wrap: wrap;
    gap: 12px;
  }
  .nav-links { gap: 20px; }

  .page { padding: 40px 24px 80px; }

  .two-col,
  .section-header,
  .entry {
    grid-template-columns: 1fr;
    gap: 8px;
  }

  .entry { padding: 18px 0; }

  .entry-date {
    opacity: 0.4;
    font-size: 11px;
  }

  .col-left .name-big { font-size: 26px; }
}
```

- [ ] **Step 2: Set the page `<title>` to include role**

Change `<title>Nidhi Mathihalli</title>` to:

```html
<title>Nidhi Mathihalli — MIT CS &amp; Math</title>
```

- [ ] **Step 3: Add a favicon placeholder meta (prevents 404 in browser console)**

Add inside `<head>`, after the title:

```html
<link rel="icon" href="data:,">
```

- [ ] **Step 4: Open in browser at narrow width and verify**

Resize browser to ~400px wide. You should see:
- Nav wraps to two lines if needed, links still readable
- Two-column layout stacks to single column
- Date appears small above content, not side-by-side
- No horizontal scrolling

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add responsive layout and minor polish"
```

---

### Task 8: GitHub Pages setup + final deploy

**Files:**
- No new files needed — GitHub Pages serves from repo root

- [ ] **Step 1: Verify repo remote is correct**

```bash
git remote -v
```

Expected output includes `nidhimath.github.io` in the remote URL. If no remote exists:

```bash
git remote add origin https://github.com/nidhimath/nidhimath.github.io.git
```

- [ ] **Step 2: Check git log looks clean**

```bash
git log --oneline
```

Expected: 7 commits, one per task, most recent at top.

- [ ] **Step 3: Push to main**

```bash
git push -u origin main
```

- [ ] **Step 4: Enable GitHub Pages in repo settings**

In GitHub: Settings → Pages → Source: Deploy from branch → Branch: `main` → Folder: `/ (root)` → Save.

GitHub Pages will deploy within ~60 seconds.

- [ ] **Step 5: Open the live site and do a final walkthrough**

Open `https://nidhimath.github.io` in a browser. Verify each tab:

- **About:** Name, tagline, contact links, bio paragraphs, skill chips
- **Experience:** WORK (BNY, Martini.ai) → RESEARCH (X-Ray, DreamSat with ↗ link) → EDUCATION (MIT)
- **Projects:** 4 entries — Coursemate, RISC-V, Currency Detector, xv6 OS
- **CV:** Click opens `Nidhi Mathihalli Resume.pdf` in new tab, page does not navigate away
- **Responsive:** Resize to mobile width — layout stacks correctly
