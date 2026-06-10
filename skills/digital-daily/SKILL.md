---
name: digital-daily
description: Create Korean "디지털 데일리" news briefs from user-provided news URLs using the bundled format.md editorial template. Use when the user asks for 디지털 데일리, 금융권 Digital 현황, 금융권 Digital 동향 및 정책, Digital 스터디 365, daily digital finance news summaries, or URL-based Korean news digest formatting.
---

# Digital Daily

## Overview

Create a Korean 『디지털 데일리』 brief from URLs supplied by the user. The output must follow the three-section editorial format in `references/format.md`, not a generic bullet summary or article rewrite.

Always read `references/format.md` before drafting the final response. Treat it as the formatting contract for card order, labels, length, and restrictions.

When the input comes from `digital-daily-curator`, treat its `confidence` values as title-only curating confidence. Do not use `confidence` as the main basis for final article ordering or section classification.

## Workflow

1. Collect all URLs and any requested date from the user's message.
2. Use today in the user's local timezone as the news collection and article selection date unless the user provides another news date. The header date is different: it must use the delivery date. Normally the delivery date is the next calendar day after the news collection date; if the news collection date is Friday, the delivery date is the following Monday. If the user explicitly provides a delivery date, use that date for the header.
3. Open each URL. If direct access fails, search for the canonical article title or source page.
4. Extract the title, source, publication context, concrete facts, key figures, institutions, policies, products, and business impact.
5. Classify each article into exactly one section based on article content, not curator `confidence`:
   - `(1) 금융권 Digital 현황`: financial company digital services, platforms, products, AI adoption, fintech business moves.
   - `(2) 금융권 Digital 동향 및 정책`: regulation, policy, supervision, security, infrastructure, market-wide digital trends.
   - `(3) Digital 스터디 365`: blog-style or magazine-style study posts about IT, digital, AI, productivity, developer tools, or transformation. Treat these as learning resources, not news articles.
   - Do not put ordinary news articles, press reports, newsroom columns, op-eds, interviews, or Naver News articles into `(3) Digital 스터디 365`. If a news article does not fit `(1)` or `(2)`, exclude it from the digest rather than moving it to `(3)`.
6. Order items by final content-based editorial importance within each section. Use the order explicitly requested by the user when one is provided. Otherwise, apply the Final Ordering Guidance in this skill after reading article bodies. If content-based importance is effectively the same, use curator output order as the next tie-breaker, and use `confidence` only after that.
7. Produce the final digest using `references/format.md`.
8. Save the completed final digest to the daily history file before returning it to the user.
9. Delete saved final digest history files older than 7 days.
10. Preserve uncertainty: if a fact cannot be verified from the article, omit it or state the limitation in the summary rather than guessing.

## Final Digest History

Maintain a short daily history of completed 『디지털 데일리』 outputs.

- Store completed final digests under `$CODEX_HOME/skills/digital-daily/history`; if `$CODEX_HOME` is unavailable, resolve it as `~/.codex`.
- Use the user's local work date as the filename in `YYYY-MM-DD.md` format, for example `history/2026-04-24.md`.
- The work date is the news collection and article selection date unless the user explicitly provides another work/news date. It is not the delivery date used in the digest header.
- Keep only one file per work date. When the skill runs again for the same work date, overwrite that date's file with the new completed final digest.
- The history file must contain only the final digest text that is returned to the user, with no metadata, notes, explanations, or wrapper text.
- If the `history/` directory does not exist, create it before saving the file.
- After saving the final digest, delete history files older than 7 days relative to the user's local work date.
- Only delete files inside `$CODEX_HOME/skills/digital-daily/history` whose filenames match `YYYY-MM-DD.md`; if `$CODEX_HOME` is unavailable, resolve it as `~/.codex`.
- Keep today's file and the previous 7 calendar days when present.
- Do not mention history file creation, overwrite status, or cleanup status in the final digest unless the user explicitly asks.

## Classification Guidance

Use these editorial judgment rules when deciding between `(1) 금융권 Digital 현황` and `(2) 금융권 Digital 동향 및 정책`.

- Put an article in `(2) 금융권 Digital 동향 및 정책` when the news is driven by a regulator, ministry, central bank, public authority, public corporation, or another influential policy-making or public infrastructure institution, especially when the story signals a policy direction, cross-border agreement, public infrastructure plan, market infrastructure shift, or market-wide implementation schedule. Example: a 금융위원장 statement about launching Korea-India QR payment interoperability within the year belongs in `(2)` because it reflects an authoritative policy and market infrastructure direction, even though it concerns a payment service.
- Put an article in `(2) 금융권 Digital 동향 및 정책` when the article is mainly about a market-wide competitive structure, ecosystem shift, infrastructure conflict, standard-setting issue, or industry power dynamic that affects multiple financial or fintech players. Example: a Toss vs KICC payment-terminal conflict belongs in `(2)` because it signals a broader VAN, POS, big-tech, and offline payment infrastructure trend, not only one company's product update.
- Put an article in `(1) 금융권 Digital 현황` when the news is mainly about specific companies executing a partnership, product launch, platform integration, technology adoption, or service expansion, even if the topic involves regulation-sensitive areas such as AML, CFT, digital assets, stablecoins, authentication, or security. Example: Lambda256 and Crystal Intelligence cooperating on digital-asset AML/CFT solutions belongs in `(1)` because it is a company-to-company business and solution partnership. Do not use this rule when a public corporation or public infrastructure institution is a core actor and the story is mainly about market or public infrastructure direction; classify those in `(2)`.
- When both angles are present, classify by the article's dominant actor and consequence: authoritative public-sector direction or market-wide structure goes to `(2)`; individual firm execution or bilateral commercial collaboration goes to `(1)`.
- Prefer `(2)` only when the title and article clearly show policy authority, supervisory impact, public infrastructure, or broad industry trend. Do not move a company partnership into `(2)` merely because the subject is compliance, security, payments, AI, or digital assets.

## Final Ordering Guidance

Use these rules for final article ordering after reading article bodies. These are content-based editorial rules and outrank curator `confidence`.

- User-specified article order always wins.
- Within each section, prioritize Shinhan Bank or Shinhan Financial Group items when they have meaningful digital finance relevance.
- Then prioritize major competing Korean financial groups and banks, including KB Kookmin Bank, NH NongHyup Bank, Woori Bank, Hana Bank, and other large Korean banking groups, when the article content shows meaningful strategic, service, platform, AI, data, security, payment, or infrastructure impact.
- Give high priority to public authorities, supervisors, central-bank or market-infrastructure institutions, and public financial institutions when the article content shows regulation, supervision, public infrastructure, security posture, market-wide implementation, or policy direction.
- Rank broad digital finance market shifts, digital asset infrastructure, payment infrastructure, financial AI, security, authentication, and platform ecosystem changes above narrow product updates when their article content has wider impact for Shinhan Bank employees.
- Do not let a high curating `confidence` keep a simple product or promotional article above a lower-confidence article whose body shows stronger policy, infrastructure, regulatory, or market-wide relevance.
- If content-based importance is effectively the same, use the curator output order as the next tie-breaker, and use `confidence` only after that.

## Title Cleanup Rules

Clean article and study post titles before writing the final digest. Preserve the original meaning, but remove bracketed labels that are not part of the substantive title.

- Remove leading, trailing, or embedded square-bracket labels such as `[단독]`, `[종합]`, `[속보]`, `[게시판]`, `[현안 인터뷰]`, `[테크스냅]`, `[특징코인]`, `[코인브리핑]`, `[마켓인]`, or similar newsroom/series/category tags when they do not add necessary subject-matter meaning.
- Also remove equivalent Korean full-width bracket labels such as `【단독】` or `［종합］` when they are editorial tags rather than substantive title text.
- Keep bracketed text only when removing it would change the core meaning of the article title, product name, law name, company name, or study post title.
- After removing a bracketed label, clean up extra spaces and awkward leftover punctuation while preserving the article's original wording as much as possible.

## News Handling Rules

- Use Korean for the digest unless the user requests another language.
- Keep each item concise and scannable.
- Prefer facts from the article body over metadata snippets.
- Include source links in each card.
- Avoid copying long passages from the source. Paraphrase and quote only short phrases when necessary.
- If multiple URLs cover the same story, merge them only when the user asks for deduplication; otherwise keep one item per URL.
- If a URL is inaccessible, say so in that card and summarize only what can be verified from accessible source metadata or search results.
- Do not add extra greetings, commentary, tables, or markdown headings beyond the format contract.
- For `(3) Digital 스터디 365`, prefer blog-style sources such as 요즘IT, tech blogs, company engineering blogs, developer documentation posts, research explainers, or other non-news learning posts. Do not use Naver News, newspaper articles, wire-service articles, newsroom columns, or opinion articles as `(3)` items.
- For `(3) Digital 스터디 365`, do not write a `▶` summary. Include only the post title, URL, and source, matching `references/format.md`.
- If no suitable blog-style study post is supplied, leave `(3) Digital 스터디 365` empty rather than filling it with a news article.
- Respect the user's requested article order when stated; otherwise use content-based importance after article-body review. If the input includes curator `confidence`, treat it as a reference-only curating signal: it must not override article content, section classification, or the Final Ordering Guidance in this skill.
- Curator output order is an initial candidate priority, not a fixed final digest order. Use it only as a tie-breaker when content-based importance is effectively the same.
- A high-confidence simple product announcement may be placed after a lower-confidence regulation, infrastructure, security, or market-wide policy article when the article body shows stronger relevance for Shinhan Bank employees.

## Format Reference

Read `references/format.md` for the required output template. Follow it exactly for:

- Date header.
- Three-section ordering.
- Divider style.
- Article item shape: title, URL, source, and `▶` summary paragraph.
- `Digital 스터디 365` exception: title, URL, and source only.
- Korean newsroom-style tone.
