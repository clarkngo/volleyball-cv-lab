← [Home](../README.md)

# Choosing the first rule

Aim: one task narrow enough to finish and publish, clear enough to label consistently.

## Candidates

| Task | What the model must do | Labeling effort | Ambiguity | Notes |
|---|---|---|---|---|
| Ball in / out | Track the ball, find its landing point, compare with court lines | | | Needs the court lines and camera angle |
| Dead ball / rally end | Detect when a rally stops | | | Could also segment full matches into rallies |
| Point / scoring | Decide which team won the rally | | | Builds on rally-end detection |
| Net touch | Detect a player touching the net | | | Small, fast events; hard to see |
| Player form (coaching) | Pose estimation on serve, spike, pass | | | Different track from officiating |

## Questions to answer
- What camera angles does our footage have? (broadcast, end line, sideline, phone)
- Does an off-the-shelf model already do this well enough?
- How many labeled examples would we need for a first result?
- How do we measure success? (accuracy against referee calls, frame error, …)

## Decision
_TBD at the next meeting._
