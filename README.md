# Hastings (1970): presentation package

An English teaching presentation for two presenters, covering Sections 1, 2.1 and 2.2 of Hastings (1970).

## Deliverables

- `Hastings_1970_presentation.pptx`: 18 main slides and 5 backup slides, with English speaker notes embedded in PowerPoint's Notes pane.
- `Hastings_1970_slides.pdf`: matching slide PDF for sharing or projection.
- `Hastings_1970_speaker_notes.pdf`: printable spoken script, timing, handoff, audience prompts and backup answers.
- `speaker_notes.md`: editable source for the speaker notes.

Presenter A delivers slides 1–9 in 21 minutes. Presenter B delivers slides 10–18 in 21 minutes. The 42-minute estimate includes equation reading, questions to the audience and pauses. If the allocation includes a separate Q&A, rehearse a 37–39-minute version using the cuts suggested at the end of the notes. Replace the Presenter A/B labels with your names in the title slide and notes.

The main slides are static, with no click-to-reveal animations. Text, shapes, arrows and matrix brackets are editable in PowerPoint. Equations are rendered images; change their expressions in the generator and rebuild to edit them. The deck uses Arial and Courier New; the matching PDF embeds metric-compatible Liberation fonts.

## Scope and source

W. K. Hastings (1970), “Monte Carlo sampling methods using Markov chains and their applications,” *Biometrika* 57(1), 97–109. DOI: https://doi.org/10.1093/biomet/57.1.97.

Assigned material: Sections 1, 2.1 and 2.2, printed pages 97–101. The two-state example and asymmetric-proposal variation are teaching examples, not numerical examples from the original paper. The recorded trajectory is an illustration, not an empirical simulation result. Deeper convergence theory and detailed-balance derivations are kept brief in the main talk, consistent with the instructor's follow-up lecture.

## Rebuild

From this repository, with Python 3.12 and the dependencies in `source/requirements.txt`:

```bash
python source/build_presentation.py
```

The command recreates the PPTX, both PDFs, and `source/slide_manifest.json`. It reads `speaker_notes.md` and embeds the matching notes into all 23 slides. `.build/` contains disposable rendering assets. The generator needs Liberation fonts under `/usr/share/fonts/truetype/liberation2` or `/usr/share/fonts/truetype/liberation` and does not require network access.

## Validation

The generated PPTX and PDFs were opened programmatically; all 23 slides have embedded notes. The matching slide PDF was visually inspected. The example's stochastic matrix, detailed balance, stationary distribution, asymmetric-proposal correction and accepted-move warning were checked numerically. No live simulation results are claimed.
