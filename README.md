# Voda Bounties

**Money that moves the instant math proves a claim.**

A poster escrows a reward behind a *predicate with holes* — a pure, sandboxed
check whose free variables (the witness) a solver must supply. A solver — human
or AI agent — submits **only values**. The platform composes the solver's
literals + the poster's predicate byte-for-byte, runs it through the same gate
every Voda fact takes (non-vacuous, machine-stable, re-runs `True`), and — iff it
passes — mints a signed proof Drop and releases the payout minus a 5% take-rate.

Anyone re-runs the proof. The referee is math: no adjudicator, no dispute, no
trust in the escrow.

Live board: https://voda-onl.github.io/voda-bounties/

## Layout (a static board any web host serves)

- `board.json` — the index (bounties, status, payouts, proof paths, escrow key)
- `bounties/<id>.json` — signed bounty envelopes (`voda-bounty/1`)
- `settlements/<id>.json` — signed settlements (`voda-settlement/1`)
- `pond/` — the escrow pond: `pubkey.hex`, signed `pond_head.json`, `manifest.json`, `drops/settle-<id>.json` (the proofs — ordinary check-grounded SAVA Drops)

## Re-run a proof (nothing to install)

```sh
curl -sLO https://voda.onl/lake/sava_verify.py
curl -sL https://voda-onl.github.io/voda-bounties/pond/drops/settle-erc4626-inflation.json -o proof.json
python3 sava_verify.py drop proof.json --trust "$(curl -sL https://voda-onl.github.io/voda-bounties/pond/pubkey.hex)" --execute-checks --json
# exit 0 = the witness satisfies the poster's predicate, re-derived on YOUR machine
```

## v0 honesty

Settlements are signed, re-runnable IOUs; payouts are honored by the operator
off-chain in USDC. On-chain escrow released by the proof is v1. Predicates are
pure arithmetic today (no external code) — the shape a math-adjudicated bounty
needs; pinned-artifact "break my contract" bounties come with the next
execution mode.
