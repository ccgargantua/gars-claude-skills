---
name: fact-checking
description: "Fact check statements, opinions, news articles, debates, speeches, quotes, or claims of any kind. Use this whenever the user asks to verify, check, confirm, or investigate whether something is true, accurate, or well-supported, or asks for a fact check of a piece of text, a person's remarks, or a specific claim."
---

Principles:
1. Principles - This list governs how the skill is followed. Always re-check output against every principle below before finalizing.
2. Minimalism - No extra wording, no em-dashes, no emojis. Write findings plainly. Do not pad a short answer to look thorough.
3. Strict Validation - Every factual verdict must be backed by a search performed in this conversation. Never rely on training data alone to confirm or deny a claim; training data can be stale or wrong. If a claim cannot be checked with available tools, say so instead of guessing.
4. No Filled Gaps - If the claim is ambiguous (unclear speaker, date, source, or version of a quote), ask the user to clarify rather than assuming the most likely version.

Process:

1. Isolate discrete claims. Long text (articles, speeches, debate transcripts) contains many checkable claims mixed with opinion and framing. Pull out the specific factual assertions before searching. Opinions and value judgments are not fact-checked as true/false; note them as opinion and move on, unless the user wants the reasoning behind them assessed.

2. Search for each claim separately. Use web_search (and web_fetch on the most authoritative-looking result) per claim rather than one broad search for the whole text. Favor primary sources, official records, and reputable independent outlets over aggregators or partisan sites. Cross-check against at least two independent sources when a claim is contested or high-stakes.

3. Rate each claim using this scale:
   - True - directly confirmed by reliable sources.
   - False - directly contradicted by reliable sources.
   - Misleading - technically true but missing context that changes its meaning.
   - Unverifiable - no reliable source confirms or denies it; state this rather than guessing.
   - Opinion - not a factual claim, no verdict given.

4. Cite sources for every verdict. Follow standard citation rules: no exact-quote reproduction beyond short attributed fragments, paraphrase content, cite where the claim was verified.

5. Present results claim by claim, not as one blended paragraph, so the user can trace each verdict to its evidence. For a short single claim, this can just be a direct answer with sourcing. For a long document or speech, use a numbered list: claim, verdict, brief evidence, source.

6. Stay neutral. Do not editorialize about the person or outlet who made the claim. Report what the evidence shows and let the rating speak for itself.

7. If asked to fact check something politically contested where reliable sources disagree or the matter is genuinely unsettled, say so explicitly rather than forcing a True/False verdict.

Example:

User: "Can you fact check this line from a stump speech: 'Unemployment is the lowest it's been in 50 years'?"

Skill applied: search for the current rate and recent history, then answer directly rather than assuming.

"Verdict: False. The most recent BLS Employment Situation report puts the US unemployment rate at 4.2 percent in June 2026 (source: bls.gov). That is not close to a 50 year low. The rate has been below 4 percent multiple times in the last decade, and lower still in the late 1960s. If the claim was about a different country or time period, the specific figures would need to be rechecked against that context."
