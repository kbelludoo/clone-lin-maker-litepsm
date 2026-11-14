# clone-lin-maker-litepsm

Experimental **LIN** clone of Maker/Sky `DssLitePsm` —
`_sellGem` / `_buyGem` (uint64-scale WAD'=1e9, explicit `to18` / `tin` / `tout` /
`HALTED`, kiss whitelist).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [sky-ecosystem/dss-lite-psm](https://github.com/sky-ecosystem/dss-lite-psm) `dbf0022225f645f5697e5517d0cf00810471bccf`
  - `src/DssLitePsm.sol` sha256 `502eed38778ac29758959cadbb3d2f36aa3af21144e7374483795581a6279ce8` blob `8e18705eed287cfdf7f6b6e0a74b398b792cc243`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_litepsm.lin`](src/lin_maker_litepsm.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_litepsm_c11.c`](test/oracles/maker_litepsm_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_litepsm_kernel.c`](test/oracles/maker_litepsm_kernel.c)
- **Proofs (lin-open only):** `examples/maker_litepsm/` and `make maker-litepsm-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256. Not a production PSM replacement.

Named goldens (uint64-scale, `to18=1000` as 6-dec gem → 9-dec wad, `WAD'=1e9`):

| id | call | value |
|---|---|---|
| sell 0 fee | gem=1e6 to18=1e3 tin=0 | **1000000000** |
| sell 0.1% | tin=1e6 | **999000000** |
| buy 0.1% | tout=1e6 | **1001000000** |
| sell halted | tin=-1 | **0** |
| nofee without kiss | kissed=0 | **0** |
| nofee with kiss | kissed=1 | **1000000000** |

`gem` / `to18` / `tin` / `tout` / `wad` / `kissed` are arguments (Solidity uses
storage `tin`/`tout`, immutable `to18ConversionFactor`, `type(uint256).max`
HALTED, `bud[msg.sender]`, `unchecked { daiOut -= fee }`). `7e9*3e9` fail-closes
while naive i64 wraps. Distinct from DaiJoin (wad→rad) and GemJoin (ilk slip).

## Reproduce proofs (in lin-open)

```
make maker-litepsm-proof
```

## Not claimed

Uint256 / mainnet USDC-6 `* 1e12` wad / `vat.frob` fill/trim/chug / pocket ERC-20.
LIN replacing Maker, Sky, or the EVM. TVL or dollar savings. zk soundness.
DaiJoin or GemJoin. Hidden PSM immutables.
