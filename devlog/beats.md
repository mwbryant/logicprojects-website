# logicprojects.net devlog

Personal site for Logic Projects: a Jekyll site on GitHub Pages at logicprojects.net.

## 1. Stripping the template down to the real thing (2026-10-08)

- **The ask:** "remove all the fpga placeholder text now and the grid pattern is very ugly."
- **What got in the way:** the generated template assumed an FPGA/embedded channel: an FPGA tagline, a "Subject: FPGA · embedded · sensing" readout, an HDL blurb on GitHub, and a sample post about clock-domain crossing. A 48px engineering-paper grid covered the whole background.
- **The fix:** swapped the tagline for the real one ("I make shitposts and devlogs about them."; the masthead also hides it when blank), dropped the Subject readout, deleted the sample post, removed the grid from `body`, and stopped the empty Logs list from telling visitors to "drop a markdown file in `_posts/`".
- **Dead end:** a 390px "phone" screenshot looked clipped. Headless Chrome won't go below a 500px viewport, so it was a capture artifact, not a site bug.
- **Hook:** The template was confidently about FPGAs. The channel isn't.
- **Media:** `img/01-before-home.png`, `img/01-before-post.png`, `img/01-after-home.png`.

## 2. From rust to navy (2026-10-08)

- **The ask:** "The orange corners seem off and the colors aren't exactly mine, lets go for more blue tinged scifi." Also: "hosted where they belong also doesn't sound human."
- **The fix:** dropped the orange crop marks, and swapped the warm oxide/brass palette for navy ground, electric-blue marks (`#3d8bff`) and cyan data (`#5fd8ff`). The color variables got renamed too (`--oxide` → `--mark`, `--brass` → `--value`, `--bone` → `--ink`) so the CSS stops describing colors that aren't there. The header square now glows. The Archives line now reads "Where the actual stuff lives."
- **Caught on the way:** Jekyll was about to publish `devlog/` to the live site. It's excluded in `_config.yml` now.
- **Hook:** Same layout, different planet.
- **Media:** `img/01-after-home.png` (before), `img/02-after-home.png`.

## 3. Cutting the instrument panel (2026-10-08)

- **The ask:** "remove status active and all the bottom box." Then: "I don't need an atom feed, idk what it even is."
- **The fix:** removed the Atom feed (not needed), the "Status: Active" readout under the masthead and the drawing-sheet title block at the foot (Sheet / Drawn by / Dwg LP-00 / Rev / Feed), plus the `facts` and `designation` config that only fed them. About 60 lines of CSS went with them.
- **Hook:** Every template ships with decoration you didn't ask for.
- **Media:** `img/02-after-home.png` (before), `img/03-after-home.png`.
