# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---
## 1. Overall Accuracy

The classifier should achieve at least 0.70 accuracy on the held-out test set across all three random seeds.

**Why this target:** Four labels make random guessing a weak baseline, and 0.70 would mean the classifier correctly labels a useful majority of posts. Testing three seeds helps show whether performance is reasonably stable.

---

## 2. Per-Label Performance

Each of the four labels should achieve an F1 score of at least 0.50 on the held-out test set across all three seeds.

**Why this target:** My labels overlap in real cooking questions, especially `ingredient` versus `recipe_troubleshooting`. I want to avoid a classifier that performs well overall while failing to recognize one of these categories. The minimum is deliberately lower than the overall accuracy target because smaller labels will have fewer test examples and noisier scores.

---

## 3. Class Balance

No label should account for more than 60% of the labeled dataset.

**Why this target:** My decision rule gives `recipe_troubleshooting` priority when a post describes an actual unexpected cooking result. This could make that label disproportionately large. A 60% cap encourages enough examples of `technique`, `equipment`, and `ingredient` for the classifier to learn their distinctions.

---

## 4. Labeling Consistency

If another person independently labeled the same 30 randomly selected posts using my README's label definitions and decision rule, their labels should match mine on at least 80% of the posts.

**Why this target:** Several posts mention multiple topics, so my primary-problem rule needs to be clear enough for another person to apply consistently. Checking 30 posts provides a practical way to identify confusing boundaries without reviewing the entire dataset twice. The ingredient and recipe_troubleshooting labels can overlap, so this target reflects my goal of writing clear enough definitions for another person to apply them consistently.

> **Revised in unit 6:** Measured instead as my agreement with staff labels on the
> 30-post staff set, labeled under the staff taxonomy (`data/staff_taxonomy.md`).
> Target unchanged: at least 80%.
>
> **Why revised:** The original couldn't be measured — no one else labeled posts
> under my cooking taxonomy, so there was nothing to compare against. The staff
> check is the closest available measure of whether I apply written definitions
> consistently. It uses a different taxonomy, so it is a substitute measure, not
> direct evidence of consistency in my own labels, and it did not test my hardest
> boundary (`ingredient` vs. `recipe_troubleshooting`).

---

## 5. Confidence

Across all three random seeds, the most confident third of predictions should achieve at least 0.75 accuracy on the held-out test set.

**Why this target:** My taxonomy has overlapping categories, especially ingredient and recipe_troubleshooting. I want the model's strongest predictions to be correct at least 75% of the time, showing that its confidence can help distinguish predictions that are more likely to be reliable.

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
