# dictate.sh

Standalone, low-latency speech transcription for Apple Silicon.

`dictate.sh` uses MLX for fast, local ASR with VAD-based turn detection, plus optional
LLM intent analysis. It ships as a single Python script with inline dependencies
so you can run it with `uv` and start talking.

## Features

- Low-latency, rolling-window ASR (MLX)
- Voice activity detection (VAD) for turn boundaries
- Optional intent analysis with a local LLM
- Live terminal UI (status, transcript, stats) via Rich
- Works offline after models are downloaded

## Requirements

- macOS on Apple Silicon (MLX)
- Python >= 3.10
- `uv` installed
- Microphone permission granted to your terminal

## Quick start

```bash
uv run qwen3.py
```

With intent analysis:

```bash
uv run qwen3.py --analyze
```

Choose a different ASR model:

```bash
uv run qwen3.py --model mlx-community/Qwen3-ASR-1.7B-8bit
```

List audio input devices:

```bash
uv run qwen3.py --list-devices
```

Use a specific input device:

```bash
uv run qwen3.py --device 3
```

## CLI options

- `--model`: ASR model (default: `mlx-community/Qwen3-ASR-0.6B-8bit`)
- `--language`: Transcription language (default: `English`)
- `--transcribe-interval`: Seconds between updates (default: `0.5`)
- `--vad-frame-ms`: VAD frame size (10/20/30, default: `30`)
- `--vad-mode`: VAD aggressiveness 0-3 (default: `2`)
- `--vad-silence-ms`: Silence to finalize a turn (default: `500`)
- `--min-words`: Minimum words to finalize a turn (default: `3`)
- `--analyze`: Enable LLM intent analysis
- `--llm-model`: LLM model to use for analysis (default: `mlx-community/Qwen3-0.6B-4bit`)
- `--list-devices`: List audio input devices
- `--device`: Audio input device index

## Recommended models

ASR (MLX Qwen3-ASR):

- `mlx-community/Qwen3-ASR-0.6B-4bit`: fastest, lowest quality
- `mlx-community/Qwen3-ASR-0.6B-8bit`: good balance (default)
- `mlx-community/Qwen3-ASR-0.6B-bf16`: higher quality, more RAM
- `mlx-community/Qwen3-ASR-1.7B-8bit`: higher quality, slower

LLM (for `--analyze`):

- `mlx-community/Qwen3-0.6B-4bit`: fastest, lowest RAM (default)
- `mlx-community/Qwen3-1.7B-4bit`: better quality, slower
- `mlx-community/Mistral-7B-Instruct-v0.2-4bit`: heavier
- `mlx-community/Llama-3.1-8B-Instruct-4bit`: heavier

## UI notes

- The Rich live UI renders on stderr to keep stdout clean for scripting.
- If you want a plain, stdout-only mode, ask and I can add a `--no-ui` flag.

## Troubleshooting

- Too many short turns: increase `--vad-silence-ms` or lower `--vad-mode`.
- No audio: check mic permissions or try `--list-devices` + `--device`.
- Laggy output: reduce `--transcribe-interval`.

## Logging

- Set `LOG_LEVEL=DEBUG` for verbose logs.
- Hugging Face HTTP request logs are suppressed by default.

## License

MIT. See `LICENSE`.
