---
name: digital-daily-curator
description: Curate important, non-duplicate candidate articles with confidence scores for Korean "디지털데일리" finance news briefs from today's Naver News economy finance section. Use before digital-daily when the user asks to review Naver News 금융탭, use the 기사 더보기 button to inspect older same-day articles, select digital-related financial news for Shinhan Bank employees, rank candidates by title-only preliminary importance, or output article-title-URL-confidence objects for later summarization.
---

# Digital Daily Curator

## Overview

Curate today's digital finance news candidates before using the `digital-daily` writing skill. Review the Naver News economy finance page and select important, non-duplicate articles published today that matter for a Shinhan Bank employee reading a 『디지털데일리』 brief. Select candidates from the article list by title only; do not open individual article links to inspect the body.

This skill performs title-only candidate curation. Its output order and `confidence` values are preliminary signals for candidate selection, not the final digest order, section classification, or article importance after body review. The `digital-daily` writing skill must make final ordering and classification decisions after reading article content.

Default source page:

```text
https://news.naver.com/breakingnews/section/101/259
```

## Workflow

1. Open the default source page unless the user provides another Naver News finance-section URL.
2. Collect only articles from today's date in the user's local timezone.
3. Review the visible article list first.
4. Use the page's `기사 더보기` button or its underlying pagination/loading mechanism to expand to older articles.
5. Continue expanding and reviewing until the list reaches articles from before today, or until the page has no more articles to load.
6. For each same-day article, judge relevance from the article title shown in the list only. Do not open the article URL, inspect the article body, or follow related links for additional context.
7. Exclude articles that are not useful for a Shinhan Bank employee preparing or reading a digital finance digest.
8. Before final ranking, load recent completed `digital-daily` final digest history and exclude articles that were already included in those final digests.
9. Remove duplicate or near-duplicate coverage of the same issue, policy, product, service, company announcement, partnership, or market event.
10. Rank the remaining candidates by title-only preliminary importance for a Shinhan Bank employee.
11. Save today's final curation result to the daily history file, overwriting any previous curation saved for the same date.
12. Delete saved curation history files older than 7 days.
13. Output only the selected candidates, including confidence scores, using the required scheme.

## Naver News Page Behavior

The Naver News finance tab initially shows only the latest portion of the article list. It includes a `기사 더보기` button that loads older articles on the same page. Treat the expanded list as part of the page that must be reviewed.

When curating "today's news":

- Do not stop after the initially visible articles.
- Keep pressing `기사 더보기` or using the equivalent loading request to inspect older articles.
- Stop only after articles from yesterday or an earlier date appear, or the page indicates there are no more articles.
- Do not include articles from yesterday or earlier even if they are relevant.
- Do not open individual news article pages. Use only the article title and URL available in the expanded list.

## Curation History and Validation

Maintain a short daily curation history for the curator's own final candidate output, but use the completed `digital-daily` final digest history as the source of truth for duplicate prevention.

### Duplicate Prevention Source

- Before selecting the final candidates, read completed final digest files from `$CODEX_HOME/skills/digital-daily/history`; if `$CODEX_HOME` is unavailable, resolve it as `~/.codex`.
- Read the most recent available final digest history files within the retention window, especially yesterday's file when it exists.
- Exclude any candidate article whose canonical URL exactly matches a URL already included in those completed `digital-daily` final digest files.
- Also exclude clearly repeated coverage already included in those completed final digests when the title indicates the same underlying issue, policy, product, service, company announcement, partnership, or market event, even if the URL differs.
- Do not open prior final digest article URLs to verify the body. Use only the saved title, URL, and digest text in `$CODEX_HOME/skills/digital-daily/history`; if `$CODEX_HOME` is unavailable, resolve it as `~/.codex`.
- If the `digital-daily` history directory or relevant recent final digest files do not exist, continue normally.

### Curator Output History

- Store this skill's final curation results under `$CODEX_HOME/skills/digital-daily-curator/history`; if `$CODEX_HOME` is unavailable, resolve it as `~/.codex`.
- Use the user's local date as the filename in `YYYY-MM-DD.md` format, for example `history/2026-04-22.md`.
- Keep only one file per date. When the skill runs again on the same date, overwrite that date's file with the new final curation result.
- The history file must include the curation date and the final selected article objects. Use the same JSON-like array structure required for final output:

```text
date: YYYY-MM-DD

[
  {
    "title": "뉴스기사명",
    "url": "URL",
    "confidence": 0.00
  }
]
```

- If this skill's history directory does not exist, create it before saving today's result.
- After saving today's result, delete this skill's history files older than 7 days relative to the user's local date.
- Only delete files inside `$CODEX_HOME/skills/digital-daily-curator/history` whose filenames match `YYYY-MM-DD.md`; if `$CODEX_HOME` is unavailable, resolve it as `~/.codex`.
- Keep today's file and the previous 7 calendar days when present.
- Do not mention history file creation, overwrite status, cleanup status, or duplicate exclusions in the final output. The final output must still follow the required JSON-like array format only.

## Selection Criteria

Select articles about:

- Bank, card, securities, insurance, fintech, or public financial institution digital services.
- AI, data analytics, cloud, GPU, internal systems, automation, app services, cybersecurity, authentication, payment platforms, digital assets, or financial platform strategy, only when the title directly ties the technology to financial institutions, financial services, financial regulation, financial infrastructure, payments, authentication, security, or fintech use cases.
- Financial policy, supervision, network separation, security controls, SaaS, open banking, MyData, or digital finance regulation.
- Competitor moves relevant to Shinhan Bank, including KakaoBank, Toss, KB, Woori, Hana, Naver Pay, Kakao Pay, and major fintechs.
- Meaningful digital strategy or service moves by major competing Korean financial groups, especially KB, Woori, Hana, and NH NongHyup. Give these higher priority when the title signals a group-level initiative, flagship service, new technology adoption, platform strategy, AI/robotics use case, data/security capability, or other move that could affect Shinhan Bank's competitive positioning.
- Digital finance actions by high-impact public or supervisory institutions such as the Financial Services Commission, Financial Supervisory Service, Bank of Korea, Financial Security Institute, Korea Financial Telecommunications and Clearings Institute, Korea Housing Finance Corporation, Korea Credit Guarantee Fund, Korea Deposit Insurance Corporation, and other market-infrastructure or public financial institutions. Rank these higher when the title signals regulation, supervision, public infrastructure, security posture, market-wide implementation, or policy direction.

Apply these criteria using title-only evidence:

- Include an article when the title itself clearly signals a digital finance, fintech, AI/data, security, payments, digital asset, platform, app/service, or financial IT angle.
- For AI articles, include only when the title directly shows a financial-sector application or impact. The word `AI` by itself, even with a financial public institution such as the Financial Services Commission or Korea Credit Guarantee Fund, is not enough.
- Exclude an article when the title requires opening the article body to confirm the digital angle.
- Prefer false negatives over spending time opening articles; speed and resource savings are part of this skill.
- Treat the actor as an important ranking signal. All else equal, prioritize items led by Shinhan Bank, major competing financial groups, large fintech/platform companies, influential public authorities, supervisory bodies, central-bank or market-infrastructure institutions, and public financial institutions over items led only by small domestic vendors.
- Treat group-level competitor moves from KB, Woori, Hana, and NH NongHyup as strategically important when they show a meaningful digital, AI, platform, data, security, robotics, payment, asset-management, or customer-experience direction. For example, a KB Financial Group article about a first-in-sector senior-care humanoid robot should rank highly because it signals a competitor's digital service strategy.
- Do not over-prioritize small subsidiary-level items from those groups when the title suggests only an ordinary product tweak, local partnership, promotion, event, donation, recruiting activity, minor award, or narrow customer notice. A major group's name alone is not enough; the title must show a meaningful digital finance signal.
- Treat domestic small and midsize company announcements as normal or lower priority unless the title clearly shows broad market impact, direct relevance to banks, partnership with a major financial institution, regulatory significance, widely reusable infrastructure, or a genuinely novel digital finance capability. A small vendor's standalone MOU, PoC, product launch, or promotional claim should not outrank a meaningful move by a major financial group or public authority.
- Exclude general AI startup support, startup incubation, founder education, regional innovation hubs, AI startup centers, lab or center openings, and opening ceremonies unless the title directly names a financial service, financial regulation, financial security, payment, authentication, bank, card, insurance, securities, or fintech application.
- Prefer articles with higher strategic relevance, policy impact, competitor intelligence value, operational impact, or broad digital finance implications.
- When multiple articles appear to cover the same underlying news, keep only the most useful one based on title clarity, source quality, and relevance to Shinhan Bank.
- Select all candidates with confidence of at least 0.80; do not cap the final output at 10 articles.
- If fewer than 10 candidates have confidence of at least 0.80, add the next-best non-excluded articles by confidence until there are at least 10 selected articles, unless fewer than 10 non-excluded articles exist.

### Confidence Scoring

Assign each selected candidate a `confidence` score from `0.00` to `1.00`. This score means title-only curation confidence: how strongly the title alone supports including the article in a Shinhan Bank employee's digital finance digest. It is not a verification score for the article body.

Treat `confidence` as a curating confidence only:

- It measures title-based inclusion confidence.
- It is not a final article importance score.
- It is not a final digest sorting score.
- It is not a section classification signal.
- It may be used by `digital-daily` only as a last tie-breaker after article-body content and curator output order have both been considered.

- `0.90` to `1.00`: Shinhan Bank, major competing financial groups, the Financial Services Commission, Financial Supervisory Service, or similarly high-impact actors are clearly involved, and the title strongly signals digital finance relevance.
- `0.80` to `0.89`: A major fintech, public financial institution, market infrastructure actor, regulation, supervision, cybersecurity, digital asset, payment, or financial-sector AI/data trend is clearly signaled in the title.
- `0.70` to `0.79`: Below the normal cutoff, but usable as a backup candidate when needed to satisfy the minimum 10-article output.
- `0.60` to `0.69`: Generally exclude. Use only as a last backup when fewer than 10 non-excluded articles remain after considering `0.70` to `0.79` backup candidates.
- Below `0.60`: Generally exclude. Use only as a last backup when the same-day candidate pool is extremely small and the title is still not clearly out of scope.

Apply these scoring corrections before final selection:

- Do not assign `0.80` or higher just because the title combines a financial public institution with `AI`; the financial-sector application must be explicit in the title.
- General AI startup support, startup incubation, AI startup hubs, innovation centers, lab openings, and similar titles should normally be below `0.60`.
- If the financial-sector application is only weakly implied by the actor or context, score the article `0.70` to `0.79` at most and use it only as a backup candidate.
- Examples to exclude or score below `0.60`: `"농구 코트의 혁신"…금융위, 서울 동북권 첫 'AI 창업 거점' 구축`; `신보 '네스트 AI랩' 개소…서울 동북권 첫 스타트업 보육시설`; `금융위, AI 스타트업 창업 생태계 조성`.
- Examples that may score `0.80` or higher: `금융위, 금융권 생성형 AI 망분리 규제 완화 추진`; `신한은행, AI 기반 이상거래 탐지 시스템 구축`; `카드사, AI 에이전트 결제 서비스 도입`.

Rank this skill's final candidate output by title-only preliminary importance for a Shinhan Bank employee. When preliminary importance is similar, rank higher-confidence articles first. This order is not binding for the final `digital-daily` digest after article bodies have been read.

Exclude articles about:

- Volunteer work, donations, social contribution, education programs, recruiting events, local ceremonies, minor awards, and one-off promotional events.
- General startup support, startup incubation, founder education, regional innovation hubs, AI startup centers, lab or center openings, and opening ceremonies, even when led by the Financial Services Commission, Korea Credit Guarantee Fund, or another financial public institution, unless the title directly names a financial service, financial regulation, financial security, payment, authentication, bank, card, insurance, securities, or fintech application.
- Pure macroeconomics, markets, real estate, insurance claims, personnel moves, earnings, labor disputes, or customer notices unless the digital finance angle is central.
- Articles where the only digital element is online application, a generic app mention, or ordinary marketing language.

## Output Format

Return only this JSON-like array. Do not add explanations, reasons, section headers, or summaries.

Order selected articles from highest to lowest title-only preliminary importance. Include every selected article's original title, canonical Naver News URL or article URL from the page, and confidence score.

```text
[
  {
    "title": "뉴스기사명",
    "url": "URL",
    "confidence": 0.00
  },
  {
    "title": "뉴스기사명",
    "url": "URL",
    "confidence": 0.00
  }
]
```

Use the article's original title when available. Use confidence values with two decimal places when practical. If there are no qualifying or backup articles, return:

```text
[]
```
