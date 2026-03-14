---
pretty_name: MMOU Videos
language:
  - en
license: apache-2.0
tags:
  - multimodal
  - video
  - audio
  - audio-visual
  - benchmark
  - long-video
---

<p align="center">
  <img src="docs_assets/logo.webp" alt="MMOU logo" width="112">
</p>

# MMOU Videos

> **This repository solely hosts the raw video files and captions for the MMOU benchmark. For the actual dataset, questions, evaluation scripts, and full documentation, please visit [`nvidia/MMOU`](https://huggingface.co/datasets/nvidia/MMOU).**

## What This Repository Contains

This repository is the storage companion for MMOU. It contains the heavy media assets needed to run the benchmark:

- **Videos:** 9,038 real-world videos stored as MP4 files and sampled at 720p.
- **Captions (`MMOU_Captions.jsonl`):** detailed audio-visual captions describing visuals, speech, sound, and music.

The caption file is intended **only** for the open-ended evaluation judge. It should **not** be provided to models being evaluated on MMOU.

## What This Repository Does Not Contain

This repository does **not** contain the benchmark logic itself. It does not host:

- question-answer annotations
- multiple-choice options
- evaluation scripts
- leaderboard tables
- methodological documentation

For all of that, go to [`nvidia/MMOU`](https://huggingface.co/datasets/nvidia/MMOU).

## File Layout

Videos are stored as MP4 files named by source video ID:

```text
<youtube_id>.mp4
```

Example:

```text
Rja2JeIaH_c.mp4
```

## Usage

You can load the video repository with the Hugging Face `datasets` library:

```python
from datasets import load_dataset

videos = load_dataset("sonalkum/MMOU-Videos", split="train", streaming=True)
sample = next(iter(videos))

print(sample)
```

If you need the caption file locally, load it separately as JSONL:

```python
from datasets import load_dataset

captions = load_dataset("json", data_files="MMOU_Captions.jsonl", split="train")
print(captions[0])
```

## Notes for Evaluation

- Use the videos in this repository together with the benchmark annotations from [`nvidia/MMOU`](https://huggingface.co/datasets/nvidia/MMOU).
- Use `MMOU_Captions.jsonl` only for the open-ended judge pipeline.
- Do not feed the captions to the model under evaluation if you want a valid MMOU result.

## License

Apache License 2.0

## Citation

If you use MMOU, please cite:

```bibtex
@misc{goel2026mmou,
  title={MMOU: A Massive Multi-Task Omni Understanding and Reasoning Benchmark for Long and Complex Real-World Videos},
  author={Arushi Goel and Sreyan Ghosh and Vatsal Agarwal and Nishit Anand and Kaousheik Jayakumar and Lasha Koroshinadze and Yao Xu and Katie Lyons and James Case and Karan Sapra and Kevin J. Shih and Siddharth Gururani and Abhinav Shrivastava and Ramani Duraiswami and Dinesh Manocha and Andrew Tao and Bryan Catanzaro and Mohammad Shoeybi and Wei Ping},
  year={2026},
  howpublished={Project page: https://huggingface.co/datasets/nvidia/MMOU},
  note={Preprint}
}
```
