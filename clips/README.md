← [Home](../README.md)

# Clips

Pointers to video, never the video itself. One row per clip in [`manifest.csv`](manifest.csv):

| Column | Meaning |
|---|---|
| `clip_id` | Short unique ID, e.g. `yt-0001` |
| `source` | `youtube`, `personal`, `kaggle`, `roboflow`, ... |
| `url` | Where the video lives |
| `start_s`, `end_s` | Clip range in seconds |
| `camera_angle` | `broadcast`, `endline`, `sideline`, `phone`, ... |
| `license` | License or permission status |
| `labels` | `none`, `partial`, `done`, or a path to the label file |
| `notes` | Anything else |

Local copies of video go in `clips/raw/` or `frames/`, which git ignores.
