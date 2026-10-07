# Evidence on training time estimation in ADHD

Research ticket: `.scratch/project-cherry/issues/05-time-estimation-training-evidence.md`
Date: 2026-10-07

Evidence-strength labels used below:

- **Strong** — meta-analysis or several independent replications.
- **Moderate** — a few controlled studies, or one good RCT.
- **Weak** — single small study, uncontrolled design, or conflicting results.
- **None found** — I searched and located no study that tests it.

How sources were checked: abstracts were pulled from PubMed, Crossref, Semantic Scholar/OpenAlex, or the open-access full text. Where I only confirmed the citation and not the abstract, the entry says so.

## Answer

1. **Time perception differences in ADHD are real and well replicated — in the lab, at the scale of milliseconds to about a minute.** Evidence at the scale Cherry works on (minutes to hours, real tasks) is thin: a handful of small studies.
2. **There is no direct evidence that the Cherry loop (estimate, do, guess felt time, see actual) trains time estimation in people with ADHD.** I found no controlled study testing it, in an app or otherwise. The closest ADHD trial is in children, used a multi-part intervention with coaching and assistive devices, and produced small gains on a structured test. One small adult study of repeated time-estimation practice found estimation accuracy got *worse*.
3. **In the general population, feedback on actual duration helps mainly when it is put in front of the person at the moment they make the next estimate, for a similar task.** Feedback delivered at completion and then left to memory has a mixed-to-poor record. People remember past durations as shorter than they were, and explain away past misses.
4. So the honest product framing is: **Cherry can reliably *compensate* (show real numbers when estimating); whether it *trains* an internal sense of time is unproven.** The design should not promise training, and the "very thin" similar-task hint is the best-supported part of the whole loop, not a side feature.
5. Felt duration is a reasonable thing to record and has one supporting study as a learning aid, but no ADHD evidence. Buckets versus precise numbers has not been tested for learning. Framing and reason attribution have no time-estimation-specific ADHD evidence; neighbouring evidence says keep feedback about the task rather than the person, and be wary of free-text "why was I off".

**Biggest caveat:** almost everything usable here is extrapolated — from lab timing at second scale to real tasks at hour scale, from students and software engineers to adults with ADHD, and from children's clinic programmes to a self-directed app.

## Findings

### 1. Is time perception impaired in ADHD?

**Lab-scale deficit: Strong.**

- Marx, Cortese, Koelch & Hacker (2022) meta-analysed 55 studies of timing "in the range of milliseconds to several seconds". People with ADHD were worse at discriminating brief durations, more variable when estimating intervals of several seconds, and showed estimation and production errors the authors read as an accelerated internal clock. Reproduction deficits were tied to attention and delay aversion. *Meta-analysis: Altered Perceptual Timing Abilities in Attention-Deficit/Hyperactivity Disorder.* J Am Acad Child Adolesc Psychiatry 61(7):866-880. https://doi.org/10.1016/j.jaac.2021.12.004
- Zheng, Wang, Chiu & Shum (2022) meta-analysed 27 studies in children and adolescents (1,620 ADHD, 1,249 controls): less accurate (Hedges' g > 0.40), less precise (g = 0.66), with a tendency to overestimate. Task type and stimulus modality did not moderate the effect; age and gender did. *Time Perception Deficits in Children and Adolescents with ADHD: A Meta-analysis.* J Atten Disord 26(2):267-281. https://doi.org/10.1177/1087054720978557
- Nejati & Yazdani (2020), 12 child studies, results described by the authors as inconsistent, with the problem clearest in prospective tasks at longer intervals. *Time perception in children with ADHD: Does task matter? A meta-analysis study.* Child Neuropsychol 26(7):900-916. https://doi.org/10.1080/09297049.2020.1712347

**Adults: Moderate, and mixed.**

- Barkley, Murphy & Bush (2001): 104 adults with ADHD vs 64 controls. Larger verbal estimates in ADHD disappeared after controlling for IQ; shorter and less accurate reproductions (12-60 s) survived controls for IQ and comorbidity. *Time perception and reproduction in young adults with ADHD.* Neuropsychology 15(3):351-360. https://doi.org/10.1037/0894-4105.15.3.351
- Mette (2023) reviewed adult studies from 2012-2022 and found only nine, with mixed results for both estimation and reproduction, small samples, heterogeneous methods and no validated instrument. *Time Perception in Adult ADHD: Findings from a Decade — A Review.* Int J Environ Res Public Health 20(4):3098. https://doi.org/10.3390/ijerph20043098

**Real tasks at minute scale: Weak.**

- Prevatt, Proctor, Baker, Garrett & Yelland (2011): 20 college students with ADHD vs 20 without, on a complex academic-style task. Groups differed on prospective estimate, retrospective estimate, and both estimate-vs-actual differences, but not on confidence. This is the closest published analogue to Cherry's three numbers, and it is one study with 40 people. The abstract does not state the direction of the errors. *Time estimation abilities of college students with ADHD.* J Atten Disord 15(7):531-538. https://doi.org/10.1177/1087054710370673
- Hurks & Hendriksen (2011): children with ADHD overestimated on both prospective and retrospective verbal estimation (3-90 s) and under-reproduced longer intervals. Different symptom dimensions predicted different errors. *Retrospective and prospective time deficits in childhood ADHD.* Child Neuropsychol 17(1):34-50. https://doi.org/10.1080/09297049.2010.514403

**Direction of error is not uniform.** Across these sources the pattern is "less accurate and more variable", with direction depending on task type and interval length. The popular claim that people with ADHD simply underestimate everything is not what the meta-analyses show.

**Motivation moves the numbers.** McInerney & Kerns (2003): children with ADHD reproduced time intervals better when the task included positive (sham) feedback and a reward, though still worse than controls. Controls did not change. Part of the measured "deficit" is engagement, not clock. *Time reproduction in children with ADHD: motivation matters.* Child Neuropsychol 9(2):91-108. https://doi.org/10.1076/chin.9.2.91.14506

**Medication.** Two narrative reviews from one research group report that time perception tends to normalise under stimulant medication. These are narrative, not systematic. Ptacek et al. (2019), *Clinical Implications of the Perception of Time in ADHD: A Review.* Med Sci Monit 25:3918-3924. https://doi.org/10.12659/MSM.914225; Weissenberger et al. (2021), *Time Perception is a Focal Symptom of ADHD in Adults.* Med Sci Monit 27:e933766. https://doi.org/10.12659/MSM.933766

### 2. Can time estimation be trained in ADHD?

**Evidence strength: Weak. No study of the Cherry mechanism in adults with ADHD was found.**

- **Closest positive result — children, multi-component.** Wennberg, Janeslätt, Kjellberg & Gustafsson (2018) randomised 38 medicated children aged 9-15 to a 12-week programme or education only, assessed at 24 weeks. The programme combined time-skill training with prescribed time-assistive devices. Per the full text, the training included children measuring, in minutes, how long self-chosen recurring activities took — a close cousin of Cherry's timer. Time-processing ability on a structured test improved more in the intervention group (p = 0.019; reported effect size about d = 0.4, small), mostly in *orientation to time* (clock and calendar skills), not duration sense. Parents rated daily time management as much improved (p = 0.01); **the children themselves did not**, and about half rated themselves the same or worse. Not blinded. Training and devices cannot be separated. *Effectiveness of time-related interventions in children with ADHD aged 9-15 years: a randomized controlled study.* Eur Child Adolesc Psychiatry 27(3):329-342. https://doi.org/10.1007/s00787-017-1052-5 (full text: https://pmc.ncbi.nlm.nih.gov/articles/PMC5852175/)
- **A negative result in adults.** Fontes et al. (2020): 22 adults with ADHD, crossover design, thirty days of exposure to a visual time-estimation task. Time-estimation performance was **worse** after exposure (absolute and relative error, p ≤ 0.05), while self-reported attention, impulsivity and emotional control improved. Small sample, lab task at short intervals, and the abstract does not say whether the practice included accuracy feedback. *Time estimation exposure modifies cognitive aspects and cortical activity of ADHD adults.* Int J Neurosci 130(10):999-1014. https://doi.org/10.1080/00207454.2020.1715394
- **Motor timing is a different skill.** Shaffer et al. (2001): 56 boys, Interactive Metronome training improved attention and motor measures versus controls. It trains rhythmic synchronisation at sub-second scale and did not measure task-duration estimation. *Effect of interactive metronome training on children with ADHD.* Am J Occup Ther 55(2):155-162. https://doi.org/10.5014/ajot.55.2.155
- **Adult time-management programmes improve time management, not demonstrably time sense.**
  - Solanto et al. (2010): RCT, 88 adults, 12-week group meta-cognitive therapy targeting time management, organisation and planning beat supportive therapy on blind-rated inattention (odds ratio for response 5.41, 95% CI 1.77-16.55). Time estimation accuracy was not an outcome. *Efficacy of meta-cognitive therapy for adult ADHD.* Am J Psychiatry 167(8):958-968. https://doi.org/10.1176/appi.ajp.2009.09081123
  - Holmefur et al. (2019): "Let's Get Organized", 55 adults with neurodevelopmental or mental disorders, one group, no control. Self-rated time management improved and held at 3 months. *Pilot Study of Let's Get Organized.* Am J Occup Ther 73(5):7305205020. https://doi.org/10.5014/ajot.2019.032631. One-year follow-up, also uncontrolled: Wingren et al. (2022), Scand J Occup Ther 29(4):305-314. https://doi.org/10.1080/11038128.2021.1954687
  - Antshel, McBride & Knouse (2026): RCT, 154 adults, 8 weeks of a CBT-informed app versus waitlist. Self-reported inattention improved; functional impairment did not; the authors themselves limit confidence because of the waitlist control, and one author declares a financial interest in the app company. Shows an app can shift self-reported symptoms; says nothing about time estimation. *Bridging the Gap: Digital CBT for Adults Managing ADHD Challenges.* J Atten Disord 30(3):370-385. https://doi.org/10.1177/10870547251384462
- **Guidelines.** NICE NG87 recommends, for adults where non-drug treatment is indicated, a structured supportive psychological intervention focused on ADHD that may include elements of CBT. I could not open the guideline page directly (see Unknowns) and found no indication that it addresses time-perception training. https://www.nice.org.uk/guidance/ng87

### 3. Does a prospective estimate plus immediate feedback on actual duration improve calibration?

**Evidence strength: Moderate in the general population, and conflicting. None found in ADHD.**

What the task-duration prediction literature shows:

- **People do not spontaneously use their own history.** Buehler, Griffin & Ross (1994): across five studies, predictions were too optimistic; think-aloud data showed people focus on the plan for this task, not past experience. The bias was eliminated only for participants *instructed to connect* relevant past experiences to the prediction. *Exploring the "planning fallacy": Why people underestimate their task completion times.* J Pers Soc Psychol 67(3):366-381. https://doi.org/10.1037/0022-3514.67.3.366
- **Memory of past durations is itself biased short.** Roy, Christenfeld & McKenzie (2005) review evidence that people underestimate future durations because they remember past ones as shorter than they were. *Underestimating the duration of future events: memory incorrectly used or memory bias?* Psychol Bull 131(5):738-756. https://doi.org/10.1037/0033-2909.131.5.738
- **Supplying the real number just before predicting helps.** Roy, Mitten & Christenfeld (2008): in three experiments, giving people measured duration from a previous similar task (or the average for others) before they predicted increased accuracy. *Correcting memory improves accuracy of predicted task duration.* J Exp Psychol Appl 14(3):266-275. https://doi.org/10.1037/1076-898X.14.3.266
- **But this did not replicate cleanly.** Thomas & König (2018), three experiments (N = 40, 80, 80): duration feedback on a prior task did not reduce bias; what reduced bias was the prior task being the *same* task. The authors state their result contrasts with Roy et al. (2008). *Knowledge of Previous Tasks: Task Similarity Influences Bias in Task Duration Predictions.* Front Psychol 9:760. https://doi.org/10.3389/fpsyg.2018.00760
- **Past durations act as anchors, for better or worse.** Thomas, Handley & Newstead (2007): predictions were pulled toward the duration of the preceding task, giving overestimates after a longer task and underestimates after a shorter one. Using history "does not necessarily improve accuracy". *The role of prior task experience in temporal misestimation.* Q J Exp Psychol 60(2):230-240. https://doi.org/10.1080/17470210600785091
- **Underestimation is not universal.** Halkjelsvik & Jørgensen (2012), the main integrative review, found underestimation more common than overestimation in engineering studies but not in psychology lab studies, and warn that "small tasks overestimated, large tasks underestimated" can partly be a statistical artifact of random error. *From origami to software development: a review of studies on judgment-based predictions of performance time.* Psychol Bull 138(2):238-271. https://doi.org/10.1037/a0025996

Reading these together: feedback on actual duration is not a reliable teacher on its own. It works when (a) the real number is physically present at the next estimate, and (b) the next task resembles the one the number came from. That matches Cherry's category-plus-bucket similarity key better than it matches the completion-time retro.

None of these studies ran for more than a session or two, and none involved ADHD participants. Whether months of daily feedback produce durable calibration is untested.

### 4. Is retrospective "felt duration" versus clock time a useful signal?

**Evidence strength: Weak for usefulness as a training tool; Strong that it measures something different from a prediction.**

- **It is a distinct judgment.** Block & Zakay (1997), meta-analysis of 20 experiments: judgments made when you know you will be asked ("prospective paradigm") are longer and less variable than those made when you do not ("retrospective paradigm"); the first depends on attention, the second on memory. *Prospective and retrospective duration judgments: A meta-analytic review.* Psychon Bull Rev 4(2):184-197. https://doi.org/10.3758/BF03209393
- **Task demand bends it in opposite directions.** Block, Hancock & Zakay (2010), 117 experiments: with higher cognitive load, the subjective-to-objective ratio falls when people know they will be asked and rises when they do not. *How cognitive load affects duration judgments: A meta-analytic review.* Acta Psychol 134(3):330-343. https://doi.org/10.1016/j.actpsy.2010.03.006
- **One study supports the exact mechanism.** König, Wirz, Thomas & Weidmann (2015): participants who gave a retrospective estimate of a first task before predicting a second, unrelated task underestimated the second less than controls. The authors suggest people could be trained to observe their own misestimation. Single lab study; I confirmed the citation and a summary of the abstract, not the sample size or whether participants were shown the true duration. *The Effects of Previous Misestimation of Task Duration on Estimating Future Task Duration.* Curr Psychol 34:1-13. https://doi.org/10.1007/s12144-014-9236-3
- **ADHD.** Retrospective estimates differed between ADHD and non-ADHD students in Prevatt et al. (2011) and children in Hurks & Hendriksen (2011), both cited above. So the signal is plausibly informative in this population. No study tests whether showing someone their felt-versus-actual gap changes anything.

A design consequence that follows from Block & Zakay: because Cherry's user always knows the felt guess is coming, what Cherry collects is technically the "prospective paradigm" judgment (attention-driven), not a memory-driven retrospective one. That is still a legitimate measure, and arguably the right one for "how did that feel", but it should not be assumed to behave like the retrospective estimates in the literature.

### 5. Do coarse buckets versus precise numbers matter?

**Evidence strength: None found for learning or calibration. Weak, indirect evidence that the response format changes the estimate itself.**

- Halkjelsvik & Jørgensen (2012, cited above) list request format, level of abstraction, anchoring and task decomposition among the things that shift time predictions.
- Jørgensen (2016) reports that estimating in a coarser unit yields higher estimates — work-hours give lower effort estimates than workdays — and suggests coarser units may be more realistic where underestimation is the norm. Software professionals, effort in hours to weeks; citation confirmed, abstract not re-read. *Unit effects in software project effort estimation: Work-hours gives lower effort estimates than workdays.* J Syst Softw 117:274-281. https://doi.org/10.1016/j.jss.2016.03.048
- Decomposition raises totals. Forsyth & Burt (2008): across three experiments, time allocated to a whole task was significantly smaller than the sum allocated to its subtasks. *Allocating time to future tasks: the effect of task segmentation on planning fallacy bias.* Mem Cognit 36(4):791-798. https://doi.org/10.3758/mc.36.4.791. Kruger & Evans (2004) report the same direction for "unpacking" a task into components (citation confirmed, abstract not re-read). *If you don't want to be late, enumerate: Unpacking reduces the planning fallacy.* J Exp Soc Psychol 40(5):586-598. https://doi.org/10.1016/j.jesp.2003.11.001

I found no study comparing category estimates with numeric estimates for the purpose of improving calibration, in any population.

### 6. What feedback framing avoids shame and avoidance?

**Evidence strength: None found specific to time-estimation feedback. Moderate, indirect evidence on feedback in general and on self-criticism in ADHD.**

- **Feedback often backfires, and more so when it points at the self.** Kluger & DeNisi (1996), meta-analysis of 607 effect sizes: feedback improved performance on average (d = 0.41), but over a third of feedback interventions *decreased* it, and effectiveness fell as attention moved from the task toward the self. General population, mostly work and education tasks. *The effects of feedback interventions on performance.* Psychol Bull 119(2):254-284. https://doi.org/10.1037/0033-2909.119.2.254
- **Adults with ADHD carry lower self-compassion and a history of criticism.** Beaton, Sirois & Milne (2022a): 543 adults with ADHD vs 313 without; low self-compassion statistically accounted for part of the poorer mental health in the ADHD group. Cross-sectional, self-report. *The role of self-compassion in the mental health of adults with ADHD.* J Clin Psychol 78(12):2497-2512. https://doi.org/10.1002/jclp.23354. Beaton, Sirois & Milne (2022b): qualitative, 162 participants; inattention-related behaviour was the most criticised, criticism damaged self-worth, and avoidance was one coping response. *Experiences of criticism in adults with ADHD: A qualitative study.* PLoS One 17(2):e0263366. https://doi.org/10.1371/journal.pone.0263366
- **More awareness can feel worse.** In Wennberg et al. (2018, cited above) the children's own ratings did not improve despite measured and parent-rated gains; the authors suggest heightened awareness of their difficulties. This is the authors' interpretation, not a tested finding, and it is in children.
- **Positive, engaging feedback improved measured timing** in children with ADHD (McInerney & Kerns 2003, cited above).

No study compares framings (neutral number, positive reframe, comparison to own average, and so on) for duration feedback in ADHD.

### 7. Reason attribution ("why was I off")

**Evidence strength: Weak, and what exists is discouraging. None found in ADHD.**

- **Attribution is how people avoid learning.** Buehler, Griffin & Ross (1994, cited above): participants attributed their past prediction failures to "relatively external, transient, and specific factors", which let them treat each miss as a one-off and keep predicting optimistically.
- **Structured reflection did not improve estimates.** Jørgensen & Gruschke (2009): 20 software professionals randomised; the learning group spent at least 30 minutes after each of five tasks analysing their estimation experience. Their accuracy and uncertainty realism were no better than controls'. In a follow-up with 83 professionals, feedback about *other people's* estimation performance produced more realistic uncertainty assessments than the same feedback about one's own. The authors warn that such sessions can "stimulate rather than reduce learning biases". *The Impact of Lessons-Learned Sessions on Effort Estimation and Uncertainty Assessments.* IEEE Trans Softw Eng 35(3):368-383. https://doi.org/10.1109/TSE.2009.2

I found no study showing that asking people why an estimate was off improves later estimates.

## Design implications

**Everything in this section is my inference from the findings above, not something a study has shown for Cherry.**

1. **Position the product as "see your real numbers", not "train your time sense".** Compensation is supported; training is unproven. This also protects a later public release from making a claim the evidence cannot carry.
2. **Promote the similar-task hint.** Showing "your Chores / short tasks have averaged 22 min" at the moment of estimating is the mechanism with the most support (Roy 2008; Buehler 1994 Study 4), and Thomas & König 2018 suggests it works to the extent the tasks really are alike. The map currently labels this "very thin". The evidence argues for making it dependable and visible at capture, even if the matching stays simple.
3. **Be careful what the hint anchors on.** A single previous duration can drag the next estimate the wrong way (Thomas et al. 2007). Prefer an aggregate over several similar tasks, show how many tasks it is based on, and consider showing spread rather than one number.
4. **Do not rely on the completion-time retro to do the teaching.** Keep it light and skippable as planned. Its main job is producing clean data for the hint and the insights screen. Evidence that a post-task reveal changes later behaviour by itself is weak.
5. **Keep the felt-duration guess, as a measurement and a low-cost bet.** König et al. 2015 gives it one supporting study. It is also the only one of the three numbers that speaks to the "time blindness" experience directly. Treat any insight built on it as exploratory.
6. **Buckets are defensible for capture friction; they limit what can be measured.** With a bucket estimate, "error" is only whether the actual fell inside the bucket, or how far outside. The insights screen should report in those terms rather than implying minute-level calibration. Coarser units may also nudge estimates upward (Jørgensen 2016), which would help if the user's bias is underestimation, but that is an untested transfer. Bucket boundaries will act as anchors, so their numeric ranges deserve care.
7. **Step breakdown probably helps estimates as a side effect.** Summed step estimates tend to exceed whole-task estimates (Forsyth & Burt 2008; Kruger & Evans 2004). Where a task has steps, comparing the task's bucket with the sum of its steps is a cheap, evidence-aligned check.
8. **Frame feedback about the task and the category, never the person.** "Chores usually run longer than Short" keeps attention on the task; a personal accuracy score, grade or streak moves it to the self, which is where Kluger & DeNisi found feedback turning harmful and where the ADHD self-criticism findings suggest extra risk. The standing "no streaks, pull-only insights" constraints already point the right way.
9. **Do not assume the user underestimates.** The ADHD timing literature shows inaccuracy and variability in both directions. Copy, colours and insights should treat "ran shorter than expected" as equally noteworthy, and the insights screen should be able to show variability, not only average bias.
10. **If reason attribution is included at all, make it optional structured tags, and use them in aggregate.** Free-text reasons are likely to reproduce the "one-off, not my usual" explanation that blocks learning. A small fixed set of tags, summarised over time ("interruptions tagged on 9 of the last 20 overruns"), turns supposed one-offs into a visible base rate. This is the most speculative item here; nothing tests it. Leaving attribution out of v1 costs little according to the evidence.
11. **Expect self-rated progress to lag or even dip.** If the author feels worse about time after a few weeks of seeing real numbers, that is consistent with the Wennberg finding and is not by itself a sign the app is failing.
12. **Build the data so the question can be answered for one person.** Since no trial exists, Cherry's own log is the only evidence the author will get. Storing estimate bucket, felt guess, actual, timestamp, category and data-quality flag (timer vs self-report) per item makes a later "did my calibration change over six months" check possible.

## Unknowns / could not verify

- **No controlled study of estimate-then-feedback training for task durations in ADHD, adult or child, app or paper.** Searches of PubMed and the web returned none. Absence from my searches is not proof none exists, but none of the reviews I read cite one either.
- **No long-run study in any population.** The feedback experiments last one or two sessions. Whether calibration gains persist, grow or fade over months is unknown.
- **Transfer from lab timing to real tasks is unestablished.** The meta-analyses cover milliseconds to seconds. Whether a person's error at 30 seconds predicts their error on a 45-minute task was not addressed by anything I located.
- **ADHD and the planning fallacy specifically.** A PubMed search for the two terms together returned nothing. Claims that people with ADHD show a stronger planning fallacy appear in popular sources; I could not trace them to a study.
- **Buckets versus numbers for calibration learning:** no study found.
- **Framing of duration feedback in ADHD:** no study found. The shame and avoidance concern rests on general feedback research and on cross-sectional and qualitative ADHD work.
- **Rejection sensitivity.** I did not find peer-reviewed quantitative work I could verify on rejection sensitivity and response to app feedback, so it is not cited.
- **NICE NG87:** the guideline page returned an access error, so the recommendation wording above comes from secondary descriptions of the guideline rather than my own reading. Verify before quoting it in a spec.
- **Abstracts not read directly:** Kruger & Evans (2004) and Jørgensen (2016) — citations confirmed through Crossref, findings stated as given in titles and publisher descriptions. König et al. (2015) — repository summary only.
- **Details taken from an automated summary of full text, not read line by line:** the intervention content and effect sizes for Wennberg et al. (2018), and study counts in Mette (2023). The Wennberg abstract confirms the p-values and the null child self-rating.
- **Prevatt et al. (2011):** direction and size of the ADHD group's errors are not in the abstract; full text not obtained.
- **Fontes et al. (2020):** the abstract is internally inconsistent about the direction of the EEG change, and does not say whether participants received accuracy feedback. Only the behavioural result is used here.
- **Medication effects:** supported here only by narrative reviews. I did not locate a systematic review or meta-analysis of stimulant effects on time estimation.
- **Not searched in depth:** time-perception training in other populations (ageing, brain injury), general temporal perceptual-learning literature, and non-English sources.
