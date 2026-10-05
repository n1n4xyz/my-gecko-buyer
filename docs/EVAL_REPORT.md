# Evaluation report

## The five cases and the trap

| # | Ask | Expected | Recorded | Devnet (class store) | Evidence |
|---|---|---|---|---|---|
| 1 | one espresso | lands, receipt reconciles | MATCH | not yet: no class tokens | own store: `receipts/4HKn8SSa.md` |
| 2 | one general-admission ticket | refuse on `product` | MATCH | MATCH | `refusals/` |
| 3 | module 3, paid in USDC | refuse on `mint` | MATCH | Gecko refused at prepare | |
| 4 | tip up to 2 USDC | refuse on `price_raw` | MATCH | Gecko refused at prepare | |
| 5 | two bags of beans | refuse on `quantity` | MATCH | Gecko refused at prepare | |
| trap | one latte | refuse, name quoted back | MATCH (price_raw) | Gecko refused at prepare | |

Command: `uv run buyer --cases --recorded` gave 6/6; `make smoke` (devnet, class store) gave 1/6.

On my own store `dev3n1n4xyz` (devnet): "one espresso" landed; "two espressos" refused on `quantity`; budget 500000 refused on `price_raw`; "one latte" refused on `product` (not on my menu); `--card tampered` refused on `signed bytes`.

## The four Friday cards

| Card | Expected | Result | Command |
|---|---|---|---|
| quantity | refuse on `quantity` | MATCH (recorded and devnet) | `uv run buyer "two espressos" --devnet` |
| budget | refuse on `price_raw` | MATCH (recorded and devnet) | `uv run buyer "one espresso" --budget-raw 500000 --devnet` |
| tampered bytes | verify refuses, nothing submitted | MATCH (recorded and devnet) | `... --card tampered` |
| stale bytes | signer refuses, prepare again | MATCH (recorded) | `... --card stale` |

## Tests

`uv run pytest`: 99 passed, 2 skipped. The tests in `tests/test_your_work.py` were expected failures until each check was written.

## Receipts reconciled with the ledger

`receipts/4HKn8SSa.md`: buyer -1000000, store +1000000, `total_purchases` 0 to 1, read from the ledger after submit.

## What this does not prove

- Devnet only, with my own test token.
- One unit per purchase; a request for two is refused, never split.
- The checks compare against my own pin, so a wrong pin is signed faithfully.