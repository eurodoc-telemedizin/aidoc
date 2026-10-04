# From ChatGPT to Sovereign AI — EACMFS 2026, Athens

**Summary of [smile.wien/eacmfs](https://smile.wien/eacmfs/) and the POSTOP CLEFT internal review poster (ÖGZMK 50. Jubiläumskongress 2026, Hofburg Wien, 1–3 October 2026).**

## The presentation

**From ChatGPT to Sovereign AI** — an open-source, federated clinical decision support system with foundation-model modules for oncology, implantology and plastic surgery.

Truppe M.¹, Schicho K.², Adekunle A. A.³, Adeyemo W. L.³, Ewers R.²
¹ Private Practice, Vienna · ² Medical University of Vienna · ³ Lagos University Teaching Hospital (LUTH), University of Lagos

Presented at the 28th EACMFS Congress, Athens, 15–18 September 2026, session "AI and CMF Surgery".

## The short version

A clinical decision support system that runs entirely inside the practice. No patient data leaves the premises. Every record and audit event is hash-anchored to a public timestamp chain (OpenTimestamps), so the trail is verifiable years later without trusting the operator. The language models are open-weight and run locally. **The clinician decides; the system drafts.** The project is open source and looking for clinical teams to test it.

## Why sovereign, why now

- LLMs are remarkable at language and unreliable at knowledge — a fluent, confident, wrong differential is worse than no model.
- Sending patient data to third-party APIs is a compliance question for European practices and a sovereignty question for a teaching hospital in Lagos. Both point to the same answer: inference must happen where the patient is.
- Design constraint set first: protected health information never leaves the premises.

## What the system is

- **Workstation:** local NVIDIA GB10 edge machine, 128 GB unified memory — a chassis that fits in a practice, not a data centre.
- **VP-Orchestrator:** in-house orchestration layer with the clinical reasoning loop (session context, case state, green/amber/red triage, trend detection across a longitudinal case), PostgreSQL backbone for case records and audit events, and OpenTimestamps anchoring of every record.
- **Core components** (each attached over the Model Context Protocol, each running locally, none optional, none remote):
  - **Nextcloud** — the record: anamnesis, operation reports, consent, imaging, interaction logs including runtime prompts
  - **RAGFlow** — the evidence: curated literature, guidelines and SOPs, cited passages mapped back to the case
  - **Ollama** — local model server: open-weight LLMs, on-premises inference, fallback DeepSeek-R1-8B
  - **MedlibreGPT** — knowledge body: local model over past research and clinical notes, assisting study design and reporting
- **Human oversight is not a component.** The agent drafts, a named clinician decides. The system issues no diagnoses and makes no medication changes.
- **Escalation path is deliberately dumb:** for red and amber-urgent tiers the escalation file is written *before* the caregiver receives any reply, and an independent non-LLM watchdog delivers it to the team channel within five minutes. A language model cannot suppress an escalation it has triggered, because the model is not the thing doing the delivering.

## What was evaluated in Athens — POSTOP CLEFT

An agentic front-end for caregivers after cleft lip repair, delivered over Telegram, built on OpenClaw with the Hermes agent runtime.

- Clinical setting: cleft clinic at LUTH, Lagos. The caregiver — typically the mother of an infant — submits a seven-item daily report and a wound photograph for fourteen days.
- The reasoning loop assigns a green/amber/red tier and watches the **trend across days**, not each day in isolation. Red means immediate presentation at LUTH, with readmission if needed.
- The bot answers in English, Pidgin and Yoruba, around the clock. All cleft care at LUTH is free through Smile Train.
- Athens reported the **in-silico test run** on longitudinal virtual cases preceding enrolment: whether the loop holds a 14-day trajectory, whether triage tiers behave sensibly as a case deteriorates, whether escalation fires within its window.
- The feasibility study at LUTH opened to recruitment in August 2026 and had **20 patients enrolled by September 2026**. Results are not yet reported; publication follows cohort completion, with extension to further sites.

## The September 2026 case — why the title says "from ChatGPT"

A mother at home with her infant after cleft lip repair sends a short video and one line: *"There something inside the nostril should I leave it like that."* A general-purpose assistant would answer fluently, confidently, and without anyone who knows the child having looked at the footage.

- Her seven questionnaire items were **all normal** — a questionnaire-only system would have returned green and moved on.
- The image disagreed: partial separation of the suture line near the base of the nose, and a nasal stent or suture fragment protruding from the left nostril.
- Structured output: `TIER: red · ITEMS_WORSENING: none · TREND_TWO_DAY: no · PHOTO_AGREEMENT: contradicts`.
- **`PHOTO_AGREEMENT: contradicts` carried the case** — it asks the narrow question "does what I can see match what I have been told?", and when the answer is no, the report is treated as the less reliable of the two.
- The instruction came first ("do not attempt to pull it out or touch it… until the surgeon sees the child"), then the reasoning (LUTH protocol requires immediate review for wound-edge separation or displaced surgical material), then travel advice and the safety net (bleeding, breathing difficulty, pus, fever ≥ 38.5 °C → emergency unit).
- The escalation was filed **before** the mother received any of it; the on-call surgeon was alerted to her arrival.
- The letter is signed by the operating surgeon and marked **"AI support, not signed"** — the clinician is accountable, the reader is told how it was produced.
- What the system was **not** permitted to do: decide the stent may come out, or decide Tuesday would be soon enough. Both are clinical decisions and both stayed with the surgeon.

*Published with the family's consent. No image of the child is shown.*

## The oral oncology module

- **AIDOC Oral Zero-Shot** uses DermLIP_ViT-B-16 — a multimodal dermatology vision foundation model (Nature Medicine 2025) — for zero-shot classification of de-identified intraoral photographs against a configurable oral label set, returning the three most probable classes with confidence values. No training on oral images; whether dermatological representations transfer to oral mucosa is an open empirical question — which is why it is being tested, not deployed.
- Follows the feasibility study in *Oral Oncology Reports* — *Artificial intelligence in oral cancer: a feasibility study informed by Freud's case* (Truppe, Schicho, Figl, Holawe, Perisanidis).
- Prospective work registered: **EK Nr. 1381/2026, AIDOCVISION REMOTE** — multimodal AI-assisted early detection of oral malignancies with stereotactic intraoral scanner documentation in private practice: a prospective, blinded diagnostic pilot study.

## Thirty years, one thread

1996: Interventional Video Tomography at the EACMFS Jubilee Congress in Zurich; the ARTMA Virtual Patient System became the first augmented-reality visualisation in image-guided surgery with CE Class IIa certification. The idea is unchanged: put the right information into the surgeon's field of view at the moment of decision, and leave the decision with the surgeon. What changed is where the computation lives — in 1996 a workstation in the OR, in 2026 a workstation in the practice. In both cases, in the room.

## Collaboration

Clinical teams worldwide are invited to partner on clinical studies of this open-source system — cleft follow-up pathway, oral oncology module, or the architecture itself. Contact: Michael Truppe, MD — [email protected] · Related: [onemosquito.ai](https://onemosquito.ai) · [postop.onemosquito.ai](https://postop.onemosquito.ai)

---

# Poster: Caregiver-Reported Remote Monitoring After Infant Cleft Lip Repair With a Locally Hosted AI Triage System at LUTH, Lagos, Nigeria

**POSTOP CLEFT × VP-Orchestrator · Pilot Study · LUTH Lagos × Medical University of Vienna**
ÖGZMK 50. Jubiläumskongress 2026, Hofburg Wien, 1–3 October 2026 (A0 internal review poster, vfinal, 01 Oct 2026)

*The photograph caught what the questionnaire missed — an on-premises, open-source, physician-supervised clinical AI platform: 30 years of research (1996–2026).*

Truppe M.¹ · Lingaraj J.² · Schicho K.² · Adekunle A.A.³ · Adeyemo W.L.³ · Ewers R.²
¹ Private Practice, Vienna · ² Medical University of Vienna · ³ LUTH, University of Lagos · J. Lingaraj: IAOMS Visiting Scholar, Assistant Professor, Saveetha Dental College and Hospital, Chennai; Fellowship in Cleft, Palate and Craniofacial Surgery, CCI Zurich

## Key metrics (19 Aug – 16 Sep 2026)

| Metric | Value |
|---|---|
| Cases processed with AI | **21** (P01–P16 test uploads; patients P17–22) |
| Reports | **50** (48 patient-days) |
| Photo attach rate | **100%** — every report carried an image |
| Median caregiver time | **3.8 min** per daily report |
| Escalations | **2**, endorsed by governance review |

## Why it matters in Lagos

53–59% of cleft families at this centre face catastrophic health expenditure from treatment costs alone; globally, most surgical financial catastrophe stems from the non-medical costs of reaching care — transport and lost income, not the surgery. Every remote follow-up day is a cost these families do not pay.

## Proof of concept — 5 steps

1. Caregiver under-reported — both questionnaires all-normal; on day 2 the caregiver asked "should I leave it like that" about a protruding stent.
2. Vision channel (local LLM, MedlibreGPT gemma4:31b-cloud) detected dehiscence signals on POD 3–4 — the high-risk window.
3. Concordance logic (`PHOTO_AGREEMENT: contradicts`) overrode the green questionnaire → red.
4. Physician in the loop reviewed, approved, sent correct urgent advice — safety-netting, no remote prescribing.
5. Coherent across 2 consecutive days — not a one-off false alarm.

## Cases shown

- **Simulation case P20:** unilateral left complete cleft lip & alveolus, Fisher repair 6 Sep 2026 (case S3). Both days questionnaire green — vision contradicts → TIER: red *(subject to change, final data analysis)*.
- **CASE V1 · POD 3 · 9 Sep 2026:** vision — suture-line separation, opaque discharge, crusting, spreading erythema. Action: "come to the clinic now", wound-care instructions, safety net.
- **CASE V2 · POD 4 · 10 Sep 2026:** vision — nasal stent / suture fragment at nostril. Action: do-not-manipulate, continue prescribed care, present immediately.

## Modules on the sovereign-AI blueprint

One permanent core (VP-Orchestrator, PostgreSQL record, OpenTimestamps blockchain audit, physician-in-the-loop, independent non-LLM watchdog; PHI never leaves the premises) with pluggable clinical modules:

- **POSTOP CLEFT** — reconstructive surgery: caregiver telemonitoring after cleft-lip repair, LUTH Lagos — *evaluated in this study* · Telegram bot: [t.me/postopcleftbot](https://t.me/postopcleftbot)
- **AIDOCVISION REMOTE** — oncology: early detection of oral malignancies with stereotactic intraoral scanning — MUW × Walailak University Thailand, pilot study for MDR, EK 1381/2026 (MUW)
- **HKP** — online treatment plan: 800+ teleconsultations, zero office visits; the platform captured patients dentistry loses to anxiety (77%), and one in two wanted to start immediately
- **Therapy after Cleft Lip Palate Surgery (Project India)** — AI-powered speech assessment for cleft care: hypernasality & VPD monitoring

## Why sovereign, why now

- **Regulation:** EU AI Act + MDR + GDPR make auditable on-prem AI the compliant default — sovereignty by architecture.
- **Economics:** local open-weight inference runs at near-zero marginal cost on clinic hardware — and answers to no one else.
- **Validity:** one architecture, cohorts on three continents, four aetiologies — proving the model outside Vienna.

---

**Ethics & funding:** POSTOP CLEFT — LUTH HREC/NHREC (no. pending); caregiver informed consent incl. image consent. AIDOCVISION REMOTE — EK 1381/2026, Medical University of Vienna. Sponsor: EURODOC Telemedizin Forschungsgesellschaft mbH. COI: none to declare. Data reproducible from de-identified zone-B files; photos cropped to wound area, EXIF-stripped.

**Sources:** https://smile.wien/eacmfs/ · poster `POSTOP-CLEFT-poster-OEGZMK-2026.jpg` (this folder) · bot https://t.me/postopcleftbot · contact truppe@truppe.at
