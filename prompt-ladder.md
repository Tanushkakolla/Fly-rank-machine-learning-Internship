# The Prompt Ladder

## Baseline Prompt (Weak)

**Prompt:**
"Help me with my ML assignment."

**Representative Output:**
Sure! Machine learning assignments can cover a lot of ground — could you tell me more about what you're working on? For example, is it a classification problem, regression, clustering, or something else? Also, do you have a dataset already, or are you starting from scratch? Once I know more about the task, I can help you pick an approach, suggest algorithms, and walk through how to structure your solution. Let me know the details!

---

## Version 1

**Layer Added:** Clear Goal

**Improved Prompt:**
"I need to frame a machine learning problem for prioritizing which webpages on a website should get a content refresh first. Help me figure out how to approach this as an ML task."

**Representative Output:**
This sounds like it could be framed as either a classification problem (refresh vs. don't refresh) or a ranking problem (order pages by priority). Since you're prioritizing pages rather than sorting them into two buckets, ranking is probably the better fit. You'd want to define what "needs a refresh" means, gather relevant features about each page (like age, traffic, or engagement), and decide how you'll evaluate whether the model's ranking is actually useful.

**What changed in the prompt:**
I stopped asking for generic help and actually named the real problem — prioritizing pages for content refresh.

**What improved in the output:**
The response stopped asking clarifying questions and instead gave a real, relevant suggestion — pointing toward ranking instead of classification, which was actually the right direction.

**What still failed:**
It didn't touch on the fact that there's no ready-made label for "needs a refresh" — that's something I had to bring up myself in later versions. It also didn't mention evaluation metrics or what a row in the dataset should represent.

**What I would try next:**
Tell it who the answer is for, so it doesn't explain things at too basic or too advanced a level.

---

## Version 2

**Layer Added:** Audience

**Prompt:**
"I need to frame a machine learning problem for prioritizing which webpages on a website should get a content refresh first. Explain this as if you're advising an ML intern who understands the basics of supervised learning but hasn't worked on ranking problems before."

**Representative Output:**
Since you already know supervised learning, think of ranking as an extension of that — instead of predicting a single label, you're predicting a score for each page and then sorting by that score. The tricky part with ranking problems is that you often don't have a clean "correct order" to train against, so you usually need to construct a target that approximates priority using signals like traffic drop-off, page age, or last-edit date. It's less about getting one prediction right and more about getting the overall order close to what a human reviewer would produce.

**Four notes:**
1. The explanation shifted from generic to intern-level — it assumed I knew supervised learning but not ranking specifically, which matched reality.
2. It introduced the idea of constructing a target/proxy, which the earlier version hadn't mentioned at all.
3. The tone became more like advice from a mentor than a textbook definition, which was easier to actually follow.
4. It still didn't mention anything about evaluation metrics, unit of analysis, or the FlyRank dataset specifically — the answer was accurate but still generic to "ranking problems" in general, not my assignment.

---

## Version 3

**Layer Added:** Context

**Prompt:**
"I need to frame a machine learning problem for prioritizing which webpages on a website should get a content refresh first. Explain this as if you're advising an ML intern who understands supervised learning but hasn't worked on ranking problems before. Context: I'm working with the FlyRank starter dataset for this internship task, and there are no existing labels telling me which pages need a refresh — I have to define a proxy target myself."

**Representative Output:**
Given that you don't have direct labels, the first real decision is what proxy signals reasonably stand in for "needs a refresh." Common options include time since last update, a drop in traffic or engagement over time, or a mismatch between a page's content and current search intent. None of these is a perfect substitute for a human judgment call, so it's worth being honest in your writeup about the proxy's limitations. From there, each row in your dataset would represent a single webpage, with features describing its age, performance, and content signals.

**Four notes:**
1. Adding the "no existing labels" context made the response directly address the proxy target problem instead of talking about ranking in the abstract.
2. It explicitly suggested proxy signal options relevant to a content-refresh scenario rather than generic ranking examples.
3. It brought up "one row = one webpage" on its own, which lined up with what I'd already decided, and confirmed I was framing the unit of analysis correctly.
4. It still avoided any mention of evaluation metrics — that gap carried over from the previous version and needed to be addressed directly.

---

## Version 4

**Layer Added:** Output Format

**Prompt:**
"I need to frame a machine learning problem for prioritizing which webpages on a website should get a content refresh first. Explain this as if you're advising an ML intern who understands supervised learning but hasn't worked on ranking problems before. Context: I'm working with the FlyRank starter dataset for this internship task, and there are no existing labels telling me which pages need a refresh — I have to define a proxy target myself. Format your answer as a short bulleted list, one bullet per key decision I need to make."

**Representative Output:**
- **ML Task:** Frame this as a ranking problem, not classification — you're ordering pages by priority, not sorting into two groups.
- **Target/Proxy:** Since there are no direct labels, define a proxy using signals like traffic decline, content age, or drop in engagement.
- **Unit of Analysis:** One row per webpage.
- **Next Decision:** You'll still need to pick how you'll evaluate whether your ranking is any good — that's a separate decision from defining the target.

**Four notes:**
1. Honestly, this layer helped less than I expected — the content of the answer barely changed from Version 3, it just got reformatted into bullets.
2. The bulleted structure did make it slightly easier to scan and turn directly into notes for my own writeup.
3. It didn't add any new information about evaluation metrics, which is a bit disappointing since that was the obvious next gap in the answer.
4. If anything, the bullet format made the answer feel slightly less like guidance and more like a checklist — for this particular question, the paragraph explanation in Version 3 actually did a better job of connecting the ideas together.

---

## Version 5

**Layer Added:** Quality Criteria

**Prompt:**
"I need to frame a machine learning problem for prioritizing which webpages on a website should get a content refresh first. Explain this as if you're advising an ML intern who understands supervised learning but hasn't worked on ranking problems before. Context: I'm working with the FlyRank starter dataset for this internship task, and there are no existing labels telling me which pages need a refresh — I have to define a proxy target myself. Format your answer as a short bulleted list, one bullet per key decision I need to make. Quality criteria: the answer must explicitly recommend an evaluation metric suited to ranking the top priority pages, and briefly justify why that metric fits better than plain accuracy."

**Representative Output:**
- **ML Task:** Ranking, since you're ordering pages by priority rather than classifying them into fixed categories.
- **Target/Proxy:** With no direct labels, define a proxy using signals like traffic decline or content age.
- **Unit of Analysis:** One row per webpage.
- **Evaluation Metric:** Use Precision@K (for example, Precision@50) instead of accuracy. Accuracy treats every page equally, but what actually matters here is whether the pages at the *top* of your ranked list — the ones someone would realistically act on first — are correctly prioritized. Precision@K focuses evaluation specifically on that top slice.

**Four notes:**
1. This was the layer that actually closed the gap — it's the first version to explicitly name and justify an evaluation metric instead of just alluding to needing one.
2. The justification for Precision@K over accuracy was genuinely useful and matched the reasoning I'd need for my own writeup.
3. The output stayed concise instead of turning into a wall of text, even with an extra requirement added on top of everything else.
4. The one thing it still doesn't do is explain *why ML is more appropriate than a fixed rule-based approach* — that reasoning has to come from a separate, more targeted prompt, since asking for too many things in one instruction starts diluting the depth of each answer.

---

## Final Reusable Prompt

```markdown
You are helping an ML intern frame a machine learning problem before any model development begins.

I'll describe a real-world scenario. Based on it, help me define the problem in ML terms by identifying the following, one at a time:

1. **ML Task** — What type of ML problem is this (classification, regression, ranking, clustering, etc.), and why does that framing fit better than the alternatives?
2. **Target or Proxy** — If there's no direct label available, suggest a reasonable proxy target and be upfront about its limitations.
3. **Unit of Analysis** — What does a single row in the dataset represent?
4. **Evaluation Metric** — Recommend a metric suited to this specific task, and explain briefly why it's a better fit than a generic metric like accuracy.
5. **Why ML is Appropriate** — Explain why a machine learning approach makes more sense here than a fixed, rule-based approach.

Keep the explanation practical and grounded — don't invent data, results, or performance numbers. If something depends on data I haven't provided, say so instead of assuming.

Here is my scenario:
[Describe your scenario here]
```
