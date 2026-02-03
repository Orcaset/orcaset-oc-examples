# Basic Three Statement Model

This example demonstrates a more complex income model with multiple line items nested into groups.

## Key Concepts

- **Hierarchical Grouping**: Demonstrates nesting groups (Revenue inside Cost of Revenue section).


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

Clone the project folder and run the following commands inside the project directory.

Create a new local environment with OCaml 5.4.0 and update the shell environment.

```sh
opam switch create . 5.4.0
eval $(opam env)
```
Install orcaset from GitHub.

```sh
opam pin add orcaset "https://github.com/orcaset/orcaset.git#a3d1efc1cf02d2da516fe714dc878c1bb582234e"
```

Build and run the project.

```sh
dune build
dune exec basic_three_statement
```

Running this example will print out the statement below:

```text
================================================================================
                     SIMPLE 3-STATEMENT FINANCIAL MODEL
================================================================================

                                     2025-02       2025-03       2025-04       2025-05       2025-06       2025-07       2025-08       2025-09       2025-10       2025-11       2025-12       2026-01
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Financial Model:
  Income Statement:
    Gross Profit:
      Revenue                        1000.00       1003.89       1008.21       1012.41       1016.77       1021.01       1025.40       1029.82       1034.11       1038.56       1042.89       1047.38
      COGS                           -300.00       -301.17       -302.46       -303.72       -305.03       -306.30       -307.62       -308.95       -310.23       -311.57       -312.87       -314.21
    Total Gross Profit                700.00        702.72        705.75        708.69        711.74        714.71        717.78        720.87        723.88        726.99        730.02        733.17

    Opex                             -200.00       -200.00       -200.00       -200.00       -200.00       -200.00       -200.00       -200.00       -200.00       -200.00       -200.00       -200.00
    Depreciation                      -83.33        -83.06        -82.78        -82.51        -82.25        -81.98        -81.73        -81.47        -81.22        -80.98        -80.73        -80.50
    Tax                               -83.33        -83.93        -84.59        -85.24        -85.90        -86.54        -87.21        -87.88        -88.53        -89.20        -89.86        -90.53
    Net Income                        333.33        335.73        338.37        340.94        343.59        346.18        348.84        351.52        354.12        356.81        359.43        362.14

  Cash Flow Statement:
    Operations:
      Net Income Add Back             333.33        335.73        338.37        340.94        343.59        346.18        348.84        351.52        354.12        356.81        359.43        362.14
      Depreciation Add Back            83.33         83.06         82.78         82.51         82.25         81.98         81.73         81.47         81.22         80.98         80.73         80.50
    Total Operations                  416.67        418.79        421.15        423.45        425.84        428.16        430.57        432.99        435.35        437.79        440.16        442.63

    Investing:
      Capex                           -50.00        -50.19        -50.41        -50.62        -50.84        -51.05        -51.27        -51.49        -51.71        -51.93        -52.14        -52.37
    Total Investing                   -50.00        -50.19        -50.41        -50.62        -50.84        -51.05        -51.27        -51.49        -51.71        -51.93        -52.14        -52.37

    CF Financing                        0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00
    Net Cash Change                   366.67        368.59        370.74        372.83        375.00        377.11        379.30        381.50        383.64        385.86        388.02        390.26


                                     2025-02       2025-03       2025-04       2025-05       2025-06       2025-07       2025-08       2025-09       2025-10       2025-11       2025-12       2026-01
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Balance Sheet:
  Assets:
    Cash                             1366.67       1735.26       2106.01       2478.84       2853.84       3230.95       3610.25       3991.75       4375.39       4761.26       5149.28       5539.54
    PPE Net                          9966.67       9933.81       9901.43       9869.54       9838.14       9807.20       9776.74       9746.76       9717.25       9688.20       9659.61       9631.48
  Total Assets                      11333.33      11669.07      12007.44      12348.38      12691.98      13038.15      13387.00      13738.52      14092.64      14449.45      14808.88      15171.02

  Liabilities & Equity:
    Common Stock                     5000.00       5000.00       5000.00       5000.00       5000.00       5000.00       5000.00       5000.00       5000.00       5000.00       5000.00       5000.00
    Retained Earnings                6333.33       6669.07       7007.44       7348.38       7691.98       8038.15       8387.00       8738.52       9092.64       9449.45       9808.88      10171.02
  Total Liabilities & Equity        11333.33      11669.07      12007.44      12348.38      12691.98      13038.15      13387.00      13738.52      14092.64      14449.45      14808.88      15171.02

  Check:
    Balance Check                       0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00          0.00
```

