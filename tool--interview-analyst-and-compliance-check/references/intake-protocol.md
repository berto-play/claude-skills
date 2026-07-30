# Intake Protocol

Detailed procedures for Step 1 of the Interview Analysis & Compliance Check skill.
These rules apply before any analysis begins.

---

## File Labelling

Label each file at the top before uploading if possible:

```
Document type: [interview / meeting / regulatory / bando / guideline / MDA / third-party]
Date: [date if known]
Participants: [names or roles if applicable]
```

If labels are missing the skill infers and confirms with the user before proceeding.

---

## Image Classification

Images are not automatically classified. When an image is provided, ask before
proceeding:

```
I can see an image in the provided inputs.
Before I classify it I need to know:
→ Is this a photo or scan of a regulatory document?
→ Is this interview notes, a whiteboard, or meeting material?
→ Is this something else?

I will not assign an authority level until confirmed.
```

---

## Volume Management

Scale processing to input size.

| Interviews provided | Action |
|---|---|
| 1 to 3 | Proceed directly |
| 4 to 6 | Flag quality risk, offer to batch into sets of 3 |
| 7 to 10 | Recommend batching. Confirm before starting. |

If recommending batching:

```
You have provided [N] interviews. Processing all at once
increases the risk of reduced accuracy in middle sections
due to context length.

Recommended: process in batches of 3, then synthesise
findings across batches at the end.

Would you like to batch, or proceed as one session?
```

---

## Chronological Ordering

Before analysis begins, attempt to establish timeline order from dates in
filenames, document headers, participant mentions, or file metadata.

If order cannot be determined:

```
I cannot determine the chronological order of [N] inputs.
I will note this in the output and flag any findings where
order may affect interpretation. No sequence assumptions made.
```

If the user has pre-ordered the inputs, confirm this in the intake registry
and proceed in the order provided.

---

## URL Handling

When a URL is provided, fetch its content before classification. Then classify
the fetched content using the same rules as any other input.

```
I have fetched the content at [URL].
Classifying as: [type based on content]
Assigned rank: [rank]
Is this correct?
```

Do not classify a URL by its address alone. Classify by what the content
actually contains after fetching. If the URL cannot be fetched, say so
and ask the user to paste the content directly.

---

## Supersession Check

If two versions of the same document are present, flag the conflict immediately
and ask which version is current before proceeding.
