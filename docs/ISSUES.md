# Issues

## 2026-10-05: live smoke on the class store refused at prepare, 5 of 6

- **What I saw:** `PYTHONPATH=. make smoke` printed `Gecko refused, receipt-failed ... sender_token_account HzLnpc... does not exist on devnet` for every case that reached prepare. Result 1/6.
- **What was actually wrong:** my buyer holds my own token (4EZR5K...) but no class token (Eoqdd4...), so it has no token account for the class store's mint.
- **How I found it:** Gecko's refusal names the missing account. The simulation failed before any bytes were returned.
- **What I changed:** nothing in code. Bought from my own store instead: one espresso landed (4HKn8SSa), four refusals by field on devnet.
- **What it cost:** nothing. No signature, no fee.
- **Would the checks have caught it?** No, and they did not need to: Gecko refused before my checks ran.

## 2026-10-05: "module 3" was pinned as 3 units

- **What I saw:** `uv run buyer --cases --recorded` showed `pin 3 x 'Module 3'` and `pin 2 x 'Tip'` for "tip up to 2 USDC".
- **What was actually wrong:** `parse_intent` read the first digit in the sentence as the quantity, but the 3 is part of a product name and the 2 is a price cap.
- **How I found it:** reading the pin line, not the final verdict. The cases still matched, because mint and price refused first.
- **What I changed:** quantity now comes only from number words ("one", "two").
- **What it cost:** nothing on chain. It would have caused a false `quantity` refusal for "module 3".
- **Would the checks have caught it?** Partly: `check_quantity` would refuse, but for the wrong reason.