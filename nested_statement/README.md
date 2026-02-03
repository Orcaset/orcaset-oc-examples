# Nested Statement Example

This example demonstrates a more complex income model with multiple line items nested into groups.

## Key Concepts

- **Hierarchical Grouping**: Demonstrates nesting groups (Revenue inside Cost of Revenue section).

## Statement Structure

```text
Income Statement
├── Cost of Revenue
│   ├── Revenue
│   │   ├── Recurring
│   │   └── Non-Recurring
│   ├── Recurring (Cost)
│   └── Non-Recurring (Cost)
├── Gross Profit
├── Operating Expenses
│   └── Admin
└── Operating Income
```

## Line Items

| Line Item | Logic |
|-----------|-------|
| **Revenue / Recurring** | Starts at 10,000, grows 5% annually. |
| **Revenue / Non-Recurring** | Random walk with drift. Starts at 2,000, drift 50, volatility 500. Change the seed to produce different results. |
| **Cost of Revenue / Recurring** | `Recurring Revenue * -0.30` (30% expense margin). |
| **Cost of Revenue / Non-Recurring** | `Non-Recurring Revenue * -0.40` (40% expense margin). |
| **Gross Profit** | `Total Revenue + Total Cost of Revenue`. |
| **Opex / Admin** | Starts at -1,500, grows 3% annually. |
| **Operating Income** | `Gross Profit + Admin`. |

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
dune exec nested_statement
```

Running this project will print in the form below.

```text
=== INCOME STATEMENT ===           

                                2025-01-01  2025-02-01  2025-03-01  2025-04-01  2025-05-01  2025-06-01  2025-07-01  2025-08-01  2025-09-01  2025-10-01  2025-11-01  2025-12-01
Income Statement              
  Cost of Revenue             
    Revenue                   
      Recurring                     10,000      10,038      10,082      10,124      10,167      10,210      10,254      10,298      10,341      10,385      10,428      10,473
      Non-Recurring                  2,000       2,237       2,005       2,032       1,568       1,781       1,766         975          82          55         105          10
    Total Revenue                   12,000      12,276      12,087      12,156      11,736      11,991      12,020      11,273      10,423      10,441      10,534      10,484

    Recurring                       -3,000      -3,011      -3,024      -3,037      -3,050      -3,063      -3,076      -3,089      -3,102      -3,115      -3,128      -3,142
    Non-Recurring                     -800        -895        -802        -813        -627        -712        -706        -390         -32         -22         -42          -4
  Total Cost of Revenue             -3,800      -3,906      -3,826      -3,850      -3,677      -3,775      -3,782      -3,479      -3,135      -3,137      -3,170      -3,146

  Gross Profit                       8,200       8,369       8,260       8,306       8,058       8,215       8,237       7,793       7,288       7,303       7,363       7,338
  Operating Expenses          
    Admin                           -1,500      -1,503      -1,507      -1,511      -1,515      -1,518      -1,522      -1,526      -1,530      -1,534      -1,538      -1,542

  Operating Income                   6,700       6,866       6,753       6,795       6,543       6,697       6,715       6,267       5,757       5,768       5,825       5,795


=== LINE ITEMS ===
- Recurring
- Non-Recurring
- Recurring
- Non-Recurring
- Gross Profit
- Admin
- Operating Income
```
