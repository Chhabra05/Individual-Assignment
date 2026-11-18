# Assignment 3: log and reflection

## Part 2. ASK: statistics table pasted to the Interpretation Auditor

The table from `1_attempt.md` (item, mean, median, first-to-last change). All values come from the illustrative teaching file.

## Part 3. CHECK: audit table

Colour key: green = recomputable, amber = explanation, red = causal claim or refers to data not shown.
Replace the "claim" column with the wording your AI tool actually produced; the rows below show how each type is checked.

| # | Claim | Colour | How checked | Decision |
|---|---|---|---|---|
| 1 | Sweetener D rose about 19% from first to last date | Green | Recomputed (181.5 / 152 - 1 = 19.41%) in `2_work.ipynb` | Accepted |
| 2 | Mean and median are nearly equal for every item | Green | Largest gap is Staple A at -0.58 PKR (-0.52% of median) | Accepted |
| 3 | City 2 is always 6 PKR above City 1 | Green | Unique City 2 minus City 1 gaps = {6.0} | Accepted |
| 4 | Pulse C is the most stable item | Green after recompute | Range 13.5 (smallest); coefficient of variation 1.86% (smallest) | Accepted, wording tied to range and CV |
| 5 | Prices rose every week | Red | Week-on-week falls: Staple A 5, Pulse C 4, Sweetener D 4, Oil B 0 | Rejected |
| 6 | Oil B rose because of higher import costs | Red | Causal; the file has no cost data | Rejected |
| 7 | The similar mean and median show prices are symmetric and stable | Amber | Numbers are correct, but the series trends upward; similarity says nothing about stability over time | Accepted with edit: "level measures agree; the trend is separate" |
| 8 | The mean is unreliable | Amber | Only true when outliers exist; with the stand-in typo the mean moved +33.6%, the median +0.75, but the clean file shows no gap | Edited to "the mean is sensitive to errors" |

## Part 4. CONFUSED CLASSMATE: transcript excerpt

> **Classmate (AI):** The mean is always the typical price. Why would you ever use the median?
>
> **Me:** Take five prices: 100, 101, 102, 103 and 1,000, where the last is a typo. The mean is 281. Not one real price is near 281. The median is 102, and four of the five prices are right next to it.
>
> **Classmate:** But why does the mean get dragged by one number?
>
> **Me:** The mean is a balance point. Each value pulls on it in proportion to how far away it is, so a huge value pulls hard. The median is a middle point: it only asks how many values are above and below, so the typo is just "one more above".
>
> **Classmate:** So why use the mean at all?
>
> **Me:** When you need totals. Mean price times quantity gives total cost. For a typical price, use the median, especially if the data may contain errors. In our clean file they agree because there is nothing extreme, but you only know that after checking.
>
> *(Classmate's questions stopped repeating after this point.)*

## Part 5. REFLECT: where did my first draft say more than the data allowed?

My first draft had "Oil B rose because of rising costs". The file only has dates, items, cities and prices, so that was an explanation (E) and I had it as an observation (O). I also assumed the mean and median would disagree and was ready to argue for the median, but in the clean file they hardly differ, so I overstated the case: the choice of median matters because errors are possible, not because of anything in this dataset. Finally, "prices rose every week" would have been wrong: three of four items dipped in some weeks.
