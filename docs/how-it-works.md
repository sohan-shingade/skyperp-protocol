# How Skyperp works

A plain-English walk through the mechanism. This is the concept level. The exact protocol parameters are tuned from internal modeling and are not published here.

## 1. The underlying: a temperature anomaly, not a temperature

Skyperp does not trade the raw temperature. It trades the **anomaly**: how far a forecast sits from the seasonal normal.

```
anomaly(today) = forecast_high(7 days out) − climate_normal(that calendar day)
```

Why anomaly instead of raw degrees? The raw temperature has a giant seasonal swing (cold in winter, hot in summer) that has nothing to do with new information. Subtracting the normal removes that predictable cycle and leaves the part that actually moves on news: the surprise. The result is a series centered near zero that swings both ways, which is exactly what you want to trade.

The forecast horizon is fixed at **7 days out**. Far enough that there is real uncertainty to price, close enough that the forecast carries genuine skill.

## 2. The rolling horizon: a perp that never resolves

A normal futures contract settles on a date. A weather bet resolves when the day arrives. Skyperp does neither.

Every day, the contract points at *the new 7-day-out forecast*. The horizon rolls forward with the calendar, so the contract is always "7 days from now" and never reaches an expiry. There is no settlement event. You hold a position as long as you like and close it whenever you want, like a crypto perpetual.

This is the constant-maturity construction. It is what makes the thing a *perp* rather than a series of expiring bets.

## 3. Three prices, kept honest

With no expiry to force convergence, price honesty has to come from somewhere else. Skyperp separates three distinct prices instead of conflating them:

- **Oracle price** — a reference value built from independent weather forecasts. This is the anchor of truth.
- **Mark price** — what positions are valued and liquidated against. A smoothed blend that resists single-tick manipulation.
- **Trade price** — where you actually fill on the orderbook, set by supply and demand.

Keeping these separate is the difference between a robust derivative and one that a single large order can push around. Liquidations reference the mark, not the last trade.

## 4. The oracle: a composite, not a single feed

The oracle blends multiple independent numerical weather models (GFS from NOAA, ECMWF, ICON) using a robust aggregation, then smooths over recent history. Two reasons:

1. **Manipulation resistance.** The external inputs are published by government and intergovernmental meteorological agencies. There is no thin market an attacker can shove with capital, the way the Mango Markets exploit moved a spot oracle. Corrupting the reference would require compromising independent national weather services at once.
2. **Stability.** A trimmed aggregate of several models is more accurate and less jumpy than any single model, which keeps the tradable series well-behaved.

## 5. Funding: carry replaces settlement

In a normal future, the contract price is dragged to the underlying by the approaching expiry. Skyperp has no expiry, so it uses a **funding rate** instead, the same convergence force crypto perps use.

When the trade price drifts above the oracle, longs pay shorts. When it drifts below, shorts pay longs. The further the dislocation, the stronger the pull. This carry cost is what keeps the market price tethered to the real forecast without any settlement date. Internal simulation across a wide range of trader populations was used to choose funding parameters that converge reliably.

## 6. Putting it together

```
forecasts (GFS · ECMWF · ICON)
        │  robust blend + smoothing
        ▼
   oracle price ──────► mark price ──────► liquidations
        │                                        ▲
        │ funding pulls trade price to oracle    │
        ▼                                        │
   orderbook ─────────► trade price ─────────────┘
```

You go long if you think the 7-day forecast will come in hotter than the market expects, short if cooler. You earn or pay funding while you hold. You close whenever you want. No expiry, no binary, no resolution cliff.

---

For the evidence that this underlying is actually tradable (enough movement, mean-reverting, no dead zones), see [`../research/tradability.md`](../research/tradability.md).
