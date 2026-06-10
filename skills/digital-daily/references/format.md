# Digital Daily Format

Use this exact editorial structure when creating a `digital-daily` output.

## Required Structure

Start with the delivery date line, not the news collection date line:

- Select and summarize news from the requested news date; when no news date is requested, use today in the user's local timezone as the news collection date.
- The opening date must reflect when the digest is provided to readers, not the date the news was collected.
- Normally, use the next calendar day after the news collection date as the delivery date.
- If the news collection date is Friday, use the following Monday as the delivery date.
- If the user explicitly provides a delivery date, use the provided delivery date for the opening line.
- Use the Korean weekday that matches the delivery date.

```text
M월 D일 (요일)  『디지털 데일리』 입니다.
```

Then list the three sections:

```text
(1) 금융권 Digital 현황
(2) 금융권 Digital 동향 및 정책
(3) Digital 스터디 365
```

Then write each section with this divider style:

```text
****************************
(1) 금융권 Digital 현황
****************************
```

Use the same divider for sections `(2)` and `(3)`.

## Article Item Format

Each article in sections `(1)` and `(2)` must follow this format:

```text
○ 기사 제목
URL
출처 : 언론사명
▶ 기사 요약문
```

After the final article in section `(1)` and after the final article in section `(2)`, insert two blank lines before the next section divider.

Each item in section `(3) Digital 스터디 365` is a blog-style or magazine-style IT/digital/AI study post, not a news article. Prefer sources such as 요즘IT, tech blogs, company engineering blogs, developer documentation posts, research explainers, or other learning resources. Do not place Naver News, newspaper articles, wire-service articles, newsroom columns, op-eds, or interviews in this section. If no suitable study post is supplied, leave the section empty. Do not summarize study posts. Use this format only:

```text
○ 게시글 제목
URL
출처 : 사이트명
```

## Writing Rules

- Write in Korean.
- Group each URL into one of the three required sections.
- If the user explicitly requests an article order, preserve that order within the relevant section.
- If the user does not request an order, place Shinhan Bank articles first within their section, then arrange the remaining items by importance for a Shinhan Bank employee reading a digital finance brief.
- Preserve the original article title when available.
- Put the URL on its own line.
- Use `출처 :` with one space before and after the colon marker as shown.
- Start every news summary in sections `(1)` and `(2)` with `▶`.
- Keep each news summary to one compact paragraph.
- Focus news summaries on what happened, why it matters, and any concrete figures or policy details.
- Insert two blank lines after the last article in `(1) 금융권 Digital 현황` and after the last article in `(2) 금융권 Digital 동향 및 정책`, before the next section divider.
- Do not add a `▶` summary to section `(3) Digital 스터디 365`.
- Do not use ordinary news articles as fallback items in `(3) Digital 스터디 365`; exclude unsuitable news items instead.
- Do not add bullets inside the summary paragraph.
- Do not add an introduction, conclusion, table, or extra commentary unless the user asks.
- The opening date and weekday must follow the delivery-date rule above. If an article date is needed but unavailable, do not invent it.
- If a URL cannot be accessed, keep the item format and state the access limitation in the summary.

## Example

```text
4월 22일 (수)  『디지털 데일리』 입니다.

(1) 금융권 Digital 현황
(2) 금융권 Digital 동향 및 정책
(3) Digital 스터디 365

****************************
(1) 금융권 Digital 현황
****************************

○ 카카오뱅크 앱 열어 주식 투자…2700만 고객 유입 노린다
https://naver.me/IMZVfovm
출처 : 뉴스1
▶ 카카오뱅크는 앱 내 ‘투자 탭’을 신설해 주식·펀드·MMF·채권 등 다양한 투자상품을 통합 제공하며 종합 금융 플랫폼으로 확장을 추진하고 있습니다. 카카오페이증권과의 연계를 통해 약 2700만 고객 기반 유입을 노리고 있으며, 공모주 일정, 가상자산 조회 등 기능을 강화해 투자 접근성과 편의성을 높이고 있습니다.

○ 펀드 팔아야하는데'…토스뱅크, 제재 지연에 인가 밀리나
https://naver.me/5yhXunEn
출처 : 비즈워치
▶ 토스뱅크의 펀드 판매를 위한 금융투자업 본인가가 금융당국 제재 지연으로 늦어지고 있습니다. 정기검사 및 환전 사고 관련 제재 수위가 확정되지 않아 금융위원회 심사 일정도 미정 상태입니다. 인가 지연 시 연내 펀드 판매가 어려울 수 있으며, 비이자이익 확대 전략에도 차질이 예상됩니다.

○ 수출입은행, 자체 생성형AI 플랫폼 구축…130억원 투입
https://naver.me/xfbJ2mpH
출처 : 뉴시스
▶ 한국수출입은행은 약 130억원을 투입해 자체 생성형 AI 플랫폼 ‘KEXIM AI’ 구축에 착수했습니다. 이는 2028년까지 추진되는 디지털·AI 전환 전략의 핵심 과제로, 온프레미스 방식으로 안전성과 통제력을 확보합니다. AI는 대출·보증 심사, 고객 상담, 문서 작성·검토 등 업무에 활용되며, 내부 데이터 구조를 AI 친화적으로 재편하고 관리·윤리 체계도 함께 구축할 계획입니다.


****************************
(2) 금융권 Digital 동향 및 정책
****************************

○ 금융권 내부망 SaaS 활용 '빗장' 풀린다… 20일부터 망분리 예외 적용
https://naver.me/5DZq0BrJ
출처 : 전자신문
▶ 금융당국이 전자금융감독규정을 개정해 금융권 내부망에서 SaaS 활용을 허용했습니다. 이에 따라 별도 절차 없이 협업·화상회의 등 외부 서비스 사용이 가능해져 업무 효율성이 향상될 전망입니다. 다만 개인신용정보 처리 등은 여전히 제한되며, 보안평가를 통과한 SaaS만 이용해야 합니다. 또한 보안 통제와 정기 점검 의무가 강화됐습니다.

○ 오프라인 단말기 경쟁…네이버페이 커넥트 '플랫폼' 전략, 우위 선 까닭
https://naver.me/F3EDfWBn
출처 : 블로터
▶ 네이버페이와 토스의 오프라인 단말기 경쟁은 단순 결제를 넘어 플랫폼 경쟁으로 확장되고 있습니다. 토스는 빠른 보급과 혜택 중심 전략으로 선점했지만, 네이버페이는 검색·지도·리뷰와 결제를 연결해 ‘유입→결제→리뷰→재방문’ 구조를 구축했습니다. 이로 인해 매장 유입과 데이터 축적 측면에서 강점을 보이며, 중장기적으로 플랫폼 경쟁에서 우위를 확보할 가능성이 높다고 평가됩니다.


****************************
(3) Digital 스터디 365
****************************

○ Claude Code로 코드 한 줄 없이 마케팅팀을 만드는 법
https://yozm.wishket.com/magazine/detail/3696/
출처 : 요즘IT
```
