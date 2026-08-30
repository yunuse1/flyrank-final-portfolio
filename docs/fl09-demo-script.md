# FL-09 Demo Script — Social Media Studio

Target duration: **3–5 minutes**

Recording: OBS Studio or Loom, uploaded as an **unlisted YouTube video**.

## 0:00–0:25 — Introduction

> "This is my Social Media Studio capstone. It is a FastAPI backend that takes source content, generates platform-specific variants, validates them, requires human approval, and then schedules and publishes approved content through platform adapters."

Show the GitHub repository or the running Swagger UI briefly.

## 0:25–1:05 — Ingest source content

Open Swagger UI at `/docs` and call `POST /posts/ingest` with a short source post.

Say:

> "I start with one source post. The important design goal is that the source content becomes the controlled input for the rest of the workflow rather than asking the model to invent a post from nothing."

Show the returned `post_id`.

## 1:05–1:45 — Generate AI variants

Call `POST /ai/generate` using the same source title/content.

Say:

> "Now I send the source to the optional Gemini generation layer. It returns structured variants for the supported platforms. The model is not trusted to decide whether content is valid: the application validates the structured response and applies deterministic platform constraints."

Show the X, LinkedIn, and Telegram variants and the model/token metadata.

## 1:45–2:25 — Review and approval guardrail

Call `GET /posts/{post_id}/variants` and then approve one variant with:

`PATCH /variants/{variant_id}/status?new_status=approved`

Say:

> "This is the main publishing guardrail. AI output cannot publish immediately. A human approval state is required before the publish workflow can continue. This separates content generation from authorization."

Optionally attempt to schedule a non-approved variant if the API makes the rejection obvious; otherwise show the approved state.

## 2:25–3:10 — Schedule and worker

Call `POST /publish/schedule` with an idempotency key. Use an immediate or near-future schedule that is safe for the demo.

Then show `GET /publish/jobs`.

Say:

> "The publish job is persisted in SQLite, so scheduling is not only an in-memory timer. A background worker claims due jobs and sends them through the SocialPublisher adapter."

Show the job moving to its completed state when practical.

## 3:10–3:35 — Publish history

Call `GET /publish/history`.

Say:

> "The final delivery is recorded in publish history. The idempotency key is persisted as well, so a retry with the same key does not intentionally create a second publish."

## 3:35–4:10 — Limitation

Show the README's Known Limitations section or the relevant architecture code.

Say:

> "One important limitation is that the scheduler is an in-process worker backed by SQLite. That is suitable for this single-instance capstone and is restart-safe because jobs are persisted, but it is not a distributed production job system. A production deployment would need a shared database and distributed job claiming."

## 4:10–4:30 — Close

> "The v2 evaluation covered five representative Gemini cases and all five passed the deterministic structural and platform checks. Semantic quality and hallucination detection still require human review. The project is designed around that boundary: AI assists generation, while the backend owns validation, approval, scheduling, and publishing behavior."

Stop the recording.

## Recording checklist

- [ ] 3–5 minutes
- [ ] Real running application shown
- [ ] End-to-end flow demonstrated
- [ ] Voice narration is clear
- [ ] One design decision explained
- [ ] One limitation explained
- [ ] v2 evaluation result mentioned
- [ ] No API keys or `.env` secrets visible
- [ ] Upload to YouTube as **Unlisted**
- [ ] Copy YouTube URL into the final submission portal
