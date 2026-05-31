# FAQ

## Product

**Why an anomaly instead of the raw temperature?**
The raw temperature has a huge predictable seasonal swing that carries no new information. Subtracting the seasonal normal leaves the surprise, the part that actually moves on forecast updates. It centers the series near zero and makes both directions tradable.

**Why 7 days out, not tomorrow or a month?**
Tomorrow's forecast is nearly certain, so there is little to trade. A month out, forecast skill collapses toward climatology. Seven days is the sweet spot: genuine uncertainty plus genuine skill.

**Why NYC first?**
Deep, liquid public forecast data, a large population of people who care about its weather, and a clean climatological record. It is the most legible single market to launch and prove the mechanism.

**Isn't this just a prediction market with extra steps?**
No. A prediction market is binary and expires: yes/no, fixed payout, gone on the event date. Skyperp is continuous and perpetual: every degree is a price, you trade the magnitude, you hold with leverage as long as you want, and there is no resolution event. It is to weather prediction markets what perps were to spot.

**Can I lose more than I put in?**
Like any leveraged product, positions are liquidated when margin is exhausted. You manage that with position size and leverage. Liquidations reference a manipulation-resistant mark price, not the last trade.

## Oracle

**Why a composite oracle instead of one feed?**
Robustness and accuracy. Blending several independent numerical weather models (GFS, ECMWF, ICON) with a trimmed aggregation is both more accurate than any single model and far harder to corrupt. No single source can move the reference.

**What if a forecast source goes down or prints garbage?**
The aggregation is designed to survive a single bad or missing source, and large single-source deviations trigger a circuit breaker rather than flowing straight into the mark.

**Can someone manipulate the oracle?**
The external inputs are published by government and intergovernmental weather agencies, not by a thin market an attacker can push with capital. Moving the on-venue component faces quadratic cost, and position limits bound the prize. The manipulation economics were modeled adversarially during validation.

## Substrate

**Why Solana?**
Cheap, fast finality suits a product that updates an oracle and runs an orderbook frequently. A mature perps and market-making ecosystem (Phoenix-style CLOBs, existing MM tooling) to build against.

**Why not Ethereum L2s?**
Latency and per-action cost matter for an orderbook-style perp with regular oracle updates. Solana's execution model fits the workload better for this product.

## Regulation

**Is this a swap? What about US users?**
Weather has a long history as a tradable commodity, and the CFTC has been actively shaping a framework for onchain perpetuals. The launch approach respects that landscape, including jurisdiction controls where appropriate. Formal legal review is part of the path to launch.

**Is the weather data licensed for commercial use?**
The forecast sources used are available under terms compatible with commercial derivative pricing. This was checked during validation.

## Risk

**What's the biggest thing that could kill this?**
Demand. The mechanism can be elegant and the statistics clean, but the product only matters if traders and hedgers actually want to size positions on it. That is the question we care about most, and we are testing it directly rather than assuming it.

**What's the security posture?**
Standard for a serious Solana protocol: audited programs before any real capital, conservative position limits at launch, and hardened key management. The single loudest lesson from recent DeFi history is that weak multisig kills protocols faster than weak code, and the launch plan reflects that.

---

Have a question that is not here? Open an issue.
