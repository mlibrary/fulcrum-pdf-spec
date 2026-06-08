# Fulcrum PDF Specification
The Fulcrum PDF Specification is designed to help ebook production vendors and publishers create PDF files compatible with the Fulcrum platform's Reading System and conform to [WCAG AA](https://www.w3.org/WAI/WCAG2AA-Conformance) and [PDF/UA](https://pdfa.org/iso-14289-2-pdfua-2/) Accessibility requirements.

If you have questions, feedback, or find errors or issues with this specification, please file an issue within the project or email [fulcrum-info@umich.edu](mailto:fulcrum-info@umich.edu).

## Contents
- [1.0 How Fulcrum renders PDFs](#10-how-fulcrum-renders-pdfs)
- [1.1 Types of PDFs on Fulcrum](#11-types-of-pdfs-on-fulcrum)
- [1.2 Quick Start](#12-quick-start)
- [2. File Properties](#2-file-properties)
    - [2.1 File Naming](#21-file-naming)
    - [2.2 File Size](#22-file-size)
    - [2.3 Web Optimization (linearization)](#23-web-optimization-linearization)
    - [2.4 Image handling inside the PDF](#24-image-handling-inside-the-pdf)
    - [2.5 PDF Conformance](#25-pdf-conformance)
- [3. Fonts](#3-fonts)
- [4. Document Metadata](#4-document-metadata)
- [5. PDFs created from scanning print materials](#5-pdfs-created-from-scanning-print-materials)
    - [5.1 Scanning Guidelines](#51-scanning-guidelines)
- [6. Accessibility Requirements](#6-accessibility-requirements)
    - [6.1 Tag Structure (foundational)](#61-tag-structure-foundational)
    - [6.2 Text alternative for images](#62-text-alternative-for-images---pdf1)
    - [6.3 Bookmarks](#63-bookmarks---pdf2)
    - [6.4 Tab and Reading Order](#64-tab-and-reading-order---pdf3)
    - [6.5 Tables](#65-tables---pdf6)
    - [6.6 OCR](#66-ocr---pdf7)
    - [6.7 Headings](#67-headings---pdf9)
    - [6.8 Document Language](#68-document-language---pdf16)
    - [6.9 Page Numbering](#69-page-numbering---pdf17)
    - [6.10 Document Title](#610-document-title---pdf18)
    - [6.11 Lists](#611-lists---pdf21)
    - [6.12 Links and Link Text](#612-links-and-link-text---pdf11)
    - [6.13 Footnotes and endnotes](#613-footnotes-and-endnotes---pdf11)
    - [6.14 Language for passages](#614-language-for-passages---pdf19)

## 1.0 How Fulcrum renders PDFs
Fulcrum's in-browser PDF EReader (Fulcrum EReader) is built on [Mozilla's PDF.js](https://github.com/mozilla/pdf.js/wiki/Frequently-Asked-Questions#what-types-of-pdf-files-are-slow-in-pdfjs-can-i-optimize-a-pdf-file-to-make-pdfjs-faster). PDFs render client-side in the reader's browser on older or low-powered devices. A PDF that opens without issue in desktop Adobe Acrobat may still render slowly or incorrectly on Fulcrum if these guidelines are not followed.

## 1.1 Types of PDFs on Fulcrum
- **Monograph PDFs** - the *primary representation* of a book, displayed in the Fulcrum EReader. The full specification applies.
- **Resource PDFs** - supplemental materials attached to a book (appendices, sample chapters, scanned archival documents, etc.). The accessibility requirements still apply, but with one practical accommodation. When a Resource PDF is derived from digitized page images that cannot be made fully accessible, providing the raw OCR text as a separate file is an acceptable alternative access method. This accommodation does not apply to Monograph PDFs.

## 1.2 Quick Start
Before delivering a PDF to Fulcrum, please confirm that the file:
- Is a tagged PDF (not a flat scan and not an untagged "print-style" PDF). Exception: see Resource PDFs above.
- Use OpenType or TrueType fonts only. No Type 1 fonts.
- Has complete metadata (title, author, copyright, publisher, and subject(s)) set in the file properties.
- Has bookmarks matching the Table of Contents, using a single bookmark type throughout, with no line breaks in bookmark titles.
- Has alt text on every meaningful image and decorative images marked as artifacts.
- Has correct tag structure and reading order, verified in the Acrobat Tags panel.
- Has linearization (Fast Web View) enabled.
- Has been run through Adobe Acrobat Pro's Accessibility Checker with no failures.

If any of the above requirements cannot be confirmed, please review the relevant section below before delivering the file.

## 2. File Properties

### 2.1 File Naming
Monograph PDFs should be named with its 13-digit ISBN number.

Resource PDFs should be named using a consistent file naming convention and do not need to include a 13-digit ISBN. Please use the following rules when creating Resource PDF file names:
- Use only letters (Aa-Zz), numbers (0-9), dash (-), and underscore (_)
- Avoid any other characters, especially commas, spaces, forward-slash (/), or back-slash (\\), as these characters can cause problems on the web and in our preservation storage systems
- Be 32 characters or less

### 2.2 File Size
PDFs with a smaller file size will render faster in the Fulcrum EReader. PDF monographs greater than 250MB may experience performance issues in the Fulcrum EReader. There is no limit on total file size per book across all resources.

### 2.3 Web Optimization (linearization)
- Enable web-optimized PDF output/linearization.
- This allows the Fulcrum EReader to begin rendering before the entire file has downloaded.
- In Acrobat, use `File -> Save as Other -> Optimized PDF (Clean Up)` and confirm that "Optimize the PDF for Fast Web View" is enabled

### 2.4 Image handling inside the PDF
The following recommendations come directly from the PDF.js project's performance guidance:
- ~150 dpi is sufficient for screen reading. Higher resolutions increase file size without improving on-screen legibility.
- Use JPEG encoding for color images and photographs in the RGB color space where possible.
- Flatten transparency before export. Complex transitions and masking effects are not handled reliably by the EReader.
- Avoid PDF generators that produce ineffective output. LibreOffice, for example, creates a lot of tiny images for vector elements/pictures that it cannot represent.

### 2.5 PDF Conformance
The file must not be corrupted and must conform to the PDF 32000 specification. Non-conforming PDFs may fail in the Fulcrum EReader even if they open without issue in Acrobat.

## 3. Fonts
Use OpenType or TrueType fonts only. Type 1 (PostScript) fonts are not supported.

Adobe ended support for Type 1 fonts in Acrobat and Acrobat Reader in January 2023 ([end of support notice](https://helpx.adobe.com/fonts/kb/postscript-type-1-fonts-end-of-support.html#:~:text=Users20will%20no%20longer%20have,as%20they%20have%20all%20along)). Type 1 fonts can cause missing glyphs and text extraction failures, which break search and screen reader functionality. If source files use Type 1 fonts, convert them to OpenType before generating the PDF.

To verify fonts in Acrobat: `File -> Document Properties -> Fonts`. Every entry should show "(Embedded Subset)" or "(Embedded)". Do not rely on system fonts or font substitution.

## 4. Document Metadata
Include complete metadata in the PDF: title, author, copyright, publisher, and subject(s). The title must be the actual book title, not the filename.

In Acrobat, set these fields under `File -> Document Properties -> Description`. The title field is also used by screen readers and browsers as the document's window or tab title, so it must be populated correctly and not left as a filename.

Also, set the initial view to display the document title rather than the filename: `File -> Document Properties -> Initial View -> Show: Document Title`.

## 5. PDFs created from scanning print materials
Resource and Monograph PDF files may be created by scanning print materials when necessary. Compared to PDF files created from word processor documents, PDF files from scanned material may have increased file size and compromised viewing, printing, accessibility, or usability. Nevertheless, PDF files created from scanning are a reliable way to preserve materials when no other options are available.

### 5.1 Scanning Guidelines
- All materials should be scanned at 100% scale to the dimensions of the original.
- The PDF file should be optimized and should be in ("Searchable Image (Exact)"/"Image+Text") format.
- Perform OCR (optical character recognition) and embed the generated text in the document.
- Pages containing text and/or line art should be monochrome (black and white), 600dpi, and compressed using ITU Group IV compression.
- Oversized materials (greater than 11x16 in/28x41 cm) should be 300dpi, grayscale/color.
- Pages containing photographs and/or illustrations should be 24-bit color using the sRGB color space or 8-bit grayscale, 400dpi (or 300dpi for oversized materials greater than 11x16 in/28x41cm), and be compressed with JPEG compression using the highest quality setting.
- Missing or blank pages should be represented as blank images of the same size as the original.
- The scanned PDF still needs tags and other accessibility features as described in Accessibility Requirements.

## 6. Accessibility Requirements
The following requirements are based on [W3C WCAG 2.1 AA Techniques for PDF](https://www.w3.org/WAI/WCAG21/Techniques/#pdf). All items are required for PDFs ingested into Fulcrum.

Two additional references that can be useful are:
- [Section 508 Detailed PDF Accessibility Checklist](https://www.section508.gov/create/pdfs/)
- [HHS Guide to Tagging PDFs in Adobe Acrobat](https://www.hhs.gov/sites/default/files/pdf-tagging.pdf)

### 6.1 Tag Structure (foundational)
- Every PDF must have a structure tree (also called "tags").
- Without tags, none of the requirements below can be satisfied.
- When exporting from InDesign, enable Create Tagged PDF in the export dialog.
- In Acrobat, the tag tree can be viewed and edited under `View -> Show/Hide -> Side Panels -> Accessibility Tags`.
- Acrobat's "Autotag Document" feature produces limited results when a PDF contains complex layouts. It can be used as a starting point, but the tag tree will need manual review and correction.

### 6.2 Text alternative for images - [PDF1](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF1)
- Apply alt text to all meaningful images.
- In Acrobat, right-click a figure tag in the Accessibility Tags panel and select *Properties* to add or edit the "Alternate Text for Images" entry.
- Decorative images (rules, ornaments, background art) should be marked as artifacts rather than having an empty alt text so that screen readers can skip them entirely.

### 6.3 Bookmarks - [PDF2](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF2)
- Create bookmarks that match the book's Table of Contents (TOC).
- Use a single bookmark type throughout the document
- Mixing Named Destination and Page Number bookmarks is a known cause of TOC rendering failures on Fulcrum
- Bookmark titles must not contain line breaks; replace any newlines with spaces

### 6.4 Tab and Reading Order - [PDF3](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF3)
- Ensure correct tab and reading order
- The order of tags in the structure tree must match the logical reading order of the content, not the visual layout order.
- This is particularly important for two-column layouts, pull quotes, and sidebars.
- To set tab order in Acrobat: `Page Thumbnails -> right-click a page -> Page Properties -> Tab Order: Use Document Structure`.
- Use Acrobat's Read Out Loud feature on a sample of pages to verify reading order. (View -> Read Out Loud)

### 6.5 Tables - [PDF6](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF3)
- Use proper table elements for table markup: \<Table>, \<TR>, \<TH>, and \<TD>.
- Please do not simulate tables with tabs, spaces, or text boxes.
- Header cells should be marked as \<TH> with an appropriate Scope attribute (Row, Column, or Both).

### 6.6 OCR - [PDF7](https://www.w3.org/TR/WCAG20-TECHS/PDF7.html)
- Perform OCR on scanned PDFs to provide actual text.
- A scanned PDF without a text layer is effectively a blank document for screen reader users.
- After running OCR, proofread the recognized text on a representative sample of pages.
- Acrobat's OCR can introduce errors, particularly on title pages, tables, and margins.

### 6.7 Headings - [PDF9](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF9)
- Provide headings by marking content with heading tags (\<H1>, \<H2>, \<H3>, etc.) in the correct hierarchical order.
- Please do not skip heading levels, and do not use heading tags purely for visual styling.

### 6.8 Document Language - [PDF16](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF16)
- Set the default language for the document.
- This is required for screen readers to use the correct pronunciation.
- In Acrobat, the document language can be set under `File -> Document Properties -> Advanced -> Reading Options -> Language`.

### 6.9 Page Numbering - [PDF17](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF17)
- Specify consistent page numbering.
- PDF page labels should match the printed page numbers in the book, including front matter Roman numerals.
- In Acrobat, set page labels via the `Page Thumbnails panel: Options -> Page Labels`.

### 6.10 Document Title - [PDF18](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF18)
Specify the document title in metadata (See the Document Metadata section above).

### 6.11 Lists - [PDF21](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF21)
- Use list tags for lists: \<L>, \<LI>, \<Lbl>, and \<LBody>.
- Do not simulate lists with bullet characters and tab stops.

### 6.12 Links and Link Text - [PDF11](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF11)
- Provide links and link text using the Link annotation and /Link structure element.
- Link text should be descriptive on its own rather than generic text such as "click here" or a bare URL.

### 6.13 Footnotes and endnotes - [PDF11](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF11)
- Provide links and backlinks for footnotes and endnotes.
- References in body text should link to the note, and each note should link back to the reference point in the body.
- Use \<Note> structure elements for the notes themselves.

### 6.14 Language for passages - [PDF19](https://www.w3.org/WAI/WCAG21/Techniques/pdf/PDF19)
- Specify the language for any passage or phrase that differs from the document's main language.
- This enables screen readers to switch pronunciation correctly.
- For example, a French quotation in an English-language book should have its language set on the surrounding tag.

