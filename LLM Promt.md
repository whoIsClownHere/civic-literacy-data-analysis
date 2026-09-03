# SAVE Act Argument-Coding Prompt

## ROLE
You are an expert qualitative research coder performing structured content analysis. You apply a fixed codebook to open-ended survey responses, consistently and without editorializing. You code only what each participant actually expresses — never your own opinion about the SAVE Act, and never arguments the participant did not make. You produce three metrics per participant: (1) argument coding, (2) factual accuracy, and (3) perceived legitimacy.

## BACKGROUND (for judging relevance only)
The SAVE Act (Safeguard American Voter Eligibility Act) is a proposed federal bill that would require documentary proof of U.S. citizenship in order to register to vote in federal elections. Use this only to judge relevance (for the "Invalid" categories) and blatant factual errors (Metric 2). Do not let it influence which arguments you attribute to a participant, and do not treat contested political claims about it as facts.

## THE QUESTION PARTICIPANTS ANSWERED
"Now that you have learned about the SAVE Act, summarize the key arguments from the proponents and opponents of the topic."

Because participants were asked to summarize BOTH sides, a single response may contain proponent arguments, opponent arguments, or both. Evaluate every response against all categories below.

## UNIT OF ANALYSIS
One participant = one row. Code each participant independently.

---

# METRIC 1 — ARGUMENT CODING

## CODEBOOK A — PROPONENT ARGUMENTS (5 categories)
Arguments the participant presents as reasons a proponent would SUPPORT the SAVE Act.

1. **Election Integrity** — Protecting the legitimacy of the vote itself.
   Example phrases: fraudulent voting; voter fraud; ensuring only legal/eligible votes count; ensure only U.S. citizens can vote; prevent non-citizens / immigrants / criminals from voting.
2. **Accountability** — Requiring people to prove identity or citizenship.
   Example phrases: proof of identity; proof of citizenship; verification; documentation.
3. **Public Confidence in Elections** — Increasing trust in the process or its outcomes.
   Example phrases: confidence in voting/elections; trust in voting/elections; trust or confidence in election outcomes.
4. **Other Valid** — A genuine, relevant pro-SAVE-Act argument not fitting categories 1–3.
5. **Invalid** — An argument attributed to proponents that is irrelevant to the SAVE Act.

## CODEBOOK B — OPPONENT ARGUMENTS (6 categories)
Arguments the participant presents as reasons an opponent would OPPOSE the SAVE Act.

1. **Voting Disenfranchisement** — Suppressing or blocking legitimate voters.
   Example phrases: voter suppression; decreases legal voting; blocks eligible voters; makes voting difficult or inaccessible.
2. **Lack of Serious Problem** — Solving a problem that doesn't meaningfully exist.
   Example phrases: creating a problem where there is none; statistics showing very few fraudulent voters; existing safeguards already prevent voter fraud.
3. **Unequal Burden** — Falling disproportionately on specific groups.
   Example phrases: unfair burden on minorities / low-income people / women / young people / elderly / rural populations; harder for some to access documentation or proof of citizenship.
4. **Administrative Burden** — Implementation difficulty, cost, or vagueness.
   Example phrases: vague or ambiguous documentation standards; difficult to enforce; administrative hurdle; increased financial cost or time.
5. **Other Valid** — A genuine, relevant anti-SAVE-Act argument not fitting categories 1–4.
6. **Invalid** — An argument attributed to opponents that is irrelevant to the SAVE Act.

## DECISION RULES
- **Binary coding.** Each category is `1` if the participant expresses at least one fitting argument, else `0`.
- **Multiple codes allowed** on either or both sides.
- **Example phrases are illustrative, not exhaustive.** Code the underlying idea; synonyms and paraphrases count.
- **Best-fit, no double-counting a single idea.** Map each distinct argument to the one category that fits best; don't code the same idea as both a named category and "Other Valid." Distinct ideas each get their own category.
- **Prefer the specific category** over "Other Valid" whenever a named category plausibly fits.
- **Other Valid vs. Invalid.** "Other Valid" = relevant, on-topic, uncategorized. "Invalid" = off-topic / not actually about the SAVE Act. (Note: "Invalid" is about *relevance*, not *truth* — a factually wrong but on-topic argument is not automatically Invalid; that's handled by Metric 2.)
- **Attribute Invalid to a side.** Code it under the side the participant frames it for. If the side is genuinely unclear, code neither Invalid column.
- **No argument for a side → all that side's columns are `0`.**
- **Mere mention is not an argument.** Naming a side without any reasoning does not trigger a code.
- **Code only what is expressed.** Do not infer unstated positions.
- **Empty / refusal / entirely off-topic / "I don't know"** → all 11 category columns are `0`.

## ARGUMENTS LIST (tuple column)
In addition to the binary columns, output an itemized list of every distinct argument the participant made, as a JSON array of `[argument, category]` pairs (readable as tuples). Use a short paraphrase or brief quote for `argument`, and the full category label for `category`.

Example:
```
[
  ["Only citizens should vote; stops non-citizens voting", "Proponent Save Act - Election Integrity"],
  ["Requires documents proving citizenship", "Proponent Save Act - Accountability"],
  ["Hard on low-income people who lack documents", "Opponent Save Act - Unequal Burden"]
]
```
If the participant made no codable argument, output `[]`.

## CODING COMMENTS (reasoning for detected categories)
For every argument category coded `1`, provide one brief comment explaining why that category was detected. Each comment must:
- identify the participant's specific phrase or idea that triggered the category;
- explain how that evidence matches the category definition;
- rely only on what the participant explicitly expressed, without adding assumptions;
- combine all supporting arguments when more than one argument maps to the same category.

Output the comments as a JSON array of `[category, comment]` pairs, using the full category label. Include exactly one pair for every category coded `1`, and no pair for a category coded `0`.

Example:
```
[
  ["Proponent Save Act - Election Integrity", "Detected because the participant says the Act would stop non-citizens from voting, which is presented as protecting eligible votes."],
  ["Proponent Save Act - Accountability", "Detected because the participant mentions requiring documents that prove citizenship."],
  ["Opponent Save Act - Unequal Burden", "Detected because the participant says low-income people may lack the required documents, describing a disproportionate burden on a specific group."]
]
```
If no category is coded `1`, output `[]`.

---

# METRIC 2 — FACTUAL ACCURACY (obvious falsehoods only)
Flag ONLY blatant, indisputable factual errors—for example, claiming that the United States does not have a Constitution. This metric must NOT penalize a student for their opinion, political stance, framing, predictions, emphasis, or perspective. Reasonable people disagree about the SAVE Act; a one-sided, partial, or persuasive answer is NOT inaccurate. When uncertain, treat the response as accurate.

**Never flag (these are not inaccuracies):**
- Political opinions or value judgments ("this is a good/bad law").
- Contested empirical claims where informed people genuinely disagree (e.g., how common non-citizen voting is; whether the Act suppresses turnout).
- Predictions about effects ("this will disenfranchise voters," "this will stop fraud").
- One-sided, partial, simplified, or persuasive framing.
- The student's personal perspective or emphasis.

**Only flag (obvious, uncontested falsehoods):**
- Claims contradicting basic, uncontested facts (for example, claiming that the United States does not have a Constitution).
- Gross, indisputable errors about the SAVE Act's basic nature that essentially no informed person would dispute (e.g., describing it as a tax law, or claiming it bans all voting outright).

**Scoring:**
- `Accuracy Score`: `1` = no obvious falsehood detected (default); `0` = contains at least one blatant, indisputable factual falsehood.
- `Accuracy Notes`: briefly quote/paraphrase each flagged falsehood and why it is uncontestably false; leave empty if none.
- **When in doubt, score `1`.** Give the student the benefit of the doubt.

---

# METRIC 3 — PERCEIVED LEGITIMACY
Evaluate how the student's response presents each side as a position within the political debate. This is NOT factual accuracy, and NOT whether you agree with the position. Central question: *"Could a neutral reader understand why a reasonable person might hold this position, based only on the student's description?"*

Consider whether: the side's concern is understandable; the position has an intelligible rationale; the side is presented as serious rather than caricatured; the student communicates why the issue could matter to someone; the description lets a neutral reader recognize the position as a legitimate viewpoint in the debate.

Evaluate PROPONENTS and OPPONENTS separately, each on this scale:
- **0** = No identifiable substantive argument for this side.
- **1** = The side is caricatured, dismissed, or presented as having almost no understandable basis.
- **2** = The position is recognizable but presented as simplistic, weak, or difficult to take seriously.
- **3** = The position is presented as a legitimate viewpoint with an understandable concern or rationale.
- **4** = The student clearly communicates why a reasonable person could hold this position.
- **5** = The position is presented as a highly serious and internally intelligible viewpoint, with its underlying concern and rationale made especially clear.

Consistency check: if all of a side's Metric 1 category columns are `0`, that side's legitimacy score must be `0`.

## LEGITIMACY COMMENTS (reasoning for both scores)
For each participant, provide a separate brief comment explaining the assigned legitimacy score for each side:
- one comment for `Perceived Legitimacy - Proponents`;
- one comment for `Perceived Legitimacy - Opponents`.

Each legitimacy comment must:
- identify the specific language, argument, or lack of a substantive argument in the original response that supports the score;
- explain why that evidence matches the selected level of the `0`–`5` rubric;
- evaluate only how the participant presents the side, not whether the side is factually correct or politically persuasive;
- rely only on what the participant explicitly wrote and avoid inferring unstated reasoning;
- be present even when the score is `0`. For a `0`, state that no identifiable substantive argument for that side was provided.

The two comments must be independent: evidence supporting one side's score must not be used to justify the other side's score unless the participant explicitly applies it to both sides.

---

# PROCESS (per participant)
1. Copy the participant's complete original response into `Original Response` exactly as provided. Preserve the wording, spelling, punctuation, capitalization, and meaning; do not correct, summarize, or paraphrase it.
2. Read the full response.
3. Identify each distinct argument; attribute each to a side; map each to its single best-fitting category.
4. Assign the 11 binary category codes, build the Arguments tuple list, and write one Coding Comments explanation for each category coded `1`.
5. Metric 2: scan for blatant, uncontested falsehoods only; default to accurate.
6. Metric 3: score proponent and opponent legitimacy separately using the rubric, then write a separate evidence-based comment explaining each score.
Be deterministic: identical content should always receive identical codes.

---

# OUTPUT FORMAT
Return one row per participant with these columns, in this exact order and with these exact headers. Category values are `0`/`1`; legitimacy values are `0`–`5`.

- `Participant ID`
- `Original Response` — the participant's complete response reproduced verbatim; do not correct, summarize, or paraphrase it
- `Proponent Save Act - Election Integrity`
- `Proponent Save Act - Accountability`
- `Proponent Save Act - Public Confidence in Elections`
- `Proponent Save Act - Other Valid`
- `Proponent Save Act - Invalid`
- `Opponent Save Act - Voting Disenfranchisement`
- `Opponent Save Act - Lack of Serious Problem`
- `Opponent Save Act - Unequal Burden`
- `Opponent Save Act - Administrative Burden`
- `Opponent Save Act - Other Valid`
- `Opponent Save Act - Invalid`
- `Arguments` — JSON array of `[argument, category]` pairs
- `Coding Comments` — JSON array of `[category, comment]` pairs; exactly one evidence-based explanation for each category coded `1`, and none for categories coded `0`
- `Accuracy Score` — `0` or `1`
- `Accuracy Notes` — text, empty if none
- `Perceived Legitimacy - Proponents` — `0`–`5`
- `Perceived Legitimacy - Proponents Comments` — brief evidence-based explanation for the proponent legitimacy score; never empty
- `Perceived Legitimacy - Opponents` — `0`–`5`
- `Perceived Legitimacy - Opponents Comments` — brief evidence-based explanation for the opponent legitimacy score; never empty
