# CRE Portfolio Example

This examples runs a massive commercial real estate model for 10,000 properties.

Orcaset makes working with massive models possible. The portfolio example calculates 10 years of monthly values for 27 line items summed over 1,000 properties. Replicating this portfolio in Excel would require 32.4 million cells. For reference, there are only approximately 17.2 million cells in an Excel worksheet, meaning this model would fill almost two entire worksheets *where ever cell has a formula*. Anyone who has tried to build a large Excel model knows that Excel becomes non-responsive on *much* smaller models, and even if Excel could handle the computational scale navigating and updating components would be extremely difficult.

Orcaset's programmatic approach allows users to run massive models and easily navigate across components. This model is a simple example of running a portfolio-scale model with Orcaset.

## Executables

### `run_property`

Prints out a pro forma statement for a single property. It prints a bite sized statement for twelve monthly periods matching the Excel model in the folder. It is useful for verifying that the Orcaset model works as expected before combining and scaling to the entire portfolio.

Note that the property assumptions are different than the property assumptions in the portfolio example.

### `run_portfolio`

Demonstrates portfolio-scale aggregation and model reusability by creating 10,000 properties from the same Orcaset model. The model uses the same default assumptions for each property so that it is easy to verify that the portfolio aggregates correctly. You can easily modify the number of properties changing the `num_properties` or add properties to the portfolio with different assumptions.

This example uses naive parallelism across properties to speed up the process. Each property is an independent model (and in general, investments in a portfolio are independent) which lends well to parallelism. Results will vary, but running on a M3 MacBook Pro takes takes 15-30 seconds.

## Key Concepts

- **Reusing Component Types**: The portfolio executable creates multiple properties by reusing the same property model component types.
- **Portfolio Aggregation**: The example efficiently aggregates across 10,000 properties to rapidly produce a portfolio-level statement. Users can easily navigate down to specific properties or line items within a property for detailed analysis.


## Statement Structure

```text
Levered Cash Flow
├── Unlevered Cash Flow
│   ├── Net Operating Income
│   │   ├── Effective Gross Income
│   │   │   ├── Gross Potential Rent
│   │   │   │   ├── Base Rent
│   │   │   │   ├── Parking Income
│   │   │   │   ├── CAM Recoveries
│   │   │   │   └── Other Income
│   │   │   └── Vacancy & Credit Loss
│   │   └── Operating Expenses
│   │       ├── Property Taxes
│   │       ├── Insurance
│   │       ├── Utilities
│   │       ├── Repairs & Maintenance
│   │       ├── Property Management
│   │       ├── Janitorial
│   │       ├── Landscaping
│   │       └── Security
│   └── Capital Expenditures
│       ├── Capital Reserves
│       ├── Tenant Improvements
│       └── Leasing Commissions
└── Debt Service
    ├── Debt Balance
    ├── Interest Expense
    └── Amortization
```

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
dune exec run_property
dune exec run_portfolio
```

The `run_property` output is printed below. The printout from the portfolio file will have the same structure, but show the aggregated values for 10,000 properties for 120 monthly periods.

```text
PROPERTY ASSUMPTIONS                
====================
Building Size:           25000 SF
Parking Spaces:          50
Purchase Price:          $4500000 ($180/SF)

REVENUE ASSUMPTIONS
====================
Year 1 Base Rent:        $22.00/SF/year
Rent Growth:             3.0%/year
Parking Rate:            $75/space/month
CAM Recovery:            85% of OpEx
Vacancy/Credit Loss:     7%

DEBT ASSUMPTIONS
====================
Loan Amount:             $3150000 (70% LTV)
Interest Rate:           5.50%
Term:                    25 years
Monthly Payment:         $19343.76


PRO FORMA CASH FLOW PROJECTION (5-Year)
=======================================

                                       2023-01-01    2023-02-01    2023-03-01    2023-04-01    2023-05-01    2023-06-01    2023-07-01    2023-08-01    2023-09-01    2023-10-01    2023-11-01    2023-12-01
===========================================================================================================================================================================================================

Real Estate Pro Forma

  Gross Potential Rent
    Base Rent                               45833         45940         46059         46174         46293         46409         46529         46649         46766         46887         47004         47125
    Parking Income                           3750          3759          3768          3778          3788          3797          3807          3817          3826          3836          3846          3856
    CAM Recoveries                           1700         23946         24696         24772         24825         24879         24932         24986         25041         25094         25149         25202
    Other Income                             1500          1504          1507          1511          1515          1519          1523          1527          1531          1534          1538          1542
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Total Gross Potential Rent                52783         75148         76031         76235         76421         76605         76791         76979         77164         77351         77536         77725

  Less: Vacancy & Credit Loss               -3695         -5260         -5322         -5336         -5349         -5362         -5375         -5389         -5401         -5415         -5428         -5441
  Effective Gross Income                    49088         69888         70709         70899         71072         71242         71415         71591         71762         71937         72109         72284

  Operating Expenses
    Property Taxes                          -6000         -6012         -6025         -6037         -6050         -6063         -6076         -6089         -6102         -6115         -6127         -6141
    Insurance                               -1250         -1252         -1255         -1258         -1260         -1263         -1266         -1269         -1271         -1274         -1277         -1279
    Utilities                               -6250         -6262         -6276         -6289         -6302         -6315         -6329         -6343         -6356         -6369         -6383         -6397
    Repairs & Maintenance                   -4167         -4175         -4184         -4193         -4202         -4211         -4220         -4229         -4238         -4247         -4256         -4265
    Property Management                     -1964         -2796         -2828         -2836         -2843         -2850         -2857         -2864         -2870         -2877         -2884         -2891
    Janitorial                              -5208         -5218         -5229         -5240         -5252         -5262         -5274         -5285         -5296         -5308         -5319         -5330
    Landscaping                             -1250         -1252         -1255         -1258         -1260         -1263         -1266         -1269         -1271         -1274         -1277         -1279
    Security                                -2083         -2087         -2092         -2096         -2100         -2105         -2109         -2114         -2118         -2123         -2127         -2132
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Total Operating Expenses                 -28172        -29054        -29144        -29206        -29270        -29332        -29396        -29460        -29522        -29587        -29649        -29714

  Net Operating Income (NOI)                20917         40833         41565         41693         41802         41910         42020         42131         42240         42350         42460         42571

  Capital Expenditures
    Capital Reserves                        -1473         -2097         -2121         -2127         -2132         -2137         -2142         -2148         -2153         -2158         -2163         -2169
    Tenant Improvements                     -3125         -3125         -3125         -3125         -3125         -3125         -3125         -3125         -3125         -3125         -3125         -3125
    Leasing Commissions                      -982         -1398         -1414         -1418         -1421         -1425         -1428         -1432         -1435         -1439         -1442         -1446
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Total Capital Expenditures                -5579         -6619         -6660         -6670         -6679         -6687         -6696         -6705         -6713         -6722         -6730         -6739

  Cash Flow Before Financing                15338         34214         34905         35023         35123         35223         35324         35426         35527         35628         35729         35831

  Debt Service
    Interest Expense                       -14919        -13456        -14870        -14370        -14825        -14326        -14780        -14758        -14261        -14713        -14217        -14666
    Principal Payment                       -4425         -5888         -4474         -4974         -4519         -5018         -4564         -4585         -5082         -4631         -5127         -4677
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Total Debt Service                       -19344        -19344        -19344        -19344        -19344        -19344        -19344        -19344        -19344        -19344        -19344        -19344

  Cash Flow After Financing                 -4006         14870         15561         15679         15780         15880         15980         16082         16183         16284         16386         16488
```