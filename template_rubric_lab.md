# Rubric lab (v2.3)

The decisions aLoRA is trained on English, so the questions and levels below are written in English. The text under evaluation can be in any language. Tick "🔎 + hallucinatiecontrole" next to "Alles uitvoeren" before running: section 2 uses its result.

Calibration note: the adapter is greedy and answers near 100% whenever a level fits roughly. So write flaws as concrete, checkable statements, and put targets high (90% or more) for Yes/no rows.

## 1. A text and its evaluation

```prompt
as: v1
retrieval: history of pasta: Etruscans, Marco Polo, Jefferson
rewrite: Write in Dutch a short, clear text about the history of pasta, with years and people.
```

```evaluation
| type | description | 1 | 2 | 3 | 4 | 5 | rag | target | weight |
|--|--|--|--|--|--|--|--|--|--|
| Judge | How well does the text use the relevant facts from the retrieved chunks? | Most relevant facts are missing | Several relevant facts are missing | One or two relevant facts are missing | Nearly all relevant facts are used | All relevant facts are used, with years | facts about pasta: dates, people, places, events | 90% | 2 |
| Yes/no | Is every section of the text about the topic? | Yes: no section says that something is irrelevant, unrelated or not connected | No: a section says that something is irrelevant, unrelated or not connected | | | | | 95% | 3 |
| Yes/no | Is the text free of broken or invented-sounding words and sentences? | Yes: every sentence is correct, natural language | No: at least one sentence contains a garbled word or an unnatural construction | | | | | 95% | 2 |
| Yes/no | Does the text look complete overall? | Yes: meaningful start, middle and end | No: cut off at the start, middle or end | | | | | 90% | 2 |
| Scale | Is the text easy to understand? | Hard: jargon, long sentences, complex grammar | Mostly clear: little jargon, short sentences | Easy: no jargon, short sentences, simple grammar | | | | 80% | 1 |
| Choice | What is the register of the text? | Encyclopedic | Narrative | Promotional | Dialogue | Other | | Encyclopedic; Narrative | 1 |
| Choice | Which period does the text cover? | Antiquity only | Middle Ages only | Modern times only | Several periods | None of these | | | 1 |
```

Choice rows are classifiers: the options are mutually exclusive, and you include a catch-all yourself so the list is exhaustive. The adapter never knows which option you prefer. If the `target` cell names wanted options (separated by `;`), the app checks the winning label against them. If it is empty, the row only reports the label and its distribution, and `$v1.eval` passes that on.

## 2. Reusing the evaluation and the hallucination check

The rewrite works from the sources again and uses the first version only as a skeleton, so it can really change something. The second evaluation repeats the same criteria, so the two totals are comparable.

```prompt
as: v2
retrieval: history of pasta: Etruscans, Marco Polo, Jefferson, with years and sources
rewrite: Write in Dutch a corrected version of the draft below, using the sources. Keep what is correct and supported. Remove every section or sentence that is not supported by the sources or that says something is irrelevant. Fix garbled wording. Add no facts that are not in the sources. Draft: $v1 Problems found by the evaluation: $v1.eval Sentences the sources do not support: $v1.hallu
```

```evaluation
| type | description | 1 | 2 | 3 | 4 | 5 | rag | target | weight |
|--|--|--|--|--|--|--|--|--|--|
| Judge | How well does the text use the relevant facts from the retrieved chunks? | Most relevant facts are missing | Several relevant facts are missing | One or two relevant facts are missing | Nearly all relevant facts are used | All relevant facts are used, with years | facts about pasta: dates, people, places, events | 90% | 2 |
| Yes/no | Is every section of the text about the topic? | Yes: no section says that something is irrelevant, unrelated or not connected | No: a section says that something is irrelevant, unrelated or not connected | | | | | 95% | 3 |
| Yes/no | Is the text free of broken or invented-sounding words and sentences? | Yes: every sentence is correct, natural language | No: at least one sentence contains a garbled word or an unnatural construction | | | | | 95% | 2 |
| Yes/no | Does the text look complete overall? | Yes: meaningful start, middle and end | No: cut off at the start, middle or end | | | | | 90% | 2 |
| Scale | Is the text easy to understand? | Hard: jargon, long sentences, complex grammar | Mostly clear: little jargon, short sentences | Easy: no jargon, short sentences, simple grammar | | | | 80% | 1 |
```

## 3. Document selector

Both blocks ask the same question. The first may use the whole corpus; the second may only use `catotheelder.md`, so it can only answer from that one source. Compare the "Used chunks" lists under each output.

```prompt
as: alldocs
retrieval: What is said about pasta before the year 1500?
rewrite: none
```

```prompt
as: onlycato
docs: catotheelder.md
retrieval: What is said about pasta before the year 1500?
rewrite: none
```

A name that is not in the uploaded set gives a warning and the whole corpus is searched, so check the warning line if the filter seems ignored.
