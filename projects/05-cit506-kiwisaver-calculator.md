# 💰 KiwiSaver Lump Sum Calculator (Python)

**Course:** CIT 506, Programming Essentials · Assessment 2

## Overview
A console program that shows how a one-off KiwiSaver deposit grows at **4%, 6% and 8%** over 1–45 years, with an optional year-by-year breakdown. It's built so an accountant can show clients long-term outcomes. It follows the [flowchart and pseudocode I planned in Assessment 1](04-cit506-kiwisaver-planning.md) and needs no external libraries.

## Features
- ✅ Input validation with re-prompting (no crashes on bad input)
- ✅ Compound interest calculated year by year
- ✅ Three interest-rate scenarios side by side
- ✅ Optional yearly breakdown (starting balance, interest, ending balance)
- ✅ Documented test regime and user manual

## Code

```python
# KiwiSaver Lump Sum Calculator
# Calculates lump-sum growth at 4%, 6% and 8% with an optional yearly breakdown.

def validate_float(value):
    """Validate that input is a positive float."""
    try:
        value = float(value)
        if value <= 0:
            return None
        return value
    except ValueError:
        return None


def validate_int(value):
    """Validate that input is an integer between 1 and 45."""
    try:
        value = int(value)
        if 1 <= value <= 45:
            return value
        return None
    except ValueError:
        return None


def calculate_growth(deposit, years, rate, breakdown=False):
    """Calculate compound interest growth for a given rate."""
    starting_balance = deposit
    total_interest = 0
    breakdown_table = []

    for year in range(1, years + 1):
        interest = starting_balance * rate
        total_interest += interest
        ending_balance = starting_balance + interest

        if breakdown:
            breakdown_table.append({
                "Year": year,
                "Starting Balance": round(starting_balance, 2),
                "Interest": round(interest, 2),
                "Ending Balance": round(ending_balance, 2),
            })

        starting_balance = ending_balance

    return total_interest, breakdown_table


# MAIN PROGRAM
print("=== KiwiSaver Lump Sum Calculator ===")

while True:
    deposit = validate_float(input("Enter lump sum deposit amount: "))
    if deposit is not None:
        break
    print("Invalid deposit. Please enter a positive number.")

while True:
    years = validate_int(input("Enter number of years (1–45): "))
    if years is not None:
        break
    print("Invalid years. Please enter a whole number between 1 and 45.")

breakdown = input("Would you like a yearly breakdown? (Y/N): ").strip().upper() == "Y"

print("\n=== Results ===")
for rate in [0.04, 0.06, 0.08]:
    total_interest, table = calculate_growth(deposit, years, rate, breakdown)
    print(f"\nAmount deposited: ${deposit:.2f} at {rate*100:.0f}% per annum")
    print(f"Total interest earned over {years} years: ${total_interest:.2f}")

    if breakdown:
        print("\nYearly Breakdown:")
        for row in table:
            print(row)
```

## Sample output
Deposit **$5,000** over **10 years**:

| Rate | Total interest | Final balance |
|---|---|---|
| 4% | $2,401.22 | $7,401.22 |
| 6% | $3,954.24 | $8,954.24 |
| 8% | $5,794.62 | $10,794.62 |

## Testing
I wrote a test plan and ran it with screenshots as evidence:

| Test type | Input | Result |
|---|---|---|
| Valid input | $5,000, 10 years, breakdown Y and N | Correct totals; table shown or hidden as chosen |
| Invalid deposit | `abc`, `$5000`, empty, `10,000`, `10 000` | Rejected with error message and re-prompt |
| Invalid years | `0`, `100`, `ten`, empty, `10.5` | Rejected with error message and re-prompt |
| Accuracy | 4%, 6%, 8% vs. MoneySmart calculator | Results match |
| Breakdown | $4,500 over 5 years | Complete yearly table at all three rates |
| Large value | $100,000 over 5 years | Correct totals, no overflow or precision issues |

## How to run
```bash
python kiwisaver_calculator.py
```
Requires Python 3.10+. Valid deposit: any positive number (e.g. `5000`, `10000.50`). Valid years: whole numbers from 1 to 45.

## Reflection and future improvements
Planning with a flowchart and pseudocode first made the code easier to write. Testing showed how much real-world input varies, and that clear error messages matter. Next steps I identified: a graphical interface, real KiwiSaver provider data, and comparing multiple deposit scenarios.

**Tools / topics:** Python 3 · functions · loops · exception handling · input validation · test design · technical documentation

---

[⬅ Back to my profile](../README.md)
