# SlidesBench

This repository contains slides generation examples (NL instruction and target PPTX slide), as well as scripts to process the examples.

## Installation

```bash
pip install -r requirements.txt
export PYTHONPATH=$PYTHONPATH:$(pwd)
```

## Slides Collection

We collect 10 slide decks from different domains available on [slideshare.com](https://www.slideshare.net/), each slide deck contains 23.3 slides on average.

## Example Creation: Single Slide Generation

Given a slide deck, e.g., `examples/art_photos/art_photos.pptx`, we can parse the media and generate NL instructions for each slide.

First, run `parse_media.py` to collect all the images in the given slide.

Second, to generate instructions, we start with manually writing three instructions for the first three slides, and save them in `examples/art_photos/slide_{n}/instruction_human.txt`.

Then, first run

```bash
unoconv -f jpg ${your-pptx-file}
```

to convert the pptx file into jpg files, to input to GPT api calls.

Then, run `seed_instruction.py` to generate the NL instruction for all slides, saved as `instruction_model.txt`.

```bash
python seed_instruction.py \
--pptx_path {path-to-your-pptx-file} \  # e.g., "examples/art_photos/art_photos.pptx"
--output_path {folder-path-to-output-the-instructions}  # e.g. "examples/art_photos"
```

## Canonical Program Generation

For each slide, we provide two versions of canonical program to create the slide:

- `code_pptx.py` that uses the `python-pptx` library
- `code_library.py` that uses the functions we designed in the `mylib` library

```bash
python reproduce_code.py --slides_path examples/art_photos
```

This would produce `code_library.py` under each `slide_{n}` directory under `examples/art_photos`.

### To Check the Reference Code Quality

1. Go into the one specific slide directory

```bash
cd examples/art_photos/slide_1
```

2. Run the canonical script to generate the slide

```bash
python code_library.py
```

3. Open the `output.pptx` or transform it to `output.jpg` by

```bash
unoconv -f jpg output.pptx
```
