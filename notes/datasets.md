← [Home](../README.md)

# Candidate datasets and footage

First pass of desk research (2026-11-20), from papers, project pages and search summaries. **Nothing has been downloaded or inspected yet.** Check each license yourself before use.

## Quick take

- **No public dataset labels ball in / out or landing points for volleyball.** Whatever first rule we pick, we'll need some labels of our own. That gap is also part of what we could publish.
- **Best starting points:**
  - **VNL-STES**: professional indoor matches cut into rallies, with timed events including *score*. Closest to "dead ball / scoring".
  - **WASB-SBDT volleyball split**: ball positions per frame, released with a strong ball-tracking baseline. Closest to "ball in / out".
  - **Volleyball Dataset (Ibrahim et al.)**: the classic benchmark for player actions and team activities.
- **Licenses vary a lot.** SportsMOT is non-commercial only; most Roboflow sets are CC BY 4.0; the YouTube-sourced academic sets usually don't state a license for the video itself.
- **YouTube footage is a rights question, not just a technical one.** Its terms forbid downloading. Safer routes: YouTube's Researcher Program, Creative Commons videos, permission from rights holders, or our own footage.

## 1. Academic benchmarks

| Dataset | What's labeled | Size | Footage | License | Link | Status |
|---|---|---|---|---|---|---|
| **VNL-STES** (CVPR 2025 workshop) | When and where 6 events happen: serve, receive, set, spike, block, score | 8 full VNL matches (2022–23) → 1,028 rallies, 251k frames, 6,137 events; split 811 / 102 / 115 rallies | Indoor broadcast, used with permission from Volleyball World | check project page | [project page](https://hoangqnguyen.github.io/stes) | **look at first** |
| **Volleyball Dataset** (Ibrahim et al., CVPR 2016; extended in PAMI) | 9 player actions, 8 team activities, player boxes; separate trajectory annotations by N. Ukita | 55 YouTube videos, 4,830 labeled frames (extended version ~3× larger); split 24 / 15 / 16 videos | Indoor broadcast | Code BSD-2; **data license not stated** | [GitHub](https://github.com/mostafa-saad/deep-activity-rec) | read about |
| **SportsMOT** (ICCV 2023) | Player boxes and track IDs | 240 videos across basketball, volleyball, football; volleyball clips avg 360 frames, ~12 players | Broadcast | **CC BY-NC 4.0** (non-commercial) | [GitHub](https://github.com/MCG-NJU/SportsMOT) | read about |
| **HierVolleyball** | Team activities | 1,525 frames from 15 YouTube videos | Broadcast | unknown | cited in [this survey](https://arxiv.org/abs/2206.01038) | to check |

Download notes: SportsMOT needs a CodaLab sign-up to get the links. Unofficial Hugging Face mirrors exist but still carry the original license.

## 2. Ball tracking data

| Dataset | What's labeled | Size | License | Link | Status |
|---|---|---|---|---|---|
| **WASB-SBDT volleyball split** (BMVC 2023) | Ball position per frame | 1280×720 clips adapted from an earlier dataset (source not confirmed); frame rate unknown | check repo | [GitHub](https://github.com/nttcom/WASB-SBDT), [paper](https://arxiv.org/abs/2311.05237) | **look at first** |
| Roboflow: ball-tracking-beachvolley | Ball, players (beach) | 406 images | check page | [Roboflow](https://universe.roboflow.com/unibo-vzttz/ball-tracking-beachvolley) | to check |
| Roboflow: volleyball ball detection | Ball (RGB and grayscale versions) | Frames from what may be a single 22-second YouTube clip | CC BY 4.0 | [Roboflow](https://universe.roboflow.com/daniel-eqnnu/volleyball_ball_object_detection_dataset-3jez2) | to check |
| Austrian institute volleyball video | Unlabeled; static camera behind the court | unknown | unknown | mentioned in [this article](https://towardsdatascience.com/ball-tracking-in-volleyball-with-opencv-and-tensorflow-3d6e857bd2e7) | find the source |

For designing our own labels, two non-volleyball datasets are good templates: [PadelTracker100](https://zenodo.org/records/14653706) (per-frame ball trajectories, semi-automatic labeling then manual fixes, CC BY 4.0) and [RacketVision](https://arxiv.org/abs/2511.17045) (ball positions plus trajectory forecasting across racket sports).

## 3. Community datasets (Roboflow Universe)

Small, community-made sets. Useful for quick fine-tuning, not as research benchmarks. Check label quality before trusting them.

| Dataset | Classes | Size | License | Link |
|---|---|---|---|---|
| VolleyVision `volleyball_v2` | Built from several Roboflow sets plus manual labels; ~14k images for actions: block, defense, serve, set, spike | ~14k | CC BY 4.0 | [Roboflow](https://universe.roboflow.com/shukur-sabzaliev1/volleyball_v2) |
| beach_volleyball | net, player/other, player/serving, volleyball | ? | CC BY 4.0 | [Roboflow](https://universe.roboflow.com/hemant-rawat-5lxq6/beach_volleyball) |
| beach-volleyball-actions | ball, block, defense, player, serve, set, spike | ? | check | [Roboflow](https://universe.roboflow.com/masters-jzkco/beach-volleyball-actions) |
| beach_volley_bb | ball | 102 | check | [Roboflow](https://universe.roboflow.com/anaya-brown/beach_volley_bb) |

No Kaggle volleyball video dataset turned up in this search.

## 4. Raw footage

| Source | Pros | Cons / rights | Status |
|---|---|---|---|
| **Our own match footage** | We control the camera angle (a fixed, high end-line or sideline view makes court mapping easy) and the rights | Need consent from everyone filmed; take extra care with minors | **best long-term option** |
| YouTube matches | Huge variety | [YouTube's terms](https://www.youtube.com/static?gl=FR&template=terms) forbid downloading; fair use is a legal defense, not a permission. Options: the [YouTube Researcher Program](https://research.youtube.com/policies/terms), Creative Commons-licensed videos, or asking the channel | ask advisor / university |
| Volleyball World (VNL broadcasts) | Professional, consistent cameras | VNL-STES got permission; we'd need to ask too | idea |

Record every clip in [`../clips/manifest.csv`](../clips/manifest.csv) with its source and license.

## Gaps we'd have to fill ourselves

For **ball in / out**:
- landing frame and landing point on the court,
- court corners or line positions per clip (or per camera setup),
- the in / out call, ideally matched to the referee's call.

For **dead ball / scoring**: VNL-STES already has *score* events, so check whether its labels are enough before labeling anything.

## Suggested next steps

1. Get the **VNL-STES** and **WASB-SBDT** data and look at a few clips: camera angles, label format, frame rate.
2. Confirm the **data licenses** for VNL-STES, WASB-SBDT and the Ibrahim dataset.
3. Ask the advisor how the university handles **YouTube footage** for research.
4. Plan a short **filming session** with a fixed camera on our own matches, and a consent form.

## Sources

[VNL-STES project](https://hoangqnguyen.github.io/stes) · [VNL-STES paper](https://openaccess.thecvf.com/content/CVPR2025W/CVSPORTS/html/Nguyen_VNL-STES_A_Benchmark_Dataset_and_Model_for_Spatiotemporal_Event_Spotting_CVPRW_2025_paper.html) · [Volleyball Dataset](https://github.com/mostafa-saad/deep-activity-rec) · [SportsMOT](https://github.com/MCG-NJU/SportsMOT) · [SportsMOT paper](https://arxiv.org/abs/2304.05170) · [WASB-SBDT](https://github.com/nttcom/WASB-SBDT) · [WASB paper](https://arxiv.org/abs/2311.05237) · [PadelTracker100](https://zenodo.org/records/14653706) · [RacketVision](https://arxiv.org/abs/2511.17045) · [sports action recognition survey](https://arxiv.org/abs/2206.01038) · [YouTube Researcher Program terms](https://research.youtube.com/policies/terms) · [YouTube terms](https://www.youtube.com/static?gl=FR&template=terms) · [Waterloo copyright FAQ](https://uwaterloo.ca/copyright-at-waterloo/faq-0-9) · Roboflow Universe pages linked above
