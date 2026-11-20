# volleyball-cv-lab

My personal workspace for the volleyball computer vision research project.

**Read the notes as a website:** https://clarkngo.github.io/volleyball-cv-lab/

> **Official project repo:** _TBD (created by our advisor in the org); add the link here._
> Experiments that work move into the official repo through PRs, logged in [promoted.md](promoted.md).

## What the project is about

Using computer vision on volleyball video. We start with **one game and one simple rule** (for example, ball in or out, or dead-ball / point detection), publish what we find, and grow from there. A second angle is **player form and movement** for coaching feedback.

## Notes

| Page | Contents |
|---|---|
| [Meetings](notes/meetings/README.md) | Meeting notes, one page per meeting |
| [Choosing the first rule](notes/first-rule.md) | Candidate first tasks and how to decide |
| [Models](notes/models.md) | Candidate vision models: what they do, where they fall short, licenses |
| [Datasets](notes/datasets.md) | Candidate datasets and footage: source, labels, size, license |
| [Notebooks](notebooks/README.md) | One notebook per experiment |
| [Clips](clips/README.md) | How we record video sources (links and timestamps, no video files) |
| [Promoted](promoted.md) | Log of work moved to the official repo |

## Ground rules

- **No video files in git.** Record where footage lives (URL, timestamps, license) in `clips/manifest.csv`.
- **Note the license** of every dataset and video before using it.
- **Keep it reproducible:** each notebook says which clips, model version and settings it used.
