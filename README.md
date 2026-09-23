# human_vs_llm_coling

Annotations and agreement metrics from a study comparing five human annotators and
five LLMs on three structured sentiment analysis tasks in Brazilian Portuguese:
ABSA, ASQP and SSA.

Each task uses the same 50 sentences for every annotator, human or model.

## Contents

```
annotations/
  human/   absa|asqp|ssa_per_annotator.json    annotations of each human annotator (H1-H5)
  llm/     absa|asqp|ssa_<model>.json          annotations of each of the five models
consensus/
  absa|asqp|ssa_human_consensus.json           consensus of the five humans
  absa|asqp|ssa_llm_consensus.json             consensus of the five models
metrics/
  absa|asqp|ssa_pairwise_agreement.json        all annotator pairs: Cohen's Kappa, F1,
                                               Krippendorff's Alpha by element and by group
  absa|asqp|ssa_consensus_metrics.json         each annotator against the human consensus,
                                               against the LLM consensus, and within-group Alpha
```

Both consensuses are built with the same rule: tuples are grouped by a task-specific
identity (aspect for ABSA, category-aspect for ASQP, aspect-expression for SSA), groups
are admitted by greedily minimising the summed Jaccard distance to the annotators' sets,
and each remaining element is decided by majority vote.

## Models

DeepSeek-V4 Pro, Mistral-Nemotron, Nemotron 3 Super 120B A12B, Nemotron 3 Ultra 550B A55B
and GPT-OSS 20B, all served through the NVIDIA NIM API with default settings and
temperature 0.7. Each model annotated the sample once.

## Anonymisation

- Human annotators appear only as `H1`-`H5`, in the same order in every file.
- One review in the sample ends with its author's signature; that string is replaced by
  `[PERSON]` in the texts and in every annotation that selected it as a holder.
- Annotations made from the platform owner's own account, which are not part of the
  study, were removed.

## Source corpora

The reviews come from TripAdvisor data released by three shared tasks: ABSAPT 2024
(Bender et al.), ASQP-PT 2025 (Lopes et al.) and the SSA-PT corpus (Campos et al., 2026).
Only the 50-sentence sample annotated in this study is included here, and the annotations
are ours; the underlying texts belong to those releases.

## Not included

The raw model responses, including the reasoning traces quoted in the paper, are not part
of this release; only the parsed annotations are.

## Task notes

- In SSA only the holder was annotated; aspect, expression and polarity are frozen from
  the source corpus and are identical for every annotator, so agreement on those elements
  is an artefact and should not be read as a result.
- `null` is an explicit holder label meaning that no holder is realised in the text, not
  a missing value.
