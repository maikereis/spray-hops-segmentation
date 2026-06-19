# Spray Hops Segmentation

Binary semantic segmentation of soil paths between hop vine rows, using a lightweight U-Net trained from scratch and a U-Net with a pretrained MobileNetV2 encoder.

---

## Requirements

- Python ≥ 3.13
- [uv](https://docs.astral.sh/uv/) (recommended) **or** pip
- A CUDA-capable GPU (tested on 8 GB VRAM); CPU inference works but is slow
- A [Roboflow](https://roboflow.com/) account and API key

---

## Installation

### With uv (recommended)

```bash
git clone https://github.com/maikereis/spray-hops-segmentation.git
cd spray-hops-segmentation

uv sync
```

`uv sync` reads `pyproject.toml`, creates a virtual environment under `.venv/`, and installs all pinned dependencies in one step.

### With pip

```bash
git clone https://github.com/maikereis/spray-hops-segmentation.git
cd spray-hops-segmentation

python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate

pip install .
```

> **PyTorch + CUDA:** the default `torch` wheel targets CPU. For GPU support install the matching CUDA wheel first — see [pytorch.org/get-started](https://pytorch.org/get-started/locally/) — then run `pip install .`.

---

## Configuration

Copy the example environment file and add your Roboflow API key:

```bash
cp .env.example .env
```

Open `.env` and fill in:

```
ROBOFLOW_API_KEY=your_key_here
```

Your key is available at **roboflow.com → Settings → Roboflow API**.

---

## Usage

The project is structured as a single annotated notebook-style script. Run it cell by cell in Jupyter or execute it top-to-bottom as a plain Python script.

### Jupyter (recommended)

```bash
# uv
uv run jupyter lab spray_hops_segmentation.py

# pip / activated venv
jupyter lab spray_hops_segmentation.py
```

### Plain Python

```bash
# uv
uv run python spray_hops_segmentation.py

# pip / activated venv
python spray_hops_segmentation.py
```

The script will:

1. Download the Spray Hops dataset from Roboflow into `data/` (skipped if the folder already exists).
2. Train **LightUNet** from scratch and save the best checkpoint to `best_lightunet_scratch.pt`.
3. Train a **U-Net with pretrained MobileNetV2** encoder and save the best checkpoint to `best_lightunet_pretrained.pt`.
4. Print a latency benchmark and a results table (Test Loss / IoU / Dice / Latency / Params).
5. Display loss curves, confusion matrices, and qualitative prediction samples.

---

## Project Structure

```
spray-hops-segmentation/
├── spray_hops_segmentation.py   # main script / notebook
├── pyproject.toml               # dependencies (managed with uv)
├── .env.example                 # API key template
├── .env                         # your key — never committed
├── data/                        # dataset, created on first run
│   ├── train/
│   ├── valid/
│   └── test/
├── best_lightunet_scratch.pt    # saved after training
└── best_lightunet_pretrained.pt # saved after training
```

---

## Results

| Model | Test IoU | Test Dice | Latency (ms/img) | Params |
|---|---|---|---|---|
| LightUNet (scratch) | 0.915 | 0.954 | 6.96 ms | 0.39 M |
| U-Net (MobileNetV2) | 0.935 | 0.964 | 10.93 ms | 6.63 M |

---

## License

Dataset: [Spray Hops](https://universe.roboflow.com/evgeny-5zgzy/spray-hops/dataset/1) by Evgeny — CC BY 4.0.  
Code: MIT.