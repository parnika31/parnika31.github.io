---
layout: archive
title: "Robot Learning — Imitation Learning on the SO-101 Arm"
permalink: /robot-learning/
author_profile: true
---

A study on deploying modern imitation-learning policies, **ACT**, **π0.5**, and **SmolVLA**, on a physical **SO-101** robotic arm. I collected teleoperated demonstrations, fine-tuned each policy, and evaluated the resulting policies on the real arm.

**ACT** is trained from scratch on a separate 50-demonstration single-object task. **π0.5 carries the quantitative study**: 143 individually logged rollouts on a harder, language-conditioned multitask setup, reported a controlled ablation that isolates *where* placement fails and *why*. **SmolVLA** is trained on the same 192 demonstrations as π0.5, but with its vision backbone frozen, and is included as a qualitative comparison point rather than a measured one.

<style>
.rl-note { font-size: 0.85em; color: #6b7280; }
.rl-wrap { overflow-x: auto; margin: 1.2em 0; }
table.rl { border-collapse: collapse; width: 100%; font-size: 0.9em; }
table.rl th, table.rl td { border: 1px solid #e6e8eb; padding: 0.5em 0.7em; text-align: left; vertical-align: top; }
table.rl th { background: #f6f7f9; font-weight: 600; }
table.rl td.num { text-align: right; font-variant-numeric: tabular-nums; white-space: nowrap; }
.rl-finding { border: 1px solid #d6e2f2; background: #f4f8fd; border-left: 4px solid #1a5fb4; border-radius: 8px; padding: 0.9em 1.1em; margin: 1.3em 0; }
.rl-finding h4 { margin: 0 0 0.4em; color: #1a3a63; }
.rl-badge { display: inline-block; font-size: 0.72em; font-weight: 600; letter-spacing: 0.04em; text-transform: uppercase; padding: 0.12em 0.6em; border-radius: 999px; }
.rl-badge.pending { background: #fff3e0; color: #9a5b00; border: 1px solid #f0d0a0; }
.rl-badge.qual { background: #eef0f3; color: #57606a; border: 1px solid #dde0e4; }
.rl-badge.done { background: #e7f5ec; color: #1f7a45; border: 1px solid #bfe3cc; }
.video-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 1.1em; margin-top: 1em; }
.video-card { border: 1px solid #e0e0e0; border-radius: 8px; overflow: hidden; background: #fff; }
.video-card .thumb { position: relative; aspect-ratio: 16 / 9; background: linear-gradient(135deg, #f4f5f7 0%, #e6e9ee 100%); display: flex; flex-direction: column; align-items: center; justify-content: center; color: #8a919c; }
.video-card .thumb .play { width: 46px; height: 46px; border-radius: 50%; background: rgba(0,0,0,0.08); display: flex; align-items: center; justify-content: center; font-size: 1.1em; margin-bottom: 0.4em; }
.video-card .thumb .soon { font-size: 0.75em; letter-spacing: 0.07em; text-transform: uppercase; }
.video-card .meta { padding: 0.7em 0.9em 0.9em; }
.video-card .meta h3 { margin: 0 0 0.25em; font-size: 0.95em; }
.video-card .meta p { margin: 0; font-size: 0.82em; color: #6b7280; }
.video-card video { width: 100%; height: auto; display: block; background: #000; }
/* Typography: this page uses the `archive` layout, so the wrapper is
   .archive (not .page__content, which site-wide _custom.scss targets).
   Prose runs the full content width; only size, leading and colour are set. */
.archive p,
.archive ul,
.archive ol { font-size: 0.95em; line-height: 1.7; color: #383d43; }
.archive li { line-height: 1.7; color: #383d43; }
.archive strong { color: #23272c; font-weight: 600; }
.archive h2 { font-size: 1.45em; letter-spacing: -0.005em; margin-top: 1.9em; }
.archive h3 { font-size: 1.15em; margin-top: 1.6em; }
.archive h4 { font-size: 1.02em; margin-top: 1.4em; }
/* small print keeps its smaller scale */
.archive p.rl-note,
.archive .rl-note { font-size: 0.85em; line-height: 1.6; color: #6f757d; }
.archive .video-card .meta p { font-size: 0.85em; line-height: 1.5; color: #6b7280; }
.archive .fig-grid figcaption { font-size: 0.82em; line-height: 1.5; color: #6b7280; }
.archive table.rl td,
.archive table.rl th { font-size: 0.9em; line-height: 1.45; color: #383d43; }
.archive table.rl th { color: #23272c; }
.fig-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1em; margin: 1.2em 0; }
.fig-grid figure { margin: 0; }
.fig-grid img { width: 100%; height: auto; display: block; border: 1px solid #e2e5e9; border-radius: 8px; }
.fig-grid figcaption { font-size: 0.8em; color: #6b7280; line-height: 1.45; margin-top: 0.45em; }
.video-single { max-width: 430px; margin: 1.2em 0; }
.speed-tag { display: inline-block; font-size: 0.7em; font-weight: 600; letter-spacing: 0.04em; background: #eef0f3; color: #57606a; border: 1px solid #dde0e4; border-radius: 4px; padding: 0.05em 0.4em; margin-left: 0.35em; vertical-align: middle; }
.img-slot { border: 1px dashed #cbd0d6; border-radius: 8px; background: #fafbfc; color: #9aa1a9; text-align: center; padding: 2.4em 1em; font-size: 0.85em; margin: 1em 0; }
.cite-box { border: 1px solid #e2e5e9; background: #fafbfc; border-radius: 8px; padding: 0.9em 1.1em; margin: 1em 0; }
.cite-box pre { margin: 0.55em 0 0; padding: 0.8em 0.9em; background: #fff; border: 1px solid #e6e8eb; border-radius: 6px; overflow-x: auto; font-size: 0.82em; line-height: 1.5; }
.cite-box code { background: none; padding: 0; font-size: inherit; }
.feedback { border: 1px solid #e2e5e9; border-radius: 8px; padding: 1em 1.15em 1.15em; margin: 1em 0; background: #fff; max-width: 620px; }
.feedback label { display: block; font-size: 0.85em; font-weight: 600; color: #33373c; margin: 0.7em 0 0.25em; }
.feedback input, .feedback textarea { width: 100%; box-sizing: border-box; font: inherit; font-size: 0.9em; padding: 0.5em 0.6em; border: 1px solid #d7dbe0; border-radius: 6px; background: #fff; color: #23272c; }
.feedback textarea { min-height: 7.5em; resize: vertical; }
.feedback input:focus, .feedback textarea:focus { outline: none; border-color: #1a5fb4; box-shadow: 0 0 0 3px rgba(26,95,180,0.12); }
.feedback button { margin-top: 0.95em; padding: 0.45em 1.25em; font: inherit; font-size: 0.9em; font-weight: 600; color: #fff; background: #1a5fb4; border: 1px solid #1a5fb4; border-radius: 999px; cursor: pointer; }
.feedback button:hover { background: #17508f; }
.feedback button[disabled] { opacity: 0.6; cursor: default; }
.feedback .hp { position: absolute; left: -5000px; width: 1px; }
.form-status { margin-top: 0.75em; font-size: 0.85em; min-height: 1.2em; }
.form-status.ok { color: #1f7a45; }
.form-status.err { color: #a3323d; }
</style>

## Task setup

The workspace holds two placement targets — a **larger cream plate** and a **smaller white bowl** — and a set of coloured, differently-shaped objects. The policy is given a language instruction (e.g. *"pick blue star and place on the cream coloured plate"*) and must localize the correct object, grasp it, and place it on the correct target.

Two tasks of deliberately different difficulty:

- **Easy (ACT):** a single object, picked from a fixed location and placed on the plate at a fixed location.
- **Hard (π0.5, SmolVLA):** three objects (blue star, red circular block, purple cube), picked from varied locations, placed on either the plate or the bowl depending on the instruction. This demands object recognition, language grounding to the commanded object *and* target, and correct placement.

<div class="fig-grid">
  <figure>
    <img src="/images/setup_workspace.jpg" alt="SO-101 workspace: cream plate and white bowl with blue star, red circular block and purple cube">
    <figcaption><strong>Workspace.</strong> The cream plate and white bowl — both at fixed positions throughout data collection — with the three training objects: blue star, red circular block, purple cube.</figcaption>
  </figure>
  <figure>
    <img src="/images/setup_novel_green.jpg" alt="Two green cubes of different shades used as novel objects">
    <figcaption><strong>Novel green objects.</strong> Two shades of green, neither of them present in the training data.</figcaption>
  </figure>
  <figure>
    <img src="/images/setup_novel_yellow.jpg" alt="Novel yellow cube alongside the blue star">
    <figcaption><strong>Novel yellow object.</strong> Not present in the training data.</figcaption>
  </figure>
</div>

## Results at a glance

**ACT** — trained from scratch on **50 demonstrations** of the easy single-object task (purple cube → cream plate, both at fixed positions). Succeeds on **9 of 10** rollouts, establishing that the pipeline works end to end: teleoperated collection → training → deployment on the arm.

**π0.5** — LoRA fine-tune of `pi05_base` on the **192-demonstration** language-conditioned multitask set. Places on the cream plate in **58–70%** of rollouts and generalizes to objects never seen in training, but **never places on the white bowl (0/20)**: placement follows a memorized location rather than the visible target, which a relocation ablation confirms. Grasping proves geometry-dependent — the cylindrical red block is picked in only **3 of 23** rollouts despite an equal share of the training data.

**SmolVLA** — the **same 192 demonstrations**, but fine-tuned with the vision backbone frozen and only the action expert updated. Performs pick-and-place at times, with weak object-level language grounding — it will grasp an object other than the one named. Included as a **qualitative** comparison point; not yet quantified.

<p class="rl-note">Scoring for the hard task: each rollout scores <strong>0.5 for a successful pick + 0.5 for a successful place</strong> (max 1.0), logged individually with object, target, position and orientation. The quantitative results in the π0.5 section below come from the evaluation log, 143 recorded π0.5 rollouts and every figure is computed from it. ACT and SmolVLA are not part of this log: the ACT figure comes from a separate 10-rollout evaluation of the easy task, and SmolVLA has not been quantified yet.</p>

## π0.5 — detailed evaluation

Training data is **balanced across both objects and targets**: 3 objects × 2 targets × 32 demonstrations = **192 episodes / 51,801 recorded timesteps**, i.e. 96 plate and 96 bowl episodes, and 64 episodes per object. 

**Fair comparison.** Some conditions were evaluated at two object start positions (and two orientations); others at one. The tables below therefore report the **common slice — object position 1, orientation 1** — so every row describes the same starting configuration. Full counts across all positions and orientations follow.

#### In-distribution tasks &nbsp;<span class="rl-note">(objects and targets both seen in training)</span>

<div class="rl-wrap">
<table class="rl">
<thead><tr><th>Task</th><th>Rollouts</th><th>Pick</th><th>Place</th><th>Mean score</th></tr></thead>
<tbody>
<tr><td>blue star → <strong>cream plate</strong></td><td class="num">10</td><td class="num">8/10 &nbsp;(80%)</td><td class="num">7/10 &nbsp;(70%)</td><td class="num">0.75</td></tr>
<tr><td>purple cube → <strong>cream plate</strong></td><td class="num">10</td><td class="num">7/10 &nbsp;(70%)</td><td class="num">6/10 &nbsp;(60%)</td><td class="num">0.65</td></tr>
<tr><td>purple cube → <strong>white bowl</strong></td><td class="num">10</td><td class="num">8/10 &nbsp;(80%)</td><td class="num"><strong>0/10 &nbsp;(0%)</strong></td><td class="num">0.40</td></tr>
<tr><td>red circular block → <strong>cream plate</strong></td><td class="num">13</td><td class="num">3/13 &nbsp;(23%)</td><td class="num">1/13 &nbsp;(8%)</td><td class="num">0.15</td></tr>
</tbody>
</table>
</div>

#### Generalization tasks &nbsp;<span class="rl-note">(objects absent from training; target seen in training)</span>

<div class="rl-wrap">
<table class="rl">
<thead><tr><th>Task</th><th>Rollouts</th><th>Pick</th><th>Place</th><th>Mean score</th></tr></thead>
<tbody>
<tr><td>green object → <strong>cream plate</strong></td><td class="num">10</td><td class="num">6/10 &nbsp;(60%)</td><td class="num">6/10 &nbsp;(60%)</td><td class="num">0.60</td></tr>
<tr><td>yellow object → <strong>cream plate</strong></td><td class="num">10</td><td class="num">5/10 &nbsp;(50%)</td><td class="num">5/10 &nbsp;(50%)</td><td class="num">0.50</td></tr>
</tbody>
</table>
</div>

#### Full record — all positions and orientations

<div class="rl-wrap">
<table class="rl">
<thead><tr><th>Task</th><th>Rollouts</th><th>Pick</th><th>Place</th><th>Mean score</th></tr></thead>
<tbody>
<tr><td>purple cube → cream plate <span class="rl-note">(2 positions × 2 orientations)</span></td><td class="num">40</td><td class="num">25/40 &nbsp;(63%)</td><td class="num">23/40 &nbsp;(58%)</td><td class="num">0.60</td></tr>
<tr><td>blue star → cream plate <span class="rl-note">(2 positions)</span></td><td class="num">20</td><td class="num">14/20 &nbsp;(70%)</td><td class="num">12/20 &nbsp;(60%)</td><td class="num">0.65</td></tr>
<tr><td>purple cube → white bowl <span class="rl-note">(2 positions)</span></td><td class="num">20</td><td class="num">14/20 &nbsp;(70%)</td><td class="num"><strong>0/20 &nbsp;(0%)</strong></td><td class="num">0.35</td></tr>
<tr><td>red circular block → cream plate <span class="rl-note">(2 positions)</span></td><td class="num">23</td><td class="num">3/23 &nbsp;(13%)</td><td class="num">1/23 &nbsp;(4%)</td><td class="num">0.09</td></tr>
<tr><td>green object → cream plate <span class="rl-note">(novel)</span></td><td class="num">10</td><td class="num">6/10 &nbsp;(60%)</td><td class="num">6/10 &nbsp;(60%)</td><td class="num">0.60</td></tr>
<tr><td>yellow object → cream plate <span class="rl-note">(novel, 2 positions)</span></td><td class="num">20</td><td class="num">9/20 &nbsp;(45%)</td><td class="num">9/20 &nbsp;(45%)</td><td class="num">0.45</td></tr>
<tr><td>purple cube → cream plate <span class="rl-note">(<strong>plate relocated</strong> — ablation)</span></td><td class="num">10</td><td class="num">8/10 &nbsp;(80%)</td><td class="num"><strong>0/10 &nbsp;(0%)</strong></td><td class="num">0.40</td></tr>
</tbody>
</table>
</div>

<div class="rl-finding">
<h4>Key finding — placement is a memorized position prior, not visual grounding</h4>
<p>The bowl result is unambiguous: π0.5 <strong>grasps the object in 70% of bowl rollouts but places it in 0 of 20</strong>. Grasping is not the bottleneck — target placement is. Training data was balanced across the two targets (96 plate / 96 bowl), so this is not a data-imbalance artifact.</p>
<p>The <strong>relocation ablation isolates the cause</strong>. It repeats the in-distribution purple-cube → cream-plate task at the same object position, changing only the plate's location: picking is unaffected (8/10 vs 7/10) while placement collapses from <strong>6/10 to 0/10</strong>. The arm travels to where the plate sat during training and releases there. In bowl rollouts the same behaviour shows up as the object landing on the cream plate, between the two targets, or at the bowl's training location even when the bowl has been moved or removed from the scene.</p>
<p>Because both targets were spatially <em>fixed</em> during data collection, the policy could minimise its loss by memorising <em>where</em> to release rather than learning to <em>find the target and release there</em>. The thesis: <strong>the policy generalises over the factors that were randomised in training (object position and, as the generalization table shows, object appearance) and fails on the one that was not — target position.</strong></p>
</div>

<div class="video-card video-single">
  <video src="/videos/pi05_purplecube_white_bowl_3x.mp4" poster="/images/video-posters/pi05_purplecube_white_bowl_3x.jpg" controls muted loop playsinline preload="none"></video>
  <div class="meta"><h3>Bowl instruction → plate placement <span class="speed-tag">3× speed</span></h3><p>Instruction: <em>"pick purple cube and place on the white coloured bowl."</em> The cube is grasped successfully and placed on the <strong>cream plate</strong> instead; the bowl stays empty throughout.</p></div>
</div>

### Plate-relocation ablation &nbsp;<span class="rl-badge done">Result: 0/10</span>

**Test.** Move the cream plate to a location unseen in training, issue the in-distribution instruction *"pick purple cube and place on the cream coloured plate"*, and re-run 10 episodes. The logic:

- If placement **follows** the relocated plate → the policy grounds the target visually.
- If placement still goes to the **training** location → placement relies on a position prior, with no visual grounding of the target.

**Result: 0/10 placements on the relocated plate**, with picking unaffected (8/10). Against the identical task with the plate at its training location — 7/10 pick, **6/10 place**. The only thing that changed is the plate's position, and placement went to zero. The arm continued to travel to the plate's *training* location and released there. This resolves the ambiguity above: **placement is driven by a learned position prior, with no visual grounding of the target.** The plate-placement success in the in-distribution evaluation is therefore attributable to the plate sitting where it sat during training, not to the policy locating it.

<div class="video-card video-single">
  <video src="/videos/ablation_plate_moved_purplecube_3x.mp4" poster="/images/video-posters/ablation_plate_moved_purplecube_3x.jpg" controls muted loop playsinline preload="none"></video>
  <div class="meta"><h3>Ablation rollouts <span class="speed-tag">3× speed</span></h3><p>The cream plate has been moved to a new location (lower left). The arm grasps the purple cube and carries it to the plate's <em>training</em> location instead.</p></div>
</div>

**Fix (future work):** randomize target positions during data collection (including bowl-in-varied-locations demonstrations), forcing the policy to ground the target visually instead of memorizing its coordinates.

### Grasping is geometry-dependent &nbsp;<span class="rl-badge done">3/23 picks</span>

The **red circular block** is not short of data: it received **64 demonstrations, identical to the other two objects** (32 per target). Yet it is grasped in only **3 of 23 rollouts (13%)** — against 63% for the purple cube and 70% for the blue star. The policy approaches the object, so localization is not the problem; it fails to close a successful grasp. 

Working hypothesis: a smooth cylindrical body offers fewer stable contact points and demands tighter gripper alignment than a flat-faced cube or a star with protrusions. So, a harder geometry might need *more* and higher quality data. 

## Generalization to novel objects

Two objects absent from the training set — a green object and a yellow object, in colours that appear in neither the demonstrations nor the instructions — were evaluated on the cream plate.

**Both are picked *and* placed, at rates comparable to the in-distribution objects:** green 6/10 pick and 6/10 place, yellow 5/10 and 5/10, against 6/10 place for the in-distribution purple cube at the same object position. Every successful grasp also produced a successful placement. So unseen object colour and shape are handled: the policy's generalization gap is **not** about object appearance — it is specifically about target position, as the ablation above shows.

<div class="video-grid">
  <div class="video-card">
    <video src="/videos/gen_green_cream_plate_3x.mp4" poster="/images/video-posters/gen_green_cream_plate_3x.jpg" controls muted loop playsinline preload="none"></video>
    <div class="meta"><h3>Novel green object <span class="speed-tag">3× speed</span></h3><p>A green object, absent from the training set, picked and placed on the cream plate.</p></div>
  </div>
  <div class="video-card">
    <video src="/videos/gen_yellow_cream_plate_3x.mp4" poster="/images/video-posters/gen_yellow_cream_plate_3x.jpg" controls muted loop playsinline preload="none"></video>
    <div class="meta"><h3>Novel yellow object <span class="speed-tag">3× speed</span></h3><p>A yellow object, absent from the training set, under a cream-plate instruction.</p></div>
  </div>
</div>

## ACT — baseline &nbsp;<span class="rl-badge done">9/10</span>

On the easy single-object, fixed-location task, ACT — trained from scratch on 50 demonstrations, on Apple Silicon, for 29k steps — succeeds on **9 of 10** rollouts — a clean baseline confirming the pipeline (data collection → training → on-arm deployment) and isolating the hard task's difficulty as the object of study. 

## SmolVLA — qualitative &nbsp;<span class="rl-badge qual">Eval pending</span>

SmolVLA performs pick-and-place *sometimes*, but grounds the language instruction to the object poorly — asked for the blue star, it will grasp a different object in the scene.

The fine-tuning was done with **`freeze_vision_encoder = true`** *and* **`train_expert_only = true`**, so the SmolVLM2 backbone was held fixed and **only the action expert was updated**. The perception and language representations therefore never adapted to these particular objects — the policy learned *how to move* from the demonstrations, but not *what this scene's objects look like*.

Next steps: (1) a quantified eval under the same 0.5/0.5 scoring, measuring the *commanded-object-picked* rate; (2) a retrain with the vision encoder unfrozen, which turns the explanation above into a verified test.

## Method notes

- **Data:** teleoperated demonstrations collected with `lerobot-record`, published as the LeRobot dataset `parn31/transformed_picknplace_multitask` — **192 episodes / 51,801 timesteps**, balanced across objects and targets.
- **π0.5:** fine-tuned with the **openpi** codebase. **SmolVLA & ACT:** trained with **lerobot**.
- **Scoring:** per-rollout, 0.5 (pick) + 0.5 (place); every episode logged with object, target, position/orientation, and outcome.

### π0.5 fine-tuning setup

<div class="rl-wrap">
<table class="rl">
<tbody>
<tr><th>Base checkpoint</th><td>π0.5 (<code>pi05_base</code>), PyTorch backend, bfloat16 with gradient checkpointing and <code>torch.compile</code> (max-autotune)</td></tr>
<tr><th>Adaptation</th><td><strong>LoRA</strong> on both the PaliGemma VLM backbone (<code>gemma_2b_lora</code>) and the action expert (<code>gemma_300m_lora</code>); no modules frozen</td></tr>
<tr><th>Observations</th><td>Overhead and wrist cameras, joint state; delta joint actions; the language prompt is taken from the task string</td></tr>
<tr><th>Action head</th><td>Action horizon 25, action dimension 32, max token length 200</td></tr>
<tr><th>Optimiser</th><td>β₁ 0.9, β₂ 0.95, ε 1e-8, weight decay 1e-10, gradient-norm clip 1.0; no EMA</td></tr>
<tr><th>LR schedule</th><td>1,000-step warm-up → peak <strong>5e-5</strong> → decay toward 5e-6 over 12,000 steps</td></tr>
<tr><th>Batch / seed</th><td>Batch size 32, seed 42</td></tr>
<tr><th>Hardware</th><td>Single <strong>NVIDIA A40</strong>, ~11.8 s per step</td></tr>
<tr><th>Steps completed</th><td><strong>8,000 </strong></td></tr>
<tr><th>Training loss</th><td>0.055 → <strong>0.0041</strong>, gradient norm ≈ 0.06 at step 8,000</td></tr>
</tbody>
</table>
</div>

### ACT and SmolVLA training setup

<div class="rl-wrap">
<table class="rl">
<thead><tr><th></th><th>ACT <span class="rl-note">(easy task)</span></th><th>SmolVLA <span class="rl-note">(hard task)</span></th></tr></thead>
<tbody>
<tr><th>Initialisation</th><td>trained from scratch</td><td><code>lerobot/smolvla_base</code></td></tr>
<tr><th>Dataset</th><td><code>parn31/so101_t1_initial50</code> (50 demos)</td><td><code>parn31/picknplace_multitask</code></td></tr>
<tr><th>Vision encoder</th><td>ResNet-18</td><td>SmolVLM2-500M-Video-Instruct, <strong>frozen</strong></td></tr>
<tr><th>Trainable parts</th><td>whole policy</td><td><strong>action expert only</strong> (<code>train_expert_only</code>)</td></tr>
<tr><th>Action chunk</th><td>100 (100 executed)</td><td>50 (50 executed)</td></tr>
<tr><th>Batch / LR / decay</th><td>8 / 1e-5 / 1e-4</td><td>64 / 1e-4 / 1e-10, 1,000-step warm-up</td></tr>
<tr><th>Steps</th><td><strong>29,200 of 60,000</strong> (run stopped early), 17.4 epochs</td><td><strong>20,000 of 20,000</strong> completed, 24.7 epochs</td></tr>
<tr><th>Hardware / time</th><td>Apple Silicon (MPS), 6.2 h</td><td>CUDA GPU, 5.9 h</td></tr>
<tr><th>Final training loss</th><td>0.108 <span class="rl-note">(L1 0.109, KLD 0.0006)</span></td><td>0.044</td></tr>
</tbody>
</table>
</div>

<p class="rl-note">Both multitask policies were trained on the <strong>same 192 demonstrations</strong>. The two dataset entries differ only in storage format — the SmolVLA copy was converted to the LeRobot v3.0 layout — so the two runs see identical underlying data. (π0.5 additionally applies a delta-joint-action transform at load time, as listed in its setup above.)</p>

## Practical notes

Lessons from bringing imitation learning onto real hardware: camera resolution must be matched to the inference compute budget; teleoperation speed should be kept consistent with the target control frequency, or the policy sees a distribution shift and can stall at the initial states of a rollout; and demonstration coverage must be balanced across object positions. 

## Clips

<div class="video-grid">
  <div class="video-card">
    <video src="/videos/act_purplecube_cream_plate_3x.mp4" poster="/images/video-posters/act_purplecube_cream_plate.jpg" controls muted loop playsinline preload="none"></video>
    <div class="meta"><h3>ACT — success <span class="speed-tag">3× speed</span></h3><p>Purple cube picked and placed on the cream plate (easy task, fixed positions).</p></div>
  </div>
  <div class="video-card">
    <video src="/videos/pi05_purplecube_cream_plate_3x.mp4" poster="/images/video-posters/pi05_purplecube_cream_plate_3x.jpg" controls muted loop playsinline preload="none"></video>
    <div class="meta"><h3>π0.5 — purple cube → plate <span class="speed-tag">3× speed</span></h3><p>In-distribution rollouts: purple cube picked and placed on the cream plate.</p></div>
  </div>
  <div class="video-card">
    <video src="/videos/pi05_bluestar_cream_plate_3x.mp4" poster="/images/video-posters/pi05_bluestar_cream_plate_3x.jpg" controls muted loop playsinline preload="none"></video>
    <div class="meta"><h3>π0.5 — blue star → plate <span class="speed-tag">3× speed</span></h3><p>In-distribution rollouts: blue star picked and placed on the cream plate.</p></div>
  </div>
    <div class="video-card">
    <video src="/videos/smolvla_purplecube_cream_plate_3x.mp4" poster="/images/video-posters/smolvla_purplecube_cream_plate_3x.jpg" controls muted loop playsinline preload="none"></video>
    <div class="meta"><h3>SmolVLA rollouts <span class="speed-tag">3× speed</span></h3><p>Instruction: <em>"pick purple cube and place on the cream coloured plate."</em></p></div>
  </div>
</div>

<script>
// Only one clip streams at a time: several concurrent video streams over HTTP/1.1
// exhaust the per-origin connection limit and cause playback to stall.
document.addEventListener("play", function (e) {
  document.querySelectorAll("video").forEach(function (v) {
    if (v !== e.target) { v.pause(); }
  });
}, true);
</script>

## Cite this page

If the results or the failure analysis on this page are useful in your own work, you are very welcome to cite it.

<div class="cite-box">
<strong>BibTeX</strong>
<pre><code>{% raw %}@misc{parnika2026so101,
  author       = {{Parnika}},
  title        = {Robot Learning --- Imitation Learning on the {SO-101} Arm},
  year         = {2026},
  howpublished = {\url{https://parnika31.github.io/robot-learning/}},
  note         = {Accessed: DD Month YYYY}
}{% endraw %}</code></pre>
</div>

<p class="rl-note">The double braces around <code>{% raw %}{{Parnika}}{% endraw %}</code> are intentional: I have a single given name and no surname, and the braces stop BibTeX styles from splitting it or inventing an initial. Please cite it as <strong>Parnika</strong>, not <em>Parnika, P.</em> My ORCID iD is <a href="https://orcid.org/0009-0008-3877-2416">0009-0008-3877-2416</a>, which identifies me unambiguously regardless of how a given system formats the name.</p>

## Comments and feedback

Questions, corrections and suggestions are welcome — especially if you have run similar evaluations and reached different conclusions.

<div class="feedback">
<form id="feedback-form" action="https://formspree.io/f/xyegjaqk" method="POST">
  <label for="fb-name">Name</label>
  <input id="fb-name" type="text" name="name" autocomplete="name">
  <label for="fb-email">Email <span class="rl-note">(optional &mdash; only needed if you would like a reply)</span></label>
  <input id="fb-email" type="email" name="email" autocomplete="email">
  <label for="fb-message">Message</label>
  <textarea id="fb-message" name="message" required></textarea>
  <input class="hp" type="text" name="_gotcha" tabindex="-1" autocomplete="off" aria-hidden="true">
  <button type="submit">Send</button>
  <div class="form-status" id="form-status" role="status" aria-live="polite"></div>
</form>
</div>

<script>
// Submit via fetch so the visitor stays on the page instead of being
// redirected to the form handler's own confirmation screen.
(function () {
  var form = document.getElementById("feedback-form");
  if (!form) { return; }
  var status = document.getElementById("form-status");
  var button = form.querySelector("button[type=submit]");
  form.addEventListener("submit", function (e) {
    e.preventDefault();
    status.textContent = "Sending\u2026";
    status.className = "form-status";
    button.disabled = true;
    fetch(form.action, {
      method: "POST",
      body: new FormData(form),
      headers: { Accept: "application/json" }
    }).then(function (res) {
      if (res.ok) {
        form.reset();
        status.textContent = "Thank you \u2014 your message has been sent.";
        status.className = "form-status ok";
        return;
      }
      return res.json().then(function (d) {
        throw new Error(((d.errors || []).map(function (x) { return x.message; }).join(", ")) || "Submission failed.");
      });
    }).catch(function (err) {
      status.textContent = err.message || "Something went wrong \u2014 please email me instead.";
      status.className = "form-status err";
    }).then(function () { button.disabled = false; });
  });
})();
</script>
