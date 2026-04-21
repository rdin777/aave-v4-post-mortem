# Aave V4 Post-Mortem: Liquidation DoS via MathUtils

## Summary
This repository contains a Proof of Concept (PoC) for a Denial of Service (DoS) vulnerability found in the Aave V4 protocol. The issue resides in the mathematical libraries used for liquidation bonus calculations, which can lead to blocked liquidations for large-scale positions.

## Background
The vulnerability was identified and confirmed through Forge testing. Despite being reported, the issue was initially dismissed. However, recent market events (e.g., KelpDAO/rsETH incident) highlighted the critical nature of restrictive intermediate overflow checks in DeFi protocols.

## Vulnerability Detail
- **Location**: `src/libraries/math/MathUtils.sol` and `src/spoke/libraries/LiquidationLogic.sol:329`
- **Issue**: Intermediate overflow in `mulDivDown` causes a revert when calculating dynamic liquidation bonuses for large positions.
- **Impact**: Positions cannot be liquidated, leading to potential bad debt accumulation.

## Proof of Concept
To run the PoC:
```bash
forge test --match-test testLiquidationBonusDoS -vv
```
