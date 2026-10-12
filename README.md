# Tamil magazine generator with editable live PDF preview

The app runs locally without an AI API, API key, external editor CDN, or article
upload to an outside service. Use the original templates and supplied fonts.

```powershell
cd D:\pdf
python -m pip install -r requirements.txt
python -m streamlit run app.py
```

Open http://localhost:8501. Restart Streamlit and refresh the browser after
upgrading an existing installation.

## Editing and preview

1. Select the date and upload photos as needed.
2. Paste the article into the Article editor. Its first non-empty paragraph
   becomes the front title automatically. Mark a different paragraph HEADING
   if that should be the title instead.
3. The PDF pages update automatically after a short typing pause. These are
   raster previews of the actual PDF renderer's output, including the chosen
   images, fonts, cropping, pagination and date. They are not an approximate
   HTML imitation of the PDF.
4. Double-click text on a preview page to edit its underlying article block
   directly on the page. Add/remove words, use Shift+Enter or LINE BREAK for
   an explicit line break, and use CENTER, LEFT or JUSTIFY for alignment.
   Click outside the edit area or press Escape to finish and see the layout.
   This edits the generated article; it does not edit arbitrary existing PDF
   files or allow dragging the template artwork.
5. HEADING, SUBHEADING, BULLET, NUMBER, BOLD and CLEAR work in both the source
   editor and the page preview. Select a paragraph or heading, choose a size
   (8-96 pt) and click APPLY SIZE, or choose a text color and click APPLY COLOR.
   These controls also work on the preview page. CLEAR restores ordinary body
   text and removes custom size/color.
6. Click GENERATE PDF, then DOWNLOAD PDF. Export uses the same PDF bytes as
   the successful preview. Edits or date/photo changes invalidate the previous
   download until another generation. Preview refreshes do not create files
   in `output`; explicit generation saves a uniquely named PDF there.

Body paragraphs default to justified alignment inside an area centered on the
page. Lists have hanging indentation; subheadings default to left alignment.
Front title lines are always centered. Explicit line breaks are retained,
including titles spanning three lines. Long titles are fitted automatically
inside the same front title area with bounded type reduction (42 to 24 pt by
default, or using the chosen size as the upper bound). Titles exceeding that area produce
an edit prompt, rather than touching the artwork.

Dates and page numbers are centered horizontally and vertically by their
visible rendered ink, not just their font advance or textbox starting point.
Content numbering starts at Page-2. The front reference has no date/header,
so those are not added to the front page. Dates follow the reference format,
e.g. `15-அக்டோபர்-2026`. Curly quotes are converted to straight quotes supported
by the supplied font.

## Five fixed photo slots

| Upload | Destination | Adjustment |
| --- | --- | --- |
| Front left photo | Tilted left frame | Independent zoom/horizontal/vertical crop |
| Front center photo | Tall center frame | Independent zoom/horizontal/vertical crop |
| Front right photo | Tilted right frame | Independent zoom/horizontal/vertical crop |
| Small front photo | Shaped frame below the center photo | Independent zoom/horizontal/vertical crop |
| Oval photo on all content pages | Bottom oval on every page after the front | Independent crop repeated throughout |

PNG, JPEG and WebP are supported. EXIF orientation is applied. Photos are
masked to the template's actual frame interiors, preserving its borders,
rotation, shapes and decorations. Open Image movement / crop controls and
choose one of its five photo tabs. Each has zoom, horizontal and vertical crop
controls. They move/scale the image *inside* its existing frame. Frame positions
and dimensions remain fixed. Uploaded photos remain
selected while replacing the article or changing the date in the same session.
Slots without uploads keep the original blank placeholder.

## Project files

The repository includes `app.py`, `photos.py`, `requirements.txt`, the editor,
photo masks and geometry, publication artwork, supplied fonts, and both templates.
Bold-font assets and the `output` directory are generated automatically.
Backups, reference PDFs, tests, and temporary exports are excluded.

Keep `editor`, `assets`, `photos.py`, templates and fonts with `app.py` when
copying the project. The `.orig` font suffix is accepted as provided.

## Reference font limitations

`full.pdf` is flattened into image layers. Its hidden OCR text has unreliable
font coordinates/colors and corrupted Unicode extraction. All six reference
pages were rendered for visual inspection. The separate supplied content
template is 260.64 x 347.52 pt; it is transformed into the reference page size,
595.20 x 841.92 pt, without replacing its original PDF artwork. Photos are
the newly requested overlays; all other template design remains fixed.

The reference identifies Sundaram-03 for the body and Tamil006 for subheadings.
Their embedded copies are incomplete PDF subsets with blank outlines for many
new letters. The complete supplied Sundaram-01 is used explicitly until full
original files are available. The supplied legacy Tamil013 title font is used
with Unicode-to-TAM conversion. Its weight differs from the flattened visible
reference title. Exact typography matching therefore remains limited by those
missing original fonts; the Font information expander explains the fallback.

Add these complete original files for closer reference typography:

```text
fonts\Uni Ila.Sundaram-03.ttf
fonts\TAM-Tamil006 Normal.ttf
```

The app selects them automatically after checking for incomplete subsets. No
font-family selectors are exposed. Unsupported characters still produce a clear error
instead of silent substitution. Bold uses the chosen font's own outlines with
synthetic weight. The article's default line advance is the measured page-2
value, 35.001 pt. Reference subheading blue is `#20429B`, and bullet brown is
`#9F4932`.
