# Prompting Fundamentals on Real Tasks v2

## Task

For this task I needed to turn a vague business problem — "some pages on a site perform worse than others and someone has to decide what gets refreshed" — into an actual ML framing: what kind of task it is, what target or proxy stands in for "needs a refresh," what one row of data represents, which metric actually reflects success, and why this shouldn't just be a fixed rule. This whole exercise came out of the FlyRank starter dataset for the Content Refresh Prioritization project.

---

## Version 0 — Naive Prompt

> Can you help me with my ML assignment?

**Representative Output:**
```
Of course! Machine learning assignments usually follow a general
flow: define your problem, gather and clean data, pick a model
type, train it, and evaluate the results. If you share more
details about your specific assignment, I can tailor this advice
further.
```

**Reflection:** Reading this back, it's basically filler. I hadn't told the AI anything about my actual problem, so it gave me the same five bullet points you'd get for any ML task ever assigned. Nothing here would've helped me actually start.

---

## Version 1 — Role Assignment

**Technique:** Role Assignment

**Prompt:**
> Act as a senior ML engineer mentoring an intern on problem framing. I need to frame the task of deciding which webpages should be prioritized for a content refresh.

**Representative Output:**
```
Alright — before we touch metrics, let's nail down the task type.
Are we ranking pages by urgency, or classifying each one as
"refresh" / "don't refresh"? Given you want a prioritized list
rather than a flat yes-no split, I'd lean toward ranking. Once
that's settled, everything else — target, metric — follows from it.
```

**Reflection:** Just telling it to act like a mentor changed the shape of the answer completely. Instead of handing me a finished checklist, it started asking the question a real mentor would ask first. It felt less like reading documentation and more like getting pushed to think.

---

## Version 2 — Context & Motivation

**Technique:** Context and Motivation

**Prompt:**
> Act as a senior ML engineer mentoring an intern on problem framing. I need to frame the task of deciding which webpages should be prioritized for a content refresh. This is a real internship deliverable — I have a starter dataset of webpages with SEO and traffic-related columns, and the goal right now is only the framing, not a working model.

**Representative Output:**
```
Good — knowing this is framing-only changes what I'd focus on.
Skip modeling entirely for now. Nail down: (1) is this ranking or
classification, (2) what proxy you'll use since you almost
certainly don't have a labeled "needs refresh" column, (3) what
one row in your dataset means, and (4) a metric that actually
reflects a ranked priority list, not just accuracy.
```

**Reflection:** This is where the output stopped drifting toward "and then you train a model." Telling it explicitly that I only needed the framing kept it inside the actual scope of my assignment, which the earlier versions didn't manage on their own.

---

## Version 3 — Few-shot Examples

**Technique:** Few-shot Examples

**Prompt:**
> Act as a senior ML engineer mentoring an intern on problem framing. Here are two framed examples for reference:
> Churn prediction — Task: classification, Target: customer churned (yes/no), Unit: one customer, Metric: recall.
> Search re-ranking — Task: ranking, Target: relevance score, Unit: one result per query, Metric: Precision@K.
> Using the same style, frame the task of prioritizing webpages for a content refresh.

**Representative Output:**
```
Following the same pattern as your ranking example:
- Task: ranking
- Target/Proxy: a signal representing declining page performance
- Unit: one webpage per row
- Metric: Precision@K, since getting the top priorities right
  matters more than scoring every page correctly
This lines up much closer with search re-ranking than with churn.
```

**Reflection:** What struck me here wasn't just that the format matched — it's that the model explicitly said which of my two examples it was mirroring and why. That's a small thing, but it made the answer feel reasoned rather than just copied from a template.

---

## Version 4 — Output Structure

**Technique:** Output Structure

**Prompt:**
> Act as a senior ML engineer mentoring an intern. Frame the task of prioritizing webpages for a content refresh. Present your answer as a checklist, one line per component, in the format: [Component] → [Choice] — [why].

**Representative Output:**
```
[ML Task] → Ranking — priority order matters more than a label
[Target/Proxy] → Performance-decline signal — no explicit labels exist
[Unit of Analysis] → One row per webpage — matches dataset granularity
[Evaluation Metric] → Precision@K — top-ranked accuracy is what counts
```

**Reflection:** Compared to the table format I'd used before in other prompts, this checklist style read faster and would drop straight into a notebook markdown cell without any cleanup. Asking for a specific layout meant I stopped getting paragraphs I'd have to manually restructure afterward.

---

## Version 5 — Step Decomposition

**Technique:** Step Decomposition

**Prompt:**
> Act as a senior ML engineer mentoring an intern. Frame the task of prioritizing webpages for a content refresh. Work through it in order: first the task type, then the target/proxy, then the unit of analysis, then the metric, then why ML beats a fixed rule. Finish with a checklist summary.

**Representative Output:**
```
1. Task type: ranking — the deliverable is an ordered priority list.
2. Target/proxy: a decline-in-performance signal standing in for
   "needs refresh," since no direct label exists.
3. Unit of analysis: one webpage per row.
4. Metric: Precision@K — correctness at the top matters most.
5. Why ML fits: a fixed rule like "refresh every 6 months" ignores
   traffic and ranking movement; ML can weigh multiple signals.

Summary:
[Task] Ranking · [Target] Decline signal · [Unit] One page/row ·
[Metric] Precision@K
```

**Reflection:** Forcing the model to work in order before summarizing made the final checklist feel earned instead of asserted. If a mentor asked "why Precision@K and not accuracy," the reasoning was already sitting right above the answer instead of me having to reconstruct it after the fact.

---

## Claude vs ChatGPT Comparison

| Category | Claude | ChatGPT |
|-----------|---------|----------|
| Tone | Reads like a mentor thinking out loud before landing on an answer | Gets to the point faster, less visible reasoning along the way |
| Accuracy | Stuck to "framing only" once told, never suggested training a model | Once suggested "you could also try a quick baseline" despite being told this was framing-only |
| Structure | Held the requested checklist/table format consistently across every version I tried | Mostly matched the format, but tacked on an extra summary paragraph after the table more than once |
| Strengths | Better at explaining *why* a choice was made, not just naming it | Quicker for a plain structured answer when I didn't need the reasoning spelled out |
| Weaknesses | Tends to run a little long even for a simple checklist request | Occasionally wandered into general SEO advice instead of staying inside the framing task |

---

## Final Reusable Prompt Template

```
Act as a senior ML engineer mentoring an intern on problem framing.
Help me frame [describe your business problem in one sentence],
using [briefly describe your dataset].

Work through it in order:
1. Identify the ML task (classification, ranking, regression, etc.)
   and explain why it fits.
2. Recommend a target or proxy variable and explain the reasoning.
3. Define the unit of analysis — what one row represents.
4. Recommend an evaluation metric suited to this task, and why.
5. Explain why ML is a better fit here than a fixed rule or manual
   review.

Requirements:
- This is a framing exercise only — do not propose model training
  steps, invented metrics, or fabricated results.
- Base your answer only on the problem and dataset described above.
- Return the final answer as a Markdown checklist or table with
  clear component labels.
```
