# 3_log_reflection.md


## Part 1 - Cleaning Code Court


### Did the AI code remove valid observations or change numbers without my knowing? (D4)

- **Valid observation lost:** Staple A / City 1 / 5 Feb (108.5) is not in the AI table under that key, because it sits under Oil B.
- **Number invented:** Pulse C / City 2 / 1 Jan = 205.91.
- **Duplicate key created:** Oil B / City 1 / 5 Feb appears twice (108.5 and 318.0).
- **Rows removed silently:** Sweetener D / City 1 / 22 Jan (L7) and Staple A / City 2 / 15 Jan (L10).


Mine: 96 rows, 48 per city, 8 per date, every date/item/city slot present once.

### Did anything change in my table without a log row? (D5)

Compared every kept row with its raw row: exactly 3 rows differ (raw lines 19, 32, 42), all flagged. Unlogged changes: 0.

### Why it matters (D6)

Mean price by item and city, naive conversion of the raw file vs mine: Sweetener D / City 1 is 983.88 vs 164.32; Staple A / City 2 is 209.25 vs 114.64; Staple A / City 1 is 107.42 vs 108.17 (the duplicate and the mislabelled row pulled it down). A "which city is more expensive" answer from the raw file would be wrong.

## Part 2 - Cleaning log


## Part 3 - Reflection

**Which of my decisions differed from the AI's, and which was better supported?**

1. **`oil b`.** The AI changed the case; I corrected the label to Staple A. Mine was better supported: the price level (108.5 vs 318) and the empty Staple A slot both pointed to Staple A, and the AI's version produced a double entry for Oil B and a 25-row item.
2. **9999.** The AI deleted the row; I blanked the price and kept the row. Mine was better supported: the slot still exists in the weekly grid, and the item count stays at 24. I had planned to drop the row in `1_attempt.md`, so this is a place where the court changed my mind.
3. **1,250.** The AI removed it through the IQR fence; I excluded the value but kept the row and logged why. The number ends up out of the averages either way. The difference is that mine is documented and does not depend on one fence that would also let a 500 through.
4. **Missing price.** The AI filled 205.91; I left it missing. Mine was better supported: an average that says "93 of 96 prices used" is honest, and a filled number looks like an observation.
5. **`unit` / `data_label`.** The AI dropped both; I kept them. Mine was better supported because the label carries the "illustrative, not PBS" warning.
6. **Where we agreed:** parse dates, strip spaces, drop the exact duplicate, reset the index.

**What would I have missed alone?**

- I had not planned any check on the consequences of my own decisions. Running the AI code and counting rows per item (Oil B 25, Staple A 22, Sweetener D 23) is what showed that deleting rows and title-casing labels damage the grid. My own plan to drop the 9999 row would have left Sweetener D at 23.
- The cumulative step table was the most useful tool: one line looked harmless but shifted counts for a different item, and only a line-by-line view showed which line was responsible.
- I had not tested the idea of an outlier rule. Looking at the pooled fence made me test deviation within each item/city series, which showed a very clear gap (10% for valid prices, 982% for 1,250).

**What I am still unsure about**

- The `oil b` correction is an inference from price and structure, not a fact from the source. It is flagged so the reader can reverse it.
- The 50% rule is a choice. It separates the data here because valid prices move by at most 10%, but it is not a general law.
- `1,250` might be a typo for 125.0 or similar. Without the source I did not guess. The question for the intern is in `1_attempt.md`.
- For any analysis I would use `price.dropna()` and report the count of prices used alongside each average.
