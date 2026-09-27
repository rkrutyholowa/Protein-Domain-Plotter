# Protein Domain Plotter

A single, self-contained Google Colab notebook for drawing publication-quality protein
domain diagrams — no local install, no dependencies to manage, just open and run.

Fetch a protein straight from **UniProt** (or define one by hand, or import a CSV), lay
out its annotated domains as clean rounded or square boxes on a scaled backbone, style
everything to taste, and export the result as high-resolution **PNG**, **JPG**, vector
**SVG**, and a **CSV** of the underlying data — all from an in-notebook control panel, no
coding required.

## Features

- **Multiple sources for protein data**
  - Fetch directly from the UniProt REST API by accession — single ID or a
    comma-separated batch (`P04637, P00533, Q9Y6K9`) — pulling sequence length and
    `Domain` / `Region` / `Motif` / `Repeat` features automatically.
  - Add proteins manually by name + length (or a raw sequence).
  - Import a previously-exported CSV to restore a whole session.
  - Two same-named proteins from different species (e.g. an ortholog fetched twice)
    are automatically disambiguated by organism, instead of one silently overwriting
    the other.

- **Multiple proteins, one figure**
  Every registered protein is drawn as its own row, stacked in a single diagram, so
  comparing a handful of homologs or domain architectures is a single function call.

- **Three ways to scale the x-axis**
  - `true` — real amino-acid coordinates on a shared axis, so a 1000 aa protein is
    visibly longer than a 400 aa one.
  - `equal` — every protein drawn at the same visual width.
  - `normalized` — same visual width, axis shown as % of protein length.
  - Real residue numbers can be annotated at every domain edge in any mode.

- **Fine-grained visual control**
  Domain shape (rounded, with true circular corners — including a proper half-circle
  "pill" end at maximum rounding — or plain rectangles), corner radius, domain height
  relative to the row, backbone thickness relative to the domain, fill color and
  transparency (auto-palette or per-domain override), edge color/width, fonts, figure
  width, row height, and more.

- **A legend that actually scales**
  Domains are grouped into one titled mini-legend per protein, flowing and wrapping
  automatically — tested up to several proteins with a dozen domains each with no
  clipping or overlap.

- **Feature-type filtering**
  Toggle `Domain` / `Region` / `Motif` / `Repeat` (or any custom type) on or off for
  display without re-fetching anything.

- **One-click exports**
  PNG (up to 1000 DPI), JPG, vector SVG (for Illustrator/Inkscape/etc.), and a CSV of
  every domain — each with its own download button — plus a "reset to defaults" button
  for the whole style panel.

- **Two ways to use it**
  A compact `ipywidgets` control panel for point-and-click use, or a plain Python API
  (`fetch_uniprot`, `add_protein_manual`, `add_domain_to_protein`, `plot_proteins`, ...)
  for scripting a figure exactly the way you want it.

## Quick start

1. Open the notebook in [Google Colab](https://colab.research.google.com/).
2. Run the cells from top to bottom once.
3. Use the control panel to fetch/add/import proteins, tune the style, and hit **Plot**.

Or skip the UI entirely:

```python
fetch_uniprot_multi("P04637, P00533")   # TP53 + EGFR in one call
plot_proteins(
    scale_mode="true",
    domain_shape="rounded",
    corner_radius=0.5,
    show_position_labels=True,
    save_prefix="p53_vs_egfr",
)
```

See the **Quick start** cell at the end of the notebook for more worked examples
(manual proteins, feature-type filtering, CSV round-tripping, species disambiguation).

## Requirements

Nothing beyond what the notebook installs itself in its first cell: `requests`,
`matplotlib`, `ipywidgets`, `pillow`. No local setup — it's designed to be run entirely
inside Colab.

## Notes

- UniProt fetches require network access, so they only work when actually run inside
  Colab (or any environment with outbound internet access), not in a fully offline
  sandbox.
- All rendering is done with plain matplotlib primitives (custom bezier paths for true
  circular corner rounding, a measured/wrapped legend layout, etc.) — no external
  plotting dependencies beyond matplotlib itself.
