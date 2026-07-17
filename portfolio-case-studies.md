# Portfolio Case Studies

## 1. Voice Card

Practical. Direct. Honest. Still learning.

Tone:
- practical
- direct
- honest
- always learning
- no buzzwords

---

## 2. Case Study

**Project:** Content Refresh Prioritization (FlyRank Machine Learning Internship)

### Problem

Most websites end up with far more pages than anyone can realistically review one by one. Some pages are outdated, some are still doing fine, and figuring out which ones actually need a content refresh usually comes down to guesswork or whoever has time that week. That doesn't scale past a handful of pages, and it definitely doesn't scale for a real site. The goal of this task was to figure out how machine learning could help prioritize which webpages should be refreshed first, instead of relying on manual review.

### What I Did

- Framed the problem as a **ranking task** rather than a simple classification or regression problem, since the real goal was ordering pages by refresh priority, not just labeling them.
- Since there was no dataset that directly said "refresh this page" or "don't," I had to identify a reasonable **target/proxy** to stand in for that missing label.
- Chose **Precision@50** as the evaluation metric, since what matters most here is getting the top of the list right — the pages most worth reviewing first — rather than getting every single page perfectly ranked.
- Defined the unit of analysis clearly: **one row represents one webpage**.
- Worked through why a fixed rule-based approach (like "flag anything older than X months") falls short compared to an ML-based approach, which can weigh multiple signals together instead of relying on one arbitrary cutoff.
- Used the **FlyRank starter dataset** provided for the internship task.
- Completed the notebook as part of the FlyRank Machine Learning internship.

This was problem framing work — no model was trained, and no results were generated as part of this task.

### Outcome

By the end of this assignment, I had a complete ML problem framing for content refresh prioritization: a defined task, a working target/proxy in place of missing labels, a chosen evaluation metric, and a clear data structure. It's the groundwork needed before any model development can start — the kind of thinking that has to happen before you write a single line of model code.

---

## 3. Bio

I'm a B.Tech Computer Science Engineering student specializing in AI and Machine Learning, currently learning through hands-on internships and real projects rather than just coursework. My interests sit around machine learning, LLMs, and AI engineering more broadly — I like understanding not just how to build something, but why it's framed the way it is. I'm still early in this journey, and I'd rather be upfront about that than pretend otherwise. Most of what I know so far has come from actually sitting with problems, getting stuck, and working through them.

---

## 4. Contact / CTA

If you're a recruiter or an AI/ML professional and this kind of work is relevant to something you're hiring for, I'd genuinely like to connect. I'm open to internship opportunities in ML and AI engineering, and always happy to talk shop about problem framing, ranking tasks, or anything ML-related. Feel free to reach out on LinkedIn.

---

## 5. Before vs After

**Before:**
"Leveraging cutting-edge AI-driven synergies, our innovative solution seamlessly transforms data into actionable insights, unlocking unprecedented value at scale."

**After:**
"We built a ranking model that tells you which pages to fix first, based on the data you already have."

