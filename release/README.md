# Affective Experiencer Alignment — A Four-Language Resource

**Anonymous release** accompanying the NLPCC 2026 submission *"HowNet-Anchored Affective Experiencer Alignment: A Four-Language Validated Resource with Belletti–Rizzi Minimal Pairs"*.

This anonymized snapshot is provided for double-blind review. Identifiable metadata (authors, institutions, contact, funding) will be restored at camera-ready.

---

## Contents

```
release/
├── README.md                                  this file
├── LICENSE                                    MIT License
│
├── ontology/
│   └── affective_subontology_v1.1.json        55-node frozen sub-ontology extracted from
│                                              HowNet. Each node carries: id, EN/ZH labels,
│                                              4-rater adjudicated polarity (P/N/U),
│                                              role-typed DEF-tree edges, BR-class hint,
│                                              frequency, example sense IDs.
│
├── alignment/
│   ├── candidates_EN_ZH_bundled.json          OpenHowNet bundled BabelNet candidates,
│                                              48 EN + 47 ZH per-node lists.
│   ├── candidates_JA.jsonl                    262 annotator-constructed Japanese
│                                              candidates with full / partial /
│                                              no_correspondence tags + free-text
│                                              register notes (one entry per line).
│   ├── candidates_KO.jsonl                    98 annotator-constructed Korean
│                                              candidates, same schema.
│   └── canonical_selections.tsv               55-row table: per-sememe canonical
│                                              translation per language with tag,
│                                              register notes, candidate counts.
│
├── benchmark/
│   ├── items_EN.jsonl                         48 minimal-pair items per language,
│   ├── items_ZH.jsonl                         balanced 24/24 across Belletti–Rizzi
│   ├── items_JA.jsonl                         BR-I (subject-experiencer) and
│   └── items_KO.jsonl                         BR-II (object-experiencer); 192 total.
│                                              Schema: item_id, language, context,
│                                              characters (with role / animacy /
│                                              gender / position), affective_event
│                                              (sememe + lexical_item + polarity),
│                                              task_format_qa, task_format_paraphrase,
│                                              experiencer_role_position, BR class,
│                                              donor_pair_pointer (for the
│                                              affect-flipped patching extension),
│                                              naturalness annotation slots.
│
├── polarity_adjudication/
│   ├── adjudicated_polarity_v1.1.json         Per-sememe adjudicated polarity
│                                              (4-rater majority + HowNet tiebreaker)
│                                              with metadata (ordinal α=0.84, nominal
│                                              α=0.75, adjudication breakdown).
│   ├── per_sememe_disagreement_log.tsv        55-row log: per-sememe 4-rater votes
│                                              (R1–R4), adjudicated verdict,
│                                              adjudication type, HowNet hint.
│   └── master.tsv                             Wide + narrow rubric ratings per rater
│                                              per sememe.
│
└── baseline_results/
    └── sister_baseline_master.csv             Per-(model × language) zero-shot
                                               counterbalanced accuracy from 10 models
                                               on the 192-item benchmark (V100 hardware).
```

## Schema notes

- **Polarity values**: `P` = positive, `N` = negative, `U` = neutral (in TSV); `pos` / `neg` / `neutral` (in JSON).
- **Three-way correspondence tag**: `full` (conventionalized single-word equivalence),
  `partial` (overlapping but not identical subsense; register / scope differences
  documented in notes), `no_correspondence` (no single-word lexicalization, paraphrastic
  rendering required).
- **Annotation source**: `annotator_constructed` for JA / KO (trained linguist per
  language); `babelnet` for EN / ZH bundled-bridge candidates.

## Inter-rater agreement (polarity)

- Ordinal Krippendorff's α = 0.84 (firm range; Krippendorff 2018)
- Nominal Krippendorff's α = 0.75 (tentative range)
- 4 trained linguist annotators × 55 retained sememes
- 40 / 55 unanimous, 9 / 55 majority (3-of-4), 5 / 55 ties (HowNet S_C / S_E hint
  tiebreaker), 1 / 55 plurality (2-1-1)

## License

Released under the MIT License (see `LICENSE`). Redistribution of any included
lexical content sourced from HowNet or BabelNet must additionally observe those
resources' own redistribution terms.

## Citation (camera-ready will replace anonymous placeholder)

```bibtex
@inproceedings{anonymous2026hownetaffective,
  title     = {HowNet-Anchored Affective Experiencer Alignment:
               A Four-Language Validated Resource with
               Belletti--Rizzi Minimal Pairs},
  author    = {Anonymous},
  booktitle = {Proceedings of NLPCC 2026},
  year      = {2026},
  publisher = {Springer LNAI}
}
```

## Replication note

For zero-shot baseline replication: V100 GPUs; seed = 114 (forced-choice argmax
under `model.eval()` is deterministic by construction); chance = 0.50; ten models
(Qwen3-{0.6B, 4B, 14B}-{Base, Instruct}, mBERT, XLM-R-Large, Swallow-13B-Instruct,
EXAONE-3.5-7.8B-Instruct). See the paper §6 for full setup.
