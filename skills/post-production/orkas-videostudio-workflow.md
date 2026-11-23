# OrkasVideoStudio Agent Production Workflow

> Turn a video brief or supplied footage into a reviewable plan, draft, and finished video with a local-first CLI and MCP toolkit.

## When to Use

- A coding agent is responsible for producing the final video.
- The project needs a readable, editable, and re-renderable timeline.
- The brief combines motion graphics, supplied footage, captions, or generated shots.
- Reviewers need to inspect a draft before the expensive final render.
- The workflow should run locally and expose the same operations through CLI and MCP.
- The team needs clear approval boundaries around provider-backed generation.

Do not use this workflow to imply that unpublished npm packages are already available.
The documented working path is installation from the source repository.

## Workflow

### Step 1: Normalize the brief

Record these facts before production:

- audience and purpose;
- duration and aspect ratio;
- language and caption requirements;
- supplied media and preservation boundaries;
- delivery format and deadline;
- whether provider-backed generation is allowed.

If the brief omits a minor detail, choose a reversible default and record it.

### Step 2: Choose the production route

Select one primary route:

1. **Compose** for deterministic typography, diagrams, UI, and motion graphics.
2. **Edit** for trimming, reframing, captioning, mixing, or repackaging supplied footage.
3. **Generate** for genuinely new imagery or footage from a configured provider.
4. **End-to-end** when several routes must be woven into one deliverable.

State the route in the production proposal and do not switch it silently.

### Step 3: Install from source

```bash
git clone https://github.com/Orkas-AI/Orkas-VideoStudio.git
cd Orkas-VideoStudio
pnpm install
pnpm build
node packages/cli/dist/index.js doctor
```

Create a local alias for the built CLI:

```bash
alias ovs="node $PWD/packages/cli/dist/index.js"
ovs doctor
```

### Step 4: Build the editable plan

Represent the production in `plan.json`.
Give every segment a purpose, timing boundary, source, and expected output.
Keep visible text and narration in reviewable source files.
Reference supplied media rather than replacing it.

Before rendering, run:

```bash
ovs plan validate plan.json
ovs plan summarize plan.json
ovs plan promise-check plan.json
```

### Step 5: Produce the draft

For a composition project:

```bash
ovs composition prepare composition
ovs draft composition --out draft.mp4 --report draft-report.json
```

For an edit project, inspect inputs first:

```bash
ovs edit probe input.mp4
ovs quality input.mp4
```

Review the draft at the opening, every scene transition, and the closing frame.

### Step 6: Finish and verify

Check text legibility, timing, caption accuracy, audio levels, source preservation,
and whether the final cut fulfills the delivery promise.
Render only after the draft defects are resolved.

```bash
ovs render composition --out final.mp4
ovs quality final.mp4
ovs plan promise-check plan.json
```

Deliver the video, `plan.json`, caption files, and any report needed for revision.

## Tools

| Tool | Purpose | Price | Best For |
|------|---------|-------|----------|
| OrkasVideoStudio CLI | Plan, compose, edit, transcribe, assemble, and validate | Free from source | Coding-agent video workflows |
| OrkasVideoStudio MCP server | Expose the same operations as typed tools | Free from source | MCP-capable agents |
| FFmpeg and ffprobe | Deterministic media processing and inspection | Free | Local edit and QA |
| HyperFrames dependency | Deterministic HTML composition and rendering | Open-source dependency | Motion graphics and explainers |
| User-selected providers | Optional image, speech, or video generation | Varies | Shots that cannot be produced locally |

Provider services are optional. Confirm their cost and data path before use.

## Templates

### Template 1: Production proposal

```markdown
Goal: [what the video must achieve]
Audience: [primary viewer]
Duration / aspect: [for example, 45 seconds / 9:16]
Primary route: [compose | edit | generate | end-to-end]
Inputs to preserve: [files and protected content]
Generated elements: [none, or approved list]
Review artifact: draft.mp4 + draft-report.json
Delivery: final.mp4 + plan.json + captions
```

### Template 2: Draft review

```markdown
- Opening communicates the topic within the first scene.
- Every scene has enough reading time.
- Captions match names, numbers, and spoken wording.
- Transitions support the narrative rather than distract from it.
- Supplied footage is not cropped or replaced beyond the approved boundary.
- Audio remains intelligible on speakers and headphones.
- The closing frame contains the intended call to action.
```

### Template 3: Transcript-led finishing

```bash
ovs transcribe interview.mp4 --out transcript.json
ovs edit trim-silence interview.mp4 --out tightened.mp4
ovs edit burnsubs tightened.mp4 --srt subtitles.srt --out captioned.mp4
ovs edit normalize-loudness captioned.mp4 --out final.mp4
ovs quality final.mp4
```

Review every automated cut against the transcript before accepting it.

### Template 4: Handoff note

```markdown
Rendered file: final.mp4
Editable timeline: plan.json
Captions: subtitles.srt
Primary route: [route]
Checks: [validate, quality, promise-check]
Known limitations: [none or concise list]
Provider-backed operations: [none or provider and approved scope]
```

## Pitfalls

### Common Mistakes

1. Starting the final render before the brief is locked.
2. Treating a generated video as a substitute for requested source footage.
3. Mixing compose, edit, and generate operations without one timeline.
4. Spending provider credits without explicit approval.
5. Claiming npm installation works before the packages are published.
6. Delivering a rendered file without its editable artifacts.
7. Trusting automated captions without checking names and numbers.

### Quality Checks

- [ ] The primary route is stated.
- [ ] The `plan.json` file validates.
- [ ] The draft has been visually inspected.
- [ ] Captions have been proofread.
- [ ] Audio has been checked for clipping and intelligibility.
- [ ] Source-media preservation rules were followed.
- [ ] Provider-backed operations were approved.
- [ ] The final output plays end to end.
- [ ] The delivery promise passes.
- [ ] Editable artifacts are included.

## Pro Tips

1. Use compose as the default for explainers because text and layout stay deterministic.
2. Keep edit as the primary route whenever supplied footage is the main work object.
3. Generate only the shots that cannot be created deterministically.
4. Review a low-cost draft before rendering at delivery resolution.
5. Treat `plan.json` as the handoff contract between creative intent and execution.
6. Keep captions as separate files until wording and timing are approved.
7. Store source references beside each segment so later revisions remain traceable.
8. Report failed checks honestly instead of hiding them behind a successful render.

## 中文版本

### 适用场景

当 Coding Agent 需要从需求或已有素材直接交付成片，并且项目需要可编辑、
可审查、可重复渲染的时间线时，使用此工作流。

### 核心流程

1. 明确受众、时长、画幅、语言、素材边界和交付格式。
2. 锁定 Compose、Edit、Generate 或端到端主路线。
3. 从源码安装并运行 `doctor` 检查本地依赖。
4. 使用 `plan.json` 记录片段、时间、来源和预期结果。
5. 先输出低成本草稿，再检查字幕、节奏、画面和声音。
6. 通过质量检查与交付承诺检查后再渲染最终版本。
7. 一并交付成片、时间线、字幕和必要报告。

### 注意事项

- 不要把用户提供的素材静默替换为生成内容。
- 付费生成步骤必须先获得确认。
- npm 包尚未发布时，只描述可用的源码安装方式。
- 自动字幕必须复核人名、数字、断句和时间轴。
- 成片之外还要保留 `plan.json`，便于后续修改。

## Source and License

This workflow is adapted from the OrkasVideoStudio skill set at immutable commit
[`d5197ab5e625fc44ab719a940711d82b46c16ef5`](https://github.com/Orkas-AI/Orkas-VideoStudio/tree/d5197ab5e625fc44ab719a940711d82b46c16ef5/packages/skills).
The upstream project and this adaptation are MIT-licensed.
