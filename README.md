# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

# Milestone 1: Community and Candidate Distinctions

## Community
**r/AskCulinary** — a public cooking community where users ask questions about cooking techniques, recipes, ingredients, and kitchen equipment.

## Candidate Distinctions

1. **Technique:** The author primarily wants to know how to perform a cooking method or carry out a specific cooking action.

2. **Recipe troubleshooting:** The author wants to understand or fix an unexpected result during cooking or baking.

3. **Equipment:** The author primarily asks about choosing, using, or troubleshooting kitchen tools and appliances.

4. **Ingredient:** The author primarily asks about an ingredient's properties, selection, substitution, or suitability for a dish.

## What makes some posts more substantial than others

Topic tells me what a post is about. Reading the posts, the bigger difference was how much context the author gives:

1. **The recipe is included.** Some troubleshooting posts include the full recipe they followed. Others only describe what went wrong.
2. **The equipment is specific.** The stronger posts name the type of equipment, the brand, or where it was bought. Weaker ones just say "my pan" or "my oven."
3. **The recipe comes later.** Some posts don't give the recipe at the start and add it further down.
4. **The current method is described.** Technique questions in the stronger posts explain how the author does it now, so readers can see what to change.

## Initial Labeling Rule
Classify each post according to its primary question. If the author is asking why something went wrong or how to fix an unexpected result, use `recipe_troubleshooting`. Otherwise, use the label that best matches the main subject of the question.

These distinctions are provisional and will be tested against real posts during labeling.

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter sorts questions from r/AskCulinary, a public cooking help community, by the kind of help the poster needs. Each post gets one of four labels: `technique` (how to do a cooking method), `recipe_troubleshooting` (why a dish went wrong, or how to rescue it), `equipment` (choosing, using or fixing a tool) or `ingredient` (what an ingredient does, how to store it, or what to substitute). The classifier is DistilBERT fine-tuned on 200 posts I labelled myself. The hardest line it has to learn is between `technique` and `recipe_troubleshooting`, because many posts describe a failure but are really asking how to do the method right.



---

## Label Taxonomy

### `technique`

**Definition:** The author primarily asks how to perform or improve a cooking method, including preparation, cooking, or reheating.

**Example 1:**
> Maintaining Crispier Tofu — The author asks how to keep tofu crispy after adding gochujang sauce.

**Example 2:**
> Make-ahead short ribs — The author asks how to cook and reheat short ribs without overcooking them.

### `recipe_troubleshooting`

**Definition:** The author primarily asks why an unexpected cooking result occurred or how to diagnose or fix it.

**Example 1:**
> Making sago and it turned out bitter — The author asks why the sago became bitter and whether the flavor can be fixed.

**Example 2:**
> Why does my homemade hollandaise keep splitting even when I follow temperature rules? — The author asks why the sauce repeatedly separates despite following standard techniques.

### `equipment`

**Definition:** The author primarily asks about choosing or using or troubleshooting cookware, appliances, or other cooking tools, rather than diagnosing an unexpected result in the food.

**Example 1:**
> What material of pan to choose? — The author wants help selecting cookware that suits their cooking needs and preferences.

**Example 2:**
> My Breville Smart Oven is smarter than me — how to use the Breville Air Fryer Pro? — The author asks how to operate a specific appliance and adjust cooking instructions for it.

### `ingredient`

**Definition:** The author primarily asks about an ingredient's properties, role, selection, storage, behavior, preservation, or substitution in a dish.

**Example 1:**
> Ways to keep chopped peppers from going bad too quickly? — The author asks how to preserve cut peppers and prevent spoilage.

**Example 2:**
> What difference does lime juice make in biscotti (not just zest) — should I skip it? If I skip it, should I also remove the baking soda? — The author asks what role lime juice plays in the recipe and whether removing it would affect the need for baking soda.

### The hardest boundary

**Which two labels:** `technique` and `recipe_troubleshooting`

**The decision rule I used every time:**

<!-- Classify a post by the primary problem the author wants to solve, not simply by the ingredient, equipment, or method mentioned. If the author reports an actual unexpected cooking result and asks why it happened or how to fix it, use `recipe_troubleshooting`, even when ingredients, equipment, or technique are discussed. If there is no actual failure to diagnose, use `equipment` for equipment selection or use, `ingredient` for ingredient properties or substitutions, and `technique` for how to perform a cooking method. -->

**Decision rule:** Label posts according to the primary type of cooking help requested. Use `technique` for questions about cooking methods, process, timing, temperature, or how to achieve a desired result through a cooking method, even when the result was not as expected. Use `equipment` when the main concern is selecting, operating, or troubleshooting a cooking tool or appliance. Use `ingredient` for questions about ingredient properties, substitutions, or behavior. Use `recipe_troubleshooting` when the main concern is diagnosing or recovering from an unexpected result in the finished food or recipe. When categories overlap, prioritize the post's primary concern rather than automatically assigning every reported problem to `recipe_troubleshooting`.

**Why I revised the rule:** My first rule (kept above as a comment) sent every post that reported a failure to `recipe_troubleshooting`. When I applied it, that label grew to about a third of the dataset and absorbed posts whose real question was about a method ("how do I get crispy salmon skin?") or an ingredient ("why does my gelatin smell?"). The revised rule labels each post by the primary kind of help requested. My 20 cold labels were made under the first rule and I left them unchanged.

**Stretch Point: Why I added a fourth label:** The first three labels—`technique`, `recipe_troubleshooting`, and `equipment`—did not adequately cover posts asking about ingredient properties, substitutions, and ingredient behavior independently of a cooking method or equipment issue. I added `ingredient` to capture these questions rather than forcing them into a category that did not fit.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
I collected 200 posts from the public r/AskCulinary community using a Claude script that retrieved posts through its public search RSS feed. The dataset covers four cooking-help categories: technique, recipe troubleshooting, equipment, and ingredients.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I labelled the first 20 posts myself without looking at AI suggestions or the original post flairs. For the remaining 180 posts, Claude generated initial labels using my taxonomy and decision rule. I then reviewed the posts and corrected labels where they did not match my definitions. The original post flairs were used for initial collection and organization, not treated as ground-truth labels.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| technique | 61 | 30.5% |
| equipment | 50 | 25.0% |
| recipe_troubleshooting | 45 | 22.5% |
| ingredient | 44 | 22.0% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "Why does my pan-seared chicken breast turn dry even though I follow the recipe? … Despite this, the chicken often ends up dry and tough instead of juicy. I'm trying to understand what's going wrong—heat level, timing, thickness, or something else?"
>
> *Could have been:* `technique` or `recipe_troubleshooting`
>
> *I chose, because:* `technique`. It reports a bad result and asks "what's going wrong," which reads like troubleshooting. But every cause the author is asking about (heat, timing, thickness) is a method variable, and my rule puts timing and temperature under `technique` even when the result was not as expected. There is no batch to rescue; the author wants to cook it right next time.

**2.**
> *The post:* "Yorkshire pudding won't rise in my oven—same recipe works fine at friend's house. … I'm checking my oven against an oven thermometer now, set at 450° … consistently showing about 470°. … My husband replaced one of the elements within the last few years. Could he have inadvertently recalibrated the thermostat?"
>
> *Could have been:* `recipe_troubleshooting` or `equipment`
>
> *I chose, because:* `equipment`. On the surface it is a failed recipe, but the same recipe works in a friend's oven, so the recipe isn't the problem. The author has already traced it to the oven running hot and ends by asking about the thermostat, which is troubleshooting an appliance.

**3.**
> *The post:* "Gross gelatin smell. … I've been trying my hand at homemade jelly goods and I cannot get over how gross they smell! Its because of the gelatin, it has a very distinct and strong barnyard smell. I originally believed it would be gone by the final product but nope! … How can I fix this?"
>
> *Could have been:* `ingredient` or `recipe_troubleshooting`
>
> *I chose, because:* `ingredient`. The finished food came out wrong, which pulls toward troubleshooting, but the author has already named the cause: the gelatin's own smell. The real question is how that ingredient behaves and how to deal with it, which my rule puts under `ingredient`. Under my first decision rule this post was `recipe_troubleshooting`; revising the rule is what moved it.

---

## The Training Run
device: cpu  (CPU)
torch:  2.14.1+cpu
<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**
BASE_MODEL = "distilbert-base-uncased"




**Settings:** <!-- epochs, learning rate, batch size, seed -->
EPOCHS = 3
LEARNING_RATE = 2e-5
BATCH_SIZE = 16
SEED = 42

**Anything I changed from the defaults, and why:**
nothing was changed from the defaults. 

**Split sizes:** <!-- train / val / test, and per-label counts in the test split. If a label had fewer than about 8 in test, say so — it explains a lot of next unit's variance. -->

train 139  ·  val 31  ·  test 30

                        train  val  test
label                                   
technique                  43    9     9
recipe_troubleshooting     31    7     7
equipment                  35    8     7
ingredient                 30    7     7

[note] 'recipe_troubleshooting' has only 7 examples in the test split.
[note] 'equipment' has only 7 examples in the test split.
[note] 'ingredient' has only 7 examples in the test split.

A metric computed on that few examples moves a lot between seeds.
Write that down now — next unit it's a diagnosis, not a surprise.







---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I gave Claude my five acceptance criteria and asked whether each one could be checked by someone who couldn't ask me what I meant. I told it not to suggest numbers.
- *What came back:* Criteria 1–3 were checkable. Criterion 4 wasn't: it didn't say who the "independent reviewer" was or which 30 posts they would label. Criterion 5 asked about predictions with confidence ≥ 0.80, but the notebook doesn't record per-post confidence — it only reports accuracy for the most and least confident thirds.
- *What I changed:* I rewrote criterion 4 so another person labels 30 randomly selected posts using only my README definitions and decision rule. I rewrote criterion 5 to use what the notebook actually reports: the most confident third must reach at least 0.75 accuracy across all three seeds. The numbers stayed mine.

**Moment 2**

- *What I asked for:* I had Claude pre-label 180 posts using my definitions and my first decision rule, which sent every post that reported a failure to `recipe_troubleshooting`.
- *What came back:* `recipe_troubleshooting` came out at 67 of 200 (34%), and it had absorbed posts that were really about how to do something ("how do I get crispy salmon skin?") or about an ingredient (gelatin's smell). Claude also flagged hard cases where my rule allowed two answers.
- *What I changed:* I rewrote the decision rule to label by the primary kind of help requested, and moved my hardest boundary to `technique` vs `recipe_troubleshooting`. Claude relabelled 20 rows under the new rule; I then reviewed every row and changed more myself — the dry chicken breast post to `technique` and the Yorkshire pudding post to `equipment`.

**Pre-labelling disclosure:**

Posts were collected with a Claude-written script from r/AskCulinary's public search RSS, 50 per post flair. Flairs were used only to balance collection, not as labels. I labelled 20 posts cold (marked `cold` in the note column). Claude pre-labelled the other 180 using my definitions — first under my original decision rule, then again under the revised one — and I read and corrected every one (marked `pre-labelled by AI (Claude), checked`).

### Unit 6

**Moment 3 — Spotting the pattern in my errors**

- *What I asked for:* I asked Claude to fill in my run log from
  `results_three_seeds_before.json` and my original criteria, then to propose
  diagnoses for each miss.
- *What came back:* Claude pointed out a pattern I hadn't put into words: the model
  predicted `technique` for 30/30, 30/30 and 28/30 test posts, so 0.300 accuracy is
  exactly what always guessing `technique` scores. It also noted that mean confidence
  was about 0.31 whether right or wrong, near the 0.25 you'd get from spreading evenly
  across four labels, and that 3 epochs on ~140 training posts is only ~27 updates.
  It suggested this pointed at training rather than my labels.
- *What I changed:* I had Claude show every edit before making it, and I changed the
  framing. I required the training cause to be written as a hypothesis, not a
  confirmed cause, with Milestone 5 as the test. I chose to count the staff agreement
  check as a substitute measure for criterion 4. I made sure it says plainly that it
  used a different taxonomy and isn't direct evidence about my cooking labels. I also
  rewrote the criterion 2 explanation and the wording about confidence. I reviewed
  the full diff before committing.

**Moment 4 — Choosing the improvement**

- *What I asked for:* I asked what improvement to make for Milestone 5.
- *What came back:* Claude recommended raising `EPOCHS` from 3 to 10 and changing
  nothing else. Its reasoning was that my diagnosis pointed at training, my agreement
  check found no boundary to relabel, and my smallest labels weren't much smaller than
  the others. It suggested epochs over learning rate because 2e-5 is a typical value.
- *What I changed:* I made the change and ran the three-seed retrain myself. Claude
  drafted The Improvement section, including the after run log and "Did it help",
  from my retrain results; I reviewed and edited the drafts.

**Staff-set disclosure:**

I labeled all 30 staff posts myself under `data/staff_taxonomy.md`, with no AI
labeling. Claude only copied the post text verbatim from `staff_posts.csv` into
`my_staff_labels.csv`, so the two files would match on text. Later it renamed the
TF's answer-key file so `agreement.py` could find it, without opening it. It also
fixed my `hot take` labels to `hot_take`, a spelling difference that was causing
false disagreements. It never saw the staff labels before my 30 were saved.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.433 | 0.300 | -0.133 |
| Macro F1 | 0.354 | 0.115 | -0.239 |
| F1 — `equipment` | 0.000 | 0.000 | +0.000 |
| F1 — `ingredient` | 0.636 | 0.000 | -0.636 |
| F1 — `recipe_troubleshooting` | 0.400 | 0.000 | -0.400 |
| F1 — `technique` | 0.381 | 0.462 | +0.081 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

     I predict that my trained model will outperform the zero-shot baseline on overall accuracy because it has been trained on examples from my dataset, although it may still struggle to distinguish between similar labels.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
     The zero-shot (no training) baseline performed better than the trained model on overall accuracy (0.433 vs. 0.300) and macro F1 (0.354 vs. 0.115). The trained model performed better only on the technique F1 score, while both models had an equipment F1 score of 0.000. This suggests that training on my labeled dataset did not improve overall performance, and further investigation is needed to understand the errors.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

Device:
device: cpu  (CPU)
torch:  2.14.1+cpu

Seed results:
| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.300 | 0.300 | 0.367 | 0.067 |
| Macro F1 | 0.115 | 0.115 | 0.233 | 0.117 |
| F1 — `technique` | 0.462 | 0.462 | 0.486 | 0.025 |
| F1 — `recipe_troubleshooting` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `equipment` | 0.000 | 0.000 | 0.444 | 0.444 |
| F1 — `ingredient` | 0.000 | 0.000 | 0.000 | 0.000 |

Accuracy moved 0.067 across seeds. Enough that a criterion landing within that band can't really be called either way.

Accuracy varied from 0.300 to 0.367, a spread of 0.067 (6.7 percentage points). Macro F1 varied by 0.117. The equipment F1 score changed substantially across seeds, from 0.000 to 0.444, while recipe_troubleshooting and ingredient remained at 0.000 for all three seeds. This suggests that performance is sensitive to the random seed for some labels, and the model consistently struggles with others.

     

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 | 0.300 | 0.300 | 0.367 | MISSED |
| 2. Every label F1 | ≥ 0.50 | 0.000 (lowest) | 0.000 (lowest) | 0.000 (lowest) | MISSED |
| 3. Class balance — largest label share | ≤ 60% | 30.5% | 30.5% | 30.5% | MET |
| 4. Labeling consistency — another person on my taxonomy | ≥ 80% | 100% (substitute, not per-seed) | 100% (substitute, not per-seed) | 100% (substitute, not per-seed) | MET (substitute measure) |
| 5. Accuracy of most confident third | ≥ 0.75 | 0.60 | 0.50 | 0.50 | MISSED |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | technique | recipe_troubleshooting | equipment | ingredient |
|---|---|---|---|---|
| **technique** | 9 | 0 | 0 | 0 |
| **recipe_troubleshooting** | 7 | 0 | 0 | 0 |
| **equipment** | 7 | 0 | 0 | 0 |
| **ingredient** | 7 | 0 | 0 | 0 |

Biggest confusion: 7 real `recipe_troubleshooting` posts called `technique`.
The other direction: 0 real `technique` posts called `recipe_troubleshooting`.

That's lopsided — the model leans toward `technique` on this pair.
A direction like that usually points at your labelling rather than the
model. Your agreement report is the thing that can tell you which.

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

     My biggest off-diagonal number is 7: all seven real `recipe_troubleshooting` posts were predicted as `technique`, while zero real `technique` posts were predicted as `recipe_troubleshooting`. This shows a one-way confusion between these categories, with the model favoring `technique`. More broadly, the model predicted `technique` for every test post, so it failed to distinguish any of the other three labels.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.70 on all three seeds | MISSED | Seeds scored 0.300, 0.300, 0.367. None reached 0.70. |
| 2 | Every label F1 | ≥ 0.50 for each label on all three seeds | MISSED | The criterion requires every label to achieve F1 ≥ 0.50 on all three seeds. `recipe_troubleshooting` and `ingredient` scored 0.000 on every seed, so the criterion was not met. |
| 3 | Class balance | No label above 60% of the dataset | MET | Largest label is `technique`, 61/200 = 30.5%. This is a property of the dataset, so it's the same for all seeds. |
| 4 | Labeling consistency | ≥ 80% agreement | MET (substitute measure) | The original couldn't be measured. On the substitute (staff taxonomy, 30 posts) I agreed on 30/30 = 100%. This is indirect evidence about my own taxonomy, not direct. |
| 5 | Accuracy of most confident third | ≥ 0.75 on all three seeds | MISSED | Seeds scored 0.60, 0.50, 0.50. None reached 0.75. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

**The pattern behind all three misses.** On every seed the model predicted
`technique` for almost every post: 30/30 test posts on seeds 42 and 7, and 28/30
on seed 2024. Nine of the 30 test posts are truly `technique`, so 0.300 is exactly
the accuracy of always guessing `technique`. The model collapsed onto one label
rather than confusing particular pairs.

**Criterion 1 — Overall accuracy (MISSED).**
Likely cause (hypothesis, not confirmed): training did not move the model far
enough from its starting point to separate the labels, which points to a training
setting such as too few epochs or too low a learning rate.
Evidence:
- The model's mean confidence is about 0.31 whether it is right or wrong. These
  confidence values are close to 0.25, the equal-probability reference for four
  labels, which is consistent with weak separation between classes.
- The zero-shot baseline reached 0.433 with no training on my data, so the task is
  not out of reach from the label definitions alone.

Causes I can mostly rule out:
- Too few examples: my labels range from 44 to 61 posts each, fairly balanced.
- Inconsistent labeling: my 30/30 staff agreement makes this less likely, but that
  check used a different taxonomy, so it's indirect evidence only.

What would confirm it: I haven't checked the training loss or tried a different
setting. Milestone 5's retrain is the test of this hypothesis.

**Criterion 2 — Every label F1 (MISSED).**
Likely cause: the same collapse. `ingredient` and `recipe_troubleshooting` score
0.000 because the model never predicted either one on any seed. `equipment`'s
0.444 on seed 2024 comes from the only two non-`technique` predictions it made.
Evidence against too few examples: `ingredient` (44) and `recipe_troubleshooting`
(45) are only slightly smaller than `equipment` (50).
What I can't tell yet: whether `ingredient` vs. `recipe_troubleshooting` is a
genuinely hard pair. The matrix shows no confusion between them only because every
post went to `technique`, and my agreement check didn't test this boundary. That
question stays open until the model predicts these labels at all.

**Criterion 5 — Confidence (MISSED).**
Likely cause: the confidence scores carry little signal because the model barely
separated the labels. Mean confidence when right versus wrong differs by less than
0.013 on every seed (0.315 vs. 0.303, 0.306 vs. 0.303, 0.309 vs. 0.305). Ranking
predictions into thirds is sorting very small differences.
This depends on the same training hypothesis as criterion 1: if a retrain produces
a model that actually separates labels, confidence should become more meaningful.



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** _30__ / 30 = _100__%

I labeled all 30 staff posts using the staff taxonomy before comparing my labels with the staff labels. I achieved **100% agreement (30 out of 30 posts)**.

There were no disagreements to adjudicate, so I did not need to classify any cases into the three disagreement categories. The set felt relatively straightforward to label because the taxonomy's decision rules clearly distinguished evidence-based arguments (`analysis`), unsupported opinions (`hot_take`), and personal reactions to recent events (`reaction`). However, matching the staff labels on this set does not necessarily mean the rules will be equally clear for every post; more ambiguous examples could reveal gaps in the definitions.


<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**

There were no disagreements to adjudicate: I matched the staff labels on all 30 posts, so the blocks above have nothing to fill. The lack of disagreements suggests I applied a written taxonomy's rules consistently, but it doesn't test the hardest boundary in my own taxonomy (`technique` vs. `recipe_troubleshooting`).



---

## The Improvement

**What I changed:**

One training setting: `EPOCHS` went from 3 to 10 (notebook section 2). Everything
else stayed the same — learning rate 2e-5, batch size 16, the same base model, the
same 200 labels, and the same three seeds (42, 7, 2024), so the before and after
runs use identical splits. I made no other changes, so any difference between the
two run logs comes from training length alone.

**Which diagnosis pointed at it:**

My Verdicts and Diagnoses section traced all three misses (criteria 1, 2 and 5) to
one pattern: the model predicted `technique` for 30/30, 30/30 and 28/30 test posts,
and its mean confidence was about 0.31 whether right or wrong, close to the 0.25
equal-probability reference for four labels. My hypothesis was that training did not
move the model far enough from its starting point to separate the labels.

With about 140 training posts and a batch size of 16, each epoch is roughly 9
updates, so the original 3 epochs gave the model only about 27 updates in total.
Raising epochs to 10 (about 90 updates) tests the hypothesis directly.

I ruled out the other two options because the evidence didn't point at them:
- **Relabeling a boundary:** my agreement check found no disagreements (30/30, on the
  staff taxonomy as a substitute measure), and the model never predicted three of my
  labels, so there is no confused boundary in the matrix to fix.
- **Adding examples to the thinnest label:** `ingredient` (44) and
  `recipe_troubleshooting` (45) are close in size to `equipment` (50), and the model
  ignored all three non-`technique` labels, not just the smallest.

I chose epochs over learning rate because 2e-5 is a typical value for fine-tuning
this kind of model, and too few updates is the simpler explanation to test first.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.70 | 0.567 | 0.633 | 0.533 | MISSED |
| 2. Every label F1 | ≥ 0.50 | 0.429 (lowest) | 0.500 (lowest) | 0.200 (lowest) | MISSED |
| 3. Class balance — largest label share | ≤ 60% | 30.5% | 30.5% | 30.5% | MET |
| 4. Labeling consistency — another person on my taxonomy | ≥ 80% | 100% (substitute, not per-seed) | 100% (substitute, not per-seed) | 100% (substitute, not per-seed) | MET (substitute measure) |
| 5. Accuracy of most confident third | ≥ 0.75 | 0.90 | 0.80 | 0.80 | MET |

Seed results after the change: (device - cpu)

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.567 | 0.633 | 0.533 | 0.100 |
| Macro F1 | 0.567 | 0.635 | 0.486 | 0.149 |
| F1 — `technique` | 0.560 | 0.500 | 0.522 | 0.060 |
| F1 — `recipe_troubleshooting` | 0.429 | 0.615 | 0.200 | 0.415 |
| F1 — `equipment` | 0.833 | 0.824 | 0.778 | 0.056 |
| F1 — `ingredient` | 0.444 | 0.600 | 0.444 | 0.156 |

Confusion matrix after the change (seed 42):

| true \ predicted | technique | recipe_troubleshooting | equipment | ingredient |
|---|---|---|---|---|
| **technique** | 7 | 2 | 0 | 0 |
| **recipe_troubleshooting** | 4 | 3 | 0 | 0 |
| **equipment** | 2 | 0 | 5 | 0 |
| **ingredient** | 3 | 2 | 0 | 2 |

**Did it help, and how do I know:**

Yes. It helped on every measure, though it didn't reach my accuracy or per-label targets.

- **Accuracy** rose on every seed: from 0.300 / 0.300 / 0.367 (mean 0.322) to
  0.567 / 0.633 / 0.533 (mean 0.578). Every seed now beats the 0.300 you'd get by
  always guessing `technique`, and every seed beats the zero-shot baseline's 0.433.
- **The collapse is gone.** Before, the model predicted `technique` for 30, 30 and 28
  of 30 posts. Now it predicts all four labels on every seed (`technique` 16, 11 and
  14 times). `ingredient` and `recipe_troubleshooting`, which scored 0.000 on every
  seed before, now score between 0.200 and 0.615.
- **Confidence now carries signal.** The gap between mean confidence when right and
  when wrong grew from under 0.013 to 0.06–0.09, and criterion 5 went from MISSED
  (0.60 / 0.50 / 0.50) to MET (0.90 / 0.80 / 0.80).

Because epochs was the only thing I changed, with the same data and the same three
seeds, I can attribute the improvement to it. That supports my hypothesis that the
original run was undertrained. It doesn't prove 10 epochs is the right number.

Still missed:
- **Criterion 1:** accuracy is still under 0.70 on all three seeds.
- **Criterion 2:** seed 7 actually met it (lowest F1 exactly 0.500), but seeds 42
  and 2024 didn't. `recipe_troubleshooting` was the weakest label, swinging from
  0.200 to 0.615.
- **Accuracy spread:** it's now 0.100, right at the ten-point line Milestone 2 warns
  about, so a 30-post test split is still a noisy measure.

What the new matrix shows that the old one couldn't: with the model now predicting
every label, the remaining errors land on `recipe_troubleshooting` vs. `technique`
(4 one way, 2 the other on seed 42). That's the boundary I named as my hardest in my
Label Taxonomy. `ingredient` is the other weak spot: its precision is 1.00 on every
seed but its recall is only 0.29–0.43. The model rarely guesses `ingredient`, so
most real ingredient posts go to `technique` or `recipe_troubleshooting`.

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

**Criterion 1 — Overall accuracy (still MISSED: 0.567 / 0.633 / 0.533 vs. ≥ 0.70).**
What I'd do: test my hardest boundary, `technique` vs. `recipe_troubleshooting`,
directly. I'd have another person label 30 of my cooking posts under my own
definitions, which is the measurement my original criterion 4 asked for and the
staff check couldn't give. If we disagree mostly on that pair, I'd rewrite the
decision rule and relabel only that boundary. If we agree, the pair is genuinely
hard, and I'd add examples near the boundary instead.
Why I stopped: Milestone 5 allows one measured change, and I used it on epochs
because that's what my diagnosis pointed at. Making a second change in the same
run would have made it impossible to tell which one helped.

**Criterion 2 — Every label F1 (still MISSED: lowest 0.429 / 0.500 / 0.200 vs. ≥ 0.50).**
What I'd do: the two weak labels fail differently. `recipe_troubleshooting` swings
from 0.200 to 0.615 across seeds and is tangled with `technique`, so the same
boundary check as criterion 1 applies. `ingredient` has precision 1.00 but recall
0.29–0.43, so the model under-predicts it; adding about 30 more `ingredient`
examples is the fix I'd try for that. I'd also want a larger test split, because
with only 7 posts per label one post moves F1 a lot.
Why I stopped: same reason as above, one change per run. I'd rather report one
change I can measure than two I can't separate.

**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->

I meant my labels to capture the primary kind of help a post asks for, but the
seed 42 matrix suggests the model leans on surface form instead: 4 of 7
`recipe_troubleshooting` posts and 3 of 7 `ingredient` posts were called
`technique`, as if any post asking "how do I…" counts as technique. `equipment`,
whose posts likely name distinctive objects like pans and ovens, is the one label
it learned well (F1 0.778–0.833), which fits the idea that it learned vocabulary
more than intent.



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
