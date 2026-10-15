# Codebook

## Business website identifier convention

Every website classified as type 1, a business's own website, is written as
`BW-nnn`. There are 397, `BW-001` to `BW-397`. Numbers were assigned by a
seeded random shuffle (seed 20261015) of the business domains, so they are not
alphabetical, not ordered by how often a website was cited and not ordered by
anything else that can be read off the number.

An identifier stands for one registrable domain and is the same in every file
and every row. Inside the pre-registration copies an identifier appears in
square brackets, `[BW-077]`, marking a substitution made during preparation.

Each business website carries a `business_category`: the categories of the
questions it was cited for (Hotels, Clinics, Food, Education, Professional,
Retail), comma separated. It describes where the website was cited, not a
judgment of what the business does.

## Joins

`run_id` joins runs.csv and citations.csv. `question_id` (`Q01` to `Q24`) joins
both to questions.csv. `query_id` is the identifier the pre-registration and the
publisher's archive use for a question in one language: `VN-EN-nn` for English,
`VN-VI-nn` for Vietnamese, with `nn` the same number as `question_id`.

## questions.csv

24 rows, one per question. From `preregistration/questions.md`.

| Column | Type | Description |
| --- | --- | --- |
| question_id | string | `Q01` to `Q24`. |
| query_id_en | string | `VN-EN-nn`. |
| query_id_vi | string | `VN-VI-nn`. |
| category | string | Hotels, Clinics, Food, Education, Professional or Retail. Four questions each. |
| intent | string | Find, Choose, Price or Practical. |
| city | string | HCMC (8), Hanoi (7), Nha Trang (7) or Vietnam-wide (2). |
| english | string | The English question as asked. |
| vietnamese | string | The Vietnamese question as asked. Written by Claude (`claude-opus-5-5`). |
| declared_change | string | Every deliberate difference from the English, declared before the check. |
| gemini_back_translation | string | The checker's independent English back-translation. The checker (Gemini, `gemini-3.8-flash`) saw only the Vietnamese. Machine output, quoted verbatim. |
| natural | string | The checker's verdict on whether a Vietnamese speaker would ask it this way: `yes` on 23 questions, `contested` on Q23. |

Q05 and Q08 drop an English qualifier ("for foreigners", "English-speaking"), so
in Vietnamese they ask a broader question than in English. Q11 drops "dinner".
Q23 is contested between two registers with the same meaning. See
`preregistration/questions.md`.

## runs.csv

384 rows, one per answer. Ordered by collection time. Failed transport attempts
are not answers and are not included (amendments entry 2).

| Column | Type | Description |
| --- | --- | --- |
| run_id | string | `R001` to `R384`, in collection order. |
| question_id | string | Joins to questions.csv. |
| query_id | string | `VN-EN-nn` or `VN-VI-nn`. |
| platform | string | chatgpt, claude, perplexity or gemini. 96 answers each. |
| language | string | `en` or `vi`. |
| repetition | integer | `0` or `1`. Each question was asked twice per platform and language. |
| date | ISO date | Date of collection, UTC. 2026-10-15 on every row. |
| timestamp_utc | string | Time the call was made, UTC, to the second. |
| model_id | string | The model identifier the provider reported: `gpt-5.6-sol`, `claude-opus-5-5`, `sonar` or `gemini-3.8-flash`. |
| effort | string | The reasoning effort setting for the platform, as recorded in the wave configuration. No effort was sent on any platform. |
| search_invoked | boolean | Whether the assistant used web search for this answer. An answer given without searching cites nothing and is still an answer. |
| cited_websites | integer | Distinct websites the answer cited. Equals the number of rows for this run in citations.csv. |
| retrieved_websites | integer | Distinct websites the assistant retrieved but did not cite. Blank where the platform does not expose this. See below. |
| map_links_excluded | integer | ChatGPT map links removed from the cited set under the classification rules. Zero on every row. |
| finish_reason | string | The provider's own stop reason, as returned. No answer was truncated. |

`retrieved_websites` counts distinct registrable domains among the sources the
assistant returned but did not attach to a statement, minus any domain the same
answer cited. Its meaning differs by platform. On ChatGPT and Claude it is a set
the assistant fetched. On Perplexity it is the set of search results the answer
showed but did not reference, not a fetch log. On Gemini every grounding source
was attached to a statement, so it is zero on every row. The pre-registration
reports only cited sources in its numbers, and retrieved counts are not
comparable between platforms. Every platform exposed a retrieved layer in this
wave, so no cell is blank.

## citations.csv

1,544 rows, one per answer and cited website. The counting unit of the study: a
website cited several times in one answer is one row, and a website cited in
both repetitions is two rows.

| Column | Type | Description |
| --- | --- | --- |
| run_id | string | Joins to runs.csv. |
| question_id | string | Joins to questions.csv. |
| query_id | string | As in runs.csv. |
| platform | string | As in runs.csv. |
| language | string | As in runs.csv. |
| repetition | integer | As in runs.csv. |
| date | ISO date | As in runs.csv. |
| domain | string | The registrable domain cited, for example `trip.com` or `mytour.vn`, or a `BW-nnn` identifier for a business's own website. Never a URL. |
| type_code | integer | 1 to 9. See below. |
| type | string | The type label. |
| scope | string | `VN` (based in Vietnam), `INT` (based outside Vietnam) or `UND` (undetermined). |
| business_category | string | For `BW-nnn` rows only. See above. |

Types, decided by what a website is for, one per domain, in the order the rules
apply them:

| type_code | type |
| --- | --- |
| 6 | official or government |
| 7 | social media |
| 8 | forum or community |
| 2 | map or business listing |
| 3 | review platform |
| 4 | booking or marketplace |
| 5 | editorial or media |
| 1 | business's own website |
| 9 | other |

Scope belongs to the brand that runs the website, not the domain ending and not
the place the page is about. Full rules and worked examples are in
`preregistration/classification.md`, with the four points the frozen rules left
open settled in amendments entry 3.

Cited URLs were reduced to registrable domains with the ICANN section of the
Public Suffix List. Gemini returns its sources as redirect links. Each was
resolved to the target its own redirect declared, and none was left unresolved.

## numbers.json

The six pre-registered numbers (spec section 8), copied unchanged from the
analysis. They were computed with the first coder's codes, which are the codes
in citations.csv.

| Key | What it holds |
| --- | --- |
| coverage | Answers planned, collected, truncated and with or without a cited website, per platform and language. |
| source_mix | Number 1. Share of citations by type, per platform and language. |
| local_share | Number 2. Vietnam / (Vietnam + international), per platform and language. Undetermined is counted and left out of the denominator. |
| language_overlap | Number 3. Per question, the share of websites cited in English also cited in Vietnamese, both pooled over the two repetitions. Mean over questions and pooled ratio, for all 24 questions and without Q05 and Q08. |
| platform_overlap | Number 4. Websites shared by each pair of platforms, per language and overall, with Jaccard. |
| top15 | Number 5. The most cited websites per platform and language, by number of answers citing them. A business's own website appears as "a business website (category)". Ties at 15th place that would run past 15 are reported as a group. |
| no_search | Number 6. Share of answers given without searching, per platform and language. |

## Preparation applied to every file

1. Every domain classified as a business's own website replaced with its
   `BW-nnn` identifier, in the data files and in the pre-registration copies.
2. Cited URLs reduced to registrable domains. No URL path, query or page title
   appears in the data files. In the pre-registration copies, URL paths in the
   worked examples are replaced with `[path withheld]`.
3. Answer texts and the providers' raw responses left out.

No answer was dropped. No code was altered. No number in numbers.json was
altered.
