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
5. Edit anything after auto-fill if needed, then **🎨 Export to Canva (.pptx)** (editable in Canva) or **📄 PDF**.

All colours locked to sample: green `#3EAE5B`, orange `#F5822A`, blue `#BFE4F7`.

Local: `C:\Users\itoha\odyssey-newsletter-fully-automated\index.html`
