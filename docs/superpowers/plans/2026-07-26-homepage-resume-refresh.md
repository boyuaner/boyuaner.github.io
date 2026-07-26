# Homepage and Resume Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Align the existing English Hexo academic homepage with the sanitized July 2026 Chinese CV and replace the downloadable CV with a phone-free build.

**Architecture:** Keep the existing Fengye theme and content pipeline. Update Hexo configuration and Markdown content in place, add one missing featured publication post, and compile a temporary sanitized copy of the supplied TeX into the theme's existing static PDF path.

**Tech Stack:** Hexo 7.3, EJS, Markdown, YAML, XeLaTeX, Poppler (`pdfinfo`, `pdftotext`, `pdftoppm`), pnpm.

## Global Constraints

- Keep the homepage entirely in English.
- Treat the sanitized July 2026 CV as the source of truth for all overlapping homepage facts.
- Preserve the existing theme, layout, portrait, navigation, colors, typography, and interactions.
- Do not publish the phone number.
- Do not modify `/Users/leon/Documents/resume/2026-07-16-zh-multimodal.tex`.
- Do not add new site dependencies.
- Wait for explicit user approval after local preview before pushing or deploying.

---

### Task 1: Generate and verify the sanitized CV

**Files:**
- Read: `/Users/leon/Documents/resume/2026-07-16-zh-multimodal.tex`
- Read: `/Users/leon/Documents/resume/deedy-resume-cn.cls`
- Create temporarily: `tmp/resume-sanitized/2026-07-16-zh-multimodal-no-phone.tex`
- Modify: `themes/fengye/source/pdf/cv.pdf`

**Interfaces:**
- Consumes: the supplied TeX resume and its local class/font assets.
- Produces: a valid one-page `themes/fengye/source/pdf/cv.pdf` whose extracted text contains `liboyuan@ruc.edu.cn` but not `13396229141`.

- [ ] **Step 1: Write and run the precondition check**

Run:

```bash
pdftotext themes/fengye/source/pdf/cv.pdf - | rg -q 'Being-M0.7'
```

Expected: non-zero exit status because the current site CV predates Being-M0.7.

- [ ] **Step 2: Create a private temporary TeX working tree**

Copy the source TeX, `deedy-resume-cn.cls`, and `fonts/` into `tmp/resume-sanitized/`. In the temporary TeX only, replace:

```tex
\href{mailto:liboyuan@ruc.edu.cn}{liboyuan@ruc.edu.cn}
  \quad\textbullet\quad
  13396229141
```

with:

```tex
\href{mailto:liboyuan@ruc.edu.cn}{liboyuan@ruc.edu.cn}
```

Before copying, record the original source checksum:

```bash
shasum -a 256 /Users/leon/Documents/resume/2026-07-16-zh-multimodal.tex > tmp/resume-sanitized/original-source.sha256
```

- [ ] **Step 3: Compile the sanitized PDF**

Run twice from `tmp/resume-sanitized/`:

```bash
xelatex -interaction=nonstopmode -halt-on-error 2026-07-16-zh-multimodal-no-phone.tex
```

Copy the resulting PDF to `themes/fengye/source/pdf/cv.pdf`.

- [ ] **Step 4: Verify the PDF logically**

Run:

```bash
pdfinfo themes/fengye/source/pdf/cv.pdf
pdftotext -layout themes/fengye/source/pdf/cv.pdf tmp/resume-sanitized/cv.txt
test "$(pdfinfo themes/fengye/source/pdf/cv.pdf | awk '/^Pages:/ {print $2}')" = "1"
rg -q 'liboyuan@ruc.edu.cn' tmp/resume-sanitized/cv.txt
! rg -q '13396229141' tmp/resume-sanitized/cv.txt
(cd / && shasum -a 256 -c /Users/leon/Project/boyuaner.github.io/tmp/resume-sanitized/original-source.sha256)
```

Expected: one page, email present, phone absent, original source unchanged.

- [ ] **Step 5: Render for visual verification**

Run:

```bash
pdftoppm -png -r 160 themes/fengye/source/pdf/cv.pdf tmp/resume-sanitized/cv
```

Inspect `tmp/resume-sanitized/cv-1.png` and confirm the centered header, columns, spacing, and final line remain intact.

- [ ] **Step 6: Commit the sanitized CV**

```bash
git add themes/fengye/source/pdf/cv.pdf
git commit -m "Update downloadable CV without phone number"
```

### Task 2: Align the homepage profile and experience

**Files:**
- Modify: `_config.yml`
- Modify: `source/exp/index.md`
- Modify: `themes/fengye/_config.yml`

**Interfaces:**
- Consumes: education, research focus, internship dates, and role names from the sanitized CV.
- Produces: English homepage introduction and experience content plus the label `CV (Chinese, July 2026)`.

- [ ] **Step 1: Run content checks before editing**

Run:

```bash
rg -q 'world-action models' _config.yml
rg -q 'BeingBeyond' source/exp/index.md
rg -q 'Chinese, July 2026' themes/fengye/_config.yml
```

Expected: all three checks fail against the stale homepage.

- [ ] **Step 2: Update the English profile**

Set `_config.yml` `description` to concise English copy stating:

```text
I am a Ph.D. student at the Gaoling School of Artificial Intelligence, Renmin University of China, advised by Prof. Ruihua Song. My research focuses on multimodal generation, 3D human motion generation, and world-action models. My first-author work has appeared at ACM Multimedia and ICASSP, and I have contributed to work at CVPR and ICLR.
```

- [ ] **Step 3: Update the experience timeline**

Replace `source/exp/index.md` body with English entries for:

```text
Sep. 2025 - Jun. 2026: Research Intern, Humanoid Group, BeingBeyond. Developed part-level reliability modeling and robust training for web video motions; contributed to the Video-Motion pretraining module of Being-M0.7.
Sep. 2022 - Present: Ph.D. Student in Artificial Intelligence, Gaoling School of Artificial Intelligence, Renmin University of China. Expected graduation: Jun. 2027.
Jun. 2022 - Dec. 2022: Research Intern, Multimedia Computing Group, Microsoft Research Asia. Worked on cross-skeleton motion transfer for game characters with Xbox The Coalition.
Sep. 2018 - Jun. 2022: B.E. in Computer Science and Technology, Shandong University.
```

- [ ] **Step 4: Update the CV label**

In `themes/fengye/_config.yml`, keep `cv_path: /pdf/cv.pdf` and set:

```yaml
cv_last_update_date: Chinese, July 2026
```

- [ ] **Step 5: Verify content**

Run:

```bash
rg -q 'world-action models' _config.yml
rg -q 'BeingBeyond' source/exp/index.md
rg -q 'Expected graduation: Jun. 2027' source/exp/index.md
rg -q 'cv_last_update_date: Chinese, July 2026' themes/fengye/_config.yml
! rg -n '13396229141' _config.yml source themes/fengye/_config.yml
```

Expected: every assertion passes.

- [ ] **Step 6: Commit the profile and experience refresh**

```bash
git add _config.yml source/exp/index.md themes/fengye/_config.yml
git commit -m "Align homepage profile with July 2026 CV"
```

### Task 3: Align featured publications

**Files:**
- Create: `source/_posts/Being-M0.7.md`
- Modify: `source/_posts/Robust Motion Generation.md`
- Modify: `source/_posts/OpenT2M.md`
- Verify: `source/_posts/Two in One.md`
- Verify: `source/_posts/TTR.md`
- Verify: `source/_posts/Animate and Sound an Image.md`
- Verify: `source/_posts/tevis.md`

**Interfaces:**
- Consumes: publication titles, venues, author order, and author-role claims from the sanitized CV.
- Produces: seven featured publication entries consistent with the July 2026 CV.

- [ ] **Step 1: Run publication checks before editing**

Run:

```bash
test -f source/_posts/Being-M0.7.md
rg -q 'ACM Multimedia 2026' 'source/_posts/Robust Motion Generation.md'
rg -q 'No-frills Motion Generation' source/_posts/OpenT2M.md
```

Expected: all three checks fail.

- [ ] **Step 2: Add Being-M0.7**

Create `source/_posts/Being-M0.7.md` with:

```markdown
---
title: "Being-M0.7: A Latent World-Action Model for Humanoid Robots"
date: 2026-07-01
featured: true
recruit: Technical Report 2026
---
Junpeng Yue\*, ***Boyuan Li\****, Yuxuan Wang\*, Zepeng Wang, Yuhui Fu, Feiyang Xie, Yu Zhang, Jing Zhang, Jiangxing Wang, Zongqing Lu
<!-- more -->
**Technical Report, 2026 · Co-first author**

## Overview
Being-M0.7 learns world-action priors through joint video-motion pretraining and transfers them to humanoid robot control. I contributed the model design, training, and evaluation of its Video-Motion pretraining module.
```

- [ ] **Step 3: Correct Robust Motion Generation**

Keep the title and CV author order, but replace stale `CVPR 2025` metadata and body text with `ACM Multimedia 2026`. State that the work identifies part-level reliable motion from web videos and uses part-aware encoding and masked generation for robust training. Mark Boyuan Li as first author through the existing bold-author convention.

- [ ] **Step 4: Correct OpenT2M**

Change both the front-matter title and prose spelling from `No-frill` to `No-frills`, preserve `CVPR 2026` and the CV author order, and describe the open million-scale high-quality motion data and part-aware spatiotemporal tokenizer without adding unsupported numeric claims.

- [ ] **Step 5: Verify the other four entries against the CV**

Confirm exact title, venue, and author order for Two-in-One, Think Then React, Animate and Sound an Image, and TeViS. Do not rewrite richer abstracts unless a conflict is found.

- [ ] **Step 6: Verify all seven featured entries**

Run:

```bash
test "$(rg -l '^featured: true$' source/_posts/*.md | wc -l | tr -d ' ')" = "7"
rg -q 'Technical Report, 2026 · Co-first author' source/_posts/Being-M0.7.md
rg -q 'ACM Multimedia 2026' 'source/_posts/Robust Motion Generation.md'
rg -q 'No-frills Motion Generation' source/_posts/OpenT2M.md
! rg -n 'CVPR 2025' 'source/_posts/Robust Motion Generation.md'
```

Expected: seven featured publications and no stale venue for Robust Motion Generation.

- [ ] **Step 7: Commit the publication refresh**

```bash
git add source/_posts/Being-M0.7.md source/_posts/Robust\ Motion\ Generation.md source/_posts/OpenT2M.md
git commit -m "Align featured publications with latest CV"
```

### Task 4: Build, inspect, and serve the site locally

**Files:**
- Generate: `public/`
- Verify: `public/index.html`
- Verify: `public/pdf/cv.pdf`

**Interfaces:**
- Consumes: all updated Hexo configuration, content, and static assets.
- Produces: a successful local Hexo build and a running preview URL for user review.

- [ ] **Step 1: Run a clean production build**

Run:

```bash
pnpm run clean
pnpm run build
```

Expected: Hexo exits successfully with no render errors.

- [ ] **Step 2: Verify generated homepage facts and privacy**

Run:

```bash
rg -q 'BeingBeyond' public/index.html
rg -q 'Being-M0.7' public/index.html
rg -q 'ACM Multimedia 2026' 'public/2025/12/01/Robust Motion Generation/index.html'
rg -q 'CV \\(Chinese, July 2026\\)' public/index.html
cmp themes/fengye/source/pdf/cv.pdf public/pdf/cv.pdf
! rg -n '13396229141' _config.yml source themes/fengye/_config.yml public --glob '!*.pdf'
! pdftotext public/pdf/cv.pdf - | rg -q '13396229141'
```

Expected: current content is present, the public CV is identical to the verified source asset, and the phone number is absent.

- [ ] **Step 3: Inspect the generated homepage**

Open `public/index.html` through the local server at desktop and mobile viewport widths. Confirm the unchanged visual structure, readable experience section, seven publication cards, working theme toggle, and working CV link.

- [ ] **Step 4: Start the Hexo preview server**

Run:

```bash
pnpm run server -- --port 4000
```

Expected: Hexo reports `http://localhost:4000/`.

- [ ] **Step 5: Stop before publishing**

Provide the local preview URL and a concise change summary. Do not push `main` or run `pnpm run deploy` until the user explicitly approves the preview.
