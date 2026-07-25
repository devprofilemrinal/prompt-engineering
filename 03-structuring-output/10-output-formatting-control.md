# 10. Output Formatting Control

## What This Topic Covers

Topic 9 was about getting **machine-parseable structured data** (JSON, schemas). This topic is about controlling the **human-facing qualities** of output when the response is meant to be read by a person, not parsed by code: length, tone, style, reading level, and presentation format (bullets vs. paragraphs vs. tables). These controls matter constantly in real applications — a chatbot response, an auto-generated email, a summary shown in a UI.

## Length Control

### The Problem
Without explicit constraints, models tend to be verbose by default — they were trained to be thorough and helpful, which often translates into longer responses than a given UI or use case actually needs.

### Techniques (With Reasoning)

**Explicit units — sentences, words, bullets:**
```
Summarize this article in exactly 3 bullet points, each under 15 words.
```
**Reasoning:** Vague length instructions like "keep it short" are interpreted differently call to call — "short" to the model might mean 2 sentences or 8. Concrete units (sentence count, word count, bullet count) give the model an unambiguous target, producing far more consistent output length across repeated calls — which matters if your UI has fixed space for the response.

**Character/token budgets for UI-constrained spaces:**
```
Write a product description for a mobile notification. Maximum 80
characters, no line breaks.
```
**Reasoning:** If you're rendering output into a fixed UI element (a push notification, an SMS, a card preview), character limits are a hard technical constraint, not a stylistic preference — treat them as you would `maxlength` on a database column or a UI component. Still, validate the actual returned length in your code (never trust the model to hit a hard limit with 100% precision) — see "Always Verify" below.

### Always Verify Length Constraints in Code

```java
String response = llm.call(prompt);
if (response.length() > 80) {
    response = response.substring(0, 77) + "...";
    // Or better: re-prompt with feedback about the overage
}
```
**Reasoning:** Length instructions are a strong steering signal, not a guarantee — the model can and does occasionally exceed stated limits, especially with more elaborate content. Since UI-breaking overflow is a real production bug (not just an aesthetic nitpick), always enforce hard limits defensively in code, the same way you'd validate any external input against a schema before rendering it.

## Tone Control

Tone instructions shape the emotional register and formality level of the response.

```
Rewrite this system error message so a non-technical user can
understand it. Use a calm, reassuring tone. Avoid technical jargon
like "null pointer" or "stack trace."
```

**Reasoning for being this specific:** A vague instruction like "make it friendlier" leaves the model guessing at what "friendlier" means in your specific context. Naming the target audience ("non-technical user"), the desired emotional quality ("calm, reassuring"), and explicit anti-examples ("avoid jargon like...") together narrow the output space far more effectively than any single vague adjective — this is the same "be explicit, not vague" principle from Topic 2 (Prompt Structure) applied specifically to tone.

### A Useful Pattern: Before/After Tone Examples

```
Original (too technical): "Error: NullPointerException at line 42."

Rewrite in this tone: "Something went wrong on our end — we're looking
into it. Please try again in a few minutes."

Now rewrite this message in the same tone: "Error: Connection timeout
after 3 retries."
```

**Reasoning:** This is really few-shot prompting (Topic 3) applied to tone specifically — showing a concrete example of the desired voice is more reliable than describing tone abstractly in adjectives, because tone is inherently easier to demonstrate than to define in words.

## Style & Structure Control

This covers *how* information is presented — prose vs. bullet points vs. tables, formal vs. casual register, technical vs. plain-language phrasing.

```
Explain how database indexing works. Requirements:
- Use an analogy a non-engineer would understand.
- Structure as: one intro sentence, then 3 numbered points, then one
  closing sentence.
- Do not use any code snippets or SQL syntax.
```

**Reasoning:** Structural instructions ("intro sentence, then numbered points, then closing sentence") act very similarly to Constraints in Topic 2 — they don't just affect tone, they define a literal template the model should follow, which improves consistency when this same prompt is reused many times (e.g., as a template per Topic 8) across different input topics.

## Reading Level / Audience Calibration

```
Explain what an API rate limit is, written for a complete beginner
with no programming background. Avoid the words "endpoint," "payload,"
and "request" without first explaining them in plain terms.
```

**Reasoning:** Simply saying "explain simply" is vague and inconsistently interpreted call to call. Naming the specific audience and calling out specific jargon terms to avoid (or explain) gives the model concrete, checkable criteria — you (or an evaluator, see Topic 12) can literally check the output against those exact terms afterward, which a vague instruction doesn't allow.

## Formatting for Specific Output Channels

Different delivery channels have different formatting needs — a prompt destined for a Slack message, an email, and a mobile push notification should specify this explicitly, since the model has no way to know your delivery context otherwise.

| Channel | What to specify | Reasoning |
|---|---|---|
| Chat UI / conversational app | Markdown allowed or not, max message length | Determines whether `**bold**` or bullet markdown will render correctly or show as literal asterisks in your UI |
| Plain-text email | No markdown, use line breaks for paragraphs, include a subject line | Raw markdown syntax renders as ugly literal characters in plain-text email clients |
| SMS/push notification | Strict character limit, no formatting at all, no emoji unless explicitly wanted | These channels often have hard technical limits (e.g., SMS segment limits) and no rich-text rendering |
| Voice assistant response | No visual formatting at all (no bullets, no bold) — must be readable as natural spoken sentences | Bullet points and headers have no meaning when read aloud by text-to-speech; everything must work as flowing spoken language |

**Practical example:**
```
Write this response for a plain-text email (no markdown formatting,
no bullet symbols — use full sentences and line breaks between ideas).
```

## Common Mistake: Conflicting Instructions

```
Write a detailed, thorough explanation in exactly one short sentence.
```
This is self-contradictory — "detailed and thorough" pulls toward length, "one short sentence" caps it. When instructions conflict, the model has to arbitrate between them in some (not fully predictable) way, which introduces exactly the kind of inconsistency you're trying to eliminate by giving formatting instructions in the first place.

**Reasoning for avoiding this:** Just as you wouldn't write a function with contradictory parameter requirements ("required but optional"), avoid stacking formatting constraints that pull against each other. If you genuinely need both depth and brevity, resolve the tension explicitly instead of leaving it implicit — e.g., "Prioritize brevity over completeness; omit secondary details if needed to stay under 2 sentences."

## Key Takeaways

- Length, tone, and style are controlled the same way as any other instruction: through explicit, concrete criteria — vague adjectives ("short," "friendly," "simple") produce inconsistent results because they're open to interpretation.
- Concrete units (word/sentence/character counts) are far more reliable steering signals than vague qualifiers, but should still be verified defensively in code, since the model can occasionally exceed stated limits.
- Tone and style are often easier to demonstrate via a before/after example than to describe purely in adjectives — same underlying principle as few-shot prompting (Topic 3).
- Always specify the target output channel explicitly (chat, email, SMS, voice) — formatting that works in one breaks in another, and the model has no way to infer your delivery context on its own.
- Avoid stacking contradictory formatting instructions — resolve the tension explicitly rather than leaving the model to arbitrate an implicit conflict.

---
**Next up:** `11-prompt-chaining.md` — breaking a complex task into multiple sequential, connected prompts.
