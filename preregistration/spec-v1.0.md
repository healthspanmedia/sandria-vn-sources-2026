# Sandria pre-registration: Which sources AI assistants cite for businesses in Vietnam

**Status: FROZEN, v1.0, 2026-10-15.** Study `VN-SRC-2026-10`. The freeze is the commit that adds this file. Its hash, and the blob hashes of this file, `questions.md` and `classification.md`, are recorded in `amendments.md` entry 0, not here. Any change after the freeze is an amendment in `amendments.md`, never an edit to this file.

Approved draft: Sumesh, 2026-10-15. Changes from the draft are only those that step 0 or the translation forced, and each is marked **[step 0]** or **[translation]** where it appears. They are also listed at the end.

Purpose: `study`. Archive committed to git (`archive/vn-sources/`). Output: one entry on sandria.ai/findings carrying "Measured by Sandria". This is the PILOT for a four-market series (Vietnam, Singapore, Dubai, Norway). Its second job is to settle the method so the other three markets use it unchanged.

## 1. The question

When people ask AI assistants about businesses and services in Vietnam, which sources do the answers draw on, and does that change with the assistant and with the language of the question?

## 2. What we measure

- Cited: a source the answer attaches to a specific statement.
- Retrieved: a source the assistant fetched while answering but did not attach to any statement.
- Not observable: where a platform does not reveal what it retrieved, that layer is recorded as "not observable", never inferred from citations. This rule follows the Ghim AI study (R39-A).

Only "cited" feeds the headline numbers. "Retrieved" is reported where exposed.

**[step 0]** All four platforms separate cited from retrieved in their raw responses (section 4), so none is labelled "sources shown". For Perplexity, "retrieved" means search results the answer showed (returned in the response) but did not reference. It is a shown set, not a fetch log, and is labelled that way wherever it is reported.

**[step 0]** A grounded call may answer without searching. Gemini did this on the first step-0 call: the tool was offered, `groundingMetadata` was absent, and nothing was cited or retrieved. Such a row is a valid observation of what the platform does, not a failure. It counts in coverage, contributes zero cited sources, and is flagged `search_invoked = false`. The share of answers given without searching is reported per platform and language as a fixed sixth number (section 8).

## 3. Surfaces

ChatGPT, Claude, Perplexity, Gemini, all web-grounded, through their APIs. Model IDs come from the wave config the pre-flight writes (`wave-vn-sources/wave-config.json`), never hardcoded. Each passed the live grounding test on 2026-10-15 (xkcd #3305, published 2026-09-30; differential applied on ChatGPT, Claude and Gemini; not applicable on Perplexity, which has no ungrounded mode):

| Platform | Model ID | Tool config | Effort | `max_tokens_effective` |
| --- | --- | --- | --- | --- |
| ChatGPT | `gpt-5.6-sol` | Responses API, `web_search` tool, `include: ["web_search_call.action.sources"]` | medium (provider default, not sent) | 16000 |
| Claude | `claude-opus-5-5` | Messages API, `web_search_20250305`, `max_uses: 3` | medium (provider default, not sent) | 16000 |
| Perplexity | `sonar` | chat/completions, grounded by product default | n/a | 8000 |
| Gemini | `gemini-3.8-flash` | generateContent, `google_search` tool | provider default (none sent) | 16000 |

Every row records model ID, grounding condition, `max_tokens_effective`, `finish_reason`, `output_tokens`, `search_invoked`, `cited_urls`, `retrieved_urls` and the provider's raw JSON. Effort is recorded per pair in the wave config and applies to every row of that pair. Memory and personalisation are OFF: every call is a stateless API request with no account, history or profile. Limitation: API surfaces are a proxy for consumer apps.

**[step 0]** `include` was added to the ChatGPT call after step 0. It asks the API to return the sources it fetched and changes nothing about the model, the tool or the input. The ChatGPT grounding proof was run without it.

## 4. Step 0: where each platform puts its sources (observed 2026-10-15)

English question 1 ("What are the best boutique hotels in Ho Chi Minh City?") was asked once per platform through `runner.run()`. The raw responses are in `archive/vn-sources/step0/*-call1.response.json`. A second call per platform proved the runner fix (`*-call2.*`). Field paths are into the provider's JSON response.

| Platform | Cited (field path) | Retrieved (field path) | Call 1: cited / retrieved | Call 2: cited / retrieved |
| --- | --- | --- | --- | --- |
| ChatGPT | `output[type=message].content[type=output_text].annotations[type=url_citation].url` | `output[type=web_search_call].action.sources[].url`, minus cited. Empty unless the request sets `include: ["web_search_call.action.sources"]` | 8 / not observable (no `include`) | 8 / 104 |
| Claude | `content[type=text].citations[type=web_search_result_location].url` | `content[type=web_search_tool_result].content[type=web_search_result].url`, minus cited | 4 / 5 (9 fetched) | 5 / 4 (9 fetched) |
| Perplexity | `search_results[n-1].url` for each `[n]` marker in `choices[0].message.content` | `search_results[].url` not referenced by any marker. Top-level `citations[]` is the identical list of everything shown (10 = 10 on both calls) | 4 / 6 (10 shown) | 5 / 5 (10 shown) |
| Gemini | `candidates[0].groundingMetadata.groundingChunks[i].web.uri` for each `i` in any `groundingSupports[].groundingChunkIndices` | `groundingChunks[]` that no support points at | 0 / 0: did not search (`groundingMetadata` absent) | 5 / 0 (5 chunks, all supported) |

Gemini URIs are `vertexaisearch.cloud.google.com/grounding-api-redirect/...` redirects, resolved in a post-pass (section 7). Each chunk also carries `web.title`, which on step 0 held the source's domain.

**[step 0] Runner fix.** Before step 0 the runner kept one list, `source_urls`, scraped from the answer text plus each platform's source field. It held the cited set completely on all four platforms. On two it also held sources that were not cited: all 10 shown results on Perplexity, and every grounding chunk on Gemini. The runner now records `cited_urls` and `retrieved_urls` (retrieved is `null` when not observable, never an empty list taken to mean zero) from the fields above, together with `search_invoked` and the raw response. `source_urls` is unchanged and is not used by this study. Proven live on call 2: on every platform `cited_urls` is a subset of `source_urls`, and the counts match the hand counts from the raw JSON.

## 5. Questions

24 English, 24 naturalized Vietnamese. Six categories, four intents (find, choose, price, practical). Ho Chi Minh City 8, Hanoi 7, Nha Trang 7, Vietnam-wide 2. No question names a business. Marketing, SEO and AI agencies excluded (Sandria measuring its sister company's industry would read as self-interested).

| # | Category | Intent | English question |
| --- | --- | --- | --- |
| 1 | Hotels | Find | What are the best boutique hotels in Ho Chi Minh City? |
| 2 | Hotels | Choose | How do I choose a good hotel in Hanoi's Old Quarter? |
| 3 | Hotels | Price | How much does a mid-range hotel in Nha Trang cost per night? |
| 4 | Hotels | Practical | Which hotels in Nha Trang offer airport pickup? |
| 5 | Clinics | Find | Which dental clinics in Ho Chi Minh City are good for foreigners? |
| 6 | Clinics | Choose | How do I find a reliable international clinic in Hanoi? |
| 7 | Clinics | Price | How much do dental implants cost in Ho Chi Minh City? |
| 8 | Clinics | Practical | How do I book an appointment with an English-speaking doctor in Nha Trang? |
| 9 | Food | Find | What are the best Vietnamese restaurants in Hanoi? |
| 10 | Food | Choose | Where should I eat for a business dinner in Ho Chi Minh City? |
| 11 | Food | Price | How much does a seafood dinner cost in Nha Trang? |
| 12 | Food | Practical | Which restaurants in Ho Chi Minh City take online reservations? |
| 13 | Education | Find | What are the best English centers for children in Ho Chi Minh City? |
| 14 | Education | Choose | How do I choose an international school in Hanoi? |
| 15 | Education | Price | How much do IELTS courses cost in Ho Chi Minh City? |
| 16 | Education | Practical | Which language schools in Nha Trang offer trial classes? |
| 17 | Professional | Find | What are the best accounting firms for small businesses in Hanoi? |
| 18 | Professional | Choose | How do I find a reliable lawyer for company registration in Hanoi? |
| 19 | Professional | Price | How much does company registration cost in Vietnam through a service provider? |
| 20 | Professional | Practical | Which logistics companies in Ho Chi Minh City handle international shipping? |
| 21 | Retail | Find | Where can I buy authentic electronics in Nha Trang? |
| 22 | Retail | Choose | What are the best places to buy furniture in Hanoi? |
| 23 | Retail | Price | How much does a refurbished iPhone cost in Vietnam? |
| 24 | Retail | Practical | Which stores in Nha Trang deliver same day? |

The Vietnamese set, with every declared change, is in `questions.md`. It was written by Claude (`claude-opus-5-5`) and checked by Gemini (`gemini-3.8-flash`, a different company) on 2026-10-15, with back-translation and a naturalness check. No native human reader for this wave; stated as a limitation, bearing most on language overlap. Native readers in later waves.

**[translation]** Two English qualifiers are deliberately absent from the Vietnamese, because a local asking in Vietnamese would not include them: "for foreigners" (question 5) and "English-speaking" (question 8). Question 11 also drops "dinner". Questions 5 and 8 therefore ask a broader question in Vietnamese than in English. Language overlap (section 8, item 3) is reported for all 24 questions and again without questions 5 and 8. Question 23's Vietnamese wording is contested by the checker between two registers with the same meaning (`questions.md`). It runs as frozen.

## 6. Size

2 repetitions. 24 x 2 x 4 x 2 = 384 runs, one seeded shuffle across platform, language, question and repetition (seed 20261015; longest run of one language 7 jobs, of one platform 5). One call in flight; ChatGPT spaced 16 s (as KH). About USD 37 on the KH cost model. **[step 0]** Step 0's own costs give about USD 31 at current rates: ChatGPT about USD 0.20 a call (promotional rate to 2026-11-21; about USD 0.25 at the standard rate), Claude 0.10, Gemini 0.02, Perplexity 0.01. Vietnamese answers will tokenize longer, so USD 37 stands as the estimate. Hard stop USD 55, carried in the wave config as `budget_ceiling_usd` and read by `scripts/dispatch.py`. Spend is re-read from the append-only archive, so the stop survives restarts.

## 7. Classification

Cited URLs resolved to domains (Gemini redirects in a post-pass; unresolved counted). ChatGPT Maps search links excluded from the domain table and counted separately. One type per domain: (1) business's own website, (2) map or business listing, (3) review platform, (4) booking or marketplace, (5) editorial or media, (6) official or government, (7) social media, (8) forum or community, (9) other. Scope: Vietnam or international. 10% blind second coding; agreement reported.

The rules, the order they are applied in, and worked examples are in `classification.md`, fixed before any dispatch.

## 8. Pre-registered numbers

1. Source mix by platform and language.
2. Local share by platform and language.
3. Language overlap per platform (share of a question's English-cited domains also cited in Vietnamese). **[translation]** Also reported without questions 5 and 8.
4. Platform overlap (shared cited domains per platform pair).
5. Top 15 domains per platform per language. Directories, reviews, media, official and social named; a business's own website never named, shown as "a business website (category)".
6. **[step 0]** Share of answered runs given without searching (`search_invoked = false`), by platform and language.

Descriptive only. Two repetitions show whether sources repeat but do not measure variation; no confidence intervals.

## 9. Failures and coverage

Failed rows are not observations. Rate-limited cells are missing data, reported as coverage. A platform-by-language cell under 90% coverage carries no headline number. Truncated answers flagged and excluded from citation counts. **[step 0]** The draft said "all three raw `finish_reason` spellings". There are four, and the dispatcher's `TRUNCATION_FINISH_REASONS` matches all four exactly: `length` (Perplexity), `max_tokens` (Claude), `MAX_TOKENS` (Gemini), `max_output_tokens` (ChatGPT, Responses API). No parallel dispatch.

## 10. Independence

Sandria takes no money from the AI platforms it measures: assistants, chatbots, agents or their makers. Businesses, including GKZ Labs clients, may appear like any other. No business is named in findings.

## 11. What this will not show

Whether a cited source changed the answer; what consumer apps show; anything about a platform below 90% coverage; anything beyond these three cities, six categories and the dates run.

## Changes from the approved draft

1. Section 4's expected table replaced by the observed field paths and counts. [step 0]
2. ChatGPT retrieved is observable (the draft expected "likely not observable"), by adding `include` to the call. [step 0]
3. Perplexity's retrieved is a shown set; Claude's and ChatGPT's are fetched sets. Labelled as such. [step 0]
4. A run answered without searching is a valid observation, flagged and reported as number 6. [step 0]
5. The runner records cited and retrieved sources separately; `source_urls` is not used. [step 0]
6. Four truncation spellings, not three. [step 0]
7. Model IDs and effort recorded in section 3; cost estimate checked against step 0. [step 0]
8. Questions 5, 8 and 11 drop an English qualifier; language overlap also reported without 5 and 8; question 23 contested. [translation]
