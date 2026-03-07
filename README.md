# Supernan AI Video Dubbing Pipeline

A modular Python pipeline that takes an English training video 
and produces a Hindi-dubbed version with lip sync and voice cloning.

## Pipeline Architecture
FFmpeg → Whisper → IndicTrans2 → Coqui XTTS → librosa → VideoReTalking → CodeFormer

## Setup Instructions
