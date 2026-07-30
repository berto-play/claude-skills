# Methods Reference

Only read this file when the user explicitly requests a specific method or deeper analysis. Default synthesis does not use these.

---

## Choosing a method

| User wants | Method |
|---|---|
| Patterns across themes | Thematic Analysis (Braun & Clarke) |
| Cluster raw observations into insights | Affinity Diagramming |
| Patterns across multiple interviews | Framework / Matrix Analysis |
| Who stands where on a decision or proposal | Stance Detection |
| Products, tools, regulations, people mentioned | Entity Extraction |
| Who spoke, who didn't, who dominated | Participation Analysis |
| Where friction or conflict sits | Tension Mapping |
| Frequency of topics across sessions | Content Analysis |
| Track how views shift across multiple sessions | Cross-Session Comparison |

---

## Key distinctions (apply in all methods)

**Observation vs Insight**
An observation is what was said. An insight is what it means. Never conflate them. Label each clearly. An insight must be defended by at least two observations from the transcript.

**Signal vs Noise**
Not everything in a transcript is meaningful. Flag low-signal passages (small talk, tech issues, repetition) and exclude them from synthesis unless the user asks for full coverage.

**Patterns vs Anti-patterns**
Anti-patterns: things speakers consistently avoid, resist, or flag as broken. These are often more informative than what people say they want.

---

## Thematic Analysis (Braun & Clarke, 6-step)

1. Familiarize: read fully, note quality issues, do not assign themes yet.
2. Code: one code per distinct idea. Cite source utterance. Label [VERBATIM] or [P].
3. Search for themes: group codes. Name themes descriptively.
4. Review themes: each theme needs 2+ distinct utterances. Merge, split, or discard as needed.
5. Define themes: name, definition, supporting quotes, disconfirming evidence, confidence level.
6. Write-up: every claim cites a speaker. Disagreements preserved. No editorializing.

---

## Affinity Diagramming

1. Extract observations: one per utterance unit. Format: `[OBS-###] Speaker X: "utterance" [VERBATIM / P]`
2. Cluster: group by natural affinity. Name clusters descriptively, not interpretively.
3. Name insights: one insight per cluster, directly defensible by its observations. Interpretation beyond the data: label [INFERRED].
4. Gaps and surprises: what did not cluster? What was absent?

---

## Framework / Matrix Analysis

Use across multiple transcripts. Build a matrix: rows = themes, columns = transcripts or participants. Fill each cell with evidence or mark empty. Surfaces convergence and gaps systematically.

---

## Stance Detection

For each named proposal or decision in the transcript, map where each speaker stands: Pro / Con / Neutral / Ambiguous. Quote the utterance that supports the stance. Do not infer stance from tone.

---

## Entity Extraction

Pull named entities from the transcript and categorize:
- Tools and products
- People and roles
- Regulations and documents
- Risks and concerns
- Open questions

Each entity: name, category, speaker who mentioned it, context (one sentence).

---

## Participation Analysis

For group sessions: note which speakers contributed substantively, which were mostly silent, whether any speaker was interrupted or talked over. Do not interpret why. Note the pattern.

---

## Tension Mapping

Identify explicit or implied friction between two positions. Format: **"[Position A] vs [Position B]"** — Speaker X held A, Speaker Y held B. Note whether the tension was resolved, deferred, or left open.

---

## Content Analysis

Count occurrences of topics, terms, or concerns across a set of transcripts. Useful for showing frequency, not just presence. Pair counts with representative quotes. Do not let frequency substitute for meaning.

---

## Cross-Session Comparison

Label sessions S1, S2, S3. Track: stable positions (cite each session), shifts (quote both versions, note but do not interpret), additions, contradictions [mark CONTRADICTION — unresolved unless speaker explained the change].

---

## Output formats available on request

- **Evidence Table**: insight → supporting quotes (default for high-stakes synthesis)
- **Keep / Change / Investigate Further**: opinionated product framing
- **Design Implications**: what this means for product or UX decisions
- **Risks and Assumptions**: what is being assumed that has not been verified
- **Codebook**: full list of codes with definitions and example utterances
