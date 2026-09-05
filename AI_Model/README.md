# Secure Land Acquisition Document Extraction

## Objective
To train or fine-tune an AI model capable of intelligently reading complex Marathi land documents (like 7-12 extracts / सातबारा उतारा), understanding dense multi-column tabular layouts, seals, and handwriting, and extracting relevant entities into a verified, structured JSON format.

## Recommended Model & Training Architecture
To train reliably on **Google Colab & Kaggle Free Tiers (NVIDIA T4 / P100 ~15-16 GB VRAM)** without Out-Of-Memory (OOM) crashes:

- **Base Model**: `Qwen/Qwen2.5-VL-3B-Instruct` (or `Qwen/Qwen2-VL-2B-Instruct`)
  - **Why 3B?**: Native multimodal reasoning, strong multilingual & Indic (Marathi/Devanagari) OCR capabilities, and small enough parameter footprint to leave sufficient VRAM for high-resolution document image tokens and gradient buffers.
- **Quantization**: **4-bit NF4 Quantization** via `bitsandbytes` (`load_in_4bit=True`, `bnb_4bit_compute_dtype=torch.bfloat16`, `bnb_4bit_use_double_quant=True`).
  - Reduces model weight footprint to ~2.2 - 2.8 GB VRAM.
- **Fine-Tuning Technique**: **QLoRA (Quantized Low-Rank Adaptation)** via Hugging Face `peft`.
  - Freezes base vision/language weights, attaching lightweight trainable LoRA adapters (`r=16`, `lora_alpha=32`, targeting projection layers).
  - Trainable parameters are < 1% of total model size.
- **Document Preprocessing & Resolution Handling**:
  - Dynamic resolution scaling with `min_pixels` and `max_pixels` limits to prevent image patch explosion and GPU memory spikes.
  - Slicing/patching strategies for ultra-dense legal documents if needed.

## Security & Privacy Constraints
- **Zero Data Leakage**: Sensitive land records must **never** be sent to third-party APIs (e.g., OpenAI, Google Cloud). All AI training and inference occur locally or on controlled private infrastructure.
- **Blockchain Integration**: Extracted structured records and raw document cryptographic hashes (SHA-256) are committed to a decentralized ledger to ensure immutability, tamper resistance, and public auditability without a single point of failure.

## Repository Structure
- `notebooks/`: Jupyter notebooks ready to run on Colab / Kaggle free GPUs (e.g. `01_qwen2_5_vl_3b_qlora_colab_kaggle.ipynb`).
- `data/`:
  - `raw/`: Scanned Marathi document samples (7-12 extracts).
  - `processed/`: Processed image-text pairs and ground-truth JSON annotations.
- `models/`: Saved LoRA adapter weights and export configurations.

