# Candidate models

First pass of desk research (2026-11-20): read from papers, READMEs and search summaries. **Nothing here has been run on our footage yet.** Numbers are as reported by the authors, usually on other sports.

## Quick take

- **Ball in / out is mostly a pipeline problem, not one model.** It needs four stages, each with existing tools:
  1. find the ball in every frame,
  2. find the court lines and map the image to a top-down court (a *homography*),
  3. find the frame where the ball lands,
  4. compare the landing point with the lines.
- **The ball is the hard part.** It's small, fast and blurred. General detectors (YOLO, RF-DETR) struggle; the TrackNet family was built for exactly this and already has a beach-volleyball fine-tune.
- **One camera can't measure ball height**, but in/out only needs the landing point, which is on the floor, so a floor homography is enough. Tennis work shows this works well overall but much worse for balls close to the line (see "Prior work").
- **I found no published volleyball in/out system with reported accuracy.** That gap is a possible contribution for the paper.
- **Licensing matters because a service is possible later.** Ultralytics YOLO is AGPL-3.0: fine for open research, but any hosted service would need the whole stack open-sourced or a paid license. RF-DETR, MMPose and SAM 2 are Apache 2.0.

## 1. Ball detection and tracking

| Model | What it is | Volleyball? | Where it may fall short | License | Status |
|---|---|---|---|---|---|
| [TrackNet](https://arxiv.org/abs/1907.03698) (V1/V2) | Heatmap network that takes several frames at once, built for tiny fast balls (tennis, badminton) | Not by the authors | Older; superseded by V3/V4 | check repo | read about |
| TrackNetV3 | Adds trajectory repair (*InpaintNet*) for frames where the ball is hidden | **Yes**: a [beach volleyball fine-tune](https://huggingface.co/deadfast/beach-volley-vision-models) on hand-labeled rally clips (InpaintNet not retrained) | Reported to be slow because of heavy blocks | check repo | **try first** |
| [TrackNetV4](https://arxiv.org/abs/2409.14543) (2024) | Add-on that uses motion attention from frame differences; improves V2 and V3 on tennis and shuttlecock | No | Not tested on volleyball | check repo | read about |
| [TrackNetV5](https://arxiv.org/abs/2512.02789) (Dec 2025 preprint) | Keeps the direction of motion that V4 throws away; small extra cost | No | Preprint, results not independently checked | check repo | read about |
| YOLOv8 / YOLOv10 + attention ([Miandashti et al., 2025](https://jad.shahroodut.ac.ir/article_3725.html)) | Volleyball ball detection with attention modules added to YOLO; reports better precision and recall at real-time speed | **Yes** | Single-frame detector: no motion cues, so blur is still a problem | AGPL-3.0 (Ultralytics base) | read about |

## 2. Court detection and in/out decision

| Approach | What it is | Volleyball? | Notes | Status |
|---|---|---|---|---|
| Court keypoints + OpenCV homography | Detect court corners and lines, map the frame to a top-down court | **Yes**: [Maribor 2025](https://press.um.si/index.php/ump/catalog/book/957/chapter/264) uses CNN court detection plus an OpenCV homography | Only accurate for points on the floor, which is all in/out needs ([Robo Referee](https://hsuanchl.github.io/robo_referee.html)) | read about |
| Landing / bounce detection | Find the frame where the ball hits the floor from the trajectory (direction change, speed) | Tennis | A [Technion tennis judge](https://crml.eelabs.technion.ac.il/?p=5316) used trajectory features with a CatBoost classifier | read about |
| 3D trajectory from one camera | Combine ball tracks with known court size to estimate 3D position | Cited in volleyball work | More complex; needed for touches or antenna calls, not landing in/out | later |

## 3. Player detection and tracking

| Model | What it is | Where it may fall short | License | Status |
|---|---|---|---|---|
| [RF-DETR](https://arxiv.org/abs/2511.09554) (Roboflow, 2025) | Real-time transformer detector (DINOv2 backbone); first real-time model over 60 AP on COCO | Larger XL/2XL models may use a different license | Apache 2.0 (Nano–Large) | read about |
| YOLO11 (Ultralytics) | Popular fast detector with easy training | — | **AGPL-3.0**, including trained models; paid license for closed or hosted use | read about |
| ByteTrack | Matches detections frame to frame, recovering low-confidence ones | No appearance model; [SportsMOT](https://arxiv.org/abs/2304.05170) shows similar-looking players get mixed up (64.1 HOTA) | MIT (check) | read about |
| [BoT-SORT](https://arxiv.org/abs/2206.14651) | ByteTrack plus camera motion compensation and re-identification | Tuned on pedestrians, not sports | check repo | read about |
| [GTA](https://arxiv.org/abs/2411.08216) (2024) | Post-processing that joins broken tracks; large gains on SportsMOT | Extra step, offline | check repo | read about |

Players are only needed for in/out if we want to know *who* touched the ball last. They matter more for the coaching angle.

## 4. Pose estimation (coaching angle)

| Model | What it is | Where it may fall short | License | Status |
|---|---|---|---|---|
| [RTMPose](https://arxiv.org/abs/2303.07399) / [RTMO](https://arxiv.org/abs/2312.07526) (MMPose) | Real-time multi-person pose; RTMO-l reports 74.8 AP on COCO at 141 FPS (V100) | Fast jumps and arm swings may blur | Apache 2.0 code; check weights | **try first** |
| [rtmlib](https://pypi.org/project/rtmlib/) | Runs RTMPose / RTMO without the heavy MMPose install | Inference only, no training | Apache 2.0 | try first |
| ViTPose | Transformer pose model, strong accuracy | Slower | **not confirmed** | to check |
| [MediaPipe Pose Landmarker](https://developers.google.com/edge/mediapipe/solutions/vision/pose_landmarker) | Lightweight, runs on phones | Built for one person close to the camera, not a full court | **weights license not confirmed** | to check |

## 5. Events and actions

| Model / benchmark | What it is | Notes | Status |
|---|---|---|---|
| [VNL-STES](https://hoangqnguyen.github.io/stes) (CVPR 2025 workshop) | Volleyball benchmark and model for *when and where* events happen: serve, receive, set, spike, block, score | Closest existing work to "dead ball / scoring". Struggles with spike vs block and score vs receive | **read closely** |
| VolleyVision action stage | YOLOv8m on ~14k images: block, defense, serve, set, spike | Single images, not video; weak on defense and side views | read about |

## 6. Labeling helpers

| Tool | Use for us | License | Status |
|---|---|---|---|
| [SAM 2](https://arxiv.org/abs/2408.00714) (Meta) | Click once on the ball or a player, and it follows them through the video; speeds up labeling | Apache 2.0 (code and weights) | try for labeling |

## Prior volleyball CV work

- **[VolleyVision](https://github.com/shukkkur/VolleyVision)** (bachelor thesis, ~240 stars): action recognition, ball tracking, court detection. The author lists slow ball detection (Roboflow API call per frame), fragile court detection, and **a ball in/out referee as unfinished future work**, which is exactly our first-rule candidate. Its dataset is on [Roboflow Universe](https://universe.roboflow.com/shukur-sabzaliev1/volleyball_v2) under CC BY 4.0.
- **[Maribor 2025 chapter](https://press.um.si/index.php/ump/catalog/book/957/chapter/264):** CNN court detection, ball tracking, top-down projection.
- **[Miandashti et al. 2025](https://jad.shahroodut.ac.ir/article_3725.html):** attention-augmented YOLO for volleyball ball detection.
- **[VNL-STES](https://openaccess.thecvf.com/content/CVPR2025W/CVSPORTS/html/Nguyen_VNL-STES_A_Benchmark_Dataset_and_Model_for_Spatiotemporal_Event_Spotting_CVPRW_2025_paper.html):** volleyball event spotting benchmark (see section 5).
- **[Roboflow volleyball tracking tutorial](https://roboflow.com/blog/automate-volleyball-tracking):** practical detector + ByteTrack walkthrough; useful as a quick baseline.
- **Kurowski et al. (2018):** referee-support ball tracking; commonly cited. Find the full reference.

Closest non-volleyball reference for in/out: **[monocular tennis line calling](https://arxiv.org/abs/2107.09255)** reports 99.4% accuracy on 394 bounces, but **81.8% on the 11 near the line**. Close calls are where the real difficulty is.

Surveys: [ball detection in sports (PeerJ CS, 2025)](https://pmc.ncbi.nlm.nih.gov/articles/PMC12453710), [sports event detection (2025)](https://arxiv.org/abs/2505.03991).

## Suggested first experiments

1. Run the **beach volleyball TrackNetV3** on a few of our clips. Record where it loses the ball (blocks, net, crowd background, indoor lighting).
2. Run **RF-DETR or YOLO** on the same clips for comparison, ball and players.
3. Manually mark four court corners on one clip, compute a **homography**, and plot the tracked ball on a top-down court.
4. Read **VNL-STES** closely to decide whether "rally end / scoring" is better served by its approach than in/out.

## Open questions for the team

- Indoor or beach volleyball first? The only fine-tuned ball tracker is beach.
- What camera angles does our footage have? A fixed end-line or high sideline view makes the homography much easier than broadcast footage that pans and zooms.
- How do we get ground truth for in/out? Referee calls, video challenges, or our own labels?
- Is Ultralytics' AGPL-3.0 acceptable for the project, given a service is possible later?
