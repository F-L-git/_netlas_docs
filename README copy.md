# Netlas Documentation Portal

This repo contains a static web-site and configuration for it.

Site created using [MKDocs](https://mkdocs.org).

## Install

With Python3 installed go to project folder and run:

```bash
python3 -m venv .venv
source .venv/bin/activate # or .venv\Scripts\activate in CMD | .venv\Scripts\Activate.ps1 in PowerShell
pip install -r requirements.txt
```

**Run locally:**

```bash
mkdocs serve
```

**Build for deploy:**

```bash
mkdocs build
```

If run fails you probably have to install some external dependencies:

Try this first:
apt-get install libcairo2-dev libfreetype6-dev libffi-dev libjpeg-dev libpng-dev libz-dev
(FOR MAC: brew install cairo freetype libffi libjpeg libpng zlib)

If still fails try this:
apt-get install pngquant
(FOR MAC: brew install pngquant)


## Configure

Project is already configured with `mkdocs.yaml`

Visit https://squidfunk.github.io/mkdocs-material/ for info on theme usage.
Visit https://mkdocstrings.github.io to understand docstring documentation generation.

## Build from  scratch

To build from scratch you need to install at least this modules:

```bash
pip install mkdocs-material "mkdocs-material[imaging]" mkdocs-open-in-new-tab
pip install netlas mkdocstrings-python
```
See https://squidfunk.github.io/mkdocs-material/getting-started/
