# Story Bank — Master STAR+R Stories

This file accumulates your best interview stories over time. Each evaluation (Block F) adds new stories here. Instead of memorizing 100 answers, maintain 5-10 deep stories that you can bend to answer almost any behavioral question.

## How it works

1. Every time `/career-ops oferta` generates Block F (Interview Plan), new STAR+R stories get appended here
2. Before your next interview, review this file — your stories are already organized by theme
3. The "Big Three" questions can be answered with stories from this bank:
   - "Tell me about yourself" → combine 2-3 stories into a narrative
   - "Tell me about your most impactful project" → pick your highest-impact story
   - "Tell me about a conflict you resolved" → find a story with a Reflection

## Stories

<!-- Stories will be added here as you evaluate offers -->
<!-- Format:
### [Theme] Story Title
**Source:** Report #NNN — Company — Role
**S (Situation):** ...
**T (Task):** ...
**A (Action):** ...
**R (Result):** ...
**Reflection:** What I learned / what I'd do differently
**Best for questions about:** [list of question types this story answers]
-->

---

### [Classification / Scale] UMD Synapse Classifier
**Source:** Report #010 — Anthropic — ML/Research Engineer, Safeguards
**S (Situation):** Multi-lab neuroscience data with no consistent labeling schema; expert manual annotation was the bottleneck.
**T (Task):** Build a system to automatically differentiate synapse subtypes and replace manual expert review at scale.
**A (Action):** Built a 3-model ensemble (SVM, Random Forest, Gradient Boosting), applied feature engineering, normalization, and dimensionality reduction using Scikit-Learn and OpenAI. Automated data collection pipeline in Python.
**R (Result):** Classifier replaced the manual review step; automated pipeline enabled large-scale experiments that were previously impractical.
**Reflection:** Would add cross-validation holdout earlier; class imbalance was a late discovery that required going back to the data. Planning for evaluation from day one would have saved a cycle.
**Best for questions about:** building classifiers, scaling ML, automating expert decisions, multi-model systems, applied ML research

---

### [Safety-Critical Pipelines] WHOOP SaMD Data Pipeline
**Source:** Report #010 — Anthropic — ML/Research Engineer, Safeguards
**S (Situation):** Medical device company with FDA-regulated data governance; analysis errors could affect regulatory submissions.
**T (Task):** Validate and analyze physiological data at scale under QMS controls, delivering actionable findings for product decisions.
**A (Action):** Used Snowflake and EzDeploy to build and run EDA pipelines; documented analytical decisions and flagged edge cases for senior review per FDA QMS standards.
**R (Result):** Maintained compliance while delivering EDA findings on schedule; pipeline findings directly informed Class I device product decisions.
**Reflection:** Regulatory rigor taught me the difference between "good enough for insight" and "good enough for decisions that go into a regulatory filing." That distinction matters in any high-stakes ML system.
**Best for questions about:** safety-critical environments, data governance, regulated ML, research-to-production, judgment under constraints

---

### [Signal Robustness / Anomaly Detection] GT Nanoengineering EEG Stress Detection
**Source:** Report #010 — Anthropic — ML/Research Engineer, Safeguards
**S (Situation):** Raw EEG signals from human subjects are noisy, artifact-heavy, and subject-variable — a hostile input environment.
**T (Task):** Build a real-time stress detection system that classifies cognitive state reliably despite data quality issues.
**A (Action):** Applied filtering, artifact removal, normalization, and feature extraction pipeline before training deep BLSTM-LSTM network. Iterated on preprocessing until the model's performance stabilized.
**R (Result):** Achieved real-time stress detection on a cleaned EEG stream with stable inference.
**Reflection:** Would instrument the preprocessing pipeline better for failure detection — artifacts that slipped through degraded model performance silently. Monitoring inputs is as important as monitoring outputs.
**Best for questions about:** ML robustness, noisy data, real-time ML, deep learning, anomaly/signal detection, pipeline design

---

### [Leadership / Communication] JHU APL Triage System + IEEE Presentation
**Source:** Report #010 — Anthropic — ML/Research Engineer, Safeguards
**S (Situation):** Novel electronic triage tagging system concept with no existing literature baseline; intern team of 3, no prior collaboration.
**T (Task):** Lead the team to design, prototype, and present to an academic and government audience — with grant funding on the line.
**A (Action):** Structured the project into testable milestones, coordinated the 3-person team, secured a $20K grant through the proposal process, and presented the prototype at the 2023 IEEE ISEC.
**R (Result):** Grant secured; prototype validated by subject matter experts; team delivered on time.
**Reflection:** The public presentation revealed assumptions we had not adequately tested before the audience asked hard questions. Learned to stress-test claims before stakeholder review, not during it.
**Best for questions about:** leadership, cross-functional teams, communication, early-stage research, presenting to non-technical audiences
