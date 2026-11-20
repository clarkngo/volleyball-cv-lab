# volleyball-cv-lab

My personal workspace for the volleyball computer vision research project.

> **Official project repo:** _TBD (created by our advisor in the org); add the link here._
> Experiments that work move into the official repo through PRs, logged in [`promoted.md`](promoted.md).

## What the project is about

Using computer vision on volleyball video. We start with **one game and one simple rule** (for example, ball in or out, or dead-ball / point detection), publish what we find, and grow from there. A second angle is **player form and movement** for coaching feedback.

## What's here

| Path | Contents |
|---|---|
| [`notes/meetings/`](notes/meetings/) | Meeting notes, one file per meeting (`YYYY-MM-DD-topic.md`) |
| [`notes/first-rule.md`](notes/first-rule.md) | Choosing the first rule or task to tackle |
| [`notes/models.md`](notes/models.md) | Candidate vision models: what they do, where they fall short |
| [`notes/datasets.md`](notes/datasets.md) | Candidate datasets and footage: source, license, size, labels |
| [`notebooks/`](notebooks/) | One notebook per experiment |
| [`clips/`](clips/) | Manifests that point to video (URLs and timestamps). No video files. |
| [`promoted.md`](promoted.md) | Log of work moved to the official repo |

## Ground rules

- **No video files in git.** Record where footage lives (URL, timestamps, license) in `clips/manifest.csv`.
- **Note the license** of every dataset and video before using it.
- **Keep it reproducible:** each notebook says which clips, model version and settings it used.
