# Supernan AI Video Dubbing Pipeline

A modular Python pipeline that takes an English training video 
and produces a Hindi-dubbed version with lip sync and voice cloning.

## Pipeline Architecture
FFmpeg → Whisper → IndicTrans2 → Coqui XTTS → librosa → VideoReTalking → CodeFormer

## Setup Instructions
1. Open `notebooks/colab_pipeline.ipynb` in Google Colab
2. Set Runtime to T4 GPU
3. Run cells sequentially

## Dependencies
- ffmpeg
- openai-whisper
- gdown
- torch
