# Uttara Government College class routine

A Bengali class-routine design project for Uttara Government College's Business Studies department. The final version is a color A4 landscape image with a recreated vector emblem, aligned header, and the complete first- and second-year timetable.

![Final A4 landscape routine](previews/routine.png)

## Finished image

[Download the full-resolution PNG](output/Uttara_College_Routine_A4_Landscape_With_Logo.png).

- A4 landscape: approximately 297 × 210 mm.
- 7016 × 4961 pixels, with 600 dpi metadata and an sRGB color profile.
- Print using **A4**, **Landscape**, and **Actual size / 100%**.
- 8 mm page margins; the logo's visible top and bottom align with the two-line college heading.
- The routine has no effective-date notice, official signatures, or approval seals. The college emblem includes its original founding-year inscription.

## Build the final version

Use Python 3.12 or newer. From this repository:

```sh
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python build_routine_a4.py
```

On Windows, activate the environment with `.venv\Scripts\Activate.ps1` in PowerShell instead of `source .venv/bin/activate`.

The image is written to `output/Uttara_College_Routine_A4_Landscape_With_Logo.png`. Bengali text requires Pillow's RAQM shaping support; the renderer checks that it is available. Font files are bundled, so normal rendering does not download fonts or access the network.

To select a different output directory or omit the logo:

```sh
python build_routine_a4.py --output-dir .work/render-check
python build_routine_a4.py --without-logo
```

To recreate the earlier portrait layout:

```sh
python build_routine.py
```

The portrait image and the earlier A4 image are also included under `output/` as earlier design versions. The final A4 version is the one shown above.

## Project contents

| Path | Purpose |
| --- | --- |
| `build_routine_a4.py` | Final A4 landscape layout and print rendering |
| `build_routine.py` | Earlier portrait layout |
| `routine_transcription.json` | All 150 timetable entries and merged-cell definitions |
| `emblem_renderer.py` | Pillow/aggdraw renderer for the recreated logo |
| `assets/uttara_emblem_vectors.json` | Logo curve coordinates, colors, ellipses, and gradient stops |
| `assets/Uttara_College_Emblem_Vector.svg` | Self-contained editable vector logo master |
| `assets/NotoSansBengali*.ttf` | Bundled variable font and static weight instances |
| `assets/OFL.txt` | Font copyright notice and SIL Open Font License |
| `references/` | Original routine and emblem supplied for this project |
| `scripts/trace_emblem.py` | Rebuild the vector logo assets from the supplied emblem |
| `scripts/prepare_fonts.py` | Rebuild static fonts from the bundled variable font |
| `output/` | Full-resolution routine images |
| `previews/routine.png` | Smaller preview of the final version |

## Editing the timetable

Edit `routine_transcription.json` and rerun the renderer. Each of the five days contains five rows: first-year sections A, B, C, D, then the second year. Each row contains six periods, with the midday break handled separately by the layout.

Special values identify combined agriculture classes (`ag_ab`, `ag_cd`), second-year parallel subjects (`electives`), sports (`sports`), and co-curricular activities (`co_curricular`). A merged-cell entry is `[starting_row, period, row_count, value]`; row and period indices begin at zero.

The co-curricular activity list, section roll ranges, room labels, and period times are defined in the rendering scripts. The A4 renderer checks for text overflow and validates the timetable shape. Detailed layout measurements and preview crops are written to `.work/inspection/`.

## Rebuilding the emblem and font assets

These steps are optional; the finished vector and font assets are already committed.

```sh
python -m pip install -r requirements-dev.txt
python scripts/trace_emblem.py
python scripts/prepare_fonts.py
python emblem_renderer.py
python build_routine_a4.py
```

The emblem uses traced Bengali lettering and book/flame outlines, fitted elliptical borders, and coded torch curves and gradients. It contains 45 vector paths. The routine renderer reads vector data and does not embed or link the original logo bitmap. The SVG master is also preserved in a compressed text metadata field inside the final PNG. No generative image tools are used.

The optional tracing process reads `references/original_emblem.jpg` and places temporary masks and traces in `.work/logo-trace/`.

## Attribution

Noto Sans Bengali is by The Noto Project Authors and is distributed under the SIL Open Font License 1.1; see [`assets/OFL.txt`](assets/OFL.txt). Its source is the [Google Fonts Noto Sans Bengali directory](https://github.com/google/fonts/tree/main/ofl/notosansbengali). The bundled 400, 500, 600, and 700 weight files are static instances of that variable font.

The college emblem and supplied source documents retain their respective ownership. This repository does not assign a new license to those materials or select a license for the project code.
