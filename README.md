# Supernan AI Video Dubbing Pipeline

A modular Python pipeline that takes a Kannada training video and produces a Hindi-dubbed version with lip sync.

## Pipeline Architecture
```
FFmpeg (extract clip) 
→ Whisper medium (transcription with word timestamps)
→ NLLB-200 (context-aware Hindi translation)
→ gTTS (Hindi audio generation)
→ librosa (duration alignment)
→ Wav2Lip GAN (lip sync)
```

## Setup Instructions
1. Open `notebooks/supernan_complete_pipeline.ipynb` in Google Colab
2. Set Runtime to T4 GPU
3. Run Cell 1 — entire pipeline runs automatically
4. Run Cell 2 — downloads final video

## Dependencies
- ffmpeg
- openai-whisper
- transformers (facebook/nllb-200-distilled-600M)
- gTTS
- librosa==0.9.2
- soundfile
- Wav2Lip (https://github.com/Rudrabha/Wav2Lip)
- torch
- gdown

## Estimated Cost Per Minute at Scale
| Component | Cost per minute |
|-----------|----------------|
| Whisper transcription | ~$0.006 (OpenAI API) or $0 (open source) |
| NLLB translation | $0 (open source) |
| gTTS | $0 (free tier) |
| Wav2Lip inference | ~$0.05 (A100 GPU on cloud) |
| **Total estimated** | **~$0.056 per minute** |

At scale on spot instances (A100): ~$0.02/min

## Known Limitations
- Wav2Lip can produce slightly blurry mouth region — CodeFormer post-processing would improve this
- gTTS does not clone the original speaker's voice — Coqui XTTS-v2 would be better but requires Python < 3.12
- Translation is context-aware but not perfect for domain-specific nanny training content
- Pipeline currently processes one video at a time

## What I'd Improve With More Time
1. **Voice cloning** — Replace gTTS with Coqui XTTS-v2 on a Python 3.11 environment for speaker-matched voice
2. **Face restoration** — Add CodeFormer post-processing after Wav2Lip to sharpen faces
3. **Better translation** — Fine-tune NLLB on nanny/childcare domain vocabulary
4. **Batch processing** — Add silence-aware chunking for long videos
5. **VideoReTalking** — Use instead of Wav2Lip for higher fidelity lip sync

## Scaling to 500 Hours Overnight
1. Split video into chunks using FFmpeg silence detection
2. Deploy pipeline on AWS/GCP with A100 spot instances
3. Use a job queue (Redis + Celery) to distribute chunks across multiple GPUs
4. Process in parallel — 10 x A100 GPUs can handle ~50 hours overnight
5. Merge outputs and upload to S3
6. Estimated cost: ~$0.02/min × 30,000 mins = ~$600 total
