# Sandria VN sources 2026: which websites four AI assistants cited for everyday questions about Vietnam, in English and Vietnamese

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23394836.svg)](https://doi.org/10.5281/zenodo.23394836)

Measured by Sandria™. Finding and method: https://sandria.ai/findings/language-changes-ai-sources/

## What this is

A dated record of the websites four AI assistants cited when asked 24 everyday
questions about businesses and services in Vietnam, once in English and once in
Vietnamese. All 384 answers were collected on one date, 2026-10-15.

The four assistants are ChatGPT, Claude, Perplexity and Gemini, each reached
through its developer API with web search available. Each of the 24 questions
was put to each assistant twice in each language, giving 384 answers. The
questions cover hotels, clinics, food, education, professional services and
retail in Ho Chi Minh City, Hanoi and Nha Trang, plus two about Vietnam as a
whole. No question names a business.

A website is cited when the answer attaches it to a specific statement. Each
cited website counts once per answer. The record holds 1,544 such citations
across 613 websites, each classified by type (nine types) and by scope
(based in Vietnam, international or undetermined).

The study was pre-registered. The questions, the classification rules and the
six reported numbers were fixed and committed before the first question was
asked. The pre-registration and its four amendment entries are in
`preregistration/`.

## What this is not

This is not a ranking of businesses or websites, and not an evaluation of any
assistant's accuracy. It does not say whether a cited website is correct, or
whether it changed what an answer said.

It is not a record of the answers. Answer texts, full cited URLs and page titles
are withheld. What is here is which websites were cited, by which assistant, for
which question, in which language.

It is not a record of the consumer apps. Developer access can behave differently.
LIMITATIONS.md sets out this and every other limit, and should be read before
the data is used for anything.

## Why business websites are coded

Every website classified as a business's own website appears as a stable
identifier of the form `BW-nnn`, together with the categories of the questions
it was cited for. 397 of the 613 websites are coded this way. Directories,
booking sites, review platforms, media, official, social and forum websites
keep their domain names.

Sandria never names a business in its findings. Publishing a business's domain
beside a count of how often AI assistants cited it would read as a ranking of
that business, which this record is not. Coding keeps the structure of the
citations open to examination without that effect.

The mapping from identifier to domain is held by the publisher and is not
published.

## Files

| File | Rows | What it holds |
| --- | --- | --- |
| questions.csv | 24 | The question set in English and Vietnamese, with category, intent, city, declared translation change, Gemini back-translation and naturalness verdict. |
| runs.csv | 384 | One row per answer: platform, language, repetition, date, model, effort, whether search was used, counts of cited and retrieved websites, finish reason. No answer text. |
| citations.csv | 1,544 | One row per answer and cited website: the website's domain (or `BW-nnn`), type and scope. No URLs. |
| numbers.json | | The six pre-registered numbers, exactly as computed in the analysis. |
| preregistration/ | 4 files | The frozen specification, question set and classification rules, with the amendments. See preregistration/README.md. |

CODEBOOK.md describes every column of every file.

## Status

Version 1.0.0. DOIs are registered with Zenodo when the release is published.

Corrections go to hello@sandria.ai. See NOTICE.md.
