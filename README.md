# Google Pay Case Study: Student Budgeting Across UPI Apps

*A product case study by [Shreyas Lal](https://linkedin.com/in/shreyaslal) · November 2026*

**Problem:** College students pay through two or three UPI apps, but Google Pay can only see and explain the payments it processes, about a third of UPI volume. So students never get a true picture of their monthly budget.

**Proposal:** *Pocket Mode*, a student budgeting layer in Google Pay. It shows the remaining budget on the pay screen before money leaves, supports parent-funded allowances through UPI Circle, and later pulls in spending from other apps and banks through the Account Aggregator framework.

**North-star metric:** weekly active budgeters, meaning students who view or adjust their budget at least once a week.

**Why now:** NPCI's 30% volume cap takes effect on 31 December 2026, so Google Pay cannot keep growing on transaction volume alone. Owning the user's whole money picture is a growth path the cap does not limit.

---

## 1. User and problem

**Target user:** an Indian college student aged 18–25 living on a monthly allowance from parents, sometimes topped up by a stipend or internship pay.

**What we know:**

- UPI adoption is highest in the 18–25 age group, at 66%, and 65% of UPI users make several digital payments every day ([Finance Ministry survey of 10,378 respondents](https://upstox.com/news/business-news/latest-updates/upi-overtakes-cash-as-preferred-payment-mode-digital-transactions-jump-11-fold-since-2021-report/article-189492)).
- Users keep more than one UPI app, partly as a backup when a payment fails ([IMDR survey](https://imdr.edu/public/IMDR_JOURNAL_2023_24_Updated/files/basic-html/page158.html)). Cashback-led apps such as Navi grew from 0.21% to 3.6% share in two years, a sign that users hop between apps for offers ([NiftyTrader, NPCI data](https://www.niftytrader.in/markets/phonepe-google-pay-share-falls/)).
- In my interviews with 10 students, **7 of 10 used two or more UPI apps**, and they tracked spending by scrolling uncategorized transaction history (section 9).

**The core insight:** a student's budget is spread across every app they use, but each app only sees its own slice. Google Pay's Gemini assistant, *Ask Google Pay* (July 2026), answers questions like "How much did I spend on food?" — but only from Google Pay activity ([Business Today](https://www.businesstoday.in/technology/news/story/google-pay-introduces-ai-powered-ask-google-pay-track-spending-understand-sips-and-access-more-credit-546164-2026-07-30)). For a student who also pays through PhonePe, that answer is confidently incomplete.

**Second insight:** today's insights all arrive *after* the money is spent. The moment a student can actually change behavior is the pay screen, and no UPI app uses it for budgeting.

## 2. Why it matters

Roughly 3 crore students are likely UPI users, and they form the cohort that decides which payment app they keep for the next 40 years.

| Measure | Figure | Source |
| --- | --- | --- |
| Students in higher education | 4.50 crore (2023–24) | [AISHE 2023–24](https://news.careers360.com/aishe-2023-24-higher-education-enrolment-reaches-4-50-crore-female-enrolment-rises-42-percentage-since-2014-15) |
| UPI adoption, ages 18–25 | 66% | [Finance Ministry survey](https://upstox.com/news/business-news/latest-updates/upi-overtakes-cash-as-preferred-payment-mode-digital-transactions-jump-11-fold-since-2021-report/article-189492) |
| Estimated student UPI users | ~3 crore (4.50 crore × 66%; the 18–25 rate is a proxy) | Derived |
| Monthly UPI volume | 24.07 billion transactions, ₹29.37 lakh crore (Sep 2026) | [StartupTalky, NPCI data](https://startuptalky.com/india-upi-transactions-september-2026/) |
| Google Pay share of UPI volume | 32.7% (May 2026) | [NiftyTrader, NPCI data](https://www.niftytrader.in/markets/phonepe-google-pay-share-falls/) |
| Accounts linked on Account Aggregator | 338.04 million (Aug 2026), up 24% since Feb | [Free Press Journal](https://www.freepressjournal.in/business/rbi-to-make-account-aggregators-interoperable-allow-cas-bank-deposits) |

**Google's strategic stake:**

- **The volume cap changes the game.** NPCI's rule limits any third-party app to 30% of UPI volume, with compliance due 31 December 2026 ([Business Today](https://www.businesstoday.in/tech-today/news/story/npci-extends-30-upi-market-share-cap-deadline-on-3rd-party-apps-to-december-2026-459171-2024-12-31)). Google Pay is above it. Engagement and financial services, not payment count, become the way to grow.
- **The duopoly is eroding.** PhonePe and Google Pay together fell below 80% share for the first time in May 2026, as cashback-funded apps gain ground ([Outlook Business](https://www.outlookbusiness.com/economy-and-policy/phonepe-google-pay-combined-upi-market-share-drops-below-80-for-first-time)). Cashback is easy to copy; a budget that holds a student's whole financial picture is not.
- **It feeds Google Pay's financial-services bet.** Google launched the Google Pay Flex SBI Card alongside Ask Google Pay ([APAC News Network](https://apacnewsnetwork.com/2026/07/google-pay-can-now-tell-you-where-your-money-goes-heres-how/)). A student who trusts Google Pay with budgeting is the natural first customer for credit, savings and investments after graduation.

## 3. What exists today

Every building block exists, but nobody has assembled them for students at the moment of payment.

| Product | What it does | Gap for a student |
| --- | --- | --- |
| Ask Google Pay (Gemini, July 2026) | Opt-in assistant in 10 Indian languages that answers spending questions from Google Pay history | Sees only Google Pay transactions; answers after the money is spent |
| UPI Circle (NPCI) | A primary user lets up to 5 secondary users pay from their account, with full delegation up to ₹15,000 a month and ₹5,000 per transaction ([Razorpay](https://razorpay.com/blog/what-is-upi-circle/)) | Built for allowances, but has no budgeting, categories or nudges for the student |
| Account Aggregator framework | Consent-based sharing of bank data across institutions; under 10% of enabled accounts were linked by March 2026 ([IMPRI](https://www.impriindia.com/?p=74028)) | Powerful data rail with a weak consumer reason to link |
| PhonePe, Paytm and bank apps | Transaction history and basic spend summaries | Each sees only its own transactions |

**The gap:** a budget that covers every app, lives on the pay screen, and is built for an allowance-funded student.

## 4. Options considered

I recommend option D: ship B and C first on rails Google Pay already controls, then add A once the habit is proven. Scores run 1 (weak) to 5 (strong).

| Option | User value | Data completeness | Speed to launch | Strategic fit | Risk (5 = low) | Total |
| --- | --- | --- | --- | --- | --- | --- |
| A. Cross-app budget via Account Aggregator | 5 | 5 | 2 | 5 | 2 | 19 |
| B. Pay-screen budget nudge on Google Pay data | 4 | 2 | 5 | 4 | 5 | 20 |
| C. Parent-funded allowance via UPI Circle | 4 | 3 | 4 | 4 | 4 | 19 |
| **D. Staged: B + C now, A in phase 2** | **5** | **4** | **4** | **5** | **4** | **22** |

**Why staged wins:**

- **A alone is too slow and too risky to lead with.** Under 10% of enabled accounts are ever linked, so asking for consent on day one would stall adoption. It also likely requires a regulated partner (see Risks).
- **B alone repeats Ask Google Pay's blind spot,** but it proves the pay-screen nudge works with zero new dependencies.
- **C brings the parent in.** A parent who funds the allowance through UPI Circle makes Google Pay the default wallet for that student's entire allowance, which raises the share of spending Google Pay can see even before A ships.

## 5. Proposed feature: Pocket Mode

The student sets one monthly budget, and Google Pay keeps it visible at the moment of every payment.

**User flow:**

1. **Set up in 30 seconds.** The student turns on Pocket Mode and enters a monthly budget, optionally split into food, travel and fun.
2. **Fund it (optional).** A parent adds the student through UPI Circle full delegation and sets the allowance, up to ₹15,000 a month. The budget pre-fills from that amount.
3. **Decide at the pay screen.** Before the student enters a PIN, the screen shows one line: *"₹1,240 left for food this month, 9 days to go."* If the payment would break the budget, the line turns amber. It never blocks the payment.
4. **Weekly check-in.** Every Sunday, Ask Google Pay sends a short summary in the student's language: what went where, and whether they are on track.
5. **See everything (phase 2).** The student can link other bank accounts through Account Aggregator, so payments made in PhonePe or Paytm count against the same budget.
6. **Close the month.** Unspent money can move into a savings goal, which makes staying under budget feel like a win rather than a restriction.

**MVP scope (phase 1):** steps 1–4 and 6, using only Google Pay transactions and UPI Circle. **Not in the MVP:** Account Aggregator linking, credit offers, and anything that blocks or delays a payment.

## 6. Business model: how Pocket Mode pays for itself

Pocket Mode is free to students, and it earns its keep by moving Google Pay's revenue away from payments, which earn almost nothing, toward financial services.

**Why payments alone don't pay.** UPI has a zero merchant discount rate (MDR), so apps earn nothing directly on a payment; the government instead pays processors a subsidy of ₹1,500 crore (FY2024-25) ([Digital in Asia](https://digitalinasia.com/upi-revenue-problem-india-digital-payments/)). Google Pay India's revenue has stayed flat at about ₹1,400–1,550 crore for five years, ₹1,547 crore in FY25, with net profit halving to ₹50 crore ([Storyboard18](https://storyboard18.com/digital/google-pay-india-reports-flat-rs-1547-crore-revenue-in-fy25-as-net-profit-halves-to-rs-50-crore-89452.htm)). Meanwhile PhonePe's financial-services revenue grew 206% in FY2025 and now drives most of its profit ([Digital in Asia](https://digitalinasia.com/upi-revenue-problem-india-digital-payments/)). The money is in what comes after the payment.

**Pricing decision: free, with no plans to charge.** 75% of UPI users say they would stop using UPI if any transaction fee were added ([LocalCircles survey via Outlook India](https://outlookindia.com/brand-studio/blogs/upi-on-the-edge-majority-of-users-may-abandon-platform-over-potential-transaction-fees)), and every competitor's spending summary is free. A paid budgeting tier would cap adoption at exactly the cohort we want to win.

**Three ways Pocket Mode creates value for Google Pay:**

| Lever | How Pocket Mode drives it | Signal to track |
| --- | --- | --- |
| Primary-app share | A budget that lives in Google Pay makes it the student's default app, so more of their payments (and data) flow through it | Share of a Pocket Mode user's UPI payments made in Google Pay |
| Family acquisition | Each UPI Circle allowance brings in a parent as the primary user, often an established earner | Parents onboarded per student |
| Financial-services funnel | A student who trusts Google Pay with their budget is the natural first customer for savings, the Flex credit card and loans after graduation | Conversion to a Google Pay financial product within 24 months |

**Illustrative sizing (every input is an assumption to test in the pilot, not a forecast):**

| Step | Assumption | Result |
| --- | --- | --- |
| Student UPI users (section 2) | ~3 crore | 3 crore |
| Adopt Pocket Mode in year 1 | 5% | 15 lakh students |
| Take a Google Pay financial product within 2 years | 10% | 1.5 lakh customers |
| Annual revenue per financial-product customer | R (unknown; set from internal data) | 1.5 lakh × R |

At an illustrative R of ₹1,000 a year, that is about ₹15 crore a year, roughly 1% of Google Pay India's FY25 revenue, from one student cohort. The cohort renews every year, and the customers stay for decades. The pilot's job is to replace each assumption with a measured number.

**Cost side:** the MVP reuses rails Google Pay already owns: its transaction history, UPI Circle and the Ask Google Pay assistant. The main costs are engineering time and the regulated partner needed for Account Aggregator in phase 2, which is why phase 2 is gated on phase 1 results.

## 7. Success metrics

Success means students come back to their budget every week without paying less through Google Pay. Targets are set after a four-week baseline from the pilot, not guessed in advance.

| Type | Metric | Why it matters |
| --- | --- | --- |
| North star | Weekly active budgeters | Captures the habit, not one-time setup |
| Input | % of new student users who set a budget within 7 days | Tests whether setup is easy and the value is obvious |
| Input | % of pay-screen nudges followed by a smaller or skipped payment | Tests whether the pay-screen moment changes behavior |
| Input | % of students funded through UPI Circle | Measures how much of the allowance flows through Google Pay |
| Input (phase 2) | Account Aggregator link rate among Pocket Mode users | Benchmarked against the national rate of under 10% |
| Guardrail | Payment completion rate | The nudge must never add friction or failed payments |
| Guardrail | Google Pay transactions per student | Budgeting could reduce spending; a large drop would hurt the core business |
| Guardrail | Opt-out and privacy-complaint rate | Students and parents must trust the feature |

## 8. Rollout and risks

Launch in four phases, each opening only when the previous one clears its gate.

| Phase | Scope | Gate to move on |
| --- | --- | --- |
| 0. Validate | Interviews plus a fake-door test: a "Set a monthly budget" button shown to a small student segment | Click-through high enough to justify building |
| 1. Campus pilot | MVP at 3–5 campuses through campus ambassadors | Weekly active budgeters hold steady for 4 weeks; guardrails flat |
| 2. See everything | Account Aggregator linking with a regulated partner | Link rate clearly above the national rate; no rise in privacy complaints |
| 3. National | All students aged 18–25, promoted around semester start and allowance day | Retention holds at scale |

**Key risks and mitigations:**

- **Regulation.** Fetching bank data through Account Aggregator generally requires a regulated financial institution as the data user. *Mitigation:* run phase 2 through a bank partner, as Google already does for credit with SBI.
- **Trust.** Urban users rate payment apps 4.11 out of 5 for security but only 3.10 for credit ([How Urban India Pays](https://www.businesstoday.in/personal-finance/story/3-in-4-urban-indians-use-payment-apps-for-investments-insurance-or-credit-report-561522-2026-10-14)), so non-payment features start with a trust deficit. *Mitigation:* opt-in only, on-device categorization where possible, and a one-tap "delete my budget data" control.
- **Cannibalization.** Students who budget may pay less. *Mitigation:* the transactions-per-student guardrail, plus the savings goal, which keeps the money inside Google Pay.
- **Parent-student tension.** Allowance visibility can feel like surveillance. *Mitigation:* parents see only totals against the allowance, never individual payments, unless the student chooses to share.
- **Competitors copy it.** PhonePe can ship a pay-screen nudge quickly. *Mitigation:* the moat is the combination of Gemini in 10 languages, UPI Circle allowances and whole-picture data, which is harder to copy than any single feature.

## 9. User interviews: what students said

I interviewed 10 students to test the three assumptions this proposal rests on.

| Assumption | What I asked | What students said | Verdict |
| --- | --- | --- | --- |
| Students use 2+ UPI apps | "Walk me through your last five payments. Which app did you use for each, and why?" | **7 of 10** use two or more UPI apps | Confirmed |
| They want a budget but lack a working method | "How do you know whether you'll run short before the month ends?" | They track spending by scrolling their transaction history, which isn't categorized, so they can't see where the money went | Confirmed: the gap is categorization, not data |
| The pay screen is the right moment | "If your remaining budget showed before you paid, would you change anything?" | Students said yes, seeing the remaining budget before paying would change what they do | Confirmed (count to be added) |

**What changed in the proposal:** the interviews sharpened the MVP. Because students already look at their transaction history, automatic categorization (food, travel, fun) moves to the centre of phase 1, so the pay-screen line can say *"₹1,240 left for food"* rather than just a single total. The 7-of-10 multi-app finding also confirms that phase 2 (cross-app data through Account Aggregator) is needed for a complete picture.

**Next step:** a larger survey and the fake-door test in phase 0 (section 8) to measure these at scale.

---

*This is an independent case study written as product practice. It is not affiliated with or endorsed by Google, NPCI or any company mentioned. All figures come from the public sources linked above.*

## Sources

- [StartupTalky: UPI transactions, September 2026 (NPCI data)](https://startuptalky.com/india-upi-transactions-september-2026/)
- [NiftyTrader: UPI app market share, May 2026 (NPCI data)](https://www.niftytrader.in/markets/phonepe-google-pay-share-falls/)
- [Outlook Business: PhonePe and Google Pay combined share below 80%](https://www.outlookbusiness.com/economy-and-policy/phonepe-google-pay-combined-upi-market-share-drops-below-80-for-first-time)
- [Business Today: NPCI 30% cap deadline extended to 31 Dec 2026](https://www.businesstoday.in/tech-today/news/story/npci-extends-30-upi-market-share-cap-deadline-on-3rd-party-apps-to-december-2026-459171-2024-12-31)
- [Business Today: Ask Google Pay launch](https://www.businesstoday.in/technology/news/story/google-pay-introduces-ai-powered-ask-google-pay-track-spending-understand-sips-and-access-more-credit-546164-2026-07-30)
- [APAC News Network: Ask Google Pay and Google Pay Flex SBI Card](https://apacnewsnetwork.com/2026/07/google-pay-can-now-tell-you-where-your-money-goes-heres-how/)
- [Razorpay: UPI Circle limits](https://razorpay.com/blog/what-is-upi-circle/)
- [Free Press Journal: Account Aggregator linked accounts, Aug 2026](https://www.freepressjournal.in/business/rbi-to-make-account-aggregators-interoperable-allow-cas-bank-deposits)
- [IMPRI: Account Aggregator enabled vs linked accounts](https://www.impriindia.com/?p=74028)
- [Careers360: AISHE 2023–24 enrolment](https://news.careers360.com/aishe-2023-24-higher-education-enrolment-reaches-4-50-crore-female-enrolment-rises-42-percentage-since-2014-15)
- [Upstox: Finance Ministry UPI survey](https://upstox.com/news/business-news/latest-updates/upi-overtakes-cash-as-preferred-payment-mode-digital-transactions-jump-11-fold-since-2021-report/article-189492)
- [IMDR Journal: multiple UPI apps as backup](https://imdr.edu/public/IMDR_JOURNAL_2023_24_Updated/files/basic-html/page158.html)
- [Storyboard18: Google Pay India FY25 financials](https://storyboard18.com/digital/google-pay-india-reports-flat-rs-1547-crore-revenue-in-fy25-as-net-profit-halves-to-rs-50-crore-89452.htm)
- [Digital in Asia: UPI zero-MDR and the revenue gap](https://digitalinasia.com/upi-revenue-problem-india-digital-payments/)
- [Outlook India: LocalCircles survey on UPI fees](https://outlookindia.com/brand-studio/blogs/upi-on-the-edge-majority-of-users-may-abandon-platform-over-potential-transaction-fees)
- [Business Today: How Urban India Pays survey](https://www.businesstoday.in/personal-finance/story/3-in-4-urban-indians-use-payment-apps-for-investments-insurance-or-credit-report-561522-2026-10-14)
