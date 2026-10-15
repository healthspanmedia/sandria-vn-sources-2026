# VN sources pilot: amendments

Study `VN-SRC-2026-10`. Append-only. Each entry records one change to the frozen pre-registration, the reason, and the date. The frozen files are never edited: an entry here is the only way they change.

## Entry 0: freeze

- **Freeze commit:** `6524dd5075675dcc0afb6ba2cdf5892cc67452ef` on branch `vn-sources-pilot`
- **Commit timestamp:** 2026-10-15T09:03:17+07:00 (2026-10-15T02:03:17Z)
- **Blob hashes at the freeze commit** (`git rev-parse 6524dd5:<path>`):

| File | Blob |
| --- | --- |
| `preregistration/vn-sources/spec-v1.0.md` | `392f214620fe206948d7dde85292712a4ca3ce23` |
| `preregistration/vn-sources/questions.md` | `36a7eccfa94795a97d7d1cb1c668bd2045ca1259` |
| `preregistration/vn-sources/classification.md` | `ee578cbf493ce7bc120ec54f2280827611f50087` |
| `wave-vn-sources/wave-config.json` | `b0dcae1645e96b9caf9d633a28fd3ebdd8448bb1` |
| `wave-vn-sources/inputs/query-set-v1.0.json` | `6441d33ba94f81d41521b7b7f25521a588f2fc6e` |
| `wave-vn-sources/inputs/jobs.json` | `5866090a7f022c72443863d8ec31481041b305e1` |

- **What ran before the freeze:** the pre-flight grounding probes (4 platforms), step 0 (2 calls per platform on English question 1, in `archive/vn-sources/step0/`), and the two-round Gemini translation check (`archive/vn-sources/translation-check/`). None of these is a study observation.
- **Open before dispatch** (recorded, not resolved here): `scripts/predispatch_gate.py` checks KH-baseline artifacts under `--purpose study` and fails this wave on clauses A5.7, A16.3, A12.1/A4.3/§2.4 and, at dispatch, A10/§8. Its study-archive markers do not include `archive/vn-sources`. Whether the gate is generalised, or this wave is run under a recorded exception, is the operator's decision. It is made as an amendment before the first job.

Nothing dispatched as of this entry.

## Entry 1: pre-dispatch gate scoped to the study it was written for (ruling, 2026-10-15)

- **Ruling (Sumesh, 2026-10-15):** four clauses of `scripts/predispatch_gate.py` check KH-baseline artifacts and apply only to the KH-baseline study: A5.7 (job count against `counts.json`), A10/§8 (the KH truth set, `preregistration/kh-baseline/truth-set-v1.0.csv`), A16.3 (the brand-launch deadline) and A12.1/A4.3/§2.4 (the Arm 2 toggle log). `archive/vn-sources` joins the gate's study-archive paths.
- **What changes:** for a study wave whose wave config `study_id` is not KH-baseline (`KH-BASE-*`), these four clauses return not-applicable with a recorded reason. They do not pass. For KH-baseline they run exactly as before. A wave that writes into the KH-baseline or calibration archive while declaring a non-KH `study_id` is refused, so the scoping cannot be used to drop KH clauses on KH's own archive. A16.2 (KH-ID-08's prediction) is not in the ruling and is unchanged.
- **Why:** this wave has none of those artifacts. Without the change, the gate could only pass it by running the study as `--purpose commercial` or with `--skip-gate`, and both are worse. Recorded in entry 0 as open before dispatch.
- **Order:** this entry is committed before the code change it describes.
- **Verification required before dispatch:** the gate in inspection mode passes this wave, and gives KH-baseline the same verdict on every clause as before the change.
- **Also ruled 2026-10-15:** question 23 keeps the frozen round-2 text, marked contested. The counting unit in `classification.md` section 2 (a domain counts once per run) is approved. The four model pairs are approved as resolved.

Nothing dispatched as of this entry.

## Entry 2: what the dispatch recorded that this study does not use, and the transport failures (2026-10-15)

Written after dispatch. It changes no rule and no number. It records two facts about the archive so that nobody reading the rows later mistakes them for study data.

- **Dispatched:** from the merge of PR #5 (`7a294ef`), branch `vn-sources-wave`, gate 24 of 24 (20 pass, 4 not applicable under entry 1). Sequential, one call in flight, ChatGPT spaced 16 s, $55 stop. First row 2026-10-15T02:45:39Z, last 2026-10-15T07:22:11Z. Archive `archive/vn-sources/runs.jsonl`, 401 rows, committed in `d83f728`.
- **KH-baseline's consumer search-setting observations are stamped on every row.** `scripts/dispatch.py` reads KH-baseline's Arm 2 log and writes two fields on each row: `is_consumer_default` and `toggle_observation_date`. Values are claude `true` / 2026-08-15, perplexity `true` / 2026-08-14, chatgpt and gemini `null` / 2026-08-15. These are KH-baseline's observations of the consumer apps from August, not observations made for this study. This study does not use them, and no pre-registered number reads them. The rows are left as written, because the archive is append-only. The field `family` is an empty string on every row, for the same reason (the jobs carry no KH query family), and is likewise unused.
- **17 transport failures, all retried successfully.** On the first pass, 17 calls failed before any provider answered: 16 with a local DNS failure (`[Errno 8] nodename nor servname provided, or not known`) and 1 with a connection reset (`[Errno 54]`). They came in bursts (8 within one second at 05:24:39Z, 4 at 06:15:32Z), which is this machine losing its network, not a provider refusing. The failed keys and their first-pass timestamps (UTC):

| Key | Failed at |
| --- | --- |
| chatgpt / en / VN-EN-09 / rep 0 | 03:16:17 |
| chatgpt / en / VN-EN-24 / rep 1 | 03:17:48 (connection reset) |
| perplexity / vi / VN-VI-20 / rep 1 | 03:20:44 |
| claude / en / VN-EN-11 / rep 1 | 03:22:44 |
| gemini / vi / VN-VI-08 / rep 0 | 04:27:12 |
| claude / vi / VN-VI-09 / rep 1 | 05:24:39 |
| claude / vi / VN-VI-19 / rep 0 | 05:24:39 |
| gemini / vi / VN-VI-06 / rep 0 | 05:24:39 |
| gemini / vi / VN-VI-18 / rep 1 | 05:24:39 |
| gemini / vi / VN-VI-15 / rep 1 | 05:24:39 |
| chatgpt / vi / VN-VI-01 / rep 1 | 05:24:39 |
| claude / en / VN-EN-16 / rep 0 | 05:24:39 |
| chatgpt / vi / VN-VI-07 / rep 0 | 05:24:39 |
| perplexity / vi / VN-VI-18 / rep 1 | 06:15:32 |
| gemini / en / VN-EN-22 / rep 0 | 06:15:32 |
| claude / en / VN-EN-02 / rep 1 | 06:15:32 |
| chatgpt / en / VN-EN-18 / rep 1 | 06:15:32 |

  One resume of the dispatcher on the same archive retried all 17, attempt 2, and all 17 returned `ok` (last at 07:22:11Z). Under spec section 9 the failed rows are not observations and are excluded. Each of the 384 keys has exactly one `ok` row, so coverage is 48 of 48 in every platform-by-language cell. There were no permanent failures, no rate-limit-exhausted keys, no truncations and no budget or window halt.
- **Spend:** $28.30 by the dispatcher's rate cards (ChatGPT $16.36, Claude $9.96, Gemini $1.40, Perplexity $0.58), under the $55 stop.

## Entry 3: analysis rules adopted for this and future markets (ruling, 2026-10-15)

Written after the analysis, before anything is published. The pre-registration was silent on the points below, and the analysis (`wave-vn-sources/analysis/`, README section "Choices made where the frozen rules were silent") had to decide them. Sumesh adopted them on 2026-10-15 as rules for this pilot and for the Singapore, Dubai and Norway waves, which use this method unchanged.

1. **Language overlap (spec section 8, item 3).** For each question, the English and Vietnamese cited-domain sets are each pooled over both repetitions. The question's share is |EN ∩ VI| / |EN|. The platform figure is the **mean of the per-question shares** over questions with at least one English-cited domain, and the number of such questions is reported. The **pooled ratio** Σ|EN ∩ VI| / Σ|EN| is always reported alongside.
2. **Top 15 ties (spec section 8, item 5).** Domains are ranked by the number of runs citing them. Every domain with more runs than the 15th-placed domain is listed. If listing the domains tied at the 15th place's count would take the list past 15, they are not listed one by one: they are reported as a tied group, with its size, its run count and its mix of types. A business's own website is never named in either part.
3. **Domain reduction (classification.md section 1, step 4).** A host is reduced to its registrable domain with the **ICANN section of the Public Suffix List**. The private section is not applied, so a page on a site-hosting platform counts as that platform. Every host that reduces to a different domain is listed and checked by eye before coding. A Gemini grounding redirect resolves to the target declared in its own `Location` header, read without following any further redirect.
4. **Scope evidence (classification.md section 4).** Under evidence (a), the site's own About, Contact or footer, a **footer address** and a **contact phone number** are evidence of where the brand is headquartered, alongside a registration number (still decisive). A phone number with the country's code, or an address in the country, counts towards the market's own scope. Brand knowledge used under (b) is marked as such in the evidence column.
5. **Coders are measured platforms; neither scored answers.** In this pilot the first coder was Claude (`claude-opus-5-5`) and the second coder was Gemini (`gemini-3.8-flash`). Both are platforms the study measures. Neither scored, rated or judged any answer, its own or another platform's: each coded **domains**, by the rules, from a home-page evidence packet. Some of the domains each coded were cited in its own platform's answers. Neither was shown which platform cited a domain, and the second coder never saw the first codes. Future waves record the coders the same way, together with the fact that a coder may be a measured platform.

Also settled 2026-10-15 (Sumesh): the first codes stand for all five second-coding disagreements (type agreement 57 of 61, κ 0.878; scope 58 of 61, κ 0.889). No published number changes, because the numbers were computed with the first codes.

Nothing published as of this entry.
