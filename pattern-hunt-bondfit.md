# Pattern Hunt: BondFit User Activity

Dataset: `BondFit_UserActivity_Dataset.xlsx`. It covers 4 weeks of data, from Mon 4 May to Sun 31 May 2026. It is a synthetic teaching dataset.

## 1. The summary pasted into the AI tool

> **Prompt:** "Here is 4 weeks of daily active user data for a fitness app broken down by time of day: [summary below]. What patterns can you identify and what type is each?"

| Day | Avg DAU | Avg sign-ups | Avg workouts | Avg morning sessions | Avg afternoon sessions | Avg evening sessions |
|---|---|---|---|---|---|---|
| Mon | 3,240 | 59 | 1,507 | 972 | 752 | 2,275 |
| Tue | 3,180 | 53 | 1,473 | 959 | 730 | 2,237 |
| Wed | 3,310 | 56 | 1,553 | 976 | 770 | 2,341 |
| Thu | 3,290 | 62 | 1,527 | 945 | 751 | 2,298 |
| Fri | 2,950 | 54 | 1,338 | 871 | 708 | 2,084 |
| Sat | 4,820 | 204 | 3,676 | 3,087 | 1,775 | 1,044 |
| Sun | 4,670 | 190 | 3,618 | 2,952 | 1,728 | 1,024 |

Average session length in minutes (morning / afternoon / evening): weekdays 25.8 / 21.7 / 34.2, weekends 40.8 / 32.6 / 24.5.

## 2. The AI's response, based on the summary only

| # | Pattern | Type |
|---|---|---|
| 1 | Activity repeats every 7 days. Saturday and Sunday have about 1.5× the users of a weekday. | **Cyclical / seasonal** (weekly cycle) |
| 2 | The busiest time of day flips. On weekdays, 57% of sessions are in the evening. On weekends, 52% are in the morning and only 18% are in the evening. | **Structural / behavioural** (two different usage modes) |
| 3 | Session length follows the busiest time. Evenings are the longest sessions on weekdays (34 min) and mornings are the longest on weekends (41 min). | **Correlation** (volume moves with engagement) |
| 4 | Sign-ups (×3.5) and workouts (×2.5) rise more sharply at weekends than users do (×1.5). | **Correlation**, but not proportional |
| 5 | Friday is the quietest day of the week, at about 10% below Monday–Thursday. | **Recurring anomaly**, which is part of the cycle |
| 6 | Afternoon is the smallest time band every day. | **Constant / stable** pattern |

## 3. Findings from the full daily data (28 rows)

- **Trend:** users are growing slowly. The fitted line rises by about 23 users per day. Weekly totals were 24,914, 25,215, 25,590 and 26,121, so Week 4 is about 5% higher than Week 1. Both weekdays and weekends grew.
- **Cycle:** there is a weekly cycle with a weekend peak and a Friday dip. This matches the AI.
- **Anomaly:** Friday 29 May (Week 4) was 3,126 users. That is about 8% above the 2,891 average of the earlier Fridays, and 93.6% of the Thursday before it, against the usual 88%. Its evening sessions (2,200) were also high.
- **Correlation:** DAU correlates with workouts logged at r = 0.99 and with sign-ups at r = 0.99. Workouts per active user are 0.46 on weekdays and 0.77 on weekends.
- **Stability:** average session length barely changes from one week to the next (standard deviation about 1 minute in each band). The time-of-day flip appears in all four weeks.

## 4. Comparison

**Point of agreement: the weekly cycle and the time-of-day flip.** The AI and the row-level analysis agree on both points. The data repeats on a 7-day cycle with a weekend peak and a Friday trough. Usage also switches between two modes:
- weekday-evening workouts that are short and frequent
- weekend-morning workouts that are long

Both analyses classed the weekly pattern as cyclical / seasonal.

**Gap: the AI missed the trend and the anomaly.** The AI found no growth trend and no outlier. It could not have found them, because the summary is made of day-of-week averages. Averaging the same weekday across four weeks flattens the 5% growth from Week 1 to Week 4. The same averaging hides the unusually strong Friday in Week 4. The AI also reported the Friday dip as an anomaly. It is not one: it recurs every week, so it is part of the cycle. The real anomaly is the one Friday that did not dip.

**Lesson:** the AI's pattern types are only as good as the decomposition we feed it. Summarising by day of week keeps the cycle but removes the time dimension. Trends and anomalies live in that time dimension.
