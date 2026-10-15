# VN sources pilot: classification rules v1.0

Study `VN-SRC-2026-10`. Fixed before any dispatch, as part of the freeze recorded in `amendments.md` entry 0. Implements `spec-v1.0.md` section 7. Where this file and the spec disagree, the spec wins and the disagreement is an amendment.

## 1. From URL to domain

Applied to every URL in a row's `cited_urls`, in this order. `retrieved_urls` go through the same steps when retrieved is reported.

1. **ChatGPT Maps links.** On a ChatGPT row, a URL whose host is `maps.google.*`, `maps.apple.com` or `maps.app.goo.gl`, or whose host is `google.*` with a path starting `/maps`, is removed from the domain table. It is counted per platform and language as "map links (excluded)". On the other three platforms the same URL is kept and classified as type 2.
2. **Gemini redirects.** A `vertexaisearch.cloud.google.com/grounding-api-redirect/` URL is followed to its final URL in a post-pass (`scripts/resolve_gemini_urls.py`), off the timed call. If it does not resolve, it is counted per platform and language as "unresolved" and contributes no domain to any headline number. The chunk's `web.title` usually holds the domain (it did on all five step-0 chunks). It is used only for one sensitivity line, "with unresolved redirects attributed by title", reported beside the headline and never in place of it.
3. **Normalise.** Lowercase the host. Drop scheme, port, path, query and fragment (this removes ChatGPT's `?utm_source=openai`). Drop a leading `www.`.
4. **Registrable domain.** Reduce the host to its registrable domain (eTLD+1) with the Public Suffix List: `tw.hotels.com` and `au.hotels.com` become `hotels.com`; `guide.michelin.com` becomes `michelin.com`; `[subdomain].[BW-077]` and `[another subdomain].[BW-077]` become `[BW-077]`. Country variants of one brand on different registrable domains stay separate domains (`tripadvisor.co.uk`, `tripadvisor.co.nz`, `tripadvisor.com.vn`) but always receive the same type and scope.

## 2. Counting unit

The unit is a **(run, domain) pair**. A domain cited several times in one answer counts once for that run. A domain cited in both repetitions of a question counts twice, once per run. Source mix, local share and top-15 counts are shares of these pairs. Overlap measures (spec section 8, items 3 and 4) use sets of domains, pooled over both repetitions of a question.

## 3. Type: one per domain

Every domain gets exactly one type, decided by **what the site is for**: what a visitor goes to that domain to do. It is not decided by the page that happened to be cited. Rules are applied in this order, and the first that fits wins.

| Order | Type | Fits when the domain is... |
| --- | --- | --- |
| 1 | (6) Official or government | run by a government body, regulator, ministry, embassy, municipal or provincial portal, or national tourism authority (e.g. `.gov.vn` sites, `vietnam.travel`). |
| 2 | (7) Social media | a social network or video platform where users post (Facebook, Instagram, TikTok, YouTube, X, LinkedIn, Zalo). |
| 3 | (8) Forum or community | a discussion forum or Q&A community (Reddit, Quora, Vietnamese forums such as `voz.vn`, `webtretho.com`, `tinhte.vn`). |
| 4 | (2) Map or business listing | a map, directory or registry that lists businesses with contact details as its main purpose (Google Maps on non-ChatGPT rows, Yellow Pages, `trangvangvietnam.com`, company-registry lookups). |
| 5 | (3) Review platform | a site whose main purpose is user reviews or ratings of businesses (Tripadvisor, `foody.vn`, Yelp, Trustpilot). |
| 6 | (4) Booking or marketplace | a site where you book or buy from many different businesses (Agoda, Booking.com, Expedia, Hotels.com, Trip.com, Traveloka, Klook, Shopee, Lazada, Tiki). |
| 7 | (5) Editorial or media | a publication that writes about businesses it does not run: news, magazines, guides, travel blogs, affiliate "best of" lists, and the Michelin Guide. |
| 8 | (1) Business's own website | a site run by one business, or one chain or group, about its own goods or services. This holds even when the page is a "top 10" list, including a firm ranking its competitors. A single-brand retailer's site (one company selling many brands) is type 1, not a marketplace. |
| 9 | (9) Other | anything else: encyclopedias (Wikipedia), academic or reference sites, file hosts, and sites that cannot be identified. |

Tie-breaks inside the order:

- A review platform that also takes bookings is type 3 if reviews are what it is known for (Tripadvisor). A booking site that also shows reviews is type 4 (Agoda).
- A chain's central booking site is the business's own site (type 1), not a marketplace, because it sells only that group's hotels (`[BW-077]`).
- A hotel collection or consortium that sells rooms for many independently owned member hotels is type 4.
- A site owned by a company in another type keeps its own function. Oyster is owned by Tripadvisor but is an editorial review publication, so it is type 5.

## 4. Scope: Vietnam or international

Scope belongs to the **brand that runs the domain**, not the top-level domain and not the place the page is about.

- **Vietnam:** the brand is headquartered in Vietnam, wherever it was founded. This includes Vietnamese businesses' own sites on `.com` domains, and Vietnamese public bodies.
- **International:** the brand is headquartered outside Vietnam. This holds even when it runs a Vietnam edition, a `.vn` domain or a Vietnamese subsidiary (`shopee.vn`, `tripadvisor.com.vn`), and for a foreign chain's site about its hotel in Vietnam (`[BW-077]`).
- **Evidence, in order:** (a) the site's own About, Contact or footer; a Vietnamese business registration number (mã số thuế / giấy phép kinh doanh) for the operating brand is decisive. (b) A company profile or encyclopedia entry for the brand. (c) A `.vn` domain is not evidence by itself.
- **Undetermined:** if (a) and (b) do not settle it, scope is "undetermined". It is counted and reported, and left out of the local-share denominator. Local share = Vietnam / (Vietnam + international).

## 5. Worked examples

The examples taken from step 0 (`archive/vn-sources/step0/`) were cited on English question 1. Scope for each is stated as the rule would decide it; the coder confirms it from the evidence in section 4 when the domain appears in the wave.

| URL as cited | Domain | Type | Scope | Why |
| --- | --- | --- | --- | --- |
| `https://www.[BW-255]/[path withheld]?utm_source=openai` (ChatGPT) | `[BW-255]` | (1) own website | Vietnam | A single hotel's own site; query string and `www.` dropped. Never named in findings: shown as "a business website (hotels)". |
| `https://[subdomain].[BW-077]/[path withheld]` and `https://[another subdomain].[BW-077]/[path withheld]` (ChatGPT) | `[BW-077]` | (1) own website | International | One group's own booking site; [BW-077] is headquartered in France; both subdomains collapse to one domain. |
| `https://[subdomain].[BW-189]/` (ChatGPT) | `[BW-189]` | (1) own website | Vietnam | A Vietnamese hotel group's own site, subdomain collapsed. |
| `https://guide.michelin.com/...` (ChatGPT) | `michelin.com` | (5) editorial | International | A guide writing about hotels it does not run. |
| `https://www.expedia.com/[path withheld]` (Perplexity) | `expedia.com` | (4) booking | International | Books rooms at many hotels. |
| `https://tw.hotels.com/...` and `https://au.hotels.com/...` (Perplexity) | `hotels.com` | (4) booking | International | Country subdomains collapse to one domain. |
| `https://us.trip.com/[path withheld]` (Perplexity) | `trip.com` | (4) booking | International | Headquartered outside Vietnam. |
| `https://www.tripadvisor.co.uk/...` (Perplexity), `https://www.tripadvisor.co.nz/...` (Claude) | `tripadvisor.co.uk`, `tripadvisor.co.nz` | (3) review | International | Separate registrable domains, same type and scope; reviews are what the site is known for. |
| `https://www.oyster.com/...` (Perplexity) | `oyster.com` | (5) editorial | International | Owned by Tripadvisor but publishes editorial hotel reviews. Function decides. |
| `https://thehoteljournal.com/[path withheld]` (Perplexity) | `thehoteljournal.com` | (5) editorial | International | A publication listing hotels it does not run. |
| Gemini chunk titled `youtube.com` | `youtube.com` | (7) social | International | Redirect followed in the post-pass. Had it not resolved, it would count as "unresolved", with `youtube.com` used only in the title-attribution sensitivity line. |
| `https://vietnam.travel/[path withheld]` (KH calibration archive) | `vietnam.travel` | (6) official | Vietnam | Run by the national tourism authority; official is checked first. |
| `https://maps.google.com/?cid=...` on a ChatGPT row | (excluded) | n/a | n/a | ChatGPT Maps link: counted as "map links (excluded)". The same URL on a Claude row would be `google.com`, type 2. |
| `https://shopee.vn/...` | `shopee.vn` | (4) marketplace | International | Many sellers. The brand is headquartered in Singapore; `.vn` does not make it local. |
| `https://www.[BW-165]/[path withheld]` | `[BW-165]` | (1) own website | Vietnam | One retailer selling many brands: a single business's own site, not a marketplace. |
| `https://vnexpress.net/...` | `vnexpress.net` | (5) editorial | Vietnam | Vietnamese news publication. |
| `https://www.foody.vn/[path withheld]` | `foody.vn` | (3) review | Vietnam | User reviews of restaurants are the site's purpose. |
| A law firm's page "Top 10 accounting firms in Hanoi" on its own domain | the firm's domain | (1) own website | by the firm | The operator is one business; the listicle format does not make it editorial. |

## 6. Who codes, and the agreement check

- **First coder:** codes every domain in the wave's cited set by these rules, recording type, scope and the evidence used for scope. Named, with model ID if a model, in the analysis record.
- **Second coder:** a coder who has not seen the first codes, named the same way. Codes a 10% sample of distinct domains, drawn at random with seed 20261015 from the full cited-domain list after the wave, by the same rules.
- **Reported:** percent agreement and Cohen's kappa, separately for type (nine categories) and scope (three: Vietnam, international, undetermined). Disagreements are settled by Sumesh, and the settled code is used. Both the original and the settled codes are kept.
- These rules are not changed after codes are seen. A domain the rules cannot place is "other" and listed by name in the analysis record.
