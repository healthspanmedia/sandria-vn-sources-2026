# Limitations

Read this before using the data.

Items 1 to 6 concern how the answers were collected. Items 7 to 11 concern how
the cited websites were classified. Items 12 to 16 concern what was withheld and
what the record can and cannot show.

1. **Answers came through developer access, not the consumer apps.** Every
   answer was a call to the provider's API with web search available: ChatGPT
   through the Responses API, Claude through the Messages API, Perplexity
   through its chat completions API and Gemini through generateContent. What
   the consumer apps show can differ, in the model served, the search behavior
   and the sources displayed. Gemini's rate of answering without search (item 5)
   may in particular differ in the app.

2. **Memory and personalization were off.** Every call was a stateless request
   with no account, no history and no user profile. Real users may have
   some of these, and they can change the answer. Requests were not varied by
   location.

3. **The Vietnamese questions were written and checked by AI models, with no
   native reader.** Claude (`claude-opus-5-5`) wrote them. Gemini
   (`gemini-3.8-flash`, from a different company) checked them with an
   independent back-translation and a naturalness verdict. No native Vietnamese
   speaker read them for this wave. A question that reads unnaturally to a local
   may draw different sources, so this bears most on the comparison between
   languages. Question 23's wording is contested by the checker between two
   registers.

4. **Three questions ask something slightly different in each language.**
   Questions 5 and 8 drop "for foreigners" and "English-speaking" in
   Vietnamese, because a local asking in Vietnamese would not include them.
   Question 11 drops "dinner". Language overlap is reported with and without
   questions 5 and 8 in numbers.json.

5. **Gemini usually answered without searching.** Gemini used search in 32 of
   its 96 answers and Claude in 90 of 96. ChatGPT and Perplexity searched every
   time. An answer given without searching cites nothing, so Gemini's figures
   rest on far fewer answers than the others: 19 in English and 13 in
   Vietnamese.

6. **One date, two repetitions.** All 384 answers were collected on
   2026-10-15. Two repetitions show whether sources repeat, but do not measure
   how much answers vary. No confidence intervals are given. Assistant outputs
   change with the model served, the search index and the moment, and nothing
   here should be read as a stable property of any assistant.

7. **The coders were platforms the study measures.** The first coder was Claude
   (`claude-opus-5-5`) and the second coder was Gemini (`gemini-3.8-flash`).
   Each coded websites, not answers, from a home-page evidence packet. Neither
   scored, rated or judged any answer, and neither was shown which assistant had
   cited a website. Some websites each coded were cited in its own platform's
   answers.

8. **Agreement was checked on a 10% sample only.** The second coder coded 61 of
   the 613 websites without seeing the first codes. Agreement was 57 of 61 on
   type (Cohen's kappa 0.878) and 58 of 61 on scope (kappa 0.889). The first
   codes stand for all five disagreements. The other 552 websites rest on one
   coder.

9. **Some websites state no owner or location.** Scope is undetermined for 88
   of the 613 websites. These are counted and left out of the local share. The
   finding reports the range the local share would take if all of them were
   counted one way and then the other. On 27 websites the rules did not settle
   the type or scope cleanly, and the first coder's reading was used. Examples
   are franchise hotels and accounting network member firms, coded by the brand
   that names the site.

10. **One type per website, decided by what the site is for.** A business's own
    page listing its competitors still counts as a business website. A booking
    site's blog post still counts as a booking site. The type describes the
    website, not the page that was cited.

11. **"Cited" is each platform's own mechanism.** ChatGPT marks citations as
    annotations, Claude as citation blocks, Perplexity as numbered markers and
    Gemini as grounding supports. The study counts what each platform marks.
    Comparing platforms compares four different citation mechanisms as well as
    four assistants.

12. **Answer texts are withheld.** The record says which websites an answer
    cited, not what the answer said or which statement each website was attached
    to. Full URLs and page titles are withheld too, so a page cannot be checked
    from these files. The publisher keeps the complete archive.

13. **Business websites are coded and the mapping is not published.** Anyone
    working from these files cannot tell which business a `BW-nnn` identifier
    stands for. Findings can be stated about the types and spread of sources,
    not about any particular business. Coding is not anonymization. Asking the
    same questions again would surface many of the same websites, and the
    question category narrows each identifier.

14. **Whether a cited source changed the answer is not shown.** A citation shows
    that an assistant attached a website to a statement. It does not show that
    the website shaped the answer, or that the assistant read it.

15. **The record covers these questions, these three cities, these four
    assistants and this date.** It says nothing about other places, other
    questions or other times. Vietnam is the first market measured with this
    method. Whether the same pattern holds in other countries and languages is
    not shown.

16. **Some analysis rules were settled after the data was in.** The frozen rules
    did not say how to average language overlap, how to break ties at 15th
    place, which part of the Public Suffix List to use or whether a footer
    address counts as scope evidence. These were decided during the analysis and
    adopted as amendments entry 3, before anything was published. They are
    stated so they can be challenged.
