# Homepage and Resume Refresh Design

## Goal

Refresh the existing English academic homepage from the supplied July 2026 Chinese resume while preserving the current Hexo theme, layout, portrait, navigation, colors, typography, and interactions.

## Scope

### Source of truth

- Treat the sanitized July 2026 CV generated from `/Users/leon/Documents/resume/2026-07-16-zh-multimodal.tex` as the source of truth for the homepage refresh.
- Keep all overlapping homepage facts aligned with that CV, including research focus, education and internship dates, organization and role names, publication titles and venues, author order, and first/co-first-author status.
- Translate and condense the CV content for an English academic homepage without adding claims that are absent from the CV.
- If an existing publication page contains richer technical detail, retain it only where it does not conflict with the CV.

### Homepage content

- Keep the homepage entirely in English.
- Update the introduction to describe Boyuan Li as a Ph.D. student at the Gaoling School of Artificial Intelligence, Renmin University of China, advised by Prof. Ruihua Song.
- Use the research focus from the resume: multimodal generation, 3D human motion generation, and world-action models.
- Update the experience section with:
  - BeingBeyond, Research Intern, Humanoid Group, September 2025 to June 2026.
  - Microsoft Research Asia, Research Intern, Multimedia Computing Group, June 2022 to December 2022.
  - Renmin University of China, Ph.D. in Artificial Intelligence, September 2022 to present, expected June 2027.
  - Shandong University, B.E. in Computer Science and Technology, September 2018 to June 2022.
- Preserve the existing publication-card presentation and update or add the following representative work:
  - Being-M0.7
  - Robust Motion Generation using Part-level Reliable Data from Videos
  - OpenT2M
  - Two-in-One
  - Think Then React
  - Animate and Sound an Image
  - TeViS
- Do not add the resume's awards, tool stack, or phone number to the homepage.

### Downloadable resume

- Use `/Users/leon/Documents/resume/2026-07-16-zh-multimodal.tex` as the source.
- Generate a new one-page Chinese PDF with the phone number removed from the header.
- Keep the supplied source TeX file unchanged; perform the privacy edit on a temporary working copy.
- Replace `themes/fengye/source/pdf/cv.pdf` with the newly generated PDF.
- Keep the public URL `/pdf/cv.pdf`.
- Label the homepage link as `CV (Chinese, July 2026)`.

## Implementation boundaries

- Make targeted edits only to existing homepage content, relevant publication metadata/content, theme configuration, and the resume PDF.
- Do not redesign the site or add new navigation, page types, frameworks, or dependencies.
- Do not expose the phone number in generated site files or PDF metadata/text.
- Do not leave stale homepage facts that conflict with the July 2026 CV.
- Preserve unrelated user changes if any appear during implementation.

## Verification

- Compile the sanitized resume with XeLaTeX.
- Reopen the generated PDF and confirm it is a valid one-page PDF.
- Extract PDF text and confirm the phone number is absent.
- Render the PDF to PNG and visually verify that removing the number did not disturb header alignment, spacing, or page layout.
- Run a repository-wide search over source and generated public files to confirm the phone number is absent.
- Run `hexo clean` and `hexo generate` successfully.
- Inspect the generated homepage and CV link.
- Compare all overlapping homepage facts against the sanitized CV before previewing.
- Start the local Hexo server and provide the preview URL.
- Wait for explicit user approval before pushing or deploying.

## Delivery sequence

1. Implement and verify the content and PDF updates locally.
2. Start the local Hexo preview for user review.
3. Apply any requested corrections and re-verify.
4. Only after explicit approval, commit as needed, push `main`, and run the configured Hexo deployment to the `static` branch.
