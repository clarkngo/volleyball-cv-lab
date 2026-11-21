← [Meetings](README.md)

# 2026-11-27 — Research review and first rule

**When:** Friday 2026-11-27, 2:00 PM Seattle time. It's the day after US Thanksgiving, so confirm the meeting is still on.
**Attendees:**
**Prepared ahead.** The "Decision" and "Notes" spaces get filled in during the meeting.

## Agenda (about 45 minutes)

| # | Topic | Time |
|---|---|---|
| 1 | Check the kickoff action items | 5 min |
| 2 | Models: what exists | 10 min |
| 3 | Datasets and footage: what exists | 10 min |
| 4 | **Decide the first rule** | 15 min |
| 5 | Team, grant and repo logistics | 5 min |
| 6 | Action items for next week | — |

## 1. Kickoff action items

- [x] Research existing vision models → [Models](../models.md)
- [x] Find volleyball datasets and videos → [Datasets](../datasets.md)
- [ ] Bring notes back to the group → this meeting
- [ ] Decide the exact first rule and approach → section 4

## 2. Models: three takeaways

Full notes: [Models](../models.md)

1. **Ball in / out is a pipeline, not one model:** track the ball → map the court to a top-down view → find the landing frame → compare with the lines. Each step has existing tools.
2. **The ball is the hard part.** It's small, fast and blurred. The **TrackNet** family is built for this, and someone has shared a **TrackNetV3 fine-tuned on beach volleyball**.
3. **Licensing:** Ultralytics YOLO is **AGPL-3.0**, which covers models we train with it. Fine for open research, but a later hosted service would need everything open-sourced or a paid license. RF-DETR, MMPose and SAM 2 are Apache 2.0.

Notes:

## 3. Datasets: three takeaways

Full notes: [Datasets](../datasets.md)

1. **No public volleyball dataset labels in / out or landing points.** We'll label some ourselves, and that's part of what we can publish.
2. **Start with two:** **VNL-STES** (8 pro matches, 1,028 rallies, timed events including *score*) and **WASB-SBDT** (per-frame ball positions).
3. **YouTube footage is a rights question.** Its terms forbid downloading. Options: YouTube's Researcher Program, Creative Commons videos, asking owners, or **filming our own matches**.

Notes:

## 4. Decide the first rule

Full comparison: [Choosing the first rule](../first-rule.md)

**Proposal:** ball in / out (lines only), from one fixed camera, at amateur or college level, with **rally detection as step zero**.

- In / out was about **40% of FIVB video challenges** before automated line calling.
- Elite events use multi-camera systems (Bolt6 + Hawk-Eye at Paris 2024). **An open, single-camera system with measured accuracy doesn't exist yet.**
- Rally detection is cheap to label and cuts matches into the clips in / out labeling needs anyway.

| Question | Proposal | Decision |
|---|---|---|
| First rule | Ball in / out (lines only); rally detection first | |
| Indoor or beach | Open: beach has the ready ball tracker; indoor has VNL-STES | |
| Definition of "in" | FIVB: any part of the ball touches the court or line at any moment of floor contact | |
| Footage | Our own fixed-camera filming, plus public datasets | |
| Success measure | Overall accuracy **and** accuracy on close calls, plus landing error in cm | |

Notes:

## 5. Team, grant and repo

Questions for the advisor:
- **Official org repo:** when will it exist, and what goes there versus personal repos?
- **Grant:** who are the ~5 core students, and what's the proposal timeline?
- **Paper:** target venue and deadline? The CVPR **CVsports** workshop published VNL-STES, so it's a natural fit.
- **Filming people:** do we need ethics review (IRB) or a consent form to film our own matches?
- **YouTube footage:** how does the university handle it for research?
- **Meeting cadence:** stay weekly, or add short check-ins?

Notes:

## 6. Proposed action items for next week

To assign owners at the meeting.

| Task | Owner |
|---|---|
| Download **VNL-STES** and **WASB-SBDT**; note camera angles, label format, frame rate, license | |
| Run the **beach TrackNetV3** on 3–5 clips; note where it loses the ball | |
| Mark court corners on one clip, compute a **homography**, plot the ball on a top-down court | |
| Read **VNL-STES** and the **tennis line-calling paper** closely; summarize methods and metrics | |
| Draft a **filming plan**: camera position, a second camera on one line, consent form | |
| Try a simple **rally detector** on one full match | |

## Decisions

-

## Action items (agreed)

- [ ] Owner: task
