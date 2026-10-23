# extension-pdfbox

Merge PDF files together using [Apache PDFBox](https://pdfbox.apache.org/) 3.x.
Each PDF document is treated as a **layer**, and the layers are stacked on top of
one another into a single output document.

This is the modernized successor of [jpdf](jpdf/src/docs/README.md) — same
public shape, but on the PDFBox 3.x `Loader` API.

## Usage

`PdfTemplate` exposes a fluent API:

```java
try (PdfTemplate pdf = PdfTemplate.newPdf("base.pdf")) {
    pdf.overlay("stamped.pdf");
    pdf.renderToImage(0).writeTo(Path.of("merged.png"));
    pdf.save(Path.of("merged.pdf"));
}
```

- `newPdf(resource)` — open the bottom layer.
- `overlay(resource | PDDocument | PDPage)` — stack another layer on top.
- Render to `BufferedImage` / image output, or save the merged PDF.

### Lower-level pieces

| Class | Purpose |
| --- | --- |
| `Overlay` | Low-level COS-object merging (`overlayPDF`, `OverlayCOSArray`) plus text overlay with fonts and transforms. |
| `LayoutPage` | Wraps a `PDPage` (media box, content stream, resources) so it can be used as an overlay layer. |
| `PdfTextUtils` | Cleans text for embedding — `removeUnsupportedCharacters` and a substitution map for glyphs a font cannot render. |

## Dependency

```xml
<dependency>
  <groupId>com.github.codemonstur</groupId>
  <artifactId>extension-pdfbox</artifactId>
  <version>1.0.0</version>
</dependency>
```

Requires `org.apache.pdfbox:pdfbox:3.0.0-RC1`.

## Building

```sh
make build
```

## License

MIT
