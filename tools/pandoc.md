# Pandoc

## Fedora Installation

```console
sudo dnf install pandoc texlive-metafont texlive-ec
```

## Convert Markdown to PDF

Sometimes, pandoc can tell the formats to use which makes converting easy.
However, I find that this often interprets the input format as plain text which might not be what you want:

```console
pandoc README.md -o README.pdf

```
Instead, you might want to be explicit about the input/output formats to ensure a better conversion.
In the below case, I'm specifically claiming the README.md is in Github-Flavored Markdow

```console
pandoc --from=gfm --to=pdf -o README.pdf README.md
```
