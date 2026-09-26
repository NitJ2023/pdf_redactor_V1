# PDF Redactor

A browser-based tool for covering sensitive content in PDFs and exporting a flattened, redacted copy.

**Created by Balaji** · Version **2.0.2**

[Open PDF Redactor](https://nitj2023.github.io/pdf_redactor_V1/)

## Features

- Draw black boxes over text, images, signatures, or other sensitive areas.
- Search selectable text and mark matching fragments across pages.
- Remove individual boxes, undo or redo changes, and clear a page.
- Repeat boxes in the same relative positions across pages.
- Navigate between pages and zoom in for review.
- Optionally mark pages as visually reviewed.
- Export every page, including pages that need no redaction.
- Edit the export filename. By default, `statement.pdf` becomes `statement-redacted-document.pdf`.
- Clear the current session when finished.

## How to use

1. Open the tool and select **Choose PDF**, or drag a PDF onto the page.
2. Draw boxes over sensitive content, or enter a name, number, or phrase under **Find text**.
3. Inspect the boxes on each affected page. Search may cover an entire text fragment or line.
4. Use **Remove boxes**, **Undo**, or **Redo** to correct marks.
5. Check the export filename for sensitive information and change it if needed.
6. Select **Export redacted PDF**. If some page-review checkboxes are unchecked, a single confirmation lets you continue.
7. Open the downloaded PDF and visually inspect every page before sharing it.

At least one redaction box is required to export. Review checkboxes are optional; pages without sensitive content do not need boxes or a checked review box.

### Keyboard shortcuts

| Shortcut | Action |
| --- | --- |
| Ctrl/Cmd + Z | Undo |
| Ctrl/Cmd + Shift + Z | Redo |
| Left / Right arrow | Change page when not editing an input |
| Escape | Cancel a box being drawn |

## How redaction works

The tool renders each page to an image, paints the selected areas black, and builds a new PDF from those images. It does not simply place removable rectangles over the original PDF text.

When a box fully covers the intended content, those pixels are replaced in the exported image. The export does not retain the original text layer, attachments, links, forms, or metadata. Exported pages are image-only, so text selection, text search, and screen-reader accessibility are lost. Digital signatures and password protection are not preserved.

**An empty text search is not proof of complete redaction.** Unredacted text is also converted to an image. Review the output visually for missed occurrences, exposed character edges, logos, stamps, and other identifying information. Context elsewhere in a document can still reveal information that was covered.

## Privacy and security

- PDF contents and marks are processed locally in the browser.
- The application has no document-upload function, analytics, or persistent document storage.
- PDF libraries, worker code, fonts, and character maps are embedded in the HTML file.
- A Content Security Policy restricts scripts and blocks network connections made through APIs such as fetch and XMLHttpRequest.
- Visiting the hosted page contacts GitHub Pages; the hosting provider may log website access.
- Browser extensions, device security, browser caches, and downloaded files are outside the tool's control.
- **Clear session** releases the application's current document and marks. It does not delete the original file or exported copies, and it is not a guarantee of forensic memory erasure.

For offline use, download the complete `index.html` file and open it in a compatible browser. The hosted page needs an initial internet connection. Offline browser compatibility should be checked in your intended environment.

## Requirements and limitations

- Use an up-to-date desktop Chrome, Edge, or Firefox browser.
- Maximum input size: **50 MB**.
- Maximum document length: **100 pages**.
- Very large page dimensions may be rejected to limit canvas memory use.
- There is **no OCR**. Scanned pages and text embedded in images must be marked manually.
- Search results depend on the PDF's text encoding and layout. Unusual fonts, vertical text, and complex layouts require careful inspection.
- Repeating boxes uses relative page positions; check their placement when pages have different sizes or layouts.
- Image-only exports can be larger than the original and lose vector sharpness at high zoom.
- Unsaved marks are lost when the session is cleared, the document is replaced, or the page is closed or refreshed.

This project is not certified against a regulatory framework. Use with confidential or regulated documents requires your organisation's own assessment and approval.

## Deploy on GitHub Pages

1. Download the supplied `pdf-redactor.html` file and rename it to `index.html`.
2. Upload the file directly to the repository root. Avoid copying and pasting its contents through an editor that may normalise script characters or whitespace.
3. Configure GitHub Pages to publish the branch and folder containing `index.html`.
4. After deployment, refresh the page with **Ctrl + Shift + R** and check the displayed version.
5. Test opening, marking, and exporting a synthetic PDF before using real documents.

The app is distributed as a self-contained HTML file; no server-side PDF processing is required.

### Content Security Policy troubleshooting

If **Choose PDF** stays disabled, inspect the browser console. The application enables the button only after its embedded engine starts.

The CSP contains SHA-256 hashes of the exact inline script contents. Editing or reformatting a script without updating its hash can block startup. Recalculate the relevant hashes after code changes; do not remove the policy or add `unsafe-inline` to make the error disappear.

## Validation status

Testing during development included:

- Live browser search, undo/redo, navigation, and export of an earlier revision.
- Structural and visual inspection of exported PDFs to confirm flattened images and covered content.
- Local rendering, search, and export tests with the updated PDF.js engine.
- Rotated-text search and manual redaction of a scanned page.
- A 10-page document with one marked page and no review checkboxes checked.
- Filename validation, session clearing, metadata removal, and CSP hash verification.

These checks are sample-based. They do not establish that every PDF, browser, or deployment works correctly. Version 2.0.2 requires a post-deployment browser check; mobile and broad compatibility testing remain outstanding.

## Maintenance

- Keep the PDF.js main library and worker on the same version.
- Review upstream security advisories before releases and periodically thereafter.
- Update embedded dependencies and their licence notices together.
- Regenerate CSP script hashes after changing executable code.
- Re-run synthetic document tests and inspect downloaded PDFs after each update.

## Credits

**Created and maintained by Balaji.**

Built with these open-source projects:

| Project | Purpose | Licence |
| --- | --- | --- |
| [Mozilla PDF.js](https://github.com/mozilla/pdf.js) — bundled version 6.3.289 | PDF parsing, rendering, and text extraction | Apache License 2.0 |
| [pdf-lib](https://github.com/Hopding/pdf-lib) | Creation of the exported PDF | MIT |

Additional bundled fonts and decoding resources retain their upstream licence notices in the HTML distribution. This project is not affiliated with or endorsed by Mozilla, the pdf-lib authors, or OpenAI.

## Project licence

A licence for this project's own application code has not yet been specified. The repository owner should add a `LICENSE` file to state permitted use and redistribution. Third-party components remain subject to their respective licences.

## Feedback

When reporting a problem, include the app version, browser and operating system, steps to reproduce, and any console error. Use a synthetic or safely sanitised sample PDF. Do not post confidential documents, passwords, or personal information in public issues.
