attempt-before-AI


## 1. What I loaded

`teaching_prices_messy.csv`: 97 rows x 6 columns (`date, item, city, price, unit, data_label`). Raw file is never edited; all work is on a copy.

## 2. Diagnosis and planned decisions

| # | Problem | Where (raw file line) | Code that revealed it | Planned decision | Reason |
|---|---|---|---|---|---|
| 1 | Price missing | Pulse C, City 2, 1 Jan (line 7) | `df.isna().sum()` shows 1 missing in `price` | **Flag + retain** the row, leave price missing | There is nothing to recover it from. Filling it would put a number in the ledger that nobody observed. Averages must skip it and report how many prices they used. |
| 2 | Price column is text; one entry is `1,250` | Staple A, City 2, 15 Jan (line 19) | `df.info()` shows `price` is not numeric; `pd.to_numeric(errors="coerce")` turns exactly this entry into NaN | **Correct** the type (remove the comma), then **investigate** the value | The comma is a formatting problem, but 1,250 is about 11 times the other Staple A prices (about 105-120). Parsing it fixes the format, not the value. |
| 3 | `9999` in the price column | Sweetener D, City 1, 22 Jan (line 32) | `df.describe()` after conversion: max 9999, mean 315 vs median 200 | **Exclude** the value and **flag** it. Plan: drop the row | 9999 looks like a "no data" code. The neighbours are 153-162. |
| 4 | Item label `oil b` (lower case) | Line 42 (5 Feb, City 1, price 108.5) | `df["item"].value_counts()` shows 5 labels for 4 items | **Investigate**, then **correct** if confirmed | The price 108.5 is Staple A territory (105.0 before, 107.0 after) while Oil B in the same city is 318. If I only changed the case, Oil B / City 1 would have two prices on 5 Feb and Staple A / City 1 would have none. Plan: correct to `Staple A` and flag; if the checks do not support it, flag only. |
| 5 | Exact duplicate row | Line 98 (copy of line 10: Staple A, City 1, 8 Jan, 99.5) | `df.duplicated().sum()` = 1 | **Exclude** the later copy | Same in every column, out of date order at the end of the file. One item/city/date should have one price. |
| 6 | Date stored as text | all rows | `df.info()` | **Correct** to datetime | Needed for sorting and trends. No value changes. |
| 7 | `unit` and `data_label` identical in every row | all rows | `df["unit"].unique()`, `df["data_label"].unique()` | **Retain**, leave alone | The label says the data is illustrative and not actual PBS observations. That is a warning the reader needs. |
| 8 | Patterns that look odd but are consistent: City 2 is about 6 above City 1; Pulse C hardly moves; weekly dates | all | `groupby` / `pivot` of prices by city | **Leave alone** | Regular patterns, not errors. Cleaning them away would be changing the data. |

## 3. What I expect after cleaning

- 4 items x 2 cities x 12 weekly dates = **96 rows**, **24 per item**, 48 per city, 8 per date.
- Every (date, item, city) slot appears exactly once.
- 3 prices left missing (items 1, 2, 3), all flagged. Every changed value or label has a row in the log.

## 4. Questions I would send back to the intern

- Was `1,250` meant to be 125.0 or something else? Where is the original record?
- Is `9999` the system's "no data" code?
- Why is Staple A / City 1 / 5 Feb labelled `oil b`?
- Why was the 8 Jan Staple A / City 1 row pasted twice?

