← [Home](../README.md)

# Choosing the first rule

Aim: one task narrow enough to finish and publish, clear enough to label consistently.

First pass of desk research (2026-11-20), from rulebooks, officiating news and papers. Nothing tested yet. **The decision is the team's; this page is input for the meeting.**

## Recommendation (for discussion)

**Ball in / out, from one fixed camera, at amateur or college level.** Build **rally detection** first as a stepping stone.

Why:
- **It matters to officials.** Before automated line calling, in/out made up about **40% of video challenges** and about 25% of challenge processing time at FIVB events ([FIVB / Paris 2024 announcement](https://asianvolleyball.net/new/the-fivb-and-paris-2024-announce-best-in-class-officiating-technology-partnership-for-the-olympic-games-paris-2024/)).
- **It's objective.** The rule is about where the ball touches the floor, not a judgment about intent or technique.
- **The elite version exists, the affordable one doesn't.** Paris 2024 used Bolt6 ball tracking with Hawk-Eye, which relies on many calibrated cameras. Amateur, school and college matches have line judges or nothing. **An open-source, single-camera in/out system with measured accuracy** is a clear, publishable gap; we found no published volleyball in/out system with reported accuracy (see [Models](models.md)).
- **The pieces exist.** Ball tracking (TrackNet family, including a beach-volleyball fine-tune), court mapping (homography) and bounce detection all have prior work to build on.

Why rally detection first:
- It's cheap to label (just start and end times), and similar methods reach about 95–98% in racket sports.
- It cuts long match videos into rallies, which is what in/out labeling needs anyway: every rally ends with a landing, a fault or a ball out of play.
- VNL-STES already has rally-level data (see [Datasets](datasets.md)).

## What the rule actually says

FIVB *Official Volleyball Rules*, chapter 8 "States of play" ([as reproduced by WorldOfVolley](https://www.worldofvolley.com/latest_news/71278/official-volleyball-rules-part-18-playing-actions-states-of-play.html); check the current [2025–2028 edition](https://www.fivb.com) for exact wording):

- **8.3 Ball "in":** the ball is in if, at any moment while it's touching the floor, **any part of it touches the court, lines included**.
- **8.4 Ball "out":** the ball is out if, among other cases, it lands **completely outside the lines**, hits the antenna, or crosses the net outside the antennas.

Two details that matter for the model:
- **"Any moment of contact," not "first point of contact."** The ball squashes and slides, so its footprint is larger than one point. A model that predicts a single landing point will be least reliable exactly on close calls. Some recreational rulebooks use "first contact" instead, so **choose and state one definition** ([discussion of the codes](https://www.referee.com/blurred-lines-volleyball-court-boundaries/)).
- **Ball in / out at the lines is a floor problem; antenna and crossing-space calls are not.** Those need 3D position near the net. Keep them out of the first version.

## Candidates compared

| Task | What the model must do | Labels needed | Ambiguity | Prior work | Fit as first rule |
|---|---|---|---|---|---|
| **Ball in / out** (lines only) | Track ball → map court → find landing frame → compare with lines | Landing frame, landing point, in/out call, court corners per camera setup | Low, except close calls | Tennis line calling, volleyball court mapping; no volleyball in/out accuracy published | **Strong** |
| **Rally start / end** | Classify each moment as rally or not | Start and end times only | Low | Racket sports 95–98%; one volleyball paper (Mori & Sawano); VNL-STES rally data | **Strong as a first step**, weak as a paper on its own |
| **Point / scoring** | Decide which team won the rally | Winner per rally (or read the scoreboard) | Low | Scoreboard reading is reported at 98.5% for racket-sport segmentation; VNL-STES has *score* events | Good, but a scoreboard reader may beat a vision model, which makes it less interesting |
| **Block touch** | Detect whether the ball grazed a blocker's hands | Touch / no touch per attack | **High**: tiny deflections | Multi-camera research system (2018); Italy's Serie A uses 18 cameras | Too hard first. It's also the **call referees get wrong most**: highest challenge success rate at Rio 2016 |
| **Net touch** | Detect a player touching the net between the antennas | Touch / no touch | High; occlusion | Mostly sensors (e.g. a patented net sensor), little vision work | Too hard first |
| **Player form** (coaching) | Pose estimation on serve, spike, pass | Pose (off the shelf) + expert form ratings | High: "good form" is subjective | RTMPose / RTMO etc. work out of the box | Separate track; good second project |

## How the official challenge system frames it

- FIVB's 2026 video challenge regulations list **ball in/out** (only where no automated system is in use) and **block touch** among the challengeable calls. Earlier versions also covered **net faults** and **antenna touches** ([FIVB VCS regulations 2026](https://www.fivb.com/wp-content/uploads/2024/05/VCS-Regs-2026_clean-v2.pdf)).
- Teams get a limited number of challenges per set and keep one only if it succeeds.
- A study of all challenges at Rio 2016 (76 matches) found **block-touch challenges succeeded most often**, which suggests they're the hardest call for referees ([Journal of Volleyball Sciences, 2017](https://www.jstage.jst.go.jp/article/jsvr/19/1/19_51/_article/-char/en)).

Possible framing for the paper: in/out is the **most frequent** challenge, block touch the **hardest**. We start with the most frequent one.

## Ground truth: how would we know we're right?

| Option | Pros | Cons |
|---|---|---|
| Our own labels from slow-motion review | Full control, any footage | Labor; our own close calls are uncertain |
| Referee calls in the video | Free | Referees are wrong sometimes, which is the whole point |
| Video challenge outcomes (pro broadcasts) | High-quality truth on close calls | Few per match; rights to the footage |
| A second, close-up camera on the line | Near-certain truth for our own filming | Needs our own filming setup |

A fixed camera of our own plus a second camera on one line would give clean ground truth for close calls.

## How to measure success

- **Overall accuracy** of in/out calls, and **accuracy on close calls** separately (for example, landings within 10 cm of a line). The tennis line-calling paper dropped from 99.4% overall to 81.8% near the line, so report both.
- **Landing-point error** in centimeters on the top-down court.
- **Timing:** can it run fast enough for a live call, or only for replay?

## Questions to settle at the meeting

1. Ball in / out first, with rally detection as step zero? Or another candidate?
2. Indoor or beach? Beach has the only ready ball-tracking fine-tune; indoor has VNL-STES and the classic datasets.
3. "Any contact" (FIVB) or "first contact" definition of in?
4. Can we film our own matches with a fixed, high camera, and get consent from players?
5. What accuracy target would make this worth publishing?

## Sources

[FIVB / Paris 2024 officiating technology](https://asianvolleyball.net/new/the-fivb-and-paris-2024-announce-best-in-class-officiating-technology-partnership-for-the-olympic-games-paris-2024/) · [FIVB rules, chapter 8, via WorldOfVolley](https://www.worldofvolley.com/latest_news/71278/official-volleyball-rules-part-18-playing-actions-states-of-play.html) · [Referee.com on boundary codes](https://www.referee.com/blurred-lines-volleyball-court-boundaries/) · [FIVB VCS regulations 2026](https://www.fivb.com/wp-content/uploads/2024/05/VCS-Regs-2026_clean-v2.pdf) · [Rio 2016 challenge study](https://www.jstage.jst.go.jp/article/jsvr/19/1/19_51/_article/-char/en) · [Mori & Sawano, rally start and spike timing](https://ipsj.ixsq.nii.ac.jp/record/236210/files/IPSJ-Z86-7U-06.pdf) · [EventAnchor](https://arxiv.org/abs/2101.04954) · [table tennis rally detection](https://arxiv.org/abs/2002.08097) · [badminton rally detection](https://research.monash.edu/en/publications/a-comparative-analysis-of-deep-learning-models-and-gradient-compu/) · [SmartTennisTV](https://arxiv.org/abs/1801.01430) · [multi-camera block touch (2018)](https://czasopisma.pan.pl//Content/115329/PDF/main.pdf) · [Lega Volley video check](https://legavolley.it/video-check-system-2012/?lang=en) · [net touch sensor patent](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/4398724) · [tennis line calling](https://arxiv.org/abs/2107.09255)
