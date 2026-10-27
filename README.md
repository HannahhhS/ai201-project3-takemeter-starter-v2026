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
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

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
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

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



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



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
