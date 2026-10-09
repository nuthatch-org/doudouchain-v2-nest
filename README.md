# DOUDOCHAIN_V2 nest

An installable [Nuthatch](https://github.com/nuthatch-org/nuthatch) nest for the DOUDOCHAIN_V2
event surface on Arbitrum One. It is seeded from Graph deployment
[`QmXf82bXak3752bwJ1x7SWchMiEP3Z4vWCWxUJ2HY3wdhj`](https://www.lodestar-dashboard.com/subgraphs/QmXf82bXak3752bwJ1x7SWchMiEP3Z4vWCWxUJ2HY3wdhj),
the resolved deployment for the network ID
`HS4oauJ4GSxNUroWB8bE85ER2GU4X5apckyaaGSkUYCK`.

```sh
nuthatch init --from https://github.com/nuthatch-org/doudouchain-v2-nest
nuthatch dev --dir doudouchain-v2-nest --rpc https://your-arbitrum-archive-rpc
```

The earliest source begins at block `488,650,631`. Omit `--backfill` for its full event history.
Keep an authenticated Arbitrum RPC outside the repository: public endpoints are included only as
best-effort bootstrap fallbacks and some quite understandably take a dim view of large history runs.

## What it indexes

Thirteen fixed contracts, their pinned source ABIs, and the 84 event tables declared by the Graph
manifest. This includes ticket and prize activity, the series, bundle, refund, redraw and
reward modules, VRF requests and fulfilments, collection-book events, and membership/voucher events.
Every table retains block, transaction, log, and emitting-address provenance.

## Graph compatibility

This is an event-input replacement, not an entity-for-entity Graph mapping port. The original
subgraph uses four `file/ipfs` dynamic templates to enrich IPFS content. Nuthatch deliberately
does not fetch mutable off-chain content while decoding canonical chain data, so those metadata
entities and mapping-maintained state are not present here. The 13 Ethereum data sources are all
static and their pinned ABI event selections are vendored exactly.

The source manifest calls its primary contract data source `ICHICHAIN`, which is why the raw event
tables retain the `ichichain__` prefix. That is a data-source alias, not the deployment name.
Two source data sources share its address, and two share the VRF-router address. They remain
distinct tables because their Graph handlers serve distinct entity surfaces; this is not an accidental
duplicate scanner.

## Verification

`checks/` contains fixed-window event-surface and address-scope fixtures recorded from an
archive-backed run through block `495,414,965` on 2026-08-17. Run them after a full backfill:

```sh
nuthatch check --dir .
```

Regenerate `schema.json` after changing `nuthatch.toml`:

```sh
nuthatch schema --dir .
```
