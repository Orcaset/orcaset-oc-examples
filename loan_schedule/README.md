# Loan Schedule Example

This example creates a loan scheduled for a standard fixed-rate amortizing commercial real estate loan.

## Statement Structure

```text
Three Statement Model
├── Income Statement
│   └── Net Income
│       ├── Gross Profit
│       │   ├── Revenue
│       │   └── COGS
│       ├── Opex
│       ├── Depreciation
│       └── Tax
├── Cash Flow Statement
│   ├── Operations
│   │   ├── Net Income
│   │   └── Depreciation
│   ├── Investing
│   │   └── Capex
│   └── Financing
└── Balance Sheet
    ├── Total Assets
    │   ├── Cash
    │   └── PPE Net
    └── Liabilities & Equity
        ├── Common Stock
        └── Retained Earnings
```

## Line Items

| Line Item | Logic |
|-----------|-------|
| **Revenue** | Starts at 1,000, grows 5% annually (Actual/360 day count). |
| **COGS** | Revenue * -0.30 (30% of revenue). |
| **Gross Profit** | Revenue + COGS. |
| **Opex** | Constant -200.0 per month. |
| **Depreciation** | Prior PPE Net * (10% / 12). Depreciation rate is 10% annually. |
| **Tax** | (Gross Profit + Opex + Depreciation) * -0.20. Tax rate is 20%. |
| **Net Income** | Pre-tax Income + Tax. |
| **Net Income Add Back** | Net Income (from Income Statement). |
| **Depreciation Add Back** | -Depreciation (converts negative expense to positive cash add-back). |
| **CF Operations** | Net Income Add Back + Depreciation Add Back. |
| **Capex** | Revenue * -0.05 (5% of Revenue). |
| **CF Investing** | Capex. |
| **CF Financing** | Constant 0.0. |
| **Net Cash Change** | CF Operations + CF Investing + CF Financing. |
| **Cash** | Starts at 1,000. Accumulates Net Cash Change. |
| **PPE Net** | Starts at 10,000. Increases by -Capex (positive asset addition) and decreases by -Depreciation (negative asset reduction). |
| **Common Stock** | Constant 5,000. |
| **Retained Earnings** | Starts at 6,000 (calculated as Initial Assets - Initial Stock). Accumulates Net Income. |
| **Balance Check** | Total Assets - Total Liabilities & Equity. Should be 0. |

## Run this Example

Clone the project folder and run the following commands inside the current directory:

```sh
# Create a new local switch with OCaml 5.4
opam switch create . 5.4.0

# Update shell environment
eval $(opam env)

# Install orcaset from GitHub
opam pin add orcaset https://github.com/orcaset/orcaset.git#a3d1efc1cf02d2da516fe714dc878c1bb582234e

# Build the project
dune build

# Run the executable
dune exec loan_schedule
```

Running this example will print the output below.

```text
=== Fixed-Rate Amortizing Loan Schedule ===
Loan Amount:     $50000000.00
Annual Rate:     6.500%
Term:            360 months (30.0 years)
Day Count:       30/360
Monthly Payment: $316034.01
Start Date:      2025-01-01


=== Confirm Principal Payments ===
Loan Amount: $50000000.00
Total Repaid Principal: $50000000.00
Difference: $-0.00

=== Loan Balance Queries ===
Balance on 2025-01-01: $50000000.00
Balance on 2025-01-15: $50000000.00
Balance on 2028-02-04: $48153716.97
Balance on 2040-01-07: $36279570.62

Month        Date     Beg Balance       Payment      Interest     Principal     End Balance
-----------------------------------------------------------------------------------------
    1  2025-02-01     50000000.00     316034.01     270833.33      45200.68     49954799.32
    2  2025-03-01     49954799.32     316034.01     270588.50      45445.52     49909353.81
    3  2025-04-01     49909353.81     316034.01     270342.33      45691.68     49863662.13
    4  2025-05-01     49863662.13     316034.01     270094.84      45939.18     49817722.95
    5  2025-06-01     49817722.95     316034.01     269846.00      46188.01     49771534.94
    ...
```