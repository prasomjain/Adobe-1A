# PDF Outline Extractor

A step-by-step guide to run the PDF outline extraction program from scratch.

---

## 1. Clone the repository

```bash
git clone <your_repo_url>
cd <your_repo_dir>
```

## 2. Create input and output folders

```bash
mkdir -p input output
```

## 3. Add PDF files to the input folder

```bash
# Copy or move your PDFs into the input/ directory
# Example:
cp /path/to/your/file.pdf input/
```

## 4. (Option 1) Run with Docker (Recommended)

```bash
docker build --platform=linux/amd64 -t outline-extractor .
docker run --rm \
  -v "${PWD}/input:/app/input" \
  -v "${PWD}/output:/app/output" \
  --network none \
  outline-extractor
```

## 5. (Option 2) Run with Python directly

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python outline_extractor.py -i input -o output -j 2 -t --thumb-size 300 400
```

## 6. Check the output

```bash
ls output/*.json
ls output/thumbnails/*.jpg  # If thumbnails were enabled
cat output/stats.json       # View summary statistics
```

---

## (Optional) Command-line options

- `-i`, `--input-dir`: Input folder (default: `input`)
- `-o`, `--output-dir`: Output folder (default: `output`)
- `-j`, `--jobs`: Number of parallel workers (default: 1, use 0 for all CPUs)
- `-t`, `--thumbnails`: Generate thumbnails (saved in `output/thumbnails/`)
- `--thumb-size W H`: Thumbnail size in pixels (default: 300x400)

---

## Example output

```json
{
  "source_file": "sample1.pdf",
  "title": "50 page sample PDF.indd",
  "outline": [
    { "level": "H1", "text": "[Citation Needed]", "page": 3 },
    { "level": "H2", "text": "Boring Legal Fine Print", "page": 4 }
    
  ],
  "thumbnail": "thumbnails/sample1.jpg"
}
```

```json
{
  "num_docs": 2,
  "total_headings": 66,
  "avg_headings_per_doc": 33.0
}
```
