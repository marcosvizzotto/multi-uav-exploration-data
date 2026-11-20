# Cooperative Multi-UAV Exploration with IPPO — Training and Evaluation Data

This repository contains the training logs, trained policy checkpoints, configuration files and evaluation results behind the experiments reported in:

- **PhD thesis:** M. R. Vizzotto, *Deep Reinforcement Learning Based Exploration Algorithm with Distributed Architecture for Multiple Unmanned Aerial Vehicles*, PhD thesis, Graduate Program in Electrical Engineering (PPGEE), Federal University of Rio Grande do Sul (UFRGS), Porto Alegre, Brazil, 2026.
- **Exploration article:** M. R. Vizzotto, E. P. de Freitas, J. Barata and C. E. Pereira, *Deep Reinforcement Learning Based Exploration Algorithm with Distributed Architecture for Multiple Unmanned Aerial Vehicles*, submitted to the *Journal of Field Robotics*. Folders `01` to `07` and `10`.
- **Case-study article (cattle detection and counting):** C. Figueiredo et al., *Herd Counting from Cooperative Multi-UAV Exploration: A Loss Analysis in Simulation*, *[venue, year]*. Folders `08` and `09`.

The source code is not part of this repository; it is available from the corresponding author upon reasonable request.

The work studies cooperative exploration of unknown environments by a team of UAVs. Each UAV runs its own policy, trained with Independent Proximal Policy Optimization (IPPO: each UAV has its own actor-critic network, with policy and value heads on a shared trunk; the UAVs do not share parameters with each other), on top of a set of classical modules: an occupancy-grid map, frontier detection and clustering, a cooperative target assignment, A* path planning and an artificial potential field with a hard safety projection. A case study applies the trained exploration policy to cattle counting on a simulated farm, with an onboard camera and a YOLO detector.

Only the runs whose numbers appear in the thesis or the article are included.

## Configurations

The configuration numbers used in the folder names and in the article are:

| Config | Target selection and safety |
|---|---|
| 0 | **Nearest-frontier baseline, no cooperation:** targets chosen only by frontier distance, size and unknown-mass bonus; no ownership, proximity-imbalance or reservation penalties, no sequential reservation, no team-overlap reward, learned direction preference off |
| 1 | Ablation baseline: Config 0 plus ownership, proximity-imbalance and reservation penalties, sequential reservation, team-overlap reward and learned direction preference |
| 2 | + teammate-avoidance term |
| 3 | + release from a stalled target |
| 4 | + wider unknown-mass bonus (neighborhood from 8 to 20 cells) |
| 5 | + mother-frontier penalty (weight 2) |
| 6 | mother-frontier penalty raised to 20 |
| 7 | + path-cost target ranking (all cooperative target-selection mechanisms) |
| Final | **Proposed:** Config 7 + hard UAV-UAV safety projection + diversity probe |

Each configuration from 2 to 7 adds one change to the previous one. All configurations share the occupancy map, the frontier module, A* planning and the obstacle safety layer.

## Contents

| Folder | What it contains | Used in |
|---|---|---|
| `01_ablation_chain_primary_seed42/` | The seven configurations of the ablation chain (Config 1 to Config 7), primary map, seed 42 | Ablation table and figure |
| `02_final_config_three_seeds/` | Final configuration, seeds 42, 7 and 123, on the primary, cubes and office maps (9 runs) | Seed-robustness and generalization tables; training-curve figures; path-efficiency and safety-filter results |
| `03_without_uav_uav_hard_projection/` | Config 7 without the hard UAV-UAV safety projection, seeds 42, 7 and 123 | UAV-UAV collision table ("without the projection" row) |
| `04_scalability_primary_seed42/` | Final configuration with 3 and 4 UAVs (the 2-UAV run is `02_final_config_three_seeds/primary_seed42`) | Scalability table and figure |
| `05_config0_vs_config7_overlap_exhaustion/` | Config 0 (nearest-frontier baseline) and Config 7, seeds 42 and 7, frontier-exhaustion termination, with per-episode logs | Team-territory overlap comparison |
| `06_nearest_frontier_baseline_ablation_protocol/` | Config 0, seeds 42 and 7, and Config 7, seed 7, under the ablation-chain protocol, with per-episode logs (Config 1 and Config 7 seed 42 are in `01_…`) | Nearest-frontier baseline comparison |
| `07_sequential_transfer_seed42/` | One policy fine-tuned in sequence: primary → cubes → office, plus the inference-only evaluation on the held-out validation map | Sequential-transfer table |
| `08_case_study_farm/` | Policy trained on the 25×25 m farm, and the cattle-counting results at 25×25, 50×50 and 100×100 m (26 missions each) | Thesis case-study chapter; case-study article |
| `09_article_detection_experiments_50x50/` | Detection experiments at 50×50 m (26 missions each): arena-trained policy, YOLO confidence 0.50/0.90, fusion radius 0.5/4.0 m | Case-study article |
| `10_article_pose_noise_obstacle_maps/` | Robustness to pose error and LiDAR ray loss on the cubes and office maps: final-configuration policies (seed 42), 30 evaluation episodes per condition (plus a first 10-episode screening with more conditions), with the final 2D map of every episode | Exploration article (robustness experiment) |

### Contents of each run folder

```
<run>/
  params/env.yaml          environment configuration used for the run
  params/agent.yaml        IPPO / skrl configuration used for the run
  checkpoints/best_agent.pt       checkpoint with the highest mean training reward (see below)
  checkpoints/agent_1000000.pt    checkpoint at the end of training (10^6 steps), when available
  events.out.tfevents.*    TensorBoard training log (scalars only)
  SOURCE_RUN.txt           original run identifier, as cited in the thesis
```

Paths in the configuration files are written relative to the original project folder.

## Software and hardware

- NVIDIA Isaac Sim 4.5.0, Isaac Lab 2.1.0
- skrl 1.4.3, PyTorch 2.5.1 (CUDA 12.4), Python 3.10
- Training and evaluation workstation: NVIDIA GeForce RTX 3060 (12 GB), Intel Core i9-12900KF, 64 GB RAM, Ubuntu Linux

## Training setup (summary)

- 2 UAVs unless stated otherwise (3 and 4 in the scalability runs), IPPO without parameter sharing between UAVs: each UAV has its own actor-critic network (two hidden layers of 128 units, ELU; policy and value heads share the trunk, skrl `separate: false`; 22,281 parameters per UAV). All UAVs use the same architecture but are trained with separate weights; the checkpoint files hold one set of weights per UAV (`uav_0`, `uav_1`, ...).
- 10^6 environment steps per run. One environment step is 1/60 s of simulated time (physics at 120 Hz, action decimation 2).
- Seeds 42, 7 and 123 where three seeds are reported; seed 42 otherwise.
- Episode time limit: 90 s on the primary and cubes maps, 120 s on the office map (other values are stated in each `env.yaml`).
- Action per UAV: horizontal velocity (vx, vy) and a direction preference (px, py) used by the target assignment. Flight altitude is fixed.

## Reading the TensorBoard logs

Open a run with `tensorboard --logdir <run folder>`, or read the scalars directly, for example with `tensorboard.backend.event_processing.event_accumulator.EventAccumulator`.

The thesis reports the value of each metric **at step 1,000,000**. The outcome rates are computed over a moving window of the last 100 finished episodes.

| Tag | Meaning |
|---|---|
| `Info / 1done_coverage_frac_window` | Coverage-completion rate: fraction of the last 100 episodes that reached the coverage threshold of the map (exhaustion-completion rate in runs that use the frontier-exhaustion criterion) |
| `Info / 1done_timeout_frac_window` | Timeout rate: fraction of the last 100 episodes that reached the time limit |
| `Info / 1done_collision_frac_window` | Collision rate: fraction of the last 100 episodes that ended in a collision |
| `Info / 4episode_obs_collisions_mean` | Mean number of obstacle collisions per episode |
| `Info / 4episode_uav_uav_collisions_mean` | Mean number of UAV-UAV collisions per episode |
| `Info / 6same_mother_frontier_conflict_frac` | Same-mother-frontier conflict rate (decision-level cooperation metric) |
| `Info / 2episode_team_overlap_ratio_mean` | Team-territory overlap ratio (execution-level cooperation metric) |
| `Info / 2action_safety_intervention_mean` | Mean intervention magnitude of the static-obstacle hard safety filter (m/s) |
| `Info / 2uav_safety_intervention_mean` | Mean intervention magnitude of the UAV-UAV hard safety projection (m/s) |
| `Info / trail_efficiency_mean` | Trailing efficiency factor η |
| `Info / 3episode_path_mean` | Mean path length per episode (m) |
| `Info / 3episode_time_mean_s` | Mean episode duration (s) |
| `Reward / Total reward (mean)` | Mean episode return (the quantity skrl uses to select `best_agent.pt`) |

### Which checkpoint is `best_agent.pt`

skrl saves `best_agent.pt` at the checkpoint step at which the mean training reward was highest so far, and writes it together with the regular checkpoint of that step. It is therefore not necessarily the last checkpoint. In the sequential-transfer runs, the checkpoint carried to the next stage was `best_agent.pt`: step 300k (primary), 700k (cubes) and 300k (office).

## Per-episode logs (`episode_logs/*.jsonl`)

One JSON object per finished episode:

| Field | Meaning |
|---|---|
| `global_step` | Training step at which the episode ended |
| `env_id` | Parallel environment index |
| `episode_length` | Episode length in steps (60 steps = 1 s) |
| `done_reason` | `coverage`, `exhaustion`, `timeout` or `collision` |
| `coverage_frac` | Fraction of the map covered at the end of the episode |
| `episode_team_overlap_ratio` | Team-territory overlap ratio of the episode |

## Sequential transfer: validation map

`07_sequential_transfer_seed42/validation_inference_episode_scalars.csv` contains the episode-level scalars logged while the office-stage checkpoint was evaluated, without further training, on the validation map, which was never used for training. Columns: `step` (simulation step), `tag`, `value`; a row is written only when the value of the tag changes. The outcome rates are over the last 100 episodes; the exhaustion-completion rate is not listed separately and equals `1 − 1done_timeout_frac_window − 1done_collision_frac_window` (in this experiment `1done_coverage_frac_window` is 0 by construction, since the coverage threshold is 1.0 and episodes end by frontier exhaustion). The final values are timeout 0.05 and collision 0.00, that is, an exhaustion-completion rate of 0.95.

## Cattle-counting results (`08_case_study_farm/cow_counting/`, `09_…/`)

One row per mission (episode). The column names are in Portuguese, as written by the evaluation code:

| Column | Meaning |
|---|---|
| `data_hora_inicio` | Start time of the evaluation process |
| `modelo_rede` | Checkpoint used |
| `mapa` | Scenario (`farm`) |
| `episodio` | Mission number within the evaluation process |
| `total_vacas` | Number of cows placed in the farm (ground truth) |
| `vacas_exploradas` | Cows located inside the ground area seen by the cameras during the mission |
| `true_positives` | Cows detected and correctly matched to a ground-truth cow |
| `false_positives` | Detections not matched to any ground-truth cow |
| `pct_mapa_explorado` | Despite its name, the percentage of cows located inside the area seen by the cameras (`vacas_exploradas / total_vacas`) |
| `pct_encontradas_na_area` | True positives as a percentage of the cows inside the area seen by the cameras |
| `pct_area_fisica` | Explored fraction of the farm: known cells of the LiDAR occupancy grid over all cells of the grid (%) |
| `recall_cow_thesis_pct` | Recall: true positives as a percentage of all cows (%) |
| `n_raw_detections` | Raw YOLO detections before fusion |
| `duplication_rate_pct` | (raw detections − fused detections) / fused detections (%) |
| `episode_step` | Mission length in steps (60 steps = 1 s) |
| `threshold` | Fusion radius (m): detections closer than this to a registered cow are merged into it |
| `yolo_confidence` | YOLO confidence threshold (when the column is present; otherwise 0.75) |

Cow positions are drawn at random once per evaluation process and kept for all missions of that process; the UAV start positions are drawn again at every mission.

- **Case study (`08_…`)**: policy trained on the 25×25 m farm (`farm_training_25x25/`), evaluated without retraining at 50×50 and 100×100 m. Settings: YOLO confidence 0.75, fusion radius 2.0 m.
- **Detection experiments (`09_…`)**: all at 50×50 m with 60 cows. `E1` uses the policy trained on the primary arena (`07_sequential_transfer_seed42/stage1_primary`, `best_agent.pt`) instead of the farm policy; `E2a`/`E2b` change only the YOLO confidence threshold; `E3a`/`E3b` change only the fusion radius. The detection settings do not affect how the UAVs fly, so `E2a`, `E2b`, `E3a` and `E3b` share the same trajectories.

## Robustness to pose error and LiDAR ray loss (`10_article_pose_noise_obstacle_maps/`)

Evaluation only; no policy is retrained. The policies are the final-configuration runs, seed 42, at step 1,000,000:

| Map | Policy | Episode time limit | Coverage threshold |
|---|---|---|---|
| cubes | `02_final_config_three_seeds/cubes_seed42` | 90 s | 0.85 |
| office | `02_final_config_three_seeds/office_seed42` | 120 s | 0.85 |

The experiment has two parts:

- **30 episodes per condition (`30_episodes/`)**, the results reported in the article: no noise, 10% and 30% LiDAR ray loss, and correlated pose error of 0.10 m / 2°, on both maps. Each condition combines 10 episodes with evaluation seed 42 and 20 episodes with evaluation seed 43. The seed-42 episodes of `nominal`, `drop30` and `corr2_010_yaw2` are the ones in `<map>/` (first part); `drop10` with seed 42 and all seed-43 episodes are in `30_episodes/<map>/s42/` and `s43/`. No seed-43 episode starts from the same pair of UAV positions as a seed-42 episode.
- **10 episodes per condition, evaluation seed 42 (`<map>/`)**: the first screening, which also includes the conditions with pose error redrawn at every step (i.i.d.), the position-only and yaw-only conditions and the correlated 0.20 m / 4° condition.

The noise is applied only to the **estimated** pose, which is used by the occupancy map, the frontier module and A*, the policy observation and the safety projections. Physics, contact sensors, the fence and the UAV-UAV collision check keep the true pose. The episode ends on the **estimated** coverage, as it would on a real UAV.

| Condition (folder suffix) | Pose error | LiDAR |
|---|---|---|
| `nominal` | none | all rays |
| `drop10`, `drop30` | none | 10% / 30% of the rays dropped at random in every scan |
| `pose005_yaw1`, `pose010_yaw2`, `pose020_yaw4` | position σ = 0.05 / 0.10 / 0.20 m and yaw σ = 1 / 2 / 4°, redrawn independently at every step (i.i.d.) | all rays |
| `pose010_yaw0` | position only, σ = 0.10 m, i.i.d. | all rays |
| `pose000_yaw2` | yaw only, σ = 2°, i.i.d. | all rays |
| `corr2_010_yaw2`, `corr2_020_yaw4` | position σ = 0.10 / 0.20 m and yaw σ = 2 / 4°, correlated in time: second-order Gauss-Markov process with time constant τ = 2 s (a smooth error, without step-to-step jitter, closer to the output of a localization filter) | all rays |

Not every condition was run on both maps; the summary files list the ones that were.

The i.i.d. conditions also corrupt the motion signals the policy observes: the per-step displacement computed from the noisy pose (about 0.14 m of noise against about 0.02 m of true motion per step) makes the stuck detector of the trailing-efficiency term flag the UAVs as stuck for most of the episode. The correlated conditions do not: at the end of the episode, the stuck counter is close to the episode length in every i.i.d. episode, and in the 70 correlated episodes it is 0 except in one (14 steps, one UAV, cubes).

### Files

```
10_article_pose_noise_obstacle_maps/
  30_episodes/<map>/SUMMARY_<map>_30ep.md      article results: rates in % with 95% Wilson interval; map metrics as
                                               mean ± population SD; agreement between the two seeds; start positions
  30_episodes/<map>/PER_EPISODE_<map>_30ep.csv one row per episode (all 30 of each condition; column src = folder)
  30_episodes/<map>/s42/, s43/                 same layout as <map>/ below, for the added episodes; npz_<cond>/ also
                                               holds epNNN_first.npz, the first map export (UAV start positions)
  30_episodes/queue.log                        start and end time of every evaluation
  <map>/SUMMARY_<map>.md              first screening, 10 episodes (mean ± population SD over episodes)
  <map>/PER_EPISODE_<map>.csv         one row per episode of the first screening, with the map-quality metrics below
  <map>/<MAP>_s42_<cond>_episodes.csv evaluator output, one row per episode
  <map>/<MAP>_s42_<cond>_env.yaml     environment configuration used (noise fields: eval_*)
  <map>/<MAP>_s42_<cond>_meta.json    checkpoint, noise settings, source commit, software versions
  <map>/<MAP>_s42_<cond>.log          console log of the evaluation
  <map>/npz_<cond>/epNNN_last.npz     final 2D map of episode NNN (last export, at most 30 steps before the end)
  <map>/prints/<cond>/epNNN_FIM.png   2D map viewer at the end of episode NNN
  figure/noise_obstacles_ep002.*      episode 2 on both maps: no noise, 30% ray loss, correlated 0.10 m / 2°
  test_correlated_noise_1episode/     one-episode check, cubes: no noise and correlated noise of first and
                                      second order (map exported every 30 steps)
```

### Map-quality metrics (`PER_EPISODE_*.csv`)

The estimated coverage overstates what was mapped when the pose is wrong, because a blurred wall marks more cells as known. Each final map is therefore compared with a **reference map** built from the final maps of the episodes with the true pose: a cell is occupied in the reference if most of those episodes that saw it marked it occupied, and free otherwise. Each true-pose episode is compared with a reference built from the others (leave-one-out). The map is in world coordinates and the scene is static, so the same cell is the same place in every episode.

- **30 episodes:** reference from `nominal`, `drop10` and `drop30`, 90 episodes per map. All 30 episodes of every condition, including the 10 from the first screening, are evaluated against this reference.
- **First screening:** reference from `nominal` and `drop30`, 20 episodes per map.

| Column | Meaning |
|---|---|
| `cov_est` | Estimated coverage: known cells / all grid cells (the quantity that ends the episode) |
| `cov_correta` | Correct coverage: known cells whose label (free or occupied) matches the reference / all grid cells |
| `parede_falsa_m2` | False wall: area marked occupied that is free in the reference (m²) |
| `livre_falso_m2` | False free: area marked free that is occupied in the reference (m²) |
| `nao_avaliavel` | Fraction of the grid that is known in the episode but was never seen in the reference |
| `done`, `len_csv` (`len`), `col` | Termination reason, episode length in steps (60 steps = 1 s), collisions |

The grid includes a 0.5 m band outside the fence and the interior of the obstacles, which are never seen. The reference covers 91.8% of the grid on cubes and 96.7% on office (30 episodes), and 89.8% and 95.9% (first screening).

## License

The data in this repository is released under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license.

## How to cite

If you use this data, please cite the thesis and the corresponding article listed at the top of this page.

Repository: https://github.com/marcosvizzotto/multi-uav-exploration-data
