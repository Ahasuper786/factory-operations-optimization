# Factory Operations Optimization with Linear Programming

A production-planning project that uses **linear programming** to determine how much raw material a factory should buy, process, store, and sell each month while respecting capacity constraints and maximizing total profit.

## Business problem

The factory faces five linked monthly decisions:

- raw material purchasing,
- production,
- sales,
- raw-material inventory,
- finished-goods inventory.

The model balances these decisions across time, including purchase limits, processing capacity, warehouse limits, demand, and different holding costs.

## Headline result

For the 12-month demand scenario in `data/demands.csv`, the optimal schedule produces:

**Optimal profit: £362,000**

The solution respects:

- maximum purchases of 10 tonnes per month,
- maximum processing of 15 tonnes per month,
- demand ceilings,
- 100-tonne raw and finished-goods warehouse capacities,
- month-to-month material-flow balance.

## Optimal schedule visual

![Optimal 12-month schedule](assets/optimal-schedule.svg)

## Why this is a useful analytics project

This is not a prediction model. It demonstrates a different skill set:

**Operations Research · Linear Programming · Production Planning · Inventory Optimization · Python · SciPy · Mathematical Modelling · Constraint Design · Decision Analytics**

## Model

For month *t*, the decision variables are:

- `buy_t`: raw material purchased
- `process_t`: raw material processed
- `sell_t`: finished product sold
- `raw_stock_t`: ending raw inventory
- `finished_stock_t`: ending finished inventory

The objective maximizes:

```text
sales revenue
- purchase cost
- processing cost
- raw inventory cost
- finished-goods inventory cost
```

subject to raw-material flow balance, finished-goods flow balance, capacity limits, demand limits, and non-negativity.

## Validation

The repository includes automated checks for two small scenarios and the full 12-month case.

A key correction from the original coursework is also captured in the tests: with demand `(0, 25)` across two months, processing **20 tonnes in month 2 is infeasible** because monthly processing capacity is 15 tonnes. The corrected optimum processes 5 tonnes in month 1 and 15 tonnes in month 2, stores both raw and finished inventory after month 1, sells 20 tonnes in month 2, and earns **£57,500**.

## Repository structure

```text
factory-operations-optimization/
├── README.md
├── requirements.txt
├── data/
│   └── demands.csv
├── docs/
│   └── MODEL_NOTES.md
├── results/
│   ├── optimal_schedule.csv
│   └── summary_metrics.csv
├── src/
│   ├── __init__.py
│   └── factory_optimization.py
├── tests/
│   └── test_factory_optimization.py
└── .github/
    └── workflows/
        └── tests.yml
```

## Run locally

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate

pip install -r requirements.txt
python src/factory_optimization.py
pytest -q
```

## Notes

The model is a finite-horizon deterministic linear program. It assumes the monthly demand scenario is known in advance and does not model demand uncertainty, production setup costs, supplier lead times, or salvage value. These would be natural extensions for a more advanced planning system.
