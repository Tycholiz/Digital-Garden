
Don't use money.
- Instead use `decimal` (aka. `numeric`) with 2 units of forced precision

if you only care about prices to 2 decimal places, then you shouldn't really care about using anything more precise than numeric type with 2 precision decimal places. However, this would coerce incoming values like `44.423` to be rounded to `44.42`, and the amounts could be very far off. Therefore, if we are controlling the prices coming in, and we always know that it will be to 2 decimal places then it shouldn't be a problem.
- If we are really concerned about this, then we can use microcurrencies, which store the value as a bigint 1 millionth of the numeric value. This allows lots of precisio
