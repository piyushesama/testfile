# HPB AI Program: Development Plan

Source: Plaud recording 2026-11-03, "Surgical AI Research Strategy" (first ~6 min), discussion between Dimitrios (Speaker 1) and Piyush (Speaker 2).
Prepared: 2026-11-03. Literature checked on PubMed the same day; every citation below has a DOI and was retrieved, not recalled.

---

## 0. What was actually proposed (transcript, not the Plaud summary)

Two projects, one grant target:

| # | Proposal as stated | Data named | Ground truth named |
|---|---|---|---|
| P1 | Train an imaging model on Georgetown pre/post-chemo CT/MRI of locally advanced pancreatic cancer to predict resectability and **recommend the type of secondary arterial reconstruction**. Package as a downloadable "drop your DICOMs here" tool, cross-validate at MDC, grow it by outreach cases. | Georgetown longitudinal CT/MRI; radiology (Kevin Zand, Rita) | MDC decision; what reconstruction was actually done |
| P2 | Replicate the Athens HPB paper: feed MDC cases to ChatGPT/Claude, measure concordance with MDC. Start with colorectal liver metastases (CRLM, "800–900 cases 2013–now", include transplant and HAI pump options), then pancreas, then HCC. | NCDB center file via Saba (Lombardi NCDB coordinator) → MRNs → MDC records | MDC recommendation; what was actually done |
| Grant | Georgetown/Howard/GW research scholar award (~$30k, Dimitrios called it the one "Laytham/David applied" for), then "AHPBA-level" grants. | | |

Dimitrios's stated priority: P1 ("that's the project you should do, because you are the AI person"). P2 was framed as "a quick project".

Correction to the transcript: the Athens paper did **not** use ChatGPT and the number is not 90%. Katsaros et al., HPB 2026 (1st Dept of Surgery, NKUA/Laikon) used **Gemini 3.1 Pro**, text-only summaries, all HPB MDT cases July–Dec 2025. Overall agreement 91.7% (κ 0.84), but **62.5% (κ 0.30, p = 0.09) in complex cases** [DOI](https://doi.org/10.1016/j.hpb.2026.08.006). The complex-case number is the one that matters for a surgical MDC and it is the one nobody quotes.

---

## 1. Hidden variables

1. **Concordance is not validation.** Every published LLM-vs-MDT study (table below) uses the MDT as reference standard. Agreement with the MDT tells you the model has learned guideline text; it says nothing about whether either party was right. This is exactly the flaw in the blood-signature literature Piyush already dissected ("Any Gene Set Will Do"): high agreement with a reference, no anchoring to outcome. The field is now crowded with n = 30–100 concordance papers; a Georgetown n = 100 replication is a case report in a crowded room.
2. **The failure mode is systematic, not random.** Mergen et al. 2026: LLaMA 3.3 70B hit 78.6% accuracy on pancreatic tumor-board decisions and identified **0 of 7** neoadjuvant candidates [DOI](https://doi.org/10.1186/s12911-026-03444-x). Gehrisch et al. 2026 (prospective, 45 pts, GPT-4): 73.3% overall, 50% in cases needing further workup [DOI](https://doi.org/10.1007/s00423-026-04242-9). A single concordance percentage hides the fact that the model fails precisely on borderline-resectable, transplant-candidate and HAI-pump decisions, which are the cases the Georgetown program exists for.
3. **P1 has heavyweight prior art.** PAN-VIQ (Zhang et al., npj Digit Med 2025): 3D deep-learning segmentation of tumor + CA/CHA/SMA/SMV/PV with encasement angles, trained on **2,130** cases, prospectively tested on 202, external accuracy > 90%, matched senior radiologists [DOI](https://doi.org/10.1038/s41746-025-02260-3). Also: Yimamu 2023 resectability model, 349 pts, 4 centers [DOI](https://doi.org/10.1111/jgh.16401); Miao 2024 T4-stage prediction [DOI](https://doi.org/10.1016/j.compbiomed.2024.108125); Xue et al. Gut 2025, metastasis prediction from primary-tumor CT, multinational [DOI](https://doi.org/10.1136/gutjnl-2024-334237). "Resectability from CT" is largely a solved engineering problem by groups with 10× the data.
4. **The arterial-reconstruction label is too rare to learn.** A recommendation model needs enough examples per output class. Arterial resections in PDAC at a US academic center run on the order of a handful per year (approximation; Georgetown's actual count must be pulled). Across 2013–2025 that is plausibly < 100 events split across several reconstruction types. No imaging model learns a multi-class surgical recommendation from < 100 positives. The honest version of P1 is a measurement study, not a recommender.
5. **No published search hit** for an AI model that recommends arterial/venous reconstruction type in pancreatectomy (PubMed, 2019–2026). That gap is real, but it is a gap because the labels are rare, not because nobody thought of it.
6. **PHI and the LLM.** Consumer ChatGPT/Claude cannot receive identifiable clinical summaries. Either a BAA-covered enterprise endpoint (MedStar/Georgetown Azure OpenAI, AWS Bedrock, Anthropic enterprise) or a local open-weight model on secured hardware. This decides the model list and must be settled before any case is abstracted.
7. **MDC decision capture quality is the real bottleneck for P2.** 800–900 CRLM cases exist in NCDB; the number with a retrievable, dated, structured MDC recommendation is unknown and probably far lower before ~2018–2019. Ask before promising n.
8. **Model drift.** ChatGPT today ≠ ChatGPT at submission. Palzer et al. 2026 (HCC/CCA, 50+50): ChatGPT 66–80% vs **Claude 38–56%** on identical inputs [DOI](https://doi.org/10.3390/cancers18132175). Model and version are independent variables; the design must pin them and include ≥ 3 models.
9. **Abstraction labor.** 800 vignettes × ~20 min ≈ 270 h (approximation). A fellow with clinical load does not have that. This is what the $30k is for.
10. **Second-order effect of "downloadable DICOM tool".** A tool that outputs surgical recommendations from outside images is software-as-a-medical-device territory and an institutional liability question. Keep it as the five-year vision, not a deliverable.

## 2. Assumptions

Mine: Speaker 1 is Dimitrios (transcript is unlabelled; the Greek residency reference and the Athens paper make this near-certain). Piyush has, or can get, IRB-approved retrospective access to Lombardi MDC records and imaging. The research scholar award is the GHUCCTS (Georgetown-Howard Universities Center for Clinical and Translational Science) pilot mechanism; **verify the mechanism, cycle date and eligibility, I cannot confirm from here**. AHPBA has a research grant program; **verify current amounts and deadlines**.

Dimitrios's (unstated): that CT is the decision-driving input (true pre-chemo, much weaker post-chemo, where fibrosis mimics tumor); that MDC decisions are a clean label; that a 90% concordance headline is a positive result.

## 3. Constraints

- Time: fellow with clinical and interview load; realistic effort ~4–6 h/week plus a funded coordinator.
- Data: single center; CRLM n large, PDAC arterial-reconstruction n small; MDC minutes of variable structure.
- Compute: imaging model training needs GPU; LLM benchmarking needs only API budget.
- Governance: IRB, BAA-covered LLM access, data-use for NCDB-derived list.
- Grant: $30k buys coordinator time + API/compute + stats, not an imaging-AI engineer.

## 4. Solution paths

**Path A. Do P1 as proposed (imaging recommender for arterial reconstruction).**
Pros: the project Dimitrios wants; radiology collaborators available. Cons: outcompeted by PAN-VIQ on the resectability half; underpowered by two orders of magnitude on the reconstruction half; needs an imaging-AI engineer and GPU budget the grant does not cover; 18–24 months to a first result. Expected outcome: a segmentation feasibility paper that reads as a small-n PAN-VIQ.

**Path B. Do P2 as proposed (concordance replication, CRLM first).**
Pros: 3–6 months, cheap, the pipeline reuses for pancreas and HCC. Cons: the 2026 literature already has CRLM (Yılmaz, n = 30 [DOI](https://doi.org/10.1038/s41598-026-38449-z)), HCC (Ciulli, n = 100 [DOI](https://doi.org/10.1245/s10434-026-20253-8); Palzer n = 100), pancreas (Gehrisch prospective; Mergen), breast (Dindelegan, GPT-5.5, 90.8% [DOI](https://doi.org/10.3390/jcm15197533)). Another concordance number is a mid-tier journal at best and is scientifically empty for the reason in Hidden Variable 1.

**Path C (recommended). Outcome-anchored LLM-vs-MDC benchmark in CRLM at scale, with a prospective silent-mode arm, and P1 recast as a measurement aim on the same grant.**
Keep Dimitrios's instinct (the MDC comparison, the longitudinal imaging), fix the design flaw, and make size and the transplant/HAI options the differentiators.

## 5. Verdict: Path C

Why: it is the only path that (a) produces a first-author paper inside a fellowship year, (b) says something no concordance study has said, namely *who was right when they disagreed*, (c) uses the one asset Georgetown actually has that Athens, Mersin, Monza and Hamburg do not: ~10× the CRLM volume and a transplant-plus-HAI-pump decision space, and (d) generates exactly the preliminary data an AHPBA-scale application needs. P1 as a recommender is a bet on data that does not exist; P2 as a replication is a bet on a result that is already published.

### 5.1 Primary study: CRLM, retrospective, outcome-anchored

- **Cohort.** All CRLM cases 2013–2025 from the Lombardi NCDB file (Saba) linked to a dated MDC recommendation. Expect the usable set to be 2018/2019 onward; report the attrition honestly.
- **Input.** A structured vignette per case containing only information available on the MDC date: demographics, primary tumor and RAS/BRAF/MSI, CEA, imaging report text (no images), prior therapy, performance status, liver remnant data if recorded. Blinded to MDC decision and to everything after the MDC date. Abstraction by a funded coordinator; 10% double-abstracted for agreement.
- **Decision taxonomy, fixed a priori** (map every MDC and LLM output to one): upfront resection; neoadjuvant chemo then reassess; conversion chemo with HAI pump; HAI pump adjuvant; ablation ± resection; liver transplant evaluation; systemic therapy only; best supportive care; additional workup. Two blinded raters map free text; κ reported.
- **Models.** ≥ 3 frozen, version-pinned models accessed under BAA or run locally (candidates: GPT-5.x enterprise, Claude enterprise, Gemini, one open-weight such as Llama/Qwen 70B). Identical prompt, 3 independent runs per case per model, no web access, no retrieval. Pre-register the prompt and the taxonomy (OSF) before the first case is run.
- **Primary endpoint.** Concordance (% agreement, Cohen's κ) with MDC, overall and stratified by complexity tier (guideline-clear vs borderline vs transplant/HAI decisions).
- **The endpoints that make it a real paper.**
  1. *Discordance direction*: LLM more conservative vs more aggressive than MDC, by tier (Ciulli and Yılmaz both found LLMs systematically conservative).
  2. *Outcome-anchored discordance*: OS and recurrence-free survival in MDC-concordant vs MDC-discordant cases, adjusted for stage and tier. Also: among discordant cases, did the MDC plan get executed and achieve R0/conversion? This is observational and cannot give counterfactuals; say so. It is still the first time anyone will have asked the question in HPB.
  3. *Recall on the rare curative pathways* (transplant referral, HAI pump): the Mergen "0 of 7 neoadjuvant" failure, tested where it matters.
  4. *Stability*: within-model run-to-run agreement; between-model agreement.
- **Sample size.** With κ as the estimand and an expected ~0.6–0.8, a few hundred cases give tight CIs; the outcome analysis is what needs the full 800–900 (approximation, have the biostatistician size it against the actual event rate).

### 5.2 Prospective silent-mode arm (months 5–9)

Each new CRLM/PDAC MDC case is vignetted and run through the pinned models *before* the meeting; outputs are logged, not shown. Measures prospective concordance (no retrospective-abstraction bias), time per case, and gives a live test set. Same design as Gehrisch but larger and with the transplant/HAI space. This arm is the natural preliminary data for a later step-wedge trial where the output *is* shown.

### 5.3 Extension by reuse

Same pipeline, same taxonomy logic, new condition-specific categories: PDAC (NCCN resectability + neoadjuvant + arterial-resection candidacy), then HCC (Milan/UCSF/downstaging, transplant vs resection vs LRT). Each is a separate paper at marginal cost.

### 5.4 P1, recast: "Post-neoadjuvant arterial involvement in borderline/LA PDAC: quantitative CT vs operative truth"

- Not a recommender. A measurement study using existing open segmentation (TotalSegmentator for vessels; request PAN-VIQ weights from Ruijin, availability **unverified**) to compute tumor-vessel encasement angles on *pre- and post-chemo* CT.
- Ground truth: **intraoperative finding and pathology** (arterial involvement, R status, reconstruction performed), not MDC opinion.
- Question: does change in quantitative encasement across neoadjuvant therapy predict R0 and need for arterial reconstruction better than radiologist reads? This is the niche PAN-VIQ did not address (they are single-timepoint, and post-chemo CT is where radiologists are weakest).
- Needs: Georgetown counts of BR/LA PDAC explored after neoadjuvant 2015–2025 and of arterial resections. If arterial resections < ~40, drop the reconstruction-type analysis and keep R0/exploration-futility as the outcome.
- Radiology partners (Zand, Rita) get a clean, bounded ask: validate segmentations and read blinded.
- This is Aim 2 of the $30k application and the imaging preliminary data for the AHPBA application. The downloadable tool is the five-year vision slide, not a deliverable.

## 6. Timeline (fellowship-year realistic)

| Month | Milestone |
|---|---|
| 0–1 | IRB retrospective protocol (CRLM + PDAC + HCC, 2013–2025); BAA-covered LLM access confirmed; Saba pulls CRLM list; Lombardi MDC archive assessed for structured recommendations by year; biostatistics consult; OSF pre-registration drafted |
| 1–3 | Taxonomy and prompt frozen; coordinator abstracts vignettes 2019→; outcome fields linked (OS, RFS, R0, treatment delivered) |
| 3–5 | Benchmark run; rater mapping; primary and outcome analyses; AHPBA/SSO abstract |
| 5–9 | Prospective silent-mode arm live; PDAC vignettes abstracted; P1 segmentation feasibility on 30–50 paired CTs |
| 6 | $30k research-scholar application submitted with Aim 1 (CRLM benchmark, preliminary data in hand) and Aim 2 (P1 measurement study) |
| 9–12 | CRLM manuscript submitted (target: Annals of Surgery / JAMA Surgery tier if outcome analysis holds; HPB / ASO otherwise); PDAC and HCC runs; AHPBA grant draft |

## 7. Budget sketch for the $30k (approximation, verify allowable costs)

Research coordinator abstraction time ~55–60%; enterprise LLM API + secured compute ~15%; biostatistics ~15%; radiology reader time / segmentation QC ~10%. No imaging-AI engineer salary fits; that is the AHPBA-level ask.

## 8. Decisions to put back to Dimitrios

1. Lead with CRLM outcome-anchored benchmark, not the imaging recommender. The recommender is Aim 2, recast as measurement.
2. Confirm the MDC record archive: what years have a dated, written recommendation per case?
3. Pull the two numbers that decide P1: BR/LA PDAC explored after neoadjuvant, and arterial resections performed, 2015–2025.
4. Who holds BAA-covered LLM access at MedStar/Georgetown, and which models does it cover?
5. Authorship and roles now, before data abstraction starts (consistent with the 10-07 meeting on authorship norms).

## 9. Week-one actions (Piyush)

- [ ] Email Saba: CRLM, PDAC, HCC case lists 2013–2025 with MRN, dates, treatment fields.
- [ ] Ask Lombardi tumor-board coordinator how MDC recommendations are stored, by year.
- [ ] Ask Georgetown/MedStar research IT about BAA-covered LLM endpoints.
- [ ] Open IRB retrospective protocol (one umbrella for CRLM/PDAC/HCC).
- [ ] Draft the decision taxonomy and the vignette template; send to Dimitrios for sign-off.
- [ ] Read in full: Katsaros HPB 2026; Yılmaz Sci Rep 2026; Ciulli ASO 2026; Gehrisch Langenbecks 2026; Mergen BMC 2026; Zhang PAN-VIQ npj 2025.

## 10. Prior art retrieved from PubMed (2026-11-03)

LLM vs multidisciplinary team, HPB and adjacent:

| Study | Setting | n | Model | Headline | Where it fails |
|---|---|---|---|---|---|
| Katsaros et al., HPB 2026 [DOI](https://doi.org/10.1016/j.hpb.2026.08.006) | All HPB MDT, Athens, retrospective | 6 months of cases | Gemini 3.1 Pro | 91.7%, κ 0.84 | Complex cases 62.5%, κ 0.30 |
| Yılmaz et al., Sci Rep 2026 [DOI](https://doi.org/10.1038/s41598-026-38449-z) | CRLM, Mersin, retrospective | 30 | ChatGPT | 66.7% baseline; 93.3% when resectability stated | Conservative outputs; needs resectability fed in |
| Ciulli et al., Ann Surg Oncol 2026 [DOI](https://doi.org/10.1245/s10434-026-20253-8) | HCC, Monza, retrospective | 100 | ChatGPT-4 | 81% high concordance, 53% exact technique | Older patients; MDT more aggressive |
| Palzer et al., Cancers 2026 [DOI](https://doi.org/10.3390/cancers18132175) | HCC + CCA, Aachen, retrospective | 50 + 50 | ChatGPT vs Claude | ChatGPT 66–80%; Claude 38–56% | Model-dependent; MTB more individualized |
| Gehrisch et al., Langenbecks Arch Surg 2026 [DOI](https://doi.org/10.1007/s00423-026-04242-9) | PDAC, Hamburg, **prospective** | 45 | GPT-4 ± literature prompting | 73.3%, κ 0.64 | 50% when further workup needed; literature prompting did not help |
| Mergen et al., BMC Med Inform Decis Mak 2026 [DOI](https://doi.org/10.1186/s12911-026-03444-x) | PDAC tumor board, Munich | 42 | LLaMA 3.3 70B | 78.6% accuracy | 0/7 neoadjuvant candidates identified |
| Dindelegan et al., J Clin Med 2026 [DOI](https://doi.org/10.3390/jcm15197533) | Breast, retrospective | 86 (688 decisions) | GPT-5.5 | 90.8% | Individualized surgical decisions 80–84% |
| Meyer et al., J Surg Oncol 2026 [DOI](https://doi.org/10.1002/jso.70314) | Rectal, NCCN vignettes | 135 Qs | ChatGPT-4o | 94.1% correct | Multi-step conditional pathways |

Imaging AI in PDAC relevant to P1:

| Study | What | n | Result |
|---|---|---|---|
| Zhang et al., npj Digit Med 2025, PAN-VIQ [DOI](https://doi.org/10.1038/s41746-025-02260-3) | 3D DL segmentation of tumor + 5 vessels, encasement angles | 2,130 train; 202 prospective | > 90% external accuracy; matched senior radiologists |
| Yimamu et al., J Gastroenterol Hepatol 2023 [DOI](https://doi.org/10.1111/jgh.16401) | CT + NCCN hybrid resectability model | 349, 4 centers | Multi-center resectability prediction |
| Miao et al., Comput Biol Med 2024 [DOI](https://doi.org/10.1016/j.compbiomed.2024.108125) | AI pipeline for T4 stage | 2 centers | Automated T4 prediction |
| Xue et al., Gut 2025 [DOI](https://doi.org/10.1136/gutjnl-2024-334237) | CNN metastasis prediction from primary-tumor CT | 335 + external 326 | AUROC 0.76–0.81 external; risk score beat NCCN resectability for OS |
| Guan et al., Ann Surg Oncol 2025 [DOI](https://doi.org/10.1245/s10434-025-17748-1) | DL-radiomics early-recurrence nomogram | 493 | AUC 0.92 validation |

No PubMed hit (2019–2026) for an AI model recommending arterial or venous reconstruction type in pancreatectomy. All DOIs above were returned by PubMed metadata calls on 2026-11-03.
