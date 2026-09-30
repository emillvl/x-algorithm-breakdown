# X Algorithm Breakdown

A source-grounded technical analysis of the **May 15, 2026** open-source release of X's For You recommendation system.

The report focuses on what can be established from the public `xai-org/x-algorithm` source snapshot: the request path through Home Mixer, in-network and out-of-network retrieval, Phoenix ranking, candidate-isolation attention, multi-action prediction, filtering, Grox content understanding, and the reproducible mini-model pipeline released in May.

**Read the report:** [X Algorithm Breakdown - May 2026](report/X_Algorithm_Breakdown_May_2026.pdf)

## Why this version exists

The original report was technically dense and sometimes stated interpretations more strongly than the source justified. This revision keeps the technical core but changes the way it is presented:

- clearer, more natural prose;
- a restrained research-report layout instead of a presentation-style PDF;
- explicit separation between source evidence and interpretation;
- removal or qualification of unsupported production-scale claims;
- a clearer explanation of candidate isolation and multi-action scoring;
- source notes pinned to the May 2026 release;
- a note on later August/September 2026 releases where they materially change how the May snapshot should be read.

## Scope

The primary source is:

- `xai-org/x-algorithm` at commit [`e414c17`](https://github.com/xai-org/x-algorithm/commit/e414c171ed68266341193330bc4864bf3f3534e3), published May 15, 2026.

A second May 15 commit, [`0bfc279`](https://github.com/xai-org/x-algorithm/commit/0bfc2795d308f90032544322747caacd535f75ae), updated the Git LFS pointer for the released Phoenix artifact.

This is intentionally a **historical snapshot**, not a claim to describe the complete current production system. X later published additional production-facing Phoenix code, explicit score-weight configuration, a broader visibility-filtering stack, and further transparency tooling in August and September 2026.

## What the report covers

### Home Mixer and the candidate pipeline

The report follows the request path through query hydration, candidate sourcing, candidate hydration, filtering, scoring, selection, post-selection checks, and side effects.

### Thunder and Phoenix retrieval

Thunder supplies recent in-network content from followed accounts. Phoenix handles learned out-of-network retrieval through a two-stage recommendation design.

### Phoenix ranking and candidate isolation

The May Phoenix ranker uses a special attention mask: each candidate can attend to the viewer and interaction history, and to itself, but not to other candidates in the same inference batch. The report explains why this matters for score independence and what it does - and does not - imply about computational complexity.

### Multi-action prediction

Phoenix predicts multiple possible viewer actions rather than one universal engagement score. The report also explains an important distinction that X later made explicit: action weights scale **predicted probabilities or values**, not raw engagement counts. A visible weight therefore should not be interpreted as a universal exchange rate such as "one reply equals N likes."

### Grox, filtering, and blending

The May release added Grox content-understanding components, more hydrators and candidate sources, and ads-blending code. The report keeps these separate from the ranking model instead of treating every label or classifier as a direct ranking penalty.

### Reproducibility

The May release shipped `phoenix/run_pipeline.py`, a frozen mini Phoenix model, and a demonstration corpus. The report explains what can reasonably be tested with that environment and why it should not be treated as a local clone of X's full production feed.

## Evidence boundary

The repository is useful because it lets us inspect concrete architecture and code paths. It does **not** establish every production detail.

The report therefore avoids treating the following as known unless the source supports them:

- global traffic or request volume;
- hidden production thresholds;
- closed datasets, services, or experiments;
- permanent universal ratios between engagement types;
- the assumption that every exposed signal is a direct additive rank factor.

## Repository structure

```text
x-algorithm-breakdown/
├── report/
│   └── X_Algorithm_Breakdown_May_2026.pdf
├── LICENSE
└── README.md
```

## Sources

The report cites the pinned May 2026 source directly, including:

- the root `README.md`;
- `phoenix/README.md`;
- `phoenix/grok.py` and `make_recsys_attn_mask`;
- `phoenix/run_pipeline.py`;
- the May 15 release commits.

The current official repository is also referenced only where later documentation clarifies limitations or corrects common interpretations:

- [xai-org/x-algorithm](https://github.com/xai-org/x-algorithm)

## Author

**Emil Veliyev** - [@emillvl](https://github.com/emillvl)

## License

This repository is licensed under the [Apache License 2.0](LICENSE).

X and related trademarks belong to their respective owners. This project is an independent technical analysis and is not affiliated with or endorsed by X Corp. or xAI.
