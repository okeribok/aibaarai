# Rubric lab (v2.3)

The decisions aLoRA is trained on English, so the questions and levels below are written in English. The text under evaluation can be in any language.

## 1. A text and its evaluation

```prompt
as: v1
retrieval: history of pasta: Etruscans, Marco Polo, Jefferson
rewrite: Write in Dutch a short, clear text about the history of pasta, with years and people.
```

```evaluation
| type | description | 1 | 2 | 3 | 4 | 5 | rag | target | weight |
|--|--|--|--|--|--|--|--|--|--|
| Judge | Does the text use the relevant facts from the retrieved chunks? | Several relevant facts are missing | One or two relevant facts are missing | All relevant facts are used | | | facts about pasta: dates, people, places, events | 70% | 2 |
| Yes/no | Does the text look complete overall? | Yes: meaningful start, middle and end | No: cut off at the start, middle or end | | | | | 80% | 3 |
| Scale | Is the text easy to understand? | Hard: jargon, long sentences, complex grammar | Mostly clear: little jargon, short sentences | Easy: no jargon, short sentences, simple grammar | | | | 60% | 2 |
| Choice | What is the register of the text? | ✓ Encyclopedic | ✓ Narrative | Promotional | Dialogue | | | 70% | 1 |
```

## 2. Reusing the evaluation

```prompt
as: v2
retrieval: history of pasta
rewrite: Improve the text below. Fix the failed checks and add no new facts. Write in Dutch. Text: $v1 Evaluation: $v1.eval
```

```evaluation
| type | description | 1 | 2 | 3 | 4 | 5 | rag | target | weight |
|--|--|--|--|--|--|--|--|--|--|
| Yes/no | Did the text fix the problems listed in this evaluation: $v1.eval | Yes: the listed problems are fixed | No: the listed problems remain | | | | | 70% | 2 |
| Scale | How much of the first version's content is kept? | Little | Most | Nearly all | | | | 50% | 1 |
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
