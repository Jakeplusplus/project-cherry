# Task breakdown techniques that lower the barrier to starting

Research for ticket `06-task-breakdown-techniques`. Compiled 2026-10-07.

**How to read the citations.** Each source is tagged with how far I got into it:

- **[read]** — I retrieved the full text or the owner's own page and read the relevant part.
- **[abstract]** — I retrieved only the abstract / publisher summary (directly or as returned verbatim by a search index).
- **[secondary]** — I could not open the primary; the claim rests on a secondary description. Treat as a lead, not a fact.

Evidence-strength labels used below: **Studied in adult ADHD** (RCT of a package containing the technique), **Studied, not in adult ADHD** (experimental evidence in other populations), **Method lore** (a named method's own doctrine, no outcome trial located), **Folklore** (repeated in coaching/app content, no source of record located).

---

## Answer

1. **The one breakdown rule with a clinical pedigree is a trigger rule, not a decomposition procedure:** "If I am having trouble getting started, then the first step is too big." It is a taught self-instruction in Solanto's meta-cognitive therapy, which beat an attention-matched control in an 88-person RCT ([Solanto et al. 2010](https://pmc.ncbi.nlm.nih.gov/articles/PMC3633586) [read]). Cherry's "promote a too-big step to its own task" rule is the same idea expressed structurally. Build the workflow around *detecting a non-startable first step and shrinking it*, not around producing a complete plan.
2. **Both manualised adult-ADHD CBT programmes with RCT support teach breakdown**, as one component among many: Safren's programme lists "breaking down overwhelming tasks into steps" inside its organising/planning module ([Safren et al. 2010](https://pmc.ncbi.nlm.nih.gov/articles/PMC3641654/) [read]); Solanto's lists "dismantling complex tasks into manageable parts" ([Solanto et al. 2010](https://pmc.ncbi.nlm.nih.gov/articles/PMC3633586) [read]). **Neither trial isolates breakdown.** Nobody has shown that breakdown *by itself* helps, in ADHD or elsewhere, and nobody has tested a self-guided app version.
3. **The best-specified wording for a step comes from GTD, not from the clinic:** the "next physical, visible action" ([David Allen Company](https://gettingthingsdone.com/2010/02/managing-projects-tips-from-david-allen/) [read]). It is method lore with no outcome trial I could find, but it gives a usable, checkable test for a step ("could someone watch you do it?").
4. **Plan less than feels natural.** Allen explicitly advises against listing sequential steps on the action list ("it dulls the attraction of engaging with the list") — one next action is a "bookmark" ([same page](https://gettingthingsdone.com/2010/02/managing-projects-tips-from-david-allen/) [read]). Detailed planning across many goals has been shown to *reduce* follow-through in non-ADHD samples ([Dalton & Spiller 2012](https://doi.org/10.1086/664500) [abstract]). Recommendation: the guided flow should require **one** startable step and make every further step optional.
5. **Breakdown and the time bucket reinforce each other.** Listing a task's component steps before estimating lengthens estimates and shrinks the planning fallacy in general-population experiments ([Kruger & Evans 2004](https://doi.org/10.1016/j.jesp.2003.11.001) [secondary], as described in [Moher 2012, Univ. of Waterloo thesis](https://uwspace.uwaterloo.ca/items/72e17776-1f0b-42c5-a971-f1c177d81909) [abstract]). So bucket the steps *after* listing them, and treat a step bucketed long / whole day as the "too big" signal that offers promotion.
6. **Almost every existing tool that does breakdown as a guided act does it with AI** (Goblin Tools, Tiimo). Manual breakdown in mainstream tools is just an empty subtask list. The only guided, non-AI, question-driven precedent I found is Amazing Marvin's Procrastination Wizard, and I could not get its actual question text. A manual guided breakdown is therefore a real gap — and also an untested design.

**Biggest caveat.** Everything clinical here is evidence for *12-session, therapist-delivered, multi-component packages* in which breakdown is one ingredient; the Cochrane review rates the whole CBT-for-adult-ADHD evidence base as low quality ([Lopez et al. 2018](https://www.cochrane.org/evidence/CD010840_cognitive-behavioural-therapy-attention-deficit-hyperactivity-disorder-adhd-adults) [read]). The specific question sequences in this document are my synthesis. None has been tested. Treat them as prototype candidates.

---

## Techniques and evidence strength

### 1. Breaking overwhelming tasks into steps (CBT for adult ADHD) — Studied in adult ADHD, as part of a package

**Safren et al., individual CBT.** RCT, 86 medicated adults with residual symptoms, CBT vs relaxation with educational support (attention-matched), 12 sessions. Module 1 (4 sessions) is described as "psycho-education about ADHD and training in organizing and planning (use of calendar and task list system), including problem-solving training (generating alternatives and picking the best solution, breaking down overwhelming tasks into steps)". An optional procrastination session was taken by 40 of 43 CBT patients. CBT response rates were 53% vs 23% (CGI) and 67% vs 33% (ADHD rating scale) ([Safren et al. 2010, JAMA 304(8):875–880](https://pmc.ncbi.nlm.nih.gov/articles/PMC3641654/), doi:10.1001/jama.2010.1192 [read]).

The client workbook operationalises this in Chapter 6, "Problem Solving and Managing Overwhelming Tasks". The publisher abstract defines an overwhelming task as one that "remain[s] on the task list for many days or weeks without getting completed", gives a five-step problem-solving strategy (articulate the problem, list solutions, list pros and cons, rate each solution, implement the best), and says "instructions are also given for breaking down large tasks into smaller, more manageable chunks" ([Safren, Sprich, Perlman & Otto 2017, *Mastering Your Adult ADHD*, Client Workbook, 2nd ed., OUP](https://academic.oup.com/book/1239/chapter/140165738) [abstract]). I could not read the chapter body, so the workbook's exact breakdown instructions are not reproduced here.

Two things worth taking from this even at abstract level:

- The manual treats **"I don't know how to proceed"** (needs problem solving) and **"it's too big"** (needs breakdown) as two different problems under one heading. A breakdown workflow that only ever asks "what are the steps?" misses the first.
- The entry criterion is behavioural: the task has sat on the list. That is a signal an app can observe.

**Solanto et al., group meta-cognitive therapy (MCT).** RCT, 88 adults, MCT vs supportive group therapy, 12 weeks. The programme includes "facilitation of task initiation and completion via dismantling tasks into manageable parts". It teaches self-instructions meant to be repeated until automatic; the paper gives this one verbatim: "If I am having trouble getting started (cue), then the first step is too big (solution is to break task down into parts)." Sessions 2–6 covered time-awareness, task initiation, contingent self-reward, scheduling, prioritising and visualising reward; sessions 10–11 covered planning using flow-charting. Blind-rated inattention responders: 42.2% vs 12%. Stated limits: above-average-IQ sample, no long-term follow-up ([Solanto et al. 2010, Am J Psychiatry 167(8):958–968](https://pmc.ncbi.nlm.nih.gov/articles/PMC3633586), doi:10.1176/appi.ajp.2009.09081123 [read]).

Note the form of that maxim: it is itself an if-then plan (see §3). It makes *difficulty starting* the cue for shrinking, which means the user does not have to judge step size in the abstract.

A secondary summary of Solanto's published manual lists further maxims ("If it's not in the planner it doesn't exist", "All things must be done in order of priority") and says breaking an assignment into smaller steps is practised as an in-session exercise ([Mollick, NJ-ACT summary](https://nj-act.org/solanto.html) [secondary]; the manual itself is [Solanto 2011, Guilford](https://www.guilford.com/re/9781462509638), not read).

**Strength of the package overall.** A meta-analysis of 32 studies reports CBT vs control effects of g = .65 on self-reported symptoms and .51 on functioning, smaller against active controls ([Knouse, Teller & Brooks 2017](https://pubmed.ncbi.nlm.nih.gov/28504540/) [abstract]). Cochrane (14 RCTs, 700 adults): "low-quality evidence that cognitive-behavioural-based treatments may be beneficial for treating adults with ADHD in the short term" ([Lopez et al. 2018](https://www.cochrane.org/evidence/CD010840_cognitive-behavioural-therapy-attention-deficit-hyperactivity-disorder-adhd-adults), doi:10.1002/14651858.CD010840.pub2 [read]).

**What this does not show.** No dismantling study isolating breakdown was located. No trial of self-guided or app-delivered breakdown was located.

### 2. Goal Management Training: STOP – STATE – SPLIT — Studied in adult ADHD, weak result

GMT is a neuropsychological rehabilitation protocol originally validated after brain injury ([Levine et al. 2000](https://www.tara.tcd.ie/items/28b85baf-7e90-4973-ab76-df3b4ebe2db4) [secondary]). Its sequence is: stop the "automatic pilot", state the goal, split it into subtasks, then check. One module is specifically about modifying overwhelming tasks by dividing them into subtasks using "STOP!-STATE-SPLIT", per the module table in the ADHD trial below.

In adults with ADHD: an 81-person RCT of GMT plus psychoeducation vs treatment as usual found **no significant between-group difference on perceived executive functioning (the primary outcome) or ADHD symptoms**; both groups improved, and only anxiety improved more with GMT. Limits include high attrition and no blinding ([Hanssen et al. 2023, Front Psychol 14:1212502](https://pmc.ncbi.nlm.nih.gov/articles/PMC10690829/), doi:10.3389/fpsyg.2023.1212502 [read]). An earlier small controlled study (n = 12 vs 15) exists ([In de Braek et al. 2017, J Atten Disord, doi:10.1177/1087054712468052 — Maastricht record](https://cris.maastrichtuniversity.nl/en/publications/goal-management-training-in-adults-with-adhd-an-intervention-stud) [secondary]).

Useful as a **shape** (state the goal before splitting; re-check afterwards). Not usable as evidence that splitting helps ADHD adults.

### 3. Implementation intentions (if-then plans) — Studied, not in adult ADHD

**What it is.** A plan of the form "Whenever situation x arises, I will initiate the goal-directed response y", which hands initiation to an external cue. Gollwitzer names "failing to get started" as one of the problems it targets ([Gollwitzer 1999, Am Psychol 54:493–503](https://www.socmot.uni-konstanz.de/publications/implementation-intentions-strong-effects-simple-plans), doi:10.1037/0003-066X.54.7.493 [abstract]).

**Evidence.** In the founding studies, difficult goals were completed about three times more often when furnished with an implementation intention, and action was initiated faster when the opportunity arose ([Gollwitzer & Brandstätter 1997, JPSP 73(1):186–199](https://kops.uni-konstanz.de/entities/publication/cae1c06f-6b81-4700-ae1d-a3959e2741f8), doi:10.1037/0022-3514.73.1.186 [abstract]). The standard meta-analysis is [Gollwitzer & Sheeran 2006](https://www.socmot.uni-konstanz.de/publications/implementation-intentions-and-goal-achievement-meta-analysis-effects-and-processes) (doi:10.1016/S0065-2601(06)38002-1); the commonly quoted figure is d = .65 across 94 tests, which I saw only in secondary summaries [secondary].

**In ADHD.** The direct evidence is in **children** on a **lab inhibition task**: children with ADHD given an if-then plan improved Go/NoGo inhibition to the level of children without ADHD ([Gawrilow & Gollwitzer 2008, Cogn Ther Res 32:261–280](https://kops.uni-konstanz.de/entities/publication/80d90ce7-833e-44c3-8640-12d6fed7eaa8), doi:10.1007/s10608-007-9150-1 [abstract]). I found no trial of if-then plans for *task initiation* in *adults* with ADHD. Applying this to Cherry is an extrapolation across both age and behaviour.

**The catch.** If-then planning helps for a single goal but can backfire when applied to many at once: forming detailed plans for several goals made people see how hard execution would be and weakened commitment ([Dalton & Spiller 2012, J Consum Res 39(3):600–614](https://doi.org/10.1086/664500) [abstract]). Non-ADHD samples.

**Design implication.** At most one if-then, attached to the *first* step only, and optional. Cherry has no reminders or scheduling by design, so the "if" has to be a situation the user names in words ("after I sit down with coffee"), not a notification. Whether a cue that the app never surfaces still works for this user is unknown.

### 4. GTD "next action" — Method lore

David Allen Company's own definitions: "Projects = Your outcomes that require more than one action step. Next Actions = Your next physical, visible action steps." Allen's advice on quantity: do not put sequential steps on the action list — "If you put sequential steps there, it dulls the attraction of engaging with the list to begin with"; and if a project can probably be finished in one sitting, "probably best to label it simply a next action" ([Managing Projects – Tips from David Allen](https://gettingthingsdone.com/2010/02/managing-projects-tips-from-david-allen/) [read]).

On why it works, in Allen's words: defining "the next visible physical activity required to move something forward... actually finishes the thinking you've implicitly agreed with yourself that you'll do". His example: "Mom" is an unclarified item; "Call Sis about what we should do for Mom's birthday" is a next action ([How is a Next Action List Different from a To Do List?](https://gettingthingsdone.com/2011/02/how-is-a-next-action-list-different-from-a-to-do-list/) [read]).

No outcome trial of GTD or of the next-action rule was located, in ADHD or otherwise. It is the practitioner consensus on *how to word* a step, and it maps closely onto Cherry's model (project → next action ≈ task → first step).

### 5. Proximal subgoals — Studied, not in adult ADHD

Children with large deficits and no interest in maths who worked toward proximal subgoals progressed faster and gained more self-efficacy and interest than those given a distal goal, which "had no demonstrable effects" ([Bandura & Schunk 1981, JPSP 41:586–598](https://uploads-ssl.webflow.com/59faaf5b01b9500001e95457/5bc552d85141987915dab842_Bandura%20&%20Schunk,%201981.pdf) [abstract]). This is the classic experimental support for "small near goals beat one far goal". It is one study in children on arithmetic; it supports the general direction, not any specific step size.

### 6. Unpacking before estimating — Studied, not in adult ADHD

When a task is unpacked into its procedural steps, people give longer completion-time estimates and the planning fallacy shrinks ([Kruger & Evans 2004, J Exp Soc Psychol 40:586–598](https://doi.org/10.1016/j.jesp.2003.11.001) [secondary]). A follow-up thesis reports the effect is weaker for distant-future tasks than near-future ones, because people hold fewer concrete details about them ([Moher 2012](https://uwspace.uwaterloo.ca/items/72e17776-1f0b-42c5-a971-f1c177d81909) [abstract]).

Implications for Cherry, both inferences: (a) bucket after listing steps, and expect the sum of step buckets to exceed the parent's original bucket — show that as information, not as an error; (b) breakdown is more useful close to doing the task than at capture.

### 7. "Smallest first step", "five-minute rule", "microtasks" — Folklore (compatible with §1)

These appear throughout app and coaching content without a source of record. Examples: "What is the smallest action that would move this forward?" and "put dirty towels in laundry bin" instead of "clean the bathroom" ([Tiimo, microtasks article](https://www.tiimoapp.com/resource-hub/adhd-microtasks-productivity) [read]); "put one mug in the sink" and a five-minute commitment ([Tiimo, task initiation article](https://www.tiimoapp.com/resource-hub/task-initiation-adhd) [read]). The Tiimo microtasks article cites four general ADHD neuroscience reviews; none is a test of microtasking. The mechanism claims in such content (dopamine from completions, working-memory relief) are plausible stories, not findings about this technique.

The smallest-step idea is consistent with Solanto's maxim, which is the nearest thing it has to a clinical anchor. The five-minute rule is a different technique (time-limiting, not decomposition) and I found no primary source for it.

### 8. ADHD coaching — weak, non-specific

A descriptive review counted 19 outcome studies of ADHD coaching, 17 reporting improved executive functioning or symptoms, but designs ranged from case studies to a few RCTs, over half were in college students, and the source reporting it is a coaching trade body ([ADHD Coaches Organization, research summary of Ahmann et al. 2018](https://www.adhdcoaches.org/adhd-coaching-research) [secondary]). Coaching is a relationship plus many techniques; it says nothing specific about breakdown prompts. I found no coaching source that documents a breakdown question sequence with outcome data.

---

## How existing tools do breakdown

| Tool | Mechanism | Guided? | AI? | Depth / time | Source |
|---|---|---|---|---|---|
| **Goblin Tools – Magic ToDo** | Type a task; it generates steps. A "Spiciness level" control (1–6 chillies): "How much breaking down do you need? You can change this at any time, it'll apply to all items!" Each item has edit, remove, add subtask, estimator. Tagline: "Breaking things down so you don't". | No questions; one-shot generation | Yes — "Most tools use AI technologies in the back-end" | Recursive subtasks; AI "Estimate" button | [goblin.tools/ToDo](https://goblin.tools/ToDo), [goblin.tools/About](https://goblin.tools/About) [read] |
| **Tiimo** | AI Co-planner: "Just say or type what you need to do and Tiimo turns it into clear steps, sets priorities"; "suggests a step-by-step breakdown". A per-task "Suggest Breakdown" function; subtasks can also be written by hand. | No questions | Yes for the guided path; manual is a blank list | Task → subtasks; AI time estimates; timers on subtasks referenced | [to-do list page](https://www.tiimoapp.com/product/to-do-list), [AI planning page](https://www.tiimoapp.com/product/ai-planning), [microtasks article](https://www.tiimoapp.com/resource-hub/adhd-microtasks-productivity) [read] |
| **Amazing Marvin** | (a) Subtasks strategy: a checklist inside a task. Subtasks "can't have their own labels or time estimates"; to get independent steps "turn the task into a project instead". (b) Procrastination Wizard: "this interactive guide asks you questions about what feels hard. It identifies whether you're anxious or unmotivated, then gives you targeted strategies to begin." (c) "The Smallest Step (Coming Soon): Break any task into its tiniest possible first action. 'Write report' becomes 'Open document.'" (d) Task Hints: user-defined prompts shown while creating a task. | Wizard is guided and question-driven; subtasks are not | Not stated for the wizard | Subtask (untimed) or promote task → project (timed tasks) | [subtasks help](https://help.amazingmarvin.com/en/articles/1951489-all-about-subtasks) [abstract], [for procrastinators](https://amazingmarvin.com/for/procrastinators/), [task hints](https://help.amazingmarvin.com/en/articles/9750543-task-hints) [read] |
| **Things 3** | Checklist inside a to-do: "Some things take several steps to complete but don't require a full-blown project. For those cases we now have checklists". Headings split a project. | No | No | Project → to-do → checklist (untimed) | [Things features](https://culturedcode.com/things/features/) [read] |

Observations:

- **Goblin Tools' own caveat is the argument for manual.** Its About page says output accuracy "can vary", results are "only guesswork", and users should judge validity themselves ([About](https://goblin.tools/About) [read]). A generated step list is generic; the user still has to notice that step 1 is not startable *for them*.
- **The "spiciness" dial is the best UI idea in the set and does not need AI.** It reframes granularity as "how much breaking down do you need right now", which varies by day. A manual analogue: let the user choose "just the first step" versus "lay it all out".
- **Marvin's subtask / project split is exactly Cherry's promotion rule**, and Marvin's docs show the cost of the alternative: subtasks that cannot carry estimates. Cherry's steps are estimated and timed, so Cherry's step is closer to a Marvin task-in-a-project than to a Marvin subtask.
- **Marvin's Procrastination Wizard is the only precedent for diagnosing *why* a task is stuck before prescribing.** It branches on anxious vs unmotivated, which matches the Safren manual's separation of "don't know how" from "too big" (§1). Breakdown is one remedy among several.
- **Marvin positions time estimates as an initiation aid**: "Knowing a task only takes 15 minutes makes it less scary" ([for procrastinators](https://amazingmarvin.com/for/procrastinators/) [read]). That is a product claim, not evidence, but it lines up with Cherry bucketing every step.

---

## Candidate guided-question sequences

**Everything in this section is my synthesis.** No source prescribes these sequences and none has been tested. The bracketed tags show which source each question borrows from. All three assume Cherry's fixed model: task → steps, a bucket per step, promote a too-big step.

### Sequence A — "First step only" (default; shortest path to starting)

Borrows from: GTD next action (§4), Solanto's maxim (§1), Allen's advice against sequential steps (§4).

1. **"What's the first thing you'd physically do?"** Placeholder shows a verb-first example ("Open the tax folder"). [GTD: physical, visible]
2. **"Could you start that right now, as written?"** Yes / Not really.
   - *Not really* → **"What has to happen before that?"** The answer becomes the new first step; the previous answer moves to second. Ask at most twice, then accept. [Solanto: trouble starting ⇒ first step too big]
3. **"How long is that step?"** short / medium / long / whole day. [Kruger & Evans: estimate after unpacking]
   - *long* or *whole day* → "That's a big first step. Shrink it, or make it its own task?" [Cherry promotion rule]
4. Exit with two equal buttons: **Start it** (opens timer) / **Done for now**. A smaller third option: "Add what comes after".

Why this is the default: it produces exactly one startable step, ends in a timer, and stops. It cannot be used to over-plan.

### Sequence B — "Lay it out" (for tasks where the user cannot see the shape)

Borrows from: Safren workbook's breaking large tasks into chunks (§1), GMT state-then-split (§2), unpacking (§6).

1. **"What does done look like?"** One line, skippable. [GMT: STATE]
2. **"List the steps as they come to you. Order doesn't matter yet."** Free entry, one per line. Soft cap with a nudge at about 7: "That's plenty to start with. You can add more later." [Dalton & Spiller: detailed plans can undermine commitment]
3. **"Which one comes first?"** Tap one; the rest stay in entry order (drag to reorder is available but not asked for).
4. **Run Sequence A steps 2–3 on the chosen first step only.** [Solanto]
5. **"Quick size for the others?"** One-tap bucket per remaining step, with "skip, I'll size them when I get there". Any step sized long / whole day shows the promote offer.
6. Show the total against the parent's bucket as a plain fact ("Steps add up to roughly a long. You'd called this medium."). Then the same **Start it / Done for now** exit.

Open design question: whether step 5 is worth the taps. The unpacking literature supports it; the over-planning literature argues for skipping it.

### Sequence C — "What's in the way?" (for a task or step that has been sitting)

Borrows from: Safren workbook's separation of problem solving from breakdown and its sat-on-the-list criterion (§1), Marvin's Procrastination Wizard branching (tools), Solanto's cue-based maxim (§1). Entered by the user from a stuck item; it could also be offered when a timer has been started and abandoned, which fits the "timer-tied only" constraint.

**"What's making this hard to start?"** One tap:

| Answer | Follow-up | Result |
|---|---|---|
| **It's too big** | Sequence A | A smaller first step |
| **I don't know how to begin** | "What's one thing you could find out, or who could you ask?" | A step that is a look-up or a question — itself a physical action [Safren: problem solving; GTD] |
| **I'm missing something** | "What do you need, and where is it?" | A "get X" step placed first |
| **I have to decide something first** | "What are the options? Write two." | A step "Pick between A and B" [Safren: list solutions, pick one] |
| **I just don't want to** | "What's the least you could do and still count it as started?" + optional "When will you do it? After I ___." | A token step, optionally with one if-then [Solanto; Gollwitzer] |

The last row is where breakdown is weakest: the problem is aversion, not size. Offering a token step is the folklore answer (§7). This branch should not pretend to more than that.

### Wording rules that apply to all three (synthesis)

- A step starts with a verb you could be seen doing. Flag — do not block — steps that start with "think about", "figure out", "work on", "decide". [GTD]
- Never show an empty multi-row step list as the first screen; that is the mainstream-tool pattern the guided flow exists to replace.
- Every sequence ends at a timer or a clean exit, never at "add another step?".

---

## Failure modes

| Failure mode | What it looks like | Basis | Design response (synthesis) |
|---|---|---|---|
| **Over-planning as avoidance** | A long, tidy step list and no timer started; re-editing steps instead of doing one | Allen: sequential steps on the list "dulls the attraction" ([GTD](https://gettingthingsdone.com/2010/02/managing-projects-tips-from-david-allen/) [read]). Detailed planning for multiple goals undermines commitment ([Dalton & Spiller 2012](https://doi.org/10.1086/664500) [abstract]). The framing "planning as avoidance in ADHD" is itself coaching lore — I found no study of it. | Default to Sequence A. Soft cap on steps. Exit is always "Start it". Do not reward step count. |
| **Steps still too big** | First step is a renamed copy of the task ("Start report") or is bucketed long | Solanto's maxim ([2010](https://pmc.ncbi.nlm.nih.gov/articles/PMC3633586) [read]) | Use behaviour as the detector: "could you start this now?" at creation; a started-then-abandoned timer later. Long / whole-day bucket on a step triggers the promote offer. |
| **Steps too small** | Twelve one-minute steps; ticking them costs more than doing them | Not studied that I found. Goblin Tools treats granularity as a user-set dial, implying the right size varies ([ToDo](https://goblin.tools/ToDo) [read]). | Only the first step needs to be tiny. Let later steps stay coarse. |
| **Vague or mental-verb steps** | "Think about budget", "Sort out insurance" | GTD's physical/visible test ([GTD](https://gettingthingsdone.com/2011/02/how-is-a-next-action-list-different-from-a-to-do-list/) [read]) | Verb-first placeholder; gentle flag on mental verbs. |
| **Breakdown used on the wrong problem** | Task is stuck because of a missing decision, missing information, or dread; splitting it yields smaller stuck steps | Safren workbook separates problem solving from breakdown ([OUP](https://academic.oup.com/book/1239/chapter/140165738) [abstract]); Marvin's wizard separates anxious from unmotivated ([Marvin](https://amazingmarvin.com/for/procrastinators/) [read]) | Sequence C: ask what is in the way before asking for steps. |
| **Under-estimated steps** | Every step is "short"; the whole runs three times over | Planning fallacy / unpacking ([Kruger & Evans 2004](https://doi.org/10.1016/j.jesp.2003.11.001) [secondary]) | Bucket after listing. Feed actuals back (already in Cherry's retro design). |
| **Breaking down too early** | Steps written at capture are stale or wrong by the time the task is picked | Unpacking is weaker for distant tasks ([Moher 2012](https://uwspace.uwaterloo.ca/items/72e17776-1f0b-42c5-a971-f1c177d81909) [abstract]) — an inference from estimation research, not a direct finding about stale plans | Keep breakdown out of capture (already a constraint: title + bucket only). Offer it when a task is suggested or picked. |
| **Promotion sprawl** | Promoting steps to tasks multiplies the task list, which is its own overwhelm | Marvin's rationale for hiding the master list and showing one task at a time ([Marvin](https://playground.amazingmarvin.com/how-to-beat-procrastination-with-marvin/) [read]) — a product rationale, not evidence | Promotion should be offered, not automatic. The suggestion engine already shows one task at a time. |
| **Generic steps** (AI tools) | Plausible list that does not match the user's actual situation | Goblin Tools: output is "only guesswork" ([About](https://goblin.tools/About) [read]) | Not applicable to a manual flow; relevant if on-device AI is added later. |

---

## Unknowns / could not verify

**Gaps in the evidence itself**

- **No study isolates breakdown.** Both adult-ADHD RCTs test whole programmes. Whether breakdown contributes, and how much, is unknown.
- **No study of app-delivered or self-guided breakdown in ADHD** was found. The clinical versions are taught and rehearsed with a therapist over weeks, with homework review.
- **No adult-ADHD study of implementation intentions for starting tasks.** The ADHD evidence is children plus lab inhibition tasks.
- **No evidence on optimal step size or step count**, in any population. "2 minutes", "5 minutes", "about 7 steps" in this document are my placeholders.
- **"Over-planning as avoidance" is unstudied in ADHD** as far as I could find. The supporting research is from general and student samples and is about planning specificity, not avoidance motives.
- **Whether guided questions beat a blank step list** is untested. This is the central bet of the feature and should be checked in the prototype.

**Sources I could not open**

- *Mastering Your Adult ADHD* workbook, chapter body. I have only the publisher's chapter abstract; the workbook's actual step-by-step breakdown instructions and worksheets are not verified.
- Solanto's 2011 Guilford manual. The maxims beyond the one quoted in the 2010 paper come from a third-party summary.
- Kruger & Evans 2004 full text or abstract. Title, authors and pages confirmed against the Crossref record for the DOI, but the finding is reported here via a later thesis's description (Moher 2012).
- Gollwitzer & Sheeran 2006: citation and DOI confirmed on the authors' institutional page; the d = .65 figure was seen only in secondary summaries.
- Knouse et al. 2017 and Gollwitzer 1999: abstract text obtained through a search index, not from the publisher page (PubMed blocked direct retrieval).
- Levine et al. 2000 (original GMT) and In de Braek et al. (small adult-ADHD GMT study): repository records located, content not read.
- Amazing Marvin's Procrastination Wizard: I found the marketing description but no help article and no question text. The most relevant precedent for a guided manual flow is therefore known only at headline level. Worth opening the app to transcribe it.
- Amazing Marvin "All about subtasks": the help URL returned 404 to direct fetch; quotes are from the search index's copy of the page.
- Tiimo "Suggest Breakdown": confirmed to exist in Tiimo's own article; its FAQ page did not describe mechanics, and I could not confirm what happens offline or without AI.
- Llama Life: first-party page returned no content. Third-party listings describe an AI "break it down" feature and per-task timers; excluded from the table as unverified.

**Leads noted but not verified — do not rely on these**

- Kirschenbaum, Humphrey & Malett (1981), reportedly finding that monthly plans beat daily plans for students' study habits. Directly relevant to over-planning; seen only as retold in popular secondary sources. No primary located.
- Masicampo & Baumeister (2011), "Consider it done!", reportedly showing that making a specific plan stops an unfinished goal from intruding on other tasks. Relevant in both directions (planning relieves mental load, and may also relieve the pressure to act). Seen only via a secondary write-up ([Psychology Today](https://www.psychologytoday.com/us/blog/tech-support/201310/why-your-to-do-list-drives-you-crazy)).
- Townsend & Liu (2012) on planning backfiring for people who feel far from their goal ([RePEc record](https://ideas.repec.org/a/oup/jconrs/doi10.1086-665053.html)); PDF located but unreadable.
- Blunt & Pychyl (2000) on task aversiveness and procrastination across project stages; and Niermann & Scheres (2014) reporting that inattention, not impulsivity, tracked general procrastination in 54 students ([PDF](https://www.epanlab.nl/wp-content/uploads/2017/05/Niermann-Scheres-2014.pdf)). Both would support the "I just don't want to" branch of Sequence C; neither was read.
- Ramsay (2016), "Turning intentions into actions: CBT for adult ADHD focused on implementation" (Clinical Case Studies, doi:10.1177/1534650115611483), a case study built around procrastination. Likely to contain concrete clinician prompts; publisher page returned 403.
