# Turning the Scripts into Video Clips — Options & Recommendation

Research notes for producing animated explainer clips ("moving drawings + voice-over",
consistent graphics across episodes) from the scripts in this repository, on a budget of
**no more than ~USD 100 on top of subscriptions already owned** (Google AI Pro 5 TB and/or
a z.ai GLM Coding Pro yearly plan).

> Researched May 2026. Prices/limits change — re-verify before committing money.

---

## Key insight: these scripts favour code-driven animation, not AI video generation

The scripts are not prose — they are structured production documents. Every line is tagged
(`**VO:**`, `[GFX: ...]`, `[ON SCREEN: ...]`, `[LOWER THIRD: ...]`, `[B-ROLL: ...]`,
`[SFX: ...]`, `[CHECKLIST]`), and `00-SERIES-OVERVIEW.md` is a production bible defining
recurring visual conventions (title cards, the "Spot-It" checklist card, lower-thirds,
kinetic typography).

That structure points to **code-driven animation** as the backbone, because:

- **It solves "consistent graphics across clips."** Define a `<TitleCard>`,
  `<SpotItChecklist>`, `<LowerThird>`, `<KineticText>`, `<BarChart>` component **once**, then
  every one of the 74 episodes reuses them. AI video generators (Veo, Wan) cannot hold a
  consistent visual identity across 74 episodes — every clip drifts in style.
- **It is nearly free**, and it is exactly the kind of front-end / codegen work a GLM Coding
  subscription is built for. The AI writes the animation code; rendering is just CPU.
- **AI video generation would blow the $100 instantly.** Alibaba's Wan runs ~$0.07–0.15 per
  second ([fal Wan 2.6 is $0.10/sec at 720p](https://fal.ai/models/wan/v2.6/image-to-video)).
  $100 buys ~15 minutes of generated footage *total*; the series needs ~870 minutes
  (74 eps x ~12 min). Pure AI video is a non-starter for the backbone.

---

## Recommended pipeline

| Layer | Tool | Extra cost | Why |
|---|---|---|---|
| Animation / render | **Remotion** (React/TypeScript -> MP4) | $0 | Built for kinetic typography, lower-thirds, checklists, animated charts, SVG "drawing" effects. Component reuse = consistency. Runs on Node. |
| "Moving drawings" | Animated SVG line-art in Remotion (`stroke-dashoffset` reveal = whiteboard/doodle feel) | $0 | Hand-drawn look without a video model. Build a small, consistent icon set once. |
| Voice-over | **Kokoro TTS** (local, Apache-licensed) for English; see note for Romanian | $0 | Excellent quality, runs on a normal CPU, unlimited. Top free option in [2026 open-source TTS comparisons](https://www.geeky-gadgets.com/text-to-speech-tts-ai-models/). Piper is the faster/lower-quality fallback. |

> **Romanian VO note.** Kokoro does **not** support Romanian. For Romanian narration use **Edge TTS**
> (free `ro-RO` neural voices), **Azure Speech** (full SSML — best for the `<en>`/`<emf>` emphasis +
> per-word English pronunciation; generous free tier), or **ElevenLabs multilingual** (auto-detects
> English words, paid). The Romanian scripts carry engine-neutral `<en>…</en>` / `<emf>…</emf>` markup
> in the spoken `**VO:**` lines; a small build step converts them to SSML (`<lang xml:lang="en-US">`
> / `<emphasis>`) for capable engines or strips them for engines without SSML.
| The "brain" | **GLM Coding Pro** inside Claude Code / Cline / Cursor / OpenCode | already paid | Writes the markdown->scene parser, the Remotion component library, and the TTS+render glue. |
| Optional B-roll | Public-domain stills + Ken Burns pan, OR a small amount of Wan/Veo | $0–~$30 | Only where live-action texture is genuinely wanted (e.g. 1920s archival shots). |

**Workflow:** a parser turns each `EPxx.md` into a scene list (VO segments + their `[GFX]`
cues) -> Kokoro renders the `**VO:**` lines to audio -> Remotion lays the reusable graphics
against that audio and renders the MP4. The production-bible conventions map 1:1 onto reusable
components, so episode 74 looks identical in style to episode 1.

---

## Answers to the specific questions

**"Buy access from an Alibaba model and have Antigravity / Claude CLI / Hermes use it?"**
There are two different "Alibaba models" — do not conflate them:
- **Qwen** = a text/coding LLM (an agent *brain*). Not needed — GLM Coding Pro already covers that.
- **Wan** = the *video* generator. A coding agent can call the Wan API from a script (via
  fal.ai, Replicate, or Alibaba Cloud Model Studio / DashScope). Use it only as occasional
  B-roll, not the backbone. Wan even has
  [`wan2.2-s2v`](https://www.alibabacloud.com/help/en/model-studio/wan-s2v-api) (talking-head
  lip-sync from image + audio) if a "host" is ever wanted, but that fights the
  "no fancy graphics, consistent style" goal.
- **Hermes** (Nous Research open-weight models) would just be an alternative agent brain — no
  advantage here over GLM.

Constraint: the **GLM Coding Plan's tokens are restricted to supported *coding* tools**
(Claude Code, Cline, Cursor, OpenCode, Kilo Code, Qoder...) via a
[dedicated coding API endpoint](https://docs.z.ai/devpack/quick-start). They cannot be
repurposed as a general "call the video API" budget — but they do not need to be. The agent
writes code locally; the code calls whatever media API is funded separately.

**"Can Antigravity be installed in a VM without being logged into my Gmail?"**
- *Install:* yes, it runs fine in a VM (it is a desktop IDE).
- *Use:* no — the agent features require signing into *a* Google account. It does not have to
  be the primary Gmail; a separate/dedicated Google account works. It cannot be used fully
  logged-out.
- *VM caveat:* headless VMs, VPNs, and region mismatches are a leading cause of
  "account not eligible" / login-loop failures. Antigravity checks the account's
  [home-country region](https://discuss.ai.google.dev/t/unable-to-sign-in-to-antigravity-despite-google-workspace-ultra-subscription/115510)
  and prefers a **personal** Gmail over a Workspace account.

**"Buy Kiro credits and use Opus 4.8?"**
- There is **no Opus 4.8**. Kiro currently initializes on
  [claude-opus-4.6](https://kiro.dev/) (4.7 in the changelog).
- Opus carries a [~2.2x credit multiplier vs the Auto baseline](https://kiro.dev/docs/models/);
  the $20 tier gives ~1,000 credits; overage is $0.04/credit.
- Verdict: viable but **unnecessary**. Generating Remotion/Manim code is well within
  GLM-4.7/5.1's wheelhouse, which is already paid for. Spend Opus money only when hitting a wall.

**Google AI Pro specifically:** as of I/O 2026 it moved to
[compute-based usage limits](https://www.business-standard.com/technology/tech-news/google-reshuffles-gemini-ai-plans-with-new-ultra-tier-revises-usage-limits-126052100670_1.html)
(the old fixed "1,000 credits" was removed), with quotas refreshing ~every 5 hours up to a
weekly cap. Veo video and Gemini TTS are usable but throttled — fine for occasional B-roll or a
few nicer narration passes, not for batch-rendering 74 episodes.

---

## Suggested budget (well under $100)

- Remotion + Kokoro + GLM Coding Pro: **$0 extra**
- *Optional* nicer narration: one month of ElevenLabs Creator (~$22) for a few "hero" episodes,
  or just use Gemini TTS within the AI Pro quota — **$0–22**
- *Optional* AI B-roll for historical episodes via fal/Wan: **~$20–30**
- Music: free CC libraries (and the production bible warns against ominous music anyway)

**Realistic total: $0–$50.**

> License note: Remotion is free for individuals and small teams but requires a paid company
> license above a size/revenue threshold — likely irrelevant here, but verify against
> [remotion.dev](https://www.remotion.dev/) for your situation.

---

*Content on third-party pricing/features was rephrased from the linked sources for compliance
with licensing restrictions; re-verify current details directly.*
