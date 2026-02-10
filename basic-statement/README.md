# Basic Income Statement Example

This example demonstrates a simple income statement with mutual dependencies between Revenue and Operating Expense lines.

## Key Concepts

- **Circular Dependency**: Services revenue depends on total operating expenses, while COGS depends on software revenue. This is handled using `lazy` evaluation and mutually recursive modules.

## Statement Structure

```text
Income Statement
├── Revenue
│   ├── Software
│   └── Services
├── Operating Expenses
│   ├── COGS
│   └── Admin
└── Income
```

## Line Items

| Line Item | Logic |
|-----------|-------|
| **Revenue / Software** | Starts at 1,000, grows 10% annually. |
| **Revenue / Services** | First month 500. Subsequently calculated as `Total Opex * -0.5`. (Depends on Opex). |
| **Opex / COGS** | `Software Revenue * -0.3` (30% of Software Revenue). |
| **Opex / Admin** | Starts at -100, grows 5% annually. |
| **Income** | `Total Revenue + Total Opex`. |

## Run this Example

Clone the project folder and run the following commands inside the project directory.

Create a new local environment with OCaml 5.4.0 and update the shell environment.

```sh
opam switch create . 5.4.0
eval $(opam env)
```
Install orcaset from GitHub.

```sh
opam pin add orcaset git+https://github.com/Orcaset/orcaset-oc#602fec1bde7c8bfd71c8b4d54da1ba5cc7903a9a
```

Build and run the project.

```sh
dune build
dune exec basic_statement
```

Running this example will produce the output below.

```text
                                2025-01-01    2025-04-01    2025-07-01    2025-10-01
  Software                         3024.23       3101.05       3180.97       3262.66
  Services                          500.00        617.67        631.61        645.84
Total Revenue                      3524.23       3718.71       3812.58       3908.50

  COGS                             -907.27       -930.31       -954.29       -978.80
  Admin                            -301.21       -305.02       -308.93       -312.88
Total Operating Expenses          -1208.48      -1235.33      -1263.22      -1291.68

Income                             2315.75       2483.38       2549.36       2616.82
```
