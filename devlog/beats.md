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

## 4. Slides too small to read (2026-10-11)

- **The ask:** the first log entry, a technical write-up of logic-orbit, uses its five 1920x1080 architecture slides. "The graphics aren't filling the space in a way I would like, they aren't readable."
- **What got in the way:** posts are set in a 36rem (576px) column, so each slide was drawn at 30% size. Its 14-16px labels came out at about 4-5px.
- **The fix:** a `.wide` class (kramdown `{: .wide}` under the image) spans the window with the plate's 1.25rem gutter, centered, and stops at 1920px. The slides are re-rendered at 2x (3840x2160, 3.0 MB for all five) from logic-orbit's `docs/architecture/build.py` boards, so the text stays sharp on retina screens. Each slide still links to the full-size file.
- **The numbers:** slide width at a 1440px window went from 576px to 1400px (30% -> 73%); 1880px (98%) at 1920; capped at 1920 from 1960 up. No sideways scroll at 390, 768, 1280, 1440, 1920 or 2560. A phone still shows a slide at about 350px, so there the link to the full image is the way to read it.
- **Hook:** Slides made for a 1080p video don't fit in a paperback-width column.
- **Media:** `img/04-before-post.png`, `img/04-after-post.png`, `img/04-after-pipeline.png`.

## 5. A column that fits the slides (2026-10-11)

- **The ask:** "it feels like there is just too much dead space on the edges of the text on desktop."
- **What got in the way:** posts sat in a 36rem column on the left of the 58rem sheet. With the slides now spanning the window, the text hung off-centre beneath them: at a 1440px window, 276px of empty space on its left and 588px on its right.
- **The fix:** a post's column is now 46rem (`--doc`) and centred, and body text goes from 16 to 18px from 60rem up, so a line stays around 80 characters. Wide slides centre on the column with `calc(50% - bleed / 2)`, which replaced the plate arithmetic from beat 4. `overflow-wrap: break-word` lets a long URL or identifier wrap instead of pushing the page sideways.
- **The numbers:** text at 1440px: 576px starting at x=276 -> 736px starting at x=352, the same centre as the slides. A 120-character test word with no spaces made the page scroll sideways by 959px at 390px wide; it now wraps, with no sideways scroll at 390, 1024, 1440 or 1920.
- **Hook:** The diagrams got big, so the words had to stop hiding in the corner.
- **Media:** `img/05-before-post.png`, `img/05-after-post.png`; `img/05-before-sample.png`, `img/05-after-sample.png` are staged: sample paragraphs written from the drafting notes, put into a throwaway build in place of the TODOs.

## 6. Eighty percent (2026-10-11)

- **The ask:** "I still feel like my content is only using the middle third of the screen on desktop." Then: "the text should still cover 80% of the screen, also the images should be bounded to the text width."
- **What got in the way:** the screenshot that started it showed the original styles: the browser had cached `style.css` while `jekyll serve` was serving the new one. A hard refresh fixed that. Even with the new styles, at 1882px the text used 39% of the window, and everything but the slides sat in the 58rem (928px) sheet.
- **Dead ends:** beat 4's full-window slides and beat 5's centred 46rem column both gave way to one rule: the content is 80% of the window, and everything fits inside it.
- **The fix:** `--sheet` is now `max(58rem, 80vw + 2 * gutter)`, so the header, home page and posts all share an 80% column on wide screens. Slides are plain images bounded by that column (their 3840px renders fill it); `.wide` is gone. The root font size grows from 16px at a 1280px window to 20px at 1920, so type and spacing scale up with the page.
- **The numbers:** text and slides are 1152px of 1440 (80%), 1506px of 1882 (80%) and 888px of 1024 (87%, where the 58rem minimum wins). Body text in a post: 17px at 1280, 18px at 1440, 21px at 1882. The header logo lines up with the text again. No sideways scroll from 390 to 2560.
- **Hook:** Three layouts in one afternoon, and the answer was the simplest one: use the screen.
- **Media:** `img/06-before-1882.png`, `img/06-after-1882.png`, `img/06-after-1882-slide.png`, `img/06-after-1882-home.png`.

## 7. The first log goes live (2026-10-12)

- **The ask:** publish the logic-orbit post, then fill in its repo link and the two photos.
- **The fix:** the post links the newly public repo, github.com/mwbryant/fishing-minigame (132
  commits, nothing over 5 MB). The two phone photos (4080x3072) are 2400 px wide JPEGs at
  quality 85: 0.74 MB for the finished console, 0.96 MB for the failed prints, with no
  metadata. The case and trailer slides were re-rendered with the case in the print's
  brown and white.
- **Media:** `assets/img/logic-orbit/final-game.jpg`, `assets/img/logic-orbit/failed-prints.jpg`.

## 8. Photos the size of a fish hat (2026-10-12)

- **The ask:** "shrink the real images of the game and failed prints, they can be the size of the hats."
- **What got in the way:** the photos were 2400 px wide, so they filled the 80% column. The
  hat shop screenshot sits at its natural 466 px, so the photo above it was more than twice its width.
- **The fix:** a `.photo` class (kramdown `{: .photo}` right after the image) sets both photos
  to 466 px, the hat's width; `max-width: 100%` still fits them to a phone screen. The JPEGs are
  now 932x701 (2x, sharp on retina screens) at quality 85.
- **The numbers:** photo width at a 1440 px window: 1152 px -> 466 px. Files: 0.74 -> 0.24 MB
  (finished console), 0.96 -> 0.28 MB (failed prints), 1.69 -> 0.51 MB together.
- **Media:** `img/08-before-post.png`, `img/08-after-post.png`, `img/08-after-prints.png`.
