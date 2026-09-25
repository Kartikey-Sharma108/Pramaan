# PRAMAAN V2 — Team Context & Round 1 Killer Prototype Plan

## 0. Why this document exists

This is the single shared context document for the team.

Its purpose is to make sure every teammate understands:
- what Pramaan is,
- what problem we are solving,
- what changed across our engineering phases,
- what is genuinely implemented,
- what the current architecture does,
- what we should not waste time building,
- what the judges need to understand,
- how the live prototype should flow,
- and what the final killer demo should look like.

The immediate goal is **Round 1 prototype evaluation**, not production deployment.

Target live-demo time:

> **7–10 minutes**

Core principle:

> **Keep the surface simple. Keep the engineering underneath strong.**

---

# 1. The One-Sentence Definition

## What is Pramaan?

> **PRAMAAN is an interview integrity layer for remote technical hiring that detects real-time AI proxies, face-swaps, and candidate imposture using client-side multimodal telemetry, active live presence challenges, and non-punitive fairness guarantees—without capturing or storing a single second of raw candidate video or audio.**

In simpler language:

> **“A fake face can fool a frame. It cannot fool a live interaction.”**

The core loop is:

```text
BROWSER-SIDE LANDMARK & AUDIO SENSING
  ↓
DERIVED NUMERIC TELEMETRY ONLY (ZERO RAW MEDIA)
  ↓
DETERMINISTIC MULTIMODAL COHERENCE SCORING
  ↓
ACTIVE LIVE PRESENCE CHALLENGE
  ↓
CHRONOLOGICAL EVIDENCE TIMELINE
  ↓
SEALED FORENSIC REPORT WITH HUMAN-DECISION DISCLAIMER
```

That loop matters more than any individual backend component.

---

# 2. The Problem We Want Judges to Understand

Imagine an Indian tech enterprise or global startup conducting remote technical interviews.

The hiring team tests the candidate on Google Meet or Zoom.

The candidate answers complex distributed systems and React questions flawlessly.

The hiring manager assumes:

> “We found our top engineer.”

But remote technical hiring has entered the generative AI and deepfake era.

Candidates and proxy-syndicates use:
- real-time AI face-swaps (e.g., DeepFaceLive, OBS virtual cams),
- hidden teleprompters and background audio feeds,
- proxy lip-syncing (where a senior domain expert speaks off-camera while the candidate mouths the words),
- virtual camera routing and split audio drivers.

There is another critical risk too—the opposite failure mode.

Suppose two honest candidates interview under different infrastructure conditions:

### Candidate A (Urban Fiber)
- Bangalore / South Mumbai
- 100 Mbps fiber connection
- 1080p 60fps external camera
- Crisp audio & zero packet drop

### Candidate B (Tier-2 / Tier-3 Town)
- Bareilly / Jhansi / Rural district
- 4G mobile hotspot connection
- 480p laptop webcam
- Jitter, low frame rate & compression artifacts

If an automated proctoring AI flags Candidate B as a "fraudulent deepfake" or automatically rejects them simply because their video stream degraded over mobile data, that is an unacceptable fairness failure.

### The recruiter problem

> **“How do I verify candidate authenticity in real time without turning our company into a creepy surveillance state or unfairly rejecting candidates who have poor internet connections?”**

### Pramaan answer

> **“We verify live interactive coherence on the client device, issue un-spoofable active presence challenges, enforce a non-punitive low-bandwidth fairness invariant, and give recruiters explainable decision-support evidence—with zero raw video storage.”**

---

# 3. What Pramaan Is NOT

We should avoid presenting Pramaan as:
- just another invasive proctoring spyware (like Proctorio or Honorlock),
- an automated candidate rejection machine / automated hiring bot,
- a cloud video surveillance recording vault,
- just a single-frame static deepfake image detector,
- a punitive tool that penalizes candidates for blinking or connection drops,
- or just an academic computer vision toy.

The stronger interpretation is:

> **PRAMAAN is an interview integrity and evidence layer between remote candidates and human recruiters.**

It helps a hiring team answer:

> **“Can we trust this live remote interaction, and do we have objective, privacy-first evidence to support our decision?”**

---

# 4. The Current Product Story

The cleanest product narrative is:

## A recruiter creates an interview session.
↓
## The candidate joins and grants explicit biometric & processing consent.
↓
## Browser-side computer vision extracts lightweight motion & audio landmarks.
↓
## Pramaan detects multimodal divergence (speech timing vs. facial motion).
↓
## Recruiter dispatches a dynamic, randomized live presence challenge.
↓
## Candidate responds live; browser verifies speech & physical gesture synchronously.
↓
## Pramaan enforces non-punitive low-bandwidth fairness rules if the network drops.
↓
## Recruiter generates a forensic audit report with a mandatory human-decision disclaimer.
> **Human recruiter makes the final call backed by auditable evidence.**

That is the story we should build the prototype around.

---

# 5. What Has Actually Been Built

We have four core engineering layers.

Do not think of these as four disconnected scripts.

They are four layers of the same unified integrity system.

---

## LAYER 1 — Client-Side Edge Perception & Privacy Minimization

### Purpose

Ensure biometric analysis happens entirely on the candidate's device without exposing raw video or audio to network interception or cloud storage.

### Major implementations

### Strict raw media rejection

The server strictly rejects any request payload containing raw media keywords:

```text
Incoming Signal Payload
     ↓
Contains 'video', 'audio', 'frame', 'image', 'blob', 'base64'?
     ↓
YES → HTTP 400 Bad Request (Rejected Instantly)
NO  → Accept Derived Landmark Telemetry Only
```

### Zero binary schema footprint

The database stores only numeric telemetry aggregates (0–100), timestamps, and text events. The Prisma schema has zero `Bytes` or binary blob columns.

### Client-side face & landmark tracking

Real-time MediaPipe and native face-detection pipelines run inside the candidate's browser to compute bounding box stability, natural micro-motion, and landmark presence.

### Client-side speech & audio energy correlation

Web Audio API processes live microphone audio locally, measuring real-time energy envelopes (RMS) to cross-correlate speech bursts against mouth/lip movements.

---

# 6. LAYER 2 — Multi-Modal Signal Engine (The 4 Vectors)

Pramaan does not rely on brittle high-resolution pixel textures that break on cheap 480p laptop webcams.

It evaluates cross-modal coherence across four explicit signal vectors:

1. **Face + Motion (30% weight)**: Bounding box stability, natural head micro-motion, blink presence, frame edge consistency.
2. **Voice + Lip Sync (30% weight)**: Audio energy alignment with mouth opening/closing cycles.
3. **Live Presence Challenge (25% weight)**: Candidate reaction latency and correctness on randomized real-time prompts.
4. **Stream Context (15% weight)**: Network packet stability, jitter, and frame continuity.

This weighted formula produces a continuous, deterministic integrity spectrum from **0 to 100**.

---

# 7. Live Presence Challenge — The Interactive Verification Pillar

Why static deepfake detectors fail:

> Generative face-swap tools (DeepFaceLive, LivePortrait) can convincingly render a face if the person sits still and talks normally.

However, real-time face-swapping pipelines cannot predict dynamic, randomized interactive challenges.

Pramaan dispatches unpredictable multimodal prompts:

### Challenge Examples
- *“Turn your head slightly to the right and say BLUE 47.”*
- *“Hold up three fingers and say HELLO WORLD.”*
- *“Look to your left, then back at the camera and say GREEN 12.”*
- *“Smile at the camera and say PASS CODE 7.”*
- *“Blink twice and say DELTA ECHO.”*

Conceptually:

```text
Recruiter Dispatches Challenge
              ↓
Candidate Receives Unpredictable Prompt
              ↓
20-Second Response Window Active
              ↓
Browser Speech API Listens & Verifies Transcript
              ↓
Face Tracker Verifies Head Rotation / Physical Gesture
              ↓
PASSED (100)  |  PARTIAL (50)  |  FAILED (0)
```

The product insight is:

> **Virtual camera face-swap models introduce warping artifacts, extreme angle clipping, and 1500ms+ latency when forced to execute unpredictable multi-modal commands.**

This is a premier demo moment.

---

# 8. Fairness Invariant: The Non-Punitive Low-Bandwidth Guarantee

One of Pramaan’s most critical engineering rules:

> **Network degradation is NOT evidence of dishonesty.**

If a candidate experiences packet loss, low frame rate, or connection drops:

```text
Stream Quality Score < 45  OR  Visual Evidence Temporarily Degraded
                            ↓
               DO NOT INCREASE RISK SCORE
                            ↓
          Status: INSUFFICIENT_EVIDENCE
          Risk Score: Neutral Baseline (32/100)
          Confidence: LOW
                            ↓
Explanation:
"Poor video quality lowers confidence. It does not prove dishonesty.
 Visual evidence is limited because of stream quality."
```

The system distinguishes:
- `LOW_RISK` (High Coherence, High Confidence)
- `REVIEW_RECOMMENDED` (Multimodal Divergence, Medium/High Confidence)
- `INSUFFICIENT_EVIDENCE` (Network Degraded, Low Confidence)

This guarantees fairness for candidates interviewing from Tier-2/Tier-3 Indian towns on mobile hotspots.

---

# 9. Proxy & Face-Swap Divergence Detection — The Hero Feature

The simplest explanation:

> **Compare speech timing against facial landmark movement. When a proxy speaks or a face-swap renders, speech and motion diverge.**

Example:

### State A — Normal Candidate
```text
Face Motion: 94 / 100
Lip Sync:    96 / 100
Challenge:   100 / 100
Stream:      92 / 100
Risk Score:  12 / 100 (LOW RISK)
Confidence:  HIGH
```

### State B — Simulated Proxy / Face-Swap
```text
Face Motion: 54 / 100
Lip Sync:    38 / 100  ← Divergence Anomaly
Challenge:   50 / 100  ← Lag / Incomplete
Stream:      84 / 100  ← High Quality Stream
Risk Score:  78 / 100 (REVIEW RECOMMENDED)
Confidence:  MEDIUM
```

The UI makes the divergence immediately obvious:

```text
HIGH STREAM BANDWIDTH (84)
✓ Crisp video feed
✓ Low network latency

COLLAPSED CROSS-MODAL COHERENCE
🚨 Voice/Lip Sync dropped to 38
🚨 Facial motion variance dropped to 54
🚨 Live challenge incomplete
```

Then display the explainable incident in the evidence timeline.

This is immediately understandable to judges without statistical confusion.

---

# 10. Important Evidence Wording

Do NOT say:

> “We caught a fraudster.”  
> “Candidate is confirmed fake.”  
> “Our AI automatically rejected the applicant.”

Use:

> **“PRAMAAN detected significant multimodal divergence between speech and facial motion, recommending human recruiter review.”**

For a nontechnical judge:

> **“PRAMAAN gives the recruiter an objective, un-spoofable dashboard. The human recruiter always makes the hiring decision.”**

---

# 11. Active Challenge Engine & Speech Verification

Pramaan includes automated browser speech recognition coupled with recruiter manual controls:
- Web Speech API listens for target passphrase during active challenge,
- Real-time countdown timer (20s) enforces reaction bound,
- Recruiter console provides manual override controls (`Record Passed`, `Record Partial`, `Record Failed`),
- Fallback gracefully switches to recruiter-graded verification if speech recognition is unsupported.

This demonstrates robust edge engineering.

---

# 12. LAYER 3 — Ingest → Score → Challenge → Timeline → Forensic Report

Layer 3 transforms raw telemetry into an auditable recruiter workflow:

### Signal Ingestion
Lightweight numeric snapshots arrive via HTTP/WebSocket, immediately triggering the deterministic scoring engine.

### Scoring Engine
Computes 4-vector weighted risk while enforcing fairness invariants.

### Incident Timeline
Every anomaly and verified challenge is appended to a tamper-resistant session timeline:
- Timestamp (e.g., `14:22:08`),
- Category (`VERIFIED`, `WARNING`, `SYSTEM`),
- Neutral, objective description (zero accusatory language).

### Sealed Report
A completed session generates a cryptographic audit report bundle.

The important concept:

> **Interview integrity is preserved as auditable forensic evidence, not a black-box hiring verdict.**

---

# 13. Why Real-Time Decision Support Beats Automated Proctoring

Automated AI proctoring has failed historically because:
- it generated false accusations against honest applicants,
- it stored private biometric video in vulnerable cloud buckets,
- it treated webcam glitches as cheating,
- it stripped human recruiters of context.

With Pramaan:

```text
Candidate Biometric Consent
 ↓
Edge Processing (Zero Raw Video Stored)
 ↓
Multimodal Coherence + Live Challenge
 ↓
Fairness Invariant on Bad Networks
 ↓
Explainable Recruiter Timeline
 ↓
Human-in-the-Loop Hiring Decision
```

This makes Pramaan feel like modern enterprise compliance infrastructure rather than hostile spyware.

---

# 14. LAYER 4 — Real-World Validation & Verification Evidence

Layer 4 contains the verification suite and evidence infrastructure backing the system.

Our automated verification engine (`verify-all.ts`):
- Executes 28 comprehensive system, authentication, session, consent, challenge, risk scoring, fairness, timeline, privacy, and access-control assertions.
- Confirms 100% rejection of raw media payloads across all forbidden keywords (`video`, `audio`, `frame`, `image`, `blob`, `base64`).
- Verifies that unauthenticated and candidate tokens cannot access recruiter reports or unauthorized sessions.
- Generates JSON logs and timestamped API traces:

```bash
# Run verification suite
npx tsx verify-all.ts

# Inspect test logs and evidence report
artifacts/pramaan-test-report/PRAMAAN_TEST_REPORT.md
artifacts/pramaan-test-report/generated-session-report.json
```

---

# 15. The Forensic Evidence Bundle & Report

A session report can be exported as an auditable case file containing:

```text
session_id: "PRM-CX0104"
candidate: { name: "Candidate #CX0104", role: "Junior Frontend Engineer" }
consent_record: { cameraConsent: true, micConsent: true, consentedAt: "2026-09-26T..." }
final_risk: { score: 12, status: "LOW_RISK", confidence: "HIGH" }
signal_breakdown: { face: 94, voice: 96, challenge: 100, stream: 92 }
challenge_history: [...]
evidence_timeline: [...]
privacy_attestation: { browserSideProcessing: true, rawVideoStored: false }
mandatory_disclaimer: "PRAMAAN provides decision-support signals and does not make automatic hiring decisions."
```

Describe this as:

> **tamper-evident forensic evidence**

Do not call it:
- infallible proof of fraud,
- automated rejection certificate,
- or legal guilt determination.

---

# 16. Current Architecture — Simple Mental Model

The internal codebase is clean and modular, but the team should explain it like this:

```text
                     CANDIDATE BROWSER
                             │
            ┌────────────────┴────────────────┐
            ▼                                 ▼
      MediaPipe Face                   Web Audio Energy
      Mesh & Landmarks               (RMS Lip-Sync Sync)
            │                                 │
            └────────────────┬────────────────┘
                             │
            [Client-Side Feature Extraction]
            Numeric Telemetry ONLY (0–100)
            NO RAW VIDEO / NO AUDIO SENT
                             │
                             ▼
                     PRAMAAN BACKEND
               (Express + Socket.IO + Prisma)
                             │
                             ▼
                 DETERMINISTIC RISK ENGINE
            (Weighted Coherence + Fairness Guard)
                             │
            ┌────────────────┴────────────────┐
            ▼                                 ▼
      NORMAL CANDIDATE              DIVERGENCE ANOMALY
      (Low Risk: ~12)               (Risk Spike: ~78)
            │                                 │
            │                     DISPATCH LIVE CHALLENGE
            │                     ("Turn head & say BLUE 47")
            │                                 │
            └────────────────┬────────────────┘
                             │
                             ▼
                     RECRUITER CONSOLE
              - Risk Spectrum Bar (0–100)
              - 4-Vector Signal Breakdown
              - Incident Evidence Timeline
              - Export Forensic Audit Report
```

This is the architecture judges need to understand.

---

# 17. What Judges Do NOT Need to Understand During the Main Demo

Do not spend time explaining:
- WebRTC SDP handshake negotiation,
- MediaPipe WASM chunk loading,
- SQLite WAL pragma settings,
- exact Zod schema regex lines,
- SpeechRecognition browser vendor prefixes,
- bcrypt hash iterations,
- CSS hairline border variables.

These are Q&A ammunition.

---

# 18. What Judges SHOULD Understand

Within roughly 30–60 seconds they should know:

1. Remote tech hiring faces massive deepfake and proxy interview fraud.
2. Legacy proctoring is invasive, stores private video, and unfairly flags bad Wi-Fi.
3. Pramaan does all computer vision in the browser; zero raw video leaves the candidate's machine.
4. It catches proxies by checking if speech matches face motion.
5. It uses unpredictable live challenges that deepfakes cannot spoof in real time.
6. It enforces non-punitive fairness for low-bandwidth connections and gives recruiters human-auditable evidence.

That is enough.

---

# 19. The Killer Round 1 Demo

## Target duration: 7–8 minutes

We should not try to show every setting.

The hero story:

> **Tech enterprise verifying a remote frontend engineering candidate (#CX0104).**

---

## 0:00–0:45 — Problem

Say:

> “Imagine you're hiring a remote software engineer. The interview is conducted over Zoom. The candidate answers every architecture question perfectly. But in 2026, AI face-swaps, real-time voice proxies, and teleprompter syndicates are everywhere. How can you verify that the person speaking is genuinely present—without violating their privacy or storing gigabytes of invasive video recordings?”

Then:

> “That is why we built PRAMAAN.”

---

## 0:45–1:15 — Open Product & Architecture

Landing on the Live Interview Room (`PRM-CX0104`).

Point to the TopBar and Sidebar:

> **PRAMAAN: Interview Integrity Layer**  
> TopBar indicator: **Backend active**  
> Candidate: **Candidate #CX0104 · Junior Frontend Engineer**  
> Privacy banner: **Zero raw biometric data leaves device**

Say:

> “PRAMAAN is browser-first. Notice the privacy guarantee: all face landmark detection and audio energy calculations happen client-side. Our server never receives, records, or stores raw candidate video or audio.”

---

## 1:15–2:15 — Step 1: Normal Candidate Baseline

Click / Verify:

# **Step 1: Normal Candidate**

Show the Recruiter Console:

```text
Risk Score:        12 / 100
Status:            LOW RISK (Green)
Confidence:        HIGH

Signals:
✓ Face + Motion:   94 / 100
✓ Voice + Lip:     96 / 100
✓ Live Challenge:  100 / 100
✓ Stream Quality:  92 / 100
```

Say:

> “In normal conditions, candidate signals are coherent. Facial micro-movement aligns with speech energy, stream quality is high, and the risk score sits at a healthy 12 out of 100.”

---

## 2:15–3:30 — Step 2: Live Presence Challenge (Active Defense)

Click:

# **Issue Live Challenge**

Prompt displays across both views:

> **“Turn your head slightly to the right and say BLUE 47.”**

Show live response window:
- Countdown timer starts (20s)
- Browser speech recognition listens
- Candidate performs gesture and says phrase

Click:

# **Record Passed**

Timeline immediately logs:

```text
14:02:11 | VERIFIED
Challenge Passed: Candidate responded "BLUE 47" with synchronized head turn.
```

Say:

> “This is our active defense. A pre-recorded video or deepfake filter can mimic standard dialogue, but it cannot anticipate an unpredictable live physical challenge without introducing massive latency or facial tearing.”

---

## 3:30–4:45 — Step 3: Hero Finding — Simulated Proxy / Face-Swap

Click:

# **Simulated Proxy**

Watch the Recruiter Console update in real time:

# 🚨 Multimodal Divergence Detected

```text
Risk Score:        78 / 100
Status:            REVIEW RECOMMENDED (Amber/Red)
Confidence:        MEDIUM

Signals:
→ Stream Quality:  84 / 100  (Network is solid)
🚨 Voice + Lip:    38 / 100  (Severe desynchronization)
🚨 Face Motion:    54 / 100  (Unnatural facial warping)
```

Point to the Timeline:

```text
14:04:19 | WARNING
Speech/facial divergence anomaly detected. Multimodal alignment score dropped to 38.
```

Say:

> “Look at what happened. The network is completely fine at 84. But voice-to-lip synchronization plummeted to 38 and natural face motion dropped to 54. A proxy is speaking off-camera while an AI avatar attempts to mouth the words. PRAMAAN immediately flags the divergence and alerts the recruiter.”

Pause. Let the judges absorb the 78 risk score.

---

## 4:45–5:45 — Step 4: Low-Bandwidth Fairness Guarantee

Click:

# **Low Bandwidth**

Show the result:

```text
Risk Score:        32 / 100
Status:            INSUFFICIENT EVIDENCE (Neutral Blue)
Confidence:        LOW

Signals:
🚨 Stream Quality: 24 / 100
- Face Motion:     N/A (Limited visibility)
✓ Voice:           86 / 100
```

Read the prominent explanation on screen:

> **“Poor video quality lowers confidence. It does not prove dishonesty. Visual evidence is limited because of stream quality.”**

Say:

> “Now look at our fairness invariant. If this were Candidate B in a tier-3 city on a weak 4G connection, legacy proctoring would fail them for cheating. PRAMAAN does the opposite: it recognizes network degradation, drops confidence, and clamps the risk to an unpunished baseline of 32. We never mistake bad Wi-Fi for fraud.”

---

## 5:45–6:45 — Step 5: Generate Forensic Evidence Report

Click:

# **Generate Evidence Report**

The modal pops up:

```text
PRAMAAN Forensic Interview Audit Report
Session: PRM-CX0104 · Candidate: Candidate #CX0104

✓ Biometric Consent Recorded (UTC Verified)
✓ Complete 4-Vector Signal Breakdown
✓ Chronological Incident Timeline (4 verified events)
✓ Privacy Guarantee: Browser-side processing, zero raw video stored
```

Point to the bottom banner:

> **Mandatory Disclaimer: PRAMAAN provides decision-support signals and does not make automatic hiring decisions.**

Say:

> “At the end of the interview, the recruiter exports an auditable forensic report. It contains the complete incident history, consent timestamps, and signal breakdown. And crucially: PRAMAAN provides decision-support evidence—the human hiring manager makes the final hiring call.”

---

## 6:45–7:30 — India-Specific Secondary Wow

Quickly highlight:

```text
TIER-1 FIBER (100 Mbps)  → HIGH CONFIDENCE EVALUATION
TIER-3 4G (Degraded)     → INSUFFICIENT EVIDENCE (ZERO PENALTY)
LOCAL DPDP COMPLIANCE    → ZERO RAW VIDEO STORED IN CLOUD
```

Say:

> “India is hiring millions of remote knowledge workers across diverse tiers of connectivity. PRAMAAN is designed specifically for this reality—compliant with India's Digital Personal Data Protection Act, resilient to poor bandwidth, and robust against modern AI threats.”

---

## 7:30–8:00 — Closing

Use:

> **“Legacy proctoring treated candidates like criminals while recording their private bedrooms. PRAMAAN proves that with client-side intelligence and active challenges, we can stop AI fraud while respecting privacy and fairness.”**

Then:

> **“A fake face can fool a frame. It cannot fool PRAMAAN.”**

End.

---

# 20. The Exact Product Flow the Judge Should Experience

```text
I am a recruiter conducting a remote interview
     ↓
Candidate joins and grants consent
     ↓
PRAMAAN monitors 4 signal vectors client-side
     ↓
Baseline looks normal (Risk: 12, Low Risk)
     ↓
Let's verify presence → Issue live challenge ("Turn head & say BLUE 47")
     ↓
Candidate completes it → Verified in timeline
     ↓
What happens if a candidate uses a proxy or face-swap?
     ↓
Simulate Proxy → Speech/motion divergence detected (Risk: 78, Review Recommended)
     ↓
What if the candidate just has terrible Wi-Fi?
     ↓
Simulate Low Bandwidth → Risk clamped to 32, Insufficient Evidence (Zero Penalty)
     ↓
Export forensic audit report with tamper-evident timeline
     ↓
Recruiter makes confident, informed hiring decision
```

This is the entire product.

---

# 21. UI Principles

The rule:

> **The judge should understand the integrity status before reading the technical formulas.**

Prefer simple, professional labels:

Instead of:
> Deepfake Cheat Detector

Use:
> Interview Integrity Layer

Instead of:
> Fraud Confirmed / Candidate Cheating

Use:
> Review Recommended / Multimodal Divergence

Instead of:
> Automatically Rejected

Use:
> Human Recruiter Review Recommended

Instead of:
> Frame Drop Error

Use:
> Insufficient Evidence (Fairness Invariant)

Instead of:
> Spyware Telemetry

Use:
> Privacy-Preserving Derived Signals

---

# 22. What Should Be Hidden

During the primary demo, hide:
- raw WebSocket frame payloads,
- MediaPipe coordinate tensors,
- SQLite database tables and foreign keys,
- HTTP headers and bearer tokens,
- local storage cache keys,
- verbose terminal debug streams.

Keep available for Q&A:
- the 28-assertion verification report (`PRAMAAN_TEST_REPORT.md`),
- mathematical breakdown of the 4-vector scoring engine,
- raw-media rejection tests in `verify-all.ts`,
- client-side Web Audio API RMS energy algorithms,
- DPDP Act and privacy minimization architecture.

---

# 23. What We Hardened Before the Final Demo

Our recent system audit and verification pass resolved all edge cases:

## CRITICAL — Strict Rejection of Raw Media
Verified that any request payload attempting to transmit `video`, `audio`, `frame`, `image`, `recording`, `blob`, or `base64` is immediately rejected with **HTTP 400 Bad Request**.

## HIGH — Guaranteed Low-Bandwidth Fairness Invariant
Verified that whenever `streamQualityScore < 45` or visual evidence is unavailable, the risk status strictly returns `INSUFFICIENT_EVIDENCE`, the score is clamped to `32`, and the explanation prevents unfair penalization.

## HIGH — Neutral, Non-Defamatory Audit Language
Scanned timeline events and reports to ensure zero accusatory phrasing (`fraud confirmed`, `candidate is fake`, `automatically rejected`). All outputs use objective forensic terminology.

## MEDIUM/HIGH — Token Scoping & Access Control
Verified that candidate tokens cannot access recruiter session lists or generate reports, and are strictly scoped to their assigned session ID.

## HIGH — Mandatory Human-Decision Disclaimer
Embedded the mandatory legal disclaimer into all report exports:  
> *“PRAMAAN provides decision-support signals and does not make automatic hiring decisions.”*

---

# 24. What We Should NOT Build Now

Do not start:
- WebRTC media server clustering (SFU/MCU like Janus or Mediasoup),
- heavy server-side GPU PyTorch deepfake inference,
- full ATS integrations (Workday, Greenhouse, Lever),
- PostgreSQL enterprise migration,
- Redis pub/sub clustering,
- complex candidate eye-gaze tracking models.

The current Next.js + Express + Socket.IO + SQLite architecture is lightning fast and 100% stable for Round 1.

---

# 25. Why Pramaan Can Stand Out

| Feature | Legacy Proctoring (Proctorio, etc.) | Cloud AI Video Analyzers | PRAMAAN |
|---|---|---|---|
| **Privacy Architecture** | Ingests & stores raw video/audio | Uploads video to cloud AI | **100% Client-Side; Zero raw media stored** |
| **Deepfake Resilience** | Fails completely against virtual cams | Analyzes static single frames | **Active Live Presence Challenges** |
| **Network Fairness** | Flags connection drops as cheating | Rejects low-res streams | **Non-punitive fairness invariant (Risk clamped)** |
| **Legal Posture** | Highly invasive; regulatory pushback | Data ownership concerns | **DPDP privacy-first data minimization** |
| **Workflow Role** | Automated black-box rejection | Unexplained risk percentages | **Recruiter decision-support & forensic timeline** |

That is an **engineering workflow**, not just a demo.

---

# 26. Three Strongest Things About Pramaan

## 1. Privacy-Preserving Edge Architecture
> “Zero raw video or audio leaves the candidate's device.”  
Eliminates data breach liability and complies with modern data privacy laws.

## 2. Active Live Presence Challenges
> “A fake face can fool a frame, but cannot fool an unpredictable live interaction.”  
Neutralizes real-time virtual camera face-swaps and proxies.

## 3. Built-in Fairness Invariant for Emerging Markets
> “Poor internet connection lowers confidence; it never proves dishonesty.”  
Prevents discrimination against candidates with limited infrastructure.

---

# 27. The Product Should NOT Be “Hostile Spyware”

Hostile spyware is the **wrong mental model**.

The product is:

> **Ethical Interview Integrity & Recruiter Decision Support**

Pramaan protects both sides of the table:
- **For companies**: Protects against costly proxy fraud and hiring impostors.
- **For candidates**: Protects honest candidates from false accusations, protects their biometric privacy, and safeguards those on weak network connections.

---

# 28. Judge Q&A — Simple Answers

### Why can't a proxy just memorize the challenge?
> “Challenges are randomized and generated in real time after the session begins. The candidate has a 20-second window to execute an unpredictable physical gesture combined with a specific phrase.”

### Why do you process video in the browser instead of on your server?
> “Two reasons: Privacy and latency. Transmitting raw video to cloud GPUs is expensive, slow, and creates massive biometric liability under the DPDP Act. Running lightweight landmark models in the browser extracts the necessary numeric telemetry instantly without privacy risk.”

### What if a candidate has an unstable internet connection?
> “We built a strict fairness invariant: if stream quality drops below 45 or video is unavailable, PRAMAAN clamps the risk score to a neutral 32 and sets the status to INSUFFICIENT EVIDENCE. We lower confidence; we never accuse the candidate.”

### Can someone spoof the client-side telemetry?
> “PRAMAAN correlates multiple asynchronous vectors—audio energy envelopes, facial landmarks, stream context, and challenge timings. Faking a synchronized cross-modal stream in real time while responding to recruiter questions is exponentially harder than running a virtual camera filter.”

### Does Pramaan make automated hiring decisions?
> “No. PRAMAAN strictly provides decision-support telemetry and an auditable forensic timeline. The human recruiter always reviews the findings and makes the final hiring decision.”

### What happens if the browser doesn't support Web Speech?
> “PRAMAAN gracefully degrades to manual recruiter confirmation, allowing the interviewer to verify the prompt with one click without disrupting the session.”

---

# 29. Team Execution Strategy

Work as four coordinated tracks during the demo prep:

## Track 1 — Core Scoring & Edge Stability
Own:
- MediaPipe detector initialization,
- Web Audio API microphone levels,
- riskScorer deterministic thresholds,
- raw media payload rejection.

## Track 2 — Recruiter Console & Demo UX
Own:
- TopBar status indicators,
- Confidence spectrum bar animation,
- 4-vector signal meters,
- Live challenge prompt banner and countdown,
- Guided demo automation steps.

## Track 3 — Evidence Data & Reporting
Own:
- Seed candidate profile (`Candidate #CX0104`),
- Neutral, forensic incident event logging,
- Forensic audit report export (`generated-session-report.json`),
- Mandatory human disclaimer rendering.

## Track 4 — Pitch & Judge Defense
Own:
- 7–8 minute live presentation narration,
- Problem framing (AI face-swaps & proxy syndicates),
- India tier-2/3 fairness narrative,
- Rapid Q&A defense.

---

# 30. Definition of “Round 1 Ready”

The prototype is ready when a judge can clearly see:

1. A live candidate session with active client-side video.
2. The TopBar displaying green backend connectivity and zero raw media storage.
3. The baseline normal state showing healthy 12/100 risk.
4. An active presence challenge issued, spoken, and verified live.
5. The simulated proxy trigger demonstrating immediate voice/lip divergence and risk jump to 78/100.
6. The low-bandwidth trigger proving non-punitive fairness (risk clamped to 32/100).
7. An exported forensic report with timestamps and the mandatory human-decision disclaimer.
8. The entire demo executed crisply within 7–8 minutes.

---

# 31. Final Product Diagram

```text
                           PRAMAAN
                              │
                              ▼
                   LIVE REMOTE INTERVIEW
                              │
                              ▼
                 CANDIDATE EDGE BROWSER
             (Face Landmarks + Audio Energy)
                              │
                              ▼
            NUMERIC TELEMETRY ONLY (0–100)
            [Raw Media Keywords Rejected: 400]
                              │
                              ▼
                   DETERMINISTIC ENGINE
             (30% Face + 30% Voice + 25% Challenge + 15% Stream)
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
     NORMAL FEED         PROXY DIVERGENCE    POOR NETWORK
          │                   │                   │
      Score: ~12          Score: ~78          Score: 32
       LOW RISK       REVIEW RECOMMENDED  INSUFFICIENT EVIDENCE
          │                   │                   │
          │             DISPATCH CHALLENGE   (FAIRNESS GUARD)
          │             ("Turn & say BLUE")       │
          │                   │                   │
          └───────────────────┼───────────────────┘
                              │
                              ▼
                   RECRUITER EVIDENCE ROOM
                              │
                              ▼
                  FORENSIC AUDIT REPORT
                              │
                              ▼
                HUMAN RECRUITER DECISION
```

---

# 32. Final Team Mental Model

When discussing Pramaan internally, avoid thinking:

> “We have a complex combination of WebSockets, MediaPipe models, and scoring algorithms.”

Instead think:

> **“We give remote recruiters an un-spoofable integrity layer that catches AI proxies and deepfakes live, without invading candidate privacy or punishing people with poor Wi-Fi.”**

Everything else is implementation detail.

---

# 33. Immediate Priority

The correct order now is:

```text
1. Re-verify the 28-assertion test pass (verify-all.ts)
        ↓
2. Lock the 5-step guided presentation flow (Normal → Challenge → Proxy → Low BW → Report)
        ↓
3. Ensure the risk spectrum bar and 4 signal meters animate smoothly
        ↓
4. Validate that generated forensic reports display the mandatory disclaimer
        ↓
5. Rehearse the 7–8 minute narration until seamless
```

Do not add new speculative features.

The strongest winning advantage is:

> **clarity + one rock-solid interactive demonstration + undeniable engineering depth**

---

# 34. Team Cheat Sheet

**Product**  
PRAMAAN — Interview Integrity Layer for Remote Hiring

**User**  
Technical recruiter / Hiring manager / Talent acquisition team

**Problem**  
Real-time AI face-swaps, proxy interviewers, and candidate impersonation

**Hero use case**  
Verifying Candidate #CX0104 for a Junior Frontend Engineer role

**Hero active defense**  
Dynamic live presence challenge (*“Turn your head right and say BLUE 47”*)

**Hero anomaly**  
Multimodal speech/facial divergence (lip-sync score drops to 38, risk jumps to 78)

**Hero fairness guarantee**  
Low bandwidth (< 45) clamps risk to 32 and status to `INSUFFICIENT EVIDENCE`

**Privacy guarantee**  
100% client-side landmark sensing; zero raw video/audio stored

**Final payoff**  
Sealed forensic audit report with mandatory human-decision disclaimer

**Final line**  
> **“A fake face can fool a frame. It cannot fool a live interaction.”**

---

# 35. Final Rule for the Team

Before touching any code or adding any feature, ask:

> **“Will this make the judge understand Pramaan faster, believe our integrity engine more, or remember our privacy-first architecture longer?”**

If the answer is no:

> **Do not build it now.**
