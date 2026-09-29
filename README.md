# Odyssey Newsletter — Fully Automated

Drop reports/documents → newsletter auto-builds.

## How it works (zero typing)
1. Open the site.
2. At the top you’ll see **⚡ FULL AUTO — Just upload reports** → drop **all** your `.docx/.pdf/.txt/.md` at once (or select multiple).
3. The system **auto-detects** each file:
   - filename/text contains `volunteer/intern/spotlight` → **Volunteer/Intern Spotlight** (name, bio, photo from doc images)
   - contains `impact/story/Lumina/Technovation` → **Impact Story**
   - otherwise → **Event page** (title = first heading or `Title:` line, body = `Summary:` or rest of doc, `img1/img2` = embedded docx images)
   - Month/year inferred from filename (`August`, `2026` etc.)
   - Cover intro + 3 tiles auto-filled from first event/volunteer/impact images
4. Preview updates live — every header still has the **embedded logo** (`C:\Users\itoha\odyssey-newsletter-fully-automated\logo.png` base64).
5. Edit anything after auto-fill if needed, then **🎨 Export to Canva (.pptx)** (editable in Canva) or **📄 PDF**. Each newsletter page becomes one PDF page / one Canva slide, with the newsletter structure and fonts preserved.

## A4 page model and pagination

Every page is A4 portrait — `210mm × 297mm` / `8.27in × 11.69in` / `595.28pt × 841.89pt` — everywhere: live preview, PDF, PPTX, Canva import, and every continuation page. A single frozen constant, `PAGE_SIZE`, is the only place dimensions are defined.

Pagination is a measurement engine, not a character counter:

- The newsletter is first built as ordered **content blocks** (cover → events → volunteer → impact → opportunities → closing).
- Each block is finished completely before the next one starts, so an event is never interrupted by another event and the order never changes.
- For each page the engine renders the real page template off-screen at the true A4 design width, measures the actual layout, and places the maximum amount of body text that fits. Whatever does not fit moves to a new A4 page.
- The title and hero image always stay with the first page of a block; only the body continues. Splitting happens at sentence boundaries (word boundaries only when a single sentence exceeds a full page), so no paragraph, sentence, image, or callout is ever clipped.
- Nothing is scaled down to make content fit: font sizes, line spacing, margins, and image dimensions are unchanged. If the text does not fit, a new A4 page is created instead.
- Page count is fully content-driven — it is not fixed at any number.

`paginateNewsletter()` is the single source of truth. The preview, the PDF, and the PPTX all consume the same page model, so they can never disagree. `assertA4Page()` rejects any page that is not A4 portrait, and `auditRenderedPages()` checks the live DOM for content that would spill past the A4 edge, so a page that is not A4 or is overflowing fails the export instead of being silently clipped.

PPTX uses PptxGenJS `sizing: cover/contain` matching the preview's `object-fit`; cropped image data from the frame editor is the canonical source for all three outputs. Export validation reports missing images, low-resolution frames, non-A4 pages, and any content that exceeds the A4 page.

## Fonts

**Nunito** for headings and **Poppins** for body text, used consistently in the preview, the PDF, and the PPTX. In the PPTX the theme is set *and* every text run carries an explicit `fontFace`, because a PPTX theme on its own does not reliably style text after a Canva import. Canva may substitute a font if Nunito or Poppins is unavailable in the account; the A4 geometry, hierarchy, and element positions are unchanged.

All colours locked to sample: green `#3EAE5B`, orange `#F5822A`, blue `#BFE4F7`.

Uploaded report text is automatically adapted into newsletter-style paragraphs: report headings and page artefacts are removed, useful facts are retained, common formal phrasing is simplified, and copy is grouped into readable newsletter-length paragraphs. The checkbox in the FULL AUTO panel controls this behavior.

## Text and image controls
- Text fields update the preview without rebuilding the form, so names and long text can be typed continuously without losing focus.
- Every image slot now has both **Adjust frame** and **Remove image** controls, including cover images/graphics, cover tiles, event photos/icons, volunteer/impact photos, and the logo.
- Removed images are kept in a short session history. Use **↩ Restore last image** to restore the deleted version; replacing an image or saving a crop also creates a restorable version.

## AI Text Optimizer & Summarizer
Use the AI panel to upload a report, choose Event / Volunteer / Impact, choose a summary length, click **Optimize & summarize**, review the result, then click **Insert into newsletter**. It preserves names, dates, numbers, quotations and outcomes while removing report clutter.

- **Select text to summarize:** after a report is uploaded, highlight any passage in the source text box and click **Summarize selected text**. Use **Select all text** to summarize the complete source, or **Clear selection** to switch back.
- Without an API connection, the privacy-friendly on-device newsletter adapter is used. For higher-quality AI rewriting, open **Optional AI connection** and provide an OpenAI-compatible endpoint, model, and temporary API key. The key is held only in the current browser tab and is not saved in project files.

Local: `C:\Users\itoha\odyssey-newsletter-fully-automated\index.html`
