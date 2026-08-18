---
title: "MayaTune Listener MVP"
subtitle: "Product Requirements Document"
date: "Version 0.1 · August 17, 2026"
lang: en
---

> **Product statement**  
> MayaTune helps everyday listeners turn music into an experience that adapts to their preferences, activities, and listening moments—without requiring music-production knowledge.

| Attribute | Value |
|---|---|
| Status | Draft baseline—ready for cross-functional review |
| Scope | MayaTune Listener MVP |
| Launch use case | Workout Personalization |
| Next use case | Focus Personalization |
| Core capabilities | Adjust, Transform / Music Style Transfer, Create New |
| Intended readers | Product, Design, Engineering, AI/ML, Data, Rights, Security, QA |

**North Star Metric:** Minutes listened to personalized versions that users actively select again.

# 1. Document Control

## 1.1 Metadata

| Field | Content |
|---|---|
| Document name | MayaTune Listener MVP—Product Requirements Document |
| Version | 0.1 |
| Last updated | August 17, 2026 |
| Owner | MayaTune Product |
| Status | Draft baseline |
| Confidentiality | Internal project use |
| Related documents | `MayaTune_Project_Log_EN.docx`, `README.md` |
| Next step after the PRD | UX wireframes and a technical-feasibility spike for the audio pipeline |

## 1.2 Version History

| Version | Date | Content | Status |
|---|---|---|---|
| 0.1 | August 17, 2026 | Established the Listener MVP baseline; added Music Style Transfer, user stories, requirements, analytics, data model, rollout, and acceptance gates. | Current |

## 1.3 Review Roles

| Team | Review responsibility | Approval condition |
|---|---|---|
| Product | Scope, priorities, metrics, rollout | No conflict between goals, scope, and success criteria |
| Design | Flows, screens, states, accessibility | Everyday listeners can complete the flow without music-production terminology |
| Engineering | Architecture, APIs, data, operations | Work can be divided into epics and delivered by milestone |
| AI/ML & Audio | Analysis, generation, ranking, QC | Quality measurement and fallback methods are clearly defined |
| Rights & Trust | Source rights, derivative use, export, provenance | No audio-processing flow starts before the rights policy permits it |
| Security & Privacy | Storage, deletion, access control | Audio and listening-behavior data have controlled access, retention, and auditability |
| QA | Acceptance criteria and test matrix | P0 has testable criteria, test data, and expected outcomes |

## 1.4 Priority Convention

| Level | Meaning | Criteria |
|---|---|---|
| P0 | Required for Closed Beta / MVP | Without it, the core value, rights controls, or operability would fail |
| P1 | Should be included in the MVP or immediately after beta | Improves retention and completes the experience, but may roll out after P0 |
| P2 | Post-MVP roadmap | Not required to validate the Listener-first hypothesis |

## 1.5 Working Assumptions for PRD v0.1

The assumptions below give the team a baseline for design and estimation. They do not replace business, legal, or technical-spike decisions.

| ID | Assumption | Status |
|---|---|---|
| A-001 | The prototype and Closed Beta will use a mobile-first responsive web/PWA experience; native apps may follow. | To be confirmed |
| A-002 | The launch interface will prioritize Vietnamese, but the schema and UI must be i18n-ready. | To be confirmed |
| A-003 | The pilot catalog will contain approximately 50–200 tracks with clear personalization rights, prioritizing tracks with stems. | Partner-dependent |
| A-004 | The provisional first-playable-preview target is p50 ≤ 90 seconds and p95 ≤ 240 seconds. | Technical spike required |
| A-005 | Previews will use a lower-cost pipeline; high-quality rendering will run only after a user selects a version. | Proposed |
| A-006 | Workout is the first validation funnel; Focus will open after P0 is stable. | Locked |
| A-007 | Music Style Transfer is P0 only for tracks with derivative/transform rights, or user files for which the user holds equivalent rights. | Product decision locked; contract review required |

# 2. Executive Summary

## 2.1 Problem

Everyday listeners have access to a vast amount of music but little ability to make a song fit a specific situation. A favorite track may have an intro that is too long for a run, insufficient energy, distracting vocals while working, or a genre that does not match the current mood. Traditional music-production tools are too complex, while generative tools often begin with a blank prompt and do not deeply understand a user's established preferences.

MayaTune closes the gap between **passive listening** and **professional music production** through a simple, rights-aware control experience that learns from behavior over time.

## 2.2 Solution

The user selects a context, a valid music source, and describes the desired change through natural language or a preset. MayaTune analyzes the track, creates an inspectable Edit Recipe, generates three candidates, and enables A/B listening:

- **Familiar:** light changes that prioritize familiarity.
- **Personal:** optimized for the current request and the Preference Profile.
- **Explore:** more creative while still complying with preserve locks, rights, and quality gates.

The product supports three foundational capabilities:

| Capability | Purpose | Examples |
|---|---|---|
| **Adjust** | Modify attributes without materially changing the song's identity or genre. | Increase energy, shorten the intro, reduce vocals, increase bass, change duration. |
| **Transform** | Create a new arrangement or use-context version; includes **Music Style Transfer**. | Pop → Rock, Ballad → EDM, Hip-hop → Jazz, acoustic, instrumental, workout. |
| **Create New** | Create an independent work from a Preference Profile or abstract attributes. | Preserve mood and energy while creating a new melody, lyrics, hook, and structure. |

## 2.3 MVP Hypothesis

> If MayaTune can create three acceptable-quality previews from a licensed track within a reasonable wait time, and let listeners choose using understandable controls, users will save or replay at least one personalized version instead of generating only once out of curiosity.

## 2.4 Launch Wedge

**Workout Personalization** is the first use case because its needs translate into explicit parameters: BPM, energy curve, beat strength, intro length, chorus timing, duration, and transitions. P0 also includes Music Style Transfer for eligible tracks because it is the clearest expression of the value proposition that “the song adapts to you.”

## 2.5 Baseline Scope

| Item | Baseline v0.1 |
|---|---|
| User | Everyday listener; no music-production knowledge required |
| Audio sources | Licensed catalog; user-owned/authorized files; reference-only use for other sources |
| P0 context | Workout |
| P1 context | Focus |
| Outputs | Three previews, one selected version, a Personal Station, and controlled export when rights permit |
| Personalization | Pairwise onboarding + Preference Profile + behavior-based ranking |
| Style Transfer | Preserve locked elements and transform arrangement/genre for eligible sources |
| Creator Voice | Outside the Listener MVP scope |

# 3. Goals, Non-goals, and Success Measures

## 3.1 Product Goals

| ID | Goal | Expected outcome |
|---|---|---|
| G-001 | Help users reach “first personalized value” quickly | Complete onboarding, select a track, and hear the first preview in one session |
| G-002 | Make music controls easy to understand | Users complete the flow through context, presets, and natural language rather than DAW terminology |
| G-003 | Demonstrate the value of three candidates | Users can distinguish the candidates, select one, and briefly explain why they prefer it |
| G-004 | Validate Music Style Transfer | Produce a convincing genre transformation while preserving locked elements |
| G-005 | Learn context-specific preferences | Later candidate rankings outperform initial rankings within each context |
| G-006 | Operate rights by design | No generation or export exceeds recorded rights |
| G-007 | Build an extensible foundation | Recipe, job, analysis, rights, and adapter abstractions are general enough for Focus and the Creator roadmap |

## 3.2 User Outcomes

- Obtain a version better suited to the current activity.
- Use the product without understanding BPM, stems, or harmony.
- Know which elements the system will preserve and which it will change.
- Compare, undo, regenerate, and save a recipe.
- Receive accurate information about usage rights and export availability.
- Delete the source, versions, and related personalization data.

## 3.3 Non-goals

| ID | Not included in the MVP |
|---|---|
| NG-001 | Unlimited remixing of every commercial song on the market |
| NG-002 | Directly importing audio from streaming services for editing without rights |
| NG-003 | Copying lyrics, melody, hook, master recording, or voice from a reference-only source |
| NG-004 | “Exactly like artist X” prompts or imitation of another person's voice |
| NG-005 | A full DAW with a multitrack timeline, MIDI editor, and plugin ecosystem |
| NG-006 | A public remix social feed or one-click commercial release |
| NG-007 | A voice-model marketplace |
| NG-008 | Training a foundation model on user audio by default |
| NG-009 | Guaranteeing artistic success for every genre pair or input-file quality level |
| NG-010 | Requiring native iOS/Android for Closed Beta when responsive web can support testing |

## 3.4 Metric Tree

**North Star Metric:** Minutes listened to personalized versions that users actively select again.

| Layer | Metric |
|---|---|
| Acquisition | Users who start onboarding; traffic source; beta invitation cost |
| Activation | Onboarding completion; context selection; first generation; first playable preview |
| Value | Candidate-selection rate; listening to ≥ 70% of the selected version; station saves |
| Retention | Replay within 7/28 days; return to the same station; generation on a second track |
| Quality | Audio-artifact rate; regeneration rate; “too different” / “not different enough” feedback |
| Rights & Trust | Correctly blocked jobs; exports with provenance; completed deletion |
| Economics | GPU seconds/job; cost/accepted version; cost/personalized listening minute |

## 3.5 Provisional Success Thresholds

These thresholds are an alpha-design baseline and must be calibrated after the technical spike and user testing.

| Metric | Alpha/beta target | Gate type |
|---|---|---|
| Generation job technical success | ≥ 95% on the standard track set | Technical go/no-go |
| Rights decision has a snapshot and audit event | 100% of generation/export operations | Trust go/no-go |
| First preview latency | p50 ≤ 90 seconds; p95 ≤ 240 seconds | Provisional target |
| Users receiving previews who select at least one candidate | ≥ 50% | Product signal |
| Selecting users who listen to ≥ 70% of a version | ≥ 35% | Value signal |
| Save a station or replay within 7 days | ≥ 20% | Retention signal |
| Reports of severe artifacts | ≤ 10% of technically successful jobs | Quality gate |
| Deletion on request | 100% within the defined internal SLA | Privacy gate |

# 4. Users and Use Cases

## 4.1 Primary Persona—Everyday Active Listener

| Attribute | Description |
|---|---|
| Target age | Approximately 18–35; not a hard restriction |
| Behavior | Listens to music daily while exercising, commuting, studying, or working |
| Skill level | Does not know or does not want to use music-production tools |
| Need | Music that fits each activity without manually searching for playlists |
| Friction | Long intros, mismatched energy, too many vocals, inconsistent playlists |
| Trust criteria | Can preview changes, control transformation strength, understand rights, and delete data |

## 4.2 Secondary Persona—Focus Listener

A user who needs low-distraction music with stable energy and a duration suited to a work session. This persona uses the same foundation but prioritizes reduced vocals, smoother transitions, and less variability than Workout.

## 4.3 Job to Be Done

> When I am about to begin an activity, I want the music to adapt to my state and preferences so I do not have to search for or manually edit playlists.

### Supporting JTBDs

- When a favorite song does not fit the current situation, I want to transform it into a suitable version while preserving what I love.
- When I want to explore a new style, I want to hear a familiar song in another genre before creating or finding something completely new.
- When the system creates multiple options, I want to understand the differences and choose quickly without technical terminology.
- When using a personal file, I want to understand how it is stored, processed, and deleted.

## 4.4 Context Matrix

| Context | Goal | Default adjustments | Status |
|---|---|---|---|
| Workout | Increase motivation, provide a clear beat, build energy | Higher BPM/energy, shorter intro, stronger beat, earlier chorus | P0 |
| Focus | Reduce distraction and remain stable | Lower vocals, smooth transitions, less energy variation | P1 |
| Commute | Seamless, moderate energy | Stable loudness, moderate intro/outro | P2 |
| Relax | Gentle, low stimulation | Lower tempo/energy, softer instrumentation | P2 |
| Party | High energy, clear hooks | Strong beat/bass, short transitions | P2 |

## 4.5 Product-level User Stories

| ID | User story | Priority |
|---|---|---|
| US-001 | As a new user, I want to set preferences through A/B choices so I do not need to understand music terminology. | P0 |
| US-002 | I want to select Workout and an expected duration so MayaTune understands my listening goal. | P0 |
| US-003 | I want to select a track from a clearly licensed catalog and understand what I am allowed to do. | P0 |
| US-004 | I want to upload a file I am authorized to use and declare the rights type. | P0 |
| US-005 | I want to read an AI Music Brief before generation so I know how the system understands the song. | P0 |
| US-006 | I want to say “more powerful, shorter intro” and see the request translated into explicit changes. | P0 |
| US-007 | I want to transform a Pop song into Rock or a Ballad into EDM while preserving lyrics and the main melody when rights permit. | P0 |
| US-008 | I want to lock vocals, lyrics, melody, hook, or structure before transforming. | P0 |
| US-009 | I want to listen to Familiar, Personal, and Explore for comparison. | P0 |
| US-010 | I want to request “closer to the original,” “more powerful,” or “fewer vocals” without starting over. | P0 |
| US-011 | I want to save the version and recipe as a Workout Station. | P0 |
| US-012 | I want the system to prioritize better-fitting candidates based on prior behavior. | P1 |
| US-013 | I want to create a completely new song from my preferences when the source is reference-only. | P1 |
| US-014 | I want to export a file when rights permit and clearly understand usage limitations. | P0 |
| US-015 | I want to delete a source track, version, or all related data. | P0 |
| US-016 | I want a notification and recovery guidance when generation fails or rights are insufficient. | P0 |

# 5. MVP Scope and Release Strategy

## 5.1 Three Foundational Capabilities

### Adjust

Keep the song's identity and genre relatively stable while changing tempo, energy, intro/outro, vocal presence, bass, drums, duration, or structural emphasis.

### Transform—Including Music Style Transfer

Preserve user-selected elements that rights allow while changing arrangement, harmony treatment, groove, rhythm, instrumentation, sound design, production style, and potentially the genre.

### Create New

Create an independent work based on the Preference Profile, context, or abstract attributes. Create New does not preserve the lyrics, melody, hook, recording, or voice of a reference-only track.

## 5.2 Release Slices

| Slice | Scope | Objective |
|---|---|---|
| P0-A—Foundation | Account, onboarding, catalog, rights snapshot, analysis, playback | Establish a valid source and preference profile |
| P0-B—Workout Adjust | Prompt/preset, recipe, three candidates, compare, feedback | Validate the basic personalization flow |
| P0-C—Style Transfer | Target genre, preserve locks, transformation intensity, QC | Validate Transform as a differentiated value |
| P0-D—Save & Trust | Station, version history, controlled export, deletion, provenance | Complete the value and trust loops |
| P1-A—Focus | Focus preset, vocal reduction/instrumental, smoother transitions | Open the second context |
| P1-B—Create New | Original generation from profile/reference attributes | Unlock value when derivative rights are unavailable |
| P2—Adaptive Sessions | Multitrack session, continuous energy curve, native apps | Expand after MVP validation |

## 5.3 Audio Sources and Operation Rights

| Rights mode | Analyze | Adjust | Style Transfer | Create New from attributes | Export |
|---|---:|---:|---:|---:|---:|
| Licensed catalog—derivative allowed | Yes | Yes | Yes | Yes | Per license |
| User-owned / authorized upload | Yes | Yes | Yes, if the declaration supports it | Yes | Per declaration and policy |
| Reference-only | Required attributes only | No | No | Yes | Independent output only |
| Rights unknown / rejected | No or metadata-only | No | No | Do not use audio | No |

## 5.4 In Scope for P0

- Registration, sign-in, and guest-to-account conversion before save/export.
- Pairwise onboarding to initialize the Preference Profile.
- Catalog with clear rights badges.
- File upload with declaration and validation.
- Workout, duration, and intensity selection.
- Track analysis and AI Music Brief.
- Prompt/preset → inspectable and editable Edit Recipe.
- Basic Adjust controls.
- Music Style Transfer with target genre and preserve locks.
- Three candidates: Familiar, Personal, and Explore.
- A/B listening, quick feedback, regeneration, and version history.
- Personal Station.
- Controlled export when policy permits.
- Analytics, cost tracking, audit, deletion, and support diagnostics.

## 5.5 Out of Scope for P0

- Uploading a URL or ripping audio from streaming services.
- Public user-to-user sharing.
- Collaborative editing.
- Commercial-distribution workflow.
- Full multitrack DAW.
- Creator voice cloning.
- Real-time live style transfer.
- Guaranteed transformation for every genre pair.
- Offline on-device generation.

# 6. Core Experience

## 6.1 End-to-end Flow

```text
Create account / begin as guest
        ↓
Pairwise onboarding
        ↓
Select Workout + duration
        ↓
Select eligible track / upload / reference-only source
        ↓
Rights eligibility check
        ↓
Track Analysis + AI Music Brief
        ↓
Select Adjust or Transform
        ↓
Prompt/preset + preserve locks + intensity
        ↓
Preview Edit Recipe
        ↓
Generate Familiar / Personal / Explore
        ↓
A/B listening + feedback + regeneration
        ↓
Save version / Personal Station
        ↓
Controlled export or continued in-app listening
```

## 6.2 Onboarding

Onboarding must require few interactions and prioritize audio choices over forms. Users listen to short pairs and select the version that fits them better. The system creates an initial profile while allowing every question to be skipped.

**P0 dimensions:** energy, vocal presence, bass strength, intro tolerance, familiarity/novelty, and Workout BPM range.

**Fallback:** if the user skips everything, use a context-neutral profile and learn from the first selection.

## 6.3 Context Selection

The Context Picker presents Workout as the primary choice. The user selects:

- workout type;
- expected duration;
- desired energy;
- progressive or stable energy;
- transformation level: close to original, balanced, or exploratory.

## 6.4 Source Selection

Sources are divided into three tabs: **MayaTune Catalog**, **My Uploads**, and **Use as Reference**. Each track displays badges showing allowed functions: Adjust, Style Transfer, and Export.

Users cannot begin generation until the policy engine returns the current operation permissions.

## 6.5 AI Music Brief

The brief explains the track in everyday language:

> “This song has medium energy, runs at 96 BPM, features prominent vocals, and starts the chorus at 00:47. For Workout, MayaTune recommends an eight-second intro, progressively increasing energy, and a stronger beat.”

The brief must allow users to correct observations that directly affect the recipe without turning into an overly technical screen.

## 6.6 Edit Recipe Preview

Before generation, MayaTune displays a summary of:

- what will be preserved;
- what will change;
- target context/genre;
- creativity level;
- estimated duration;
- listening, saving, and export rights;
- quality warnings or limitations, if any.

## 6.7 Generation and Progress

Generation is an asynchronous job. The UI must:

- confirm receipt of the job;
- show the steps Analyze → Arrange → Render → Quality Check;
- allow users to leave the screen and return;
- play each candidate as soon as it is ready rather than waiting for all three;
- provide retry/fallback behavior when one candidate fails;
- avoid charging credits or quota for system errors under a future business policy.

## 6.8 Compare and Feedback

The Compare Player must support:

- switching among the three candidates at an equivalent timestamp;
- explaining differences in simple language;
- like/dislike;
- “closer to the original,” “more powerful,” “fewer vocals,” and “keep this chorus” actions;
- selecting a candidate as the base for the next iteration;
- listening to the source when rights permit;
- displaying a warning when preview quality is low.

## 6.9 Save and Personal Station

After selecting a version, the user can:

- save the version;
- name a station;
- save the recipe;
- apply the recipe to another eligible track;
- view version history;
- delete or restore items within the defined retention period.

# 7. Music Style Transfer—Core Specification

## 7.1 Standard Definition

**Music Style Transfer** preserves the locked elements of a licensed song—such as lyrics, main melody, vocal, hook, or structure—while recreating the arrangement, harmony, rhythm, groove, instrumentation, sound design, and production style to move the song into another genre or style.

Examples:

| Source | Target genre | Default preserve settings | Primary changes |
|---|---|---|---|
| Pop | Rock | Lyrics, melody, vocal, hook | Live drums, guitars, bass, rock dynamics |
| Ballad | EDM | Lyrics, melody, vocal, verse/chorus | 124 BPM, build-up, drop, synth bass, electronic drums |
| Hip-hop | Jazz | Lyrics/flow, vocal, structure | Swing groove, upright bass, piano/horns, controlled reharmonization |
| Acoustic | Lo-fi | Melody, vocal, structure | Dusty drums, warm keys, texture, lower energy |

## 7.2 Preserve Locks

| Lock | Meaning | P0 default |
|---|---|---|
| Lyrics Lock | Preserve lyric content and line order | On when rights permit |
| Main Melody Lock | Preserve the main vocal melody contour within tolerance | On |
| Vocal Lock | Preserve the original vocal recording or stem | On when stem, rights, and quality permit |
| Hook Lock | Preserve the licensed recognizable hook | On |
| Structure Lock | Preserve verse/chorus/bridge and their relative order | On |
| Harmony Lock | Preserve the progression; change only voicing/instrumentation | Off by default; user may enable |
| Duration Lock | Keep duration close to the source | Off; context may change duration |

Each lock must have one of these states: `available`, `unavailable`, `required_by_policy`, `selected`, or `not_selected`.

## 7.3 Transform Controls

- Target genre/style.
- Transform intensity: Light, Balanced, Strong.
- Target BPM or Auto for context.
- Energy curve.
- Instrumentation preference.
- Harmony change: low/medium/high, only when rights and locks permit.
- Vocal treatment: original, cleaned, background, instrumental—subject to rights.
- Structure adaptation: none, context-optimized, genre-optimized.
- Production era/mood expressed through abstract descriptors; artist names are not accepted as imitation targets.

## 7.4 Candidate Behavior

| Candidate | Style intensity | Preserve priority | Objective |
|---|---|---|---|
| Familiar | Light | Highest | Introduce the new genre while remaining highly familiar |
| Personal | Balanced | Balanced | Optimize for context and profile |
| Explore | Strong | Still follows locks | Clearly express the target genre and a new arrangement |

## 7.5 Eligibility and Rights Gating

Style Transfer is available only when:

1. `rights_state` permits derivative/arrangement creation;
2. composition, lyrics, master/vocal, and export rights are resolved for the operation;
3. a user upload has an appropriate declaration and is not under a policy hold;
4. the target use does not exceed the license scope;
5. the rights snapshot is stored with the generation job.

If only composition rights are available but master/vocal rights are not, the system must disable Vocal Lock and explain the alternatives. If the source is reference-only, Style Transfer must not appear as an available action; the system transitions to Create New.

## 7.6 Minimum Recipe Schema

```json
{
  "operation": "style_transfer",
  "source_track_id": "track_123",
  "rights_snapshot_id": "rights_456",
  "context": "workout",
  "source_style": "pop_ballad",
  "target_style": "edm",
  "transform_intensity": 0.65,
  "preserve": {
    "lyrics": true,
    "main_melody": true,
    "vocal": true,
    "hook": true,
    "structure": true,
    "harmony": false
  },
  "target_bpm": 124,
  "energy_curve": "progressive",
  "instrumentation": ["electronic_drums", "synth_bass", "pads"],
  "structure_adaptation": "context_optimized",
  "candidate_profiles": ["familiar", "personal", "explore"]
}
```

## 7.7 Quality Rubric

Each Style Transfer candidate is evaluated across these dimensions:

| Dimension | Evaluation question |
|---|---|
| Lock adherence | Are lyrics, melody, vocal, hook, and structure preserved within defined tolerances? |
| Genre recognizability | Can listeners identify the target genre without seeing its label? |
| Musical coherence | Do the harmony, rhythm, arrangement, and transitions sound natural? |
| Audio quality | Are there severe clipping, phasing, warble, timing drift, or vocal artifacts? |
| Context fit | Does the version fit Workout/Focus and the requested duration? |
| Personal fit | Does candidate ranking reflect the profile and feedback? |
| Rights compliance | Are the operation and output within the rights snapshot? |

A candidate below the quality threshold is not displayed as “complete”; the system retries, changes strategy, or returns an explained partial-success state.

## 7.8 Style Transfer Acceptance Criteria

| ID | Acceptance criterion |
|---|---|
| ST-AC-001 | Users see Style Transfer as enabled only when the rights policy permits the operation. |
| ST-AC-002 | The UI clearly displays target genre, transformation intensity, and every preserve lock before generation. |
| ST-AC-003 | The recipe accurately stores lock state, rights snapshot, and model/pipeline version. |
| ST-AC-004 | On the licensed test set, ≥ 90% of displayed candidates do not violate mandatory locks. |
| ST-AC-005 | At least two of three candidates must pass the quality threshold, or the job is marked partial/failed. |
| ST-AC-006 | The A/B player switches candidates without losing the listening position beyond the UX tolerance. |
| ST-AC-007 | Users can regenerate from one candidate and change exactly one control without losing the other locks. |
| ST-AC-008 | Export is blocked when the current rights state does not permit it, even if generation completed. |
| ST-AC-009 | A reference-only source cannot enter Style Transfer; the flow recommends Create New. |
| ST-AC-010 | The system records quality scores, user feedback, cost, and latency for each candidate. |

## 7.9 Edge Cases and Fallbacks

- **No vocal stem:** disable Vocal Lock or use a mixed-vocal preservation strategy if policy and quality permit.
- **Target tempo distorts the vocal:** constrain target BPM, use a tempo map, or recommend staying closer to the source BPM.
- **Unsupported genre pair:** display only genres with model capability; do not accept the request and fail silently.
- **Lyrics alignment failure:** block the candidate when word-timing drift exceeds the threshold.
- **One candidate fails:** play ready candidates and allow retry of the missing one.
- **Rights change between generation and export:** export re-checks the current rights state rather than relying only on the old snapshot.
- **User requests an artist name:** translate the request into neutral descriptors or ask the user to choose sonic attributes; do not imitate the artist.

# 8. Functional Requirements

## 8.1 Epic A—Account and Onboarding

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-ONB-001 | Allow a guest to begin onboarding and listen to samples. | P0 | The guest has a session ID; data transfers to the account after registration. |
| FR-ONB-002 | Support registration/sign-in and account recovery. | P0 | Users can access their library and jobs on another device. |
| FR-ONB-003 | Provide pairwise onboarding across at least four dimensions. | P0 | Every question includes A/B audio, skip, and replay. |
| FR-ONB-004 | Generate an initial Preference Profile. | P0 | The profile includes a version, source signals, and confidence. |
| FR-ONB-005 | Allow users to edit or reset preferences. | P0 | Reset does not delete the library and creates an audit event. |
| FR-ONB-006 | Display consent for necessary analytics and personalization. | P0 | Consent state is stored and can be changed. |
| FR-ONB-007 | Do not require highly detailed genre selection to finish. | P0 | A neutral/default path is available. |
| FR-ONB-008 | Support context-specific re-onboarding. | P1 | Workout and Focus have separate subprofiles. |

## 8.2 Epic B—Catalog, Upload, and Rights

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-LIB-001 | Display operation-specific rights badges in the catalog. | P0 | The track card indicates Adjust, Transform, and Export availability. |
| FR-LIB-002 | Search/filter by genre, mood, BPM range, and rights. | P0 | Results update and retain filter state. |
| FR-LIB-003 | Upload supported audio formats. | P0 | Validate type, size, duration, corruption, and malware policy. |
| FR-LIB-004 | Collect a rights declaration before advanced analysis. | P0 | No job is created when the declaration is missing. |
| FR-LIB-005 | Store a rights snapshot for every generation. | P0 | The snapshot is immutable and linked to the job/output. |
| FR-LIB-006 | Support reference-only mode. | P1 | Extract only necessary attributes and do not enable Adjust/Transform. |
| FR-LIB-007 | Allow deletion of source and derived assets. | P0 | The UI explains the impact; the backend follows the retention policy. |
| FR-LIB-008 | Re-check rights before export. | P0 | A denied export includes a reason code and recovery guidance. |

## 8.3 Epic C—Context and Workout Setup

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-CTX-001 | Prioritize Workout as the default context. | P0 | Home has a clear entry point. |
| FR-CTX-002 | Let the user select a target duration. | P0 | The recipe receives a duration or session goal. |
| FR-CTX-003 | Select an energy curve: steady/progressive/interval. | P0 | The preview summary reflects the choice. |
| FR-CTX-004 | Select a familiarity level. | P0 | The value maps to candidate ranking/creativity. |
| FR-CTX-005 | Save context settings as a personal preset. | P1 | The preset can be renamed, edited, and deleted. |
| FR-CTX-006 | Use the same framework for the Focus context. | P1 | Vocal and distraction controls are available. |

## 8.4 Epic D—Track Analysis and AI Music Brief

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-AN-001 | Analyze BPM, key, loudness, sections, energy, and instrumentation. | P0 | Analysis includes confidence and pipeline version. |
| FR-AN-002 | Separate stems when the operation requires them and capability allows. | P0 | Stem status is stored; stems are not assumed to be available. |
| FR-AN-003 | Generate a waveform and section markers. | P0 | The player displays markers consistently. |
| FR-AN-004 | Generate an understandable AI Music Brief. | P0 | The brief avoids unexplained terminology. |
| FR-AN-005 | Allow users to report incorrect analysis. | P1 | Feedback links to the relevant analysis field. |
| FR-AN-006 | Cache analysis by source fingerprint. | P0 | Unnecessary reruns are avoided while rights are still evaluated separately. |

## 8.5 Epic E—Intent and Edit Recipe

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-INT-001 | Accept Vietnamese prompts and presets. | P0 | The intent parser returns a structured recipe or a clarification UI. |
| FR-INT-002 | Display a recipe summary before generation. | P0 | Users see preserved/changed elements, context, and rights. |
| FR-INT-003 | Allow controls to be edited without editing JSON. | P0 | The UI updates the recipe deterministically. |
| FR-INT-004 | Validate the recipe against capability and rights. | P0 | Invalid fields include a reason and suggestion. |
| FR-INT-005 | Version the recipe after each regeneration. | P0 | `parent_recipe_id` and a diff are stored. |
| FR-INT-006 | Save a user-friendly label for the recipe. | P1 | The recipe appears in Station/History. |

## 8.6 Epic F—Adjust

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-ADJ-001 | Adjust energy, intro, duration, and drum intensity. | P0 | Output remains within the supported range and recipe. |
| FR-ADJ-002 | Adjust vocal presence when stems/capability allow. | P0 | An unavailable control has an explained disabled state. |
| FR-ADJ-003 | Adjust tempo within a quality-safe range. | P0 | The UI warns or clamps the range. |
| FR-ADJ-004 | Preserve source identity under the Familiar profile. | P0 | The quality rubric includes an identity score. |
| FR-ADJ-005 | Support an instrumental version when rights and stems allow. | P1 | Vocal residue does not exceed the defined threshold. |

## 8.7 Epic G—Transform / Style Transfer

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-TR-001 | Select a target genre from a capability-supported list. | P0 | The list depends on source, capability, and rights. |
| FR-TR-002 | Display and manage Preserve Locks. | P0 | Complete lock state persists through regeneration. |
| FR-TR-003 | Select transformation intensity. | P0 | Familiar/Personal/Explore mappings are explicit. |
| FR-TR-004 | Select or automatically choose target BPM/energy. | P0 | The recipe and summary display the values. |
| FR-TR-005 | Generate three candidates with different arrangements. | P0 | Candidate IDs, lineage, and scores are stored. |
| FR-TR-006 | Block unsupported target styles. | P0 | Do not create doomed-to-fail jobs; offer an alternative. |
| FR-TR-007 | Check lock adherence and audio quality. | P0 | A failed candidate is not presented as a success. |
| FR-TR-008 | Re-check rights before save/share/export. | P0 | The policy decision includes a timestamp and reason. |
| FR-TR-009 | Support iteration based on a candidate. | P0 | The child version retains lineage and selected locks. |
| FR-TR-010 | Collect genre-recognition feedback. | P1 | Users can select “does not sound like the target style.” |

## 8.8 Epic H—Create New

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-NEW-001 | Create an independent song from Preference Profile + context. | P1 | Output does not use protected components from a reference-only source. |
| FR-NEW-002 | Allow reference attributes such as mood/BPM/energy. | P1 | The UI explains that the source is used only to understand attributes. |
| FR-NEW-003 | Run similarity safeguards. | P1 | A candidate above the threshold is regenerated or reviewed. |
| FR-NEW-004 | Record provenance for prompt/profile/model. | P1 | Metadata is attached to the output. |

## 8.9 Epic I—Generation Jobs and Quality Control

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-GEN-001 | Run jobs asynchronously and make them resumable. | P0 | Refreshing or changing screens does not lose state. |
| FR-GEN-002 | Play a candidate as soon as it is ready. | P0 | Partial results have a clear status. |
| FR-GEN-003 | Show progress stages and an ETA range without promising exact timing. | P0 | The UI does not appear frozen while a worker retries. |
| FR-GEN-004 | Retry based on classified errors. | P0 | Retries are bounded and a dead-letter path exists. |
| FR-GEN-005 | Run quality checks for clipping, loudness, timing, artifacts, and lock adherence. | P0 | Scores and failure reasons are stored. |
| FR-GEN-006 | Record cost telemetry by stage/model/candidate. | P0 | An internal cost dashboard is available. |
| FR-GEN-007 | Use a Generation Adapter to separate provider/model from the product API. | P0 | The adapter has contract tests. |
| FR-GEN-008 | Support cancellation when the current stage allows it. | P1 | UI and billing/quota state remain accurate. |

## 8.10 Epic J—Compare, Feedback, and Personalization

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-CMP-001 | A/B switch among candidates. | P0 | Playback position maps within tolerance. |
| FR-CMP-002 | Explain primary differences in understandable language. | P0 | Do not show only technical parameters. |
| FR-CMP-003 | Collect like/dislike and quick refinements. | P0 | The event includes candidate, recipe, and context. |
| FR-CMP-004 | Select a candidate as the accepted version. | P0 | Job outcome and profile signal are updated. |
| FR-CMP-005 | Rank using Preference Profile and quality score. | P1 | A quality-failed candidate cannot rank first. |
| FR-CMP-006 | Allow undo of the latest feedback. | P1 | The profile signal is reversed correctly. |
| FR-CMP-007 | Briefly explain “why this version is recommended.” | P1 | Use signal categories without exposing sensitive data. |

## 8.11 Epic K—Playback, Save, Station, and Export

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-OUT-001 | Provide streaming playback for previews and accepted versions. | P0 | Signed URLs/tokens expire. |
| FR-OUT-002 | Save the accepted version and recipe. | P0 | The version appears in Library. |
| FR-OUT-003 | Create a Personal Station from context + recipe. | P0 | The station has a name, context, and default settings. |
| FR-OUT-004 | Maintain version history and lineage. | P0 | Users can view source → recipe → candidate → accepted version. |
| FR-OUT-005 | Provide rights-controlled export. | P0 | The exported file contains available provenance metadata. |
| FR-OUT-006 | Display usage terms before download. | P0 | Users acknowledge them when policy requires. |
| FR-OUT-007 | Share by private link. | P2 | Implement only after rights review. |

## 8.12 Epic L—Privacy, Settings, and Support

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-PRI-001 | Allow users to view/delete a source, version, station, and profile. | P0 | Deletion requires confirmation and creates an audit record. |
| FR-PRI-002 | Do not use uploads to train a shared model by default. | P0 | Training consent is a separate choice. |
| FR-PRI-003 | Export available account data. | P1 | A job runs and completion is communicated. |
| FR-PRI-004 | Provide a support diagnostic ID for failed jobs. | P0 | Users can copy the ID without exposing a secret. |
| FR-PRI-005 | Provide preference privacy controls. | P1 | Users can disable selected behavioral signals. |
| FR-PRI-006 | Support account deletion. | P0 | A deletion workflow and valid retention exceptions are implemented. |

# 9. UX and Screens

## 9.1 Information Architecture

```text
Home
├── Start a Workout Mix
├── Continue Station
├── Library
│   ├── Catalog
│   ├── My Uploads
│   └── Saved Versions
├── Create
│   ├── Adjust
│   ├── Change Style
│   └── Create New (P1)
├── Jobs / Activity
└── Profile & Privacy
```

## 9.2 Screen Inventory

| ID | Screen | Objective | Required states |
|---|---|---|---|
| S-01 | Welcome / Auth | Start quickly as guest or account user | default, loading, auth error |
| S-02 | Pairwise Onboarding | Initialize the profile | audio loading, skip, complete |
| S-03 | Home | Entry to Workout and recent items | empty, returning, job in progress |
| S-04 | Context Setup | Select duration/energy/familiarity | valid, incomplete, preset loaded |
| S-05 | Source Picker | Catalog/upload/reference | search, filter, no rights, upload error |
| S-06 | Track Detail & Rights | Understand the track and available operations | rights allowed/limited/blocked |
| S-07 | AI Music Brief | Confirm analysis | ready, low confidence, analysis failed |
| S-08 | Adjust / Change Style Studio | Select controls and locks | capability disabled, warning, valid |
| S-09 | Recipe Review | Review preserved/changed elements and rights | ready, policy conflict, cost/quota warning |
| S-10 | Generation Progress | Monitor the job | queued, running, partial, failed, canceled |
| S-11 | Compare Player | Listen to and select candidates | 1/2/3 ready, quality warning, source unavailable |
| S-12 | Refinement Sheet | Quick feedback/regeneration | valid, conflicting control |
| S-13 | Save to Station | Save recipe/version | new, existing station, naming conflict |
| S-14 | Library / Version History | Manage assets | empty, processing, expired preview |
| S-15 | Export & Rights | Download according to rights | allowed, limited, denied, rights changed |
| S-16 | Privacy & Data | Consent, deletion, data export | confirmation, pending deletion |

## 9.3 UX Principles

- **Context first:** ask what the user wants to do before asking for parameters.
- **Plain language:** use “more powerful,” “get to the song faster,” and “keep the vocals” rather than only “transient,” “LUFS,” and “stem.”
- **Progressive disclosure:** place technical sliders behind advanced controls.
- **Explain constraints:** every disabled control has a reason and an alternative.
- **Trust before action:** show rights and preserve summaries before generation/export.
- **Compare, do not guess:** provide multiple candidates whenever cost permits.
- **Recoverable:** every refinement creates a new version and does not destroy the selected version.

## 9.4 Accessibility

- Every control has a label, value, and keyboard navigation.
- Color is not the only signal for rights/status.
- Transcripts/lyrics are shown when rights permit but are not required to operate audio controls.
- The player has a focus state, documented shortcuts, and touch targets suitable for mobile.
- Motion/progress animation respects the reduced-motion preference.
- Error copy includes a recovery action, not only an error code.
- Contrast and text scaling meet the product's target accessibility standard.

## 9.5 Copy Guidelines

| Avoid | Use |
|---|---|
| “Stem separation failed” | “MayaTune could not isolate the vocal cleanly enough for this option.” |
| “Derivative rights missing” | “This song is not currently licensed for genre transformation.” |
| “Similarity threshold exceeded” | “The new version is still too close to a reference; MayaTune is creating another option.” |
| “Generation error” | “This version could not be completed. The other two versions may still be available.” |

# 10. AI/ML and Audio Requirements

## 10.1 Pipeline Logic

```text
Ingest
  → Rights pre-check
  → Audio normalization & fingerprint
  → Analysis / stem capability
  → Intent-to-Recipe
  → Capability + rights validation
  → Candidate planning
  → Generation / transformation
  → Mixing & mastering
  → Quality + lock adherence + similarity checks
  → Ranking
  → Preview delivery
  → Feedback learning
```

## 10.2 Analysis Outputs

| Output | Purpose | Requirement |
|---|---|---|
| BPM / beat grid | Tempo, synchronization, workout mapping | Include confidence and downbeat alignment |
| Key / harmony | Transposition, arrangement | Do not present as absolute truth when confidence is low |
| Structure | Intro, verse, chorus, bridge, outro | Time ranges, confidence, and a user-correction path |
| Energy curve | Context adaptation | Normalize by track and catalog |
| Vocal/instrumentation | Locks and controls | Include capability state; do not assume perfect stems |
| Loudness / peaks | QC and consistent playback | Store measurement version |
| Embedding/fingerprint | Search, cache, safeguards | Access-controlled and purpose-limited |

## 10.3 Intent-to-Recipe

The parser must:

1. accept natural Vietnamese and presets;
2. extract context, preservation intent, target style, intensity, duration, and constraints;
3. never activate an operation that conflicts with rights;
4. return confidence and a list of ambiguities;
5. prefer a UI choice over a long conversation when one field is missing;
6. store the input, normalized intent, and recipe version for controlled debugging.

## 10.4 Candidate Planning

The candidate planner must do more than vary a single creativity parameter. Each profile has a strategy:

- **Familiar:** high lock adherence, only enough arrangement change to communicate the result, tempo close to the source.
- **Personal:** optimized for context/profile, allowing moderate structural adaptation.
- **Explore:** clear target genre and stronger instrumentation change without violating required locks.

## 10.5 Ranking

The ranking score may combine:

```text
candidate_score =
  quality_score
  × rights_validity
  + context_fit
  + preference_fit
  + requested_change_adherence
  + diversity_bonus
  - artifact_penalty
  - policy_risk
```

`rights_validity` is a hard gate, not a weight that can be offset. An invalid candidate is never ranked for the user.

## 10.6 Quality Gates

P0 quality checks:

- clipping/peak;
- loudness range;
- severe separation artifact;
- vocal timing drift;
- beat/grid discontinuity;
- empty/silent output;
- lock adherence;
- duration tolerance;
- target-genre-recognizability proxy;
- corrupted file;
- provenance completeness.

## 10.7 Model/Provider Abstraction

Minimum Generation Adapter:

```text
analyze(track, operation_context)
separate_stems(track, requested_stems)
plan_candidates(analysis, recipe, profile)
generate_variant(track, recipe, candidate_profile)
master(version, target_profile)
score_quality(source, version, recipe)
score_similarity(reference_set, version)
```

Every adapter must return capability metadata, latency, cost, model version, failure reason, and reproducibility fields to the degree supported by the provider.

## 10.8 Feedback Learning

- P0: rule-based profile updates and contextual ranking.
- P1: learning-to-rank after sufficient data and consent.
- Do not allow a single skip to change the profile substantially.
- Separate global preferences from context-specific preferences.
- Use decay or confidence so old preferences do not permanently constrain the experience.

# 11. Rights, Safety, and Privacy

## 11.1 Rights by Design

Every operation must pass through the Rights Policy Engine before generation and again before export. A policy decision includes:

- source asset;
- user/account;
- operation;
- target use;
- territory/time, when applicable;
- composition/master/lyrics/vocal permissions;
- output restrictions;
- decision, reason, and policy version.

## 11.2 User Uploads

The upload flow must collect a clear declaration rather than using one ambiguous checkbox. At minimum:

- “I own or control the necessary rights.”
- “I have a license that permits editing/derivative creation.”
- “I only want to use this as an attribute reference.”
- “I am not sure” → do not enable a high-risk generation operation.

A declaration does not automatically prove rights. The system may place the asset on hold, restrict export, or request additional documentation according to policy.

## 11.3 Reference-only Safeguards

- Extract only the attributes required and permitted by policy.
- Do not store or reuse identifiable components beyond the disclosed purpose.
- Create New must generate independent lyrics, melody, hook, and arrangement.
- Run similarity checks and provide a review path for risky candidates.
- Do not label output as a “remix” of a reference-only song.

## 11.4 Prohibited Requests

- Imitating another person's or a famous artist's voice.
- Creating a version “exactly like” a specific artist.
- Preserving protected components when rights do not permit it.
- Using output to mislead people into believing it is an official release.
- Bypassing provenance or rights restrictions.
- Uploading illegal or rights-infringing content under the applicable policy.

## 11.5 Provenance

Every output stores at least:

- source asset/fingerprint;
- recipe and lineage;
- rights snapshot/policy version;
- model/pipeline version;
- timestamp;
- user/workspace;
- quality scores;
- AI-assisted disclosure state;
- export restrictions.

## 11.6 Data Handling

- Encrypt data in transit and at rest.
- Provide object access through short-lived signed URLs/tokens.
- Separate access to production audio from ordinary administrative permissions.
- Audit sensitive access.
- Do not train shared models on uploads by default.
- Maintain a deletion workflow, retention schedule, and exception records.
- Bind behavioral data to the disclosed personalization/analytics purpose and applicable consent.

## 11.7 Trust UX

Users must know:

- why a function is locked;
- which file is being processed;
- how output may be listened to, saved, or exported;
- which data improves personalization;
- how to delete data or report a rights issue.

# 12. Data Model

## 12.1 Core Entities

| Entity | Role | Important fields |
|---|---|---|
| User | Account owner | id, locale, consent_state, status |
| PreferenceProfile | Global/context preferences | dimensions, confidence, version, signals |
| Track | Logical content unit | title, artist/owner metadata, source_type |
| SourceAsset | Original audio file | storage_key, fingerprint, duration, format |
| RightsDeclaration | User/partner declaration | basis, scope, evidence_state |
| RightsSnapshot | Rights at operation time | operation, decision, restrictions, policy_version |
| TrackAnalysis | Analysis results | bpm, key, sections, energy, stems, confidence |
| EditRecipe | Structured intent | operation, context, locks, controls, version |
| GenerationJob | Asynchronous process | state, stages, model, cost, errors |
| CandidateVersion | Each Familiar/Personal/Explore result | profile, asset, quality scores, lineage |
| Feedback | User signal | action, candidate, context, timestamp |
| PersonalStation | Saved recipe/context | name, defaults, accepted_version |
| Export | Exported file and rights | format, policy decision, provenance, status |
| AuditEvent | Trust/operations trail | actor, action, object, reason, timestamp |

## 12.2 Relationship Summary

```text
User 1──N PreferenceProfile
User 1──N Track / PersonalStation
Track 1──N SourceAsset / TrackAnalysis / EditRecipe
EditRecipe 1──N GenerationJob
GenerationJob 1──N CandidateVersion
CandidateVersion 1──N Feedback / Export
RightsSnapshot N──1 Track or SourceAsset
Every sensitive operation 1──N AuditEvent
```

## 12.3 Job State Machine

```text
created
  → rights_check
  → queued
  → analyzing
  → planning
  → generating
  → rendering
  → quality_check
  → partial_ready / ready
  → accepted / archived

Any active state → cancel_requested → canceled
Any active state → retryable_failed → queued
Any active state → terminal_failed
```

## 12.4 Rights State Examples

```text
allowed
allowed_with_restrictions
reference_only
pending_review
expired
revoked
denied
```

## 12.5 P0 Preference Dimensions

| Dimension | Global | Context-specific | Example signals |
|---|---:|---:|---|
| Energy | Yes | Yes | A/B choice, selected candidate |
| BPM range | Optional | Yes | Workout setup, completion |
| Vocal presence | Yes | Yes | slider, “fewer vocals,” skip |
| Bass/drum intensity | Yes | Yes | pairwise choice, accepted version |
| Intro tolerance | Yes | Yes | “chorus sooner,” seeking behavior |
| Novelty tolerance | Yes | Yes | Familiar vs. Explore choice |
| Preferred duration | No | Yes | session goal, replay |

# 13. API and Service Contracts

## 13.1 Endpoint Baseline

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/v1/tracks` | Create a track/upload session |
| POST | `/v1/tracks/{id}/rights-declarations` | Declare rights |
| POST | `/v1/tracks/{id}/analyze` | Start/reuse analysis |
| GET | `/v1/tracks/{id}` | Retrieve track, capability, and rights summary |
| POST | `/v1/recipes` | Create/validate an Edit Recipe |
| POST | `/v1/generation-jobs` | Create a job and three candidate profiles |
| GET | `/v1/generation-jobs/{id}` | Retrieve progress, candidates, and errors |
| POST | `/v1/candidates/{id}/feedback` | Like/refine/accept |
| POST | `/v1/candidates/{id}/iterate` | Create a child recipe/job |
| POST | `/v1/stations` | Save a Personal Station |
| GET | `/v1/users/me/preferences` | Read the aggregated profile |
| PATCH | `/v1/users/me/preferences` | Edit/reset dimensions |
| POST | `/v1/exports` | Controlled export |
| DELETE | `/v1/assets/{id}` | Delete an asset according to policy |

## 13.2 API Principles

- Use an idempotency key when creating a job/export.
- Return a job ID immediately for asynchronous operations.
- Errors include `code`, `user_message`, `retryable`, and `support_id`.
- Never return long-lived signed URLs.
- Version the API and recipe schema.
- The client cannot override a rights decision.
- Candidate metadata must not unnecessarily expose internal safety thresholds.

## 13.3 Minimum Error Taxonomy

| Code | Meaning | UX |
|---|---|---|
| `RIGHTS_OPERATION_DENIED` | Operation is not permitted | Explain and recommend Create New/reference mode |
| `CAPABILITY_UNAVAILABLE` | Model/pipeline does not support the request | Recommend another style/control |
| `SOURCE_AUDIO_INVALID` | File is corrupt or unsupported | Guide the user to upload again |
| `QUALITY_GATE_FAILED` | Candidate did not meet quality requirements | Retry or return a partial result |
| `JOB_CAPACITY_DELAY` | Queue is overloaded | Show status without losing the job |
| `EXPORT_RECHECK_FAILED` | Rights changed or a restriction applies | Block download and state the next step |

# 14. Analytics and Experimentation

## 14.1 Event Taxonomy

| Event | Trigger | Primary properties |
|---|---|---|
| `onboarding_started` | Onboarding begins | source, locale, guest/account |
| `preference_pair_answered` | A/B/skip selection | dimension, choice, latency |
| `onboarding_completed` | Profile is created | answered_count, profile_confidence |
| `context_selected` | Workout/Focus selected | context, duration, energy_curve |
| `source_selected` | Track selected | source_type, rights_mode, capabilities |
| `upload_completed` | Upload succeeds | format, duration_bucket, declaration_type |
| `rights_decision_made` | Policy returns a decision | operation, decision, policy_version |
| `analysis_completed` | Analysis is ready | latency, cache_hit, confidence |
| `music_brief_viewed` | Brief viewed | fields, low_confidence_flags |
| `operation_selected` | Adjust/Transform/Create New selected | operation |
| `preserve_lock_changed` | Lock toggled | lock, state, reason |
| `target_style_selected` | Style selected | source_style, target_style |
| `recipe_validated` | Recipe succeeds/fails validation | errors, warnings, operation |
| `generation_started` | Job created | job_id, candidate_profiles, model_route |
| `candidate_ready` | Each candidate becomes ready | profile, latency, quality_score, cost |
| `generation_completed` | Job ready/partial/failed | outcome, candidate_count, total_cost |
| `candidate_played` | Candidate played | profile, start_position |
| `candidate_switch` | A/B switch | from, to, position_delta |
| `candidate_feedback` | Like/refinement | action, reason, profile |
| `candidate_accepted` | Version selected | profile, listen_ratio, iteration_count |
| `station_saved` | Station saved | context, recipe_version |
| `version_replayed` | Version replayed | days_since_create, source_surface |
| `export_requested` | Export requested | format, rights_state |
| `export_completed` | Export completes/is blocked | outcome, reason |
| `asset_deleted` | Data deleted | asset_type, cascade_count |

## 14.2 Funnels

### Activation Funnel

```text
Onboarding started
→ Onboarding completed
→ Context selected
→ Source selected
→ Recipe validated
→ First candidate ready
→ Candidate accepted
```

### Retention Funnel

```text
Candidate accepted
→ Station saved
→ Version/station replayed within 7 days
→ Second track personalized
→ Context-specific repeat usage
```

### Style Transfer Funnel

```text
Transform selected
→ Target style selected
→ Preserve locks confirmed
→ Rights allowed
→ Candidate ready
→ Target style recognized
→ Candidate accepted
```

## 14.3 Dashboards

- Product activation and retention.
- Style Transfer quality by genre pair.
- Generation latency and failure by stage/model.
- Cost per accepted version.
- Rights decisions and export-denial reasons.
- Artifact reports and regeneration behavior.
- Preference cold start and ranking lift.

## 14.4 Experiment Backlog

| ID | Experiment | Primary metric |
|---|---|---|
| EXP-001 | Pairwise onboarding with 4 vs. 7 questions | Completion + first-candidate acceptance |
| EXP-002 | Show all 3 candidates together vs. progressive reveal | Time to first play + acceptance |
| EXP-003 | Different default preserve-lock sets | Lock violations + acceptance |
| EXP-004 | Target-genre cards vs. text prompt | Successful recipe validation |
| EXP-005 | Play Familiar first vs. Personal first | Accepted profile + replay |
| EXP-006 | Progress-stage copy | Abandonment during generation |
| EXP-007 | “Why this version” explanation | Trust rating + selection confidence |

# 15. Non-functional Requirements

## 15.1 Performance

| Requirement | v0.1 target |
|---|---|
| Standard API p95 excluding generation | ≤ 800 ms under target beta conditions |
| Playback start p95 | ≤ 2.5 seconds on the target connection |
| First candidate preview | p50 ≤ 90 seconds; p95 ≤ 240 seconds, provisional |
| UI progress freshness | Update within ≤ 5 seconds after a state change |
| Upload resume | Support large files through a multipart/resumable strategy |

## 15.2 Reliability

- Generation jobs persist through client refresh.
- Create operations are idempotent.
- Candidate-level partial success is supported.
- A dead-letter queue and replay tooling are available.
- Metadata is backed up; object durability follows the selected infrastructure.
- Runbooks exist for queue overload, provider outage, storage issues, and rights-service degradation.

## 15.3 Security

- Strong authentication and session controls.
- Least-privilege service identities.
- Encryption at rest and in transit.
- No secrets in clients or logs.
- Short-lived signed URLs.
- Malware/file validation for uploads.
- Rate limiting and abuse detection.
- Audit of administrative access and any policy override.

## 15.4 Privacy

- Data minimization.
- Consent versioning.
- User deletion/data-export workflows.
- Retention schedules by asset type.
- Do not store raw prompts/audio in telemetry logs unless necessary.
- Redact identifiers from broadly accessible diagnostics.

## 15.5 Scalability and Cost

- Separate queues for analysis, generation, rendering, and export.
- Autoscale workers by workload class.
- Cache analysis by fingerprint.
- Use lower-cost previews before high-quality rendering.
- Apply a budget guard per job/account.
- Route provider/model by cost, quality, and latency.

## 15.6 Observability

Every job has an end-to-end trace: API → rights → queue → model → render → QC → storage → player. Minimum metrics include stage latency, queue wait, retries, GPU time, cost, error code, quality score, and acceptance outcome.

## 15.7 Internationalization

- Separate UI strings into resources.
- Recipe enums are independent of display language.
- The prompt parser supports Vietnamese in P0, with a clear fallback for other languages.
- Dates, times, numbers, and units follow locale.

# 16. Rollout, QA, and Launch Gates

## 16.1 Phases

| Phase | Audience | Scope | Exit criteria |
|---|---|---|---|
| Technical Prototype | Internal | 10–20 tracks, 2–3 genre pairs | Pipeline produces output and measures quality/cost/latency |
| Internal Alpha | Project team + testers | P0 flow, 50+ tracks | Job success ≥ 95%, 100% rights audit coverage, critical bugs closed |
| Closed Beta | Invited listener cohort | Workout + Style Transfer | Product signals reach thresholds or produce clear learning |
| Public MVP | Selected market | Expanded catalog, support, privacy operations | Launch checklist and legal/security review complete |

## 16.2 Test Dataset

The test set must have clear rights and cover:

- male/female/group vocals;
- slow/medium/fast tempo;
- tracks with and without stems;
- multiple verse/chorus structures;
- P0 genre pairs;
- low-quality files and edge formats;
- short/long tracks;
- language variation;
- rights states: allowed/restricted/reference-only/revoked.

## 16.3 QA Matrix

| Layer | Tests |
|---|---|
| Unit | Recipe validation, policy rules, profile updates, state transitions |
| Contract | Generation Adapter, storage, rights provider, analytics schema |
| Integration | Upload → analysis → generation → playback → export |
| Audio regression | Golden set, objective metrics, expert listening |
| UX | First-time user, disabled states, errors, accessibility |
| Security | Authentication, signed URLs, upload abuse, administrative access |
| Privacy | Deletion, consent changes, data export, retention |
| Load | Queue bursts, concurrent playback, provider throttling |

## 16.4 P0 Launch Gates

- [ ] The pilot catalog's rights and operation matrix are approved.
- [ ] The Workout end-to-end flow works on target devices.
- [ ] Music Style Transfer works for the published genre pairs.
- [ ] Rights pre-check and export re-check are audited.
- [ ] No blocking security/privacy issues remain.
- [ ] Generation success, latency, and artifact rate meet beta thresholds.
- [ ] Analytics events and dashboards are validated.
- [ ] The support runbook, diagnostic ID, and incident owner are ready.
- [ ] The deletion workflow is tested end to end.
- [ ] User-facing copy does not promise capabilities or rights beyond reality.

# 17. Delivery Plan and Technical Backlog

## 17.1 Epics

| Epic | Content | Dependencies | Outcome |
|---|---|---|---|
| E1 Identity & Consent | Authentication, guest session, consent | — | Valid user/session |
| E2 Rights Foundation | Declaration, policy engine, snapshot, audit | E1 | Operation gating |
| E3 Audio Ingest & Analysis | Upload, normalization, fingerprint, analysis | E2 | Track ready |
| E4 Preference Onboarding | Pairwise UX, profile schema | E1 | Cold-start profile |
| E5 Recipe & Intent | Prompt/preset, validation, versioning | E2, E3 | Structured generation request |
| E6 Generation Orchestrator | Queue, adapter, progress, retry | E3, E5 | Asynchronous candidates |
| E7 Workout Adjust | P0 controls and candidate profiles | E6 | First core value |
| E8 Style Transfer | Genre, locks, transform strategies, QC | E2, E3, E6 | Differentiated Transform value |
| E9 Compare & Feedback | Player, A/B, acceptance, ranking signals | E6 | User-selection loop |
| E10 Stations & Library | Save, lineage, version history | E9 | Repeat value |
| E11 Export & Provenance | Rights re-check, render, metadata | E2, E10 | Controlled output |
| E12 Analytics & Ops | Events, dashboards, cost, runbooks | All | Learn and operate |
| E13 Privacy Lifecycle | Deletion, consent changes, data export | E1–E11 | Trust readiness |

## 17.2 Suggested Implementation Sequence

1. Rights schema + operation matrix.
2. Audio fixture set and evaluation harness.
3. Ingest/analysis prototype.
4. Generation Adapter technical spike.
5. Style Transfer spike for 2–3 genre pairs.
6. Recipe schema and policy validation.
7. Mobile-first UX wireframes and clickable prototype.
8. Orchestrator + job state + progressive candidate delivery.
9. Compare Player and analytics.
10. Station, export, provenance, and deletion.
11. Internal alpha and Closed Beta.

## 17.3 Required Technical Spikes

| Spike | Question to answer | Output |
|---|---|---|
| TS-001 Style Transfer quality | To what degree can vocal/melody be preserved while changing genre? | Benchmark + supported genre matrix |
| TS-002 Stem strategy | When should the system use available stems, separation, or full regeneration? | Decision tree + artifact data |
| TS-003 Latency/cost | Which pipeline meets the preview target? | p50/p95 + cost/candidate |
| TS-004 Lock adherence | How should lyrics/melody/structure preservation be measured? | Metrics + human rubric |
| TS-005 Rights operations | How do contracts/catalog rights map to operations? | Rights schema + sample policy |
| TS-006 Playback comparison | How should timestamps map across structural variants? | A/B UX prototype |

# 18. Risks, Dependencies, and Open Questions

## 18.1 Risk Register

| ID | Risk | Level | Mitigation |
|---|---|---|---|
| R-001 | Insufficient catalog with clear derivative rights | High | Start small, partner with independent artists, define an operation matrix per track |
| R-002 | Style Transfer cannot preserve vocal/melody cleanly enough | High | Preserve locks, supported pairs, stems-first strategy, quality gate, fallback |
| R-003 | Output contains artifacts or does not communicate the target genre | High | Evaluation harness, expert listening, candidate diversity, hide failed output |
| R-004 | Latency/cost causes abandonment | High | Progressive candidate delivery, preview tier, caching, routing |
| R-005 | Users expect to edit every commercial song | High | Catalog badges, reference-only explanation, Create New alternative |
| R-006 | Cold-start profile is inaccurate | Medium | Pairwise onboarding, neutral defaults, fast feedback loop |
| R-007 | Rights change after generation | High | Re-check before export, revocation state, audit |
| R-008 | Prompts request artist imitation | Medium | Policy parser, descriptor conversion, unsupported-request UX |
| R-009 | Metrics optimize generation rather than listening value | Medium | North Star based on accepted/replayed listening minutes |
| R-010 | Provider lock-in | Medium | Generation Adapter, capability matrix, contract tests |
| R-011 | Sensitive audio is accessed improperly | High | Encryption, signed URLs, least privilege, access audit |
| R-012 | MVP scope becomes too broad | High | Release slices, P0 gates, Create New/Focus only after core stability |

## 18.2 External Dependencies

- Catalog/licensing partners.
- Audio generation/transformation capabilities or internal models.
- Object storage/CDN and GPU compute.
- Identity/authentication provider, if used.
- Market-specific legal review.
- Payment/quota system during monetization.
- Human audio evaluators and test participants.

## 18.3 Open Questions

| ID | Question | Proposed owner | Decision deadline |
|---|---|---|---|
| OQ-001 | Responsive web/PWA or native app for Closed Beta? | Product + Engineering | Before high-fidelity UX |
| OQ-002 | Vietnam-first or another launch market? | Product + GTM + Rights | Before catalog contracts |
| OQ-003 | What is the specific pilot catalog and operation-rights matrix per track? | Partnerships + Rights | Before alpha |
| OQ-004 | Which P0 genre pairs produce the best quality? | AI/Audio + Product | After TS-001 |
| OQ-005 | What are the actual preview latency and cost? | Engineering + AI/Audio | After TS-003 |
| OQ-006 | Is export included in Closed Beta, or only in-app listening? | Product + Rights | Before beta scope freeze |
| OQ-007 | Subscription, credits, or combined monetization? | Product + Finance | Does not block the prototype |
| OQ-008 | What retention period applies to sources, previews, and deleted assets? | Privacy + Engineering | Before alpha |
| OQ-009 | Which cohorts require adjusted success thresholds? | Data + Product | After alpha |
| OQ-010 | What is the final technology stack and team capacity? | Engineering | Before the implementation plan |

# 19. Definition of Done for Listener MVP v0.1

The MVP meets the baseline when:

1. A new user completes onboarding and creates a profile.
2. The user selects Workout, an eligible track, and an operation.
3. The rights engine correctly allows/blocks the operation and creates an audit trail.
4. Analysis and the Music Brief are available, or a clear fallback exists.
5. The user can generate three candidates for Adjust or supported Style Transfer.
6. Preserve locks are stored and checked.
7. The Compare Player works with partial results.
8. The user can select, refine, save a version, and save a station.
9. Export is available only according to rights, and provenance is attached.
10. Analytics, cost, quality, and error telemetry are complete.
11. The user can delete data and change consent.
12. Closed Beta meets technical gates and generates enough learning to decide on a public MVP.

# 20. Next Actions

After PRD v0.1 review, the project team runs two workstreams in parallel:

1. **UX Specification & Wireframes v0.1:** mobile-first design for Workout, Change Style, Compare Player, rights states, and error recovery.
2. **Technical Feasibility Spike:** benchmark Style Transfer, stem strategy, lock adherence, latency, cost, and supported genre pairs on an audio set with clear rights.

The results of both workstreams will feed **System Design v0.1**, the sprint plan, and the `LOG-003` update.

# Appendix A—Glossary

| Term | Definition |
|---|---|
| Adjust | Modify attributes while keeping identity/genre relatively stable |
| Transform | Create a new arrangement or style from an eligible source |
| Music Style Transfer | Change genre/style while preserving locked components |
| Create New | Create an independent work from a profile/abstract attributes |
| Preserve Lock | A mandatory or user-selected constraint that keeps a component |
| Edit Recipe | Structured representation of intent, constraints, rights, and controls |
| Candidate | One Familiar/Personal/Explore output |
| Rights Snapshot | Immutable rights decision at operation time |
| Personal Station | Saved context + recipe + preference configuration |
| Provenance | Metadata about source, rights, recipe, model, and generation history |
| Quality Gate | A check deciding whether a candidate may be displayed/exported |

# Appendix B—Sample Acceptance Test

**Scenario:** The user transforms a Ballad → EDM for Workout.

**Given** the track is in the catalog and is licensed to preserve lyrics/vocal/melody and create derivatives.  
**And** the vocal stem meets the capability threshold.  
**When** the user selects a 20-minute Workout, target EDM, 124 BPM, preserves Lyrics + Melody + Vocal + Structure, and chooses Balanced intensity.  
**Then** the recipe is validated and the rights snapshot is stored.  
**And** the job creates Familiar/Personal/Explore.  
**And** every candidate runs through quality/lock checks.  
**And** the user can A/B compare, select Personal, request a “stronger drop,” and create a child version.  
**And** export becomes available only if the current rights state permits it.  
**And** every event, cost, latency, and lineage record is captured.

# Appendix C—PRD Review Checklist

- [ ] Product confirms goals, P0/P1/P2, and success thresholds.
- [ ] Design confirms the flow, screen inventory, and accessibility.
- [ ] Engineering confirms epics, service boundaries, and NFRs.
- [ ] AI/Audio confirms supported capabilities and the evaluation plan.
- [ ] Rights confirms the operation matrix and user-facing copy.
- [ ] Security/Privacy confirms the data lifecycle.
- [ ] QA confirms acceptance criteria are testable.
- [ ] Data confirms the event taxonomy and dashboards.
- [ ] The project owner records review decisions in the MayaTune Project Log.
