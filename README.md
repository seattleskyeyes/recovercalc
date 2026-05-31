# Vehicle Recovery Resistance Calculator

A real-time, single-page web app that estimates the force (in pounds) needed to
recover a stuck/mired vehicle under different ground conditions and slope angles.
Everything recomputes live as you type — there is no submit button.

Open `index.html` directly in any modern browser. No build step or server is required.

## Modes

- **Saved Fleet** — Add, edit, delete, sort, and reorder vehicles in your own fleet.
  Vehicles persist in your browser via `localStorage`.
- **Manual Estimate** — A quick one-off calculation for helping someone else. Nothing
  is saved. Enter the actual loaded weight or the GVWR, whichever you know.

## The driver weight (W)

Each saved vehicle has two weight inputs:

- **Gross Vehicle Weight (GVW)**
- **Trail Weight**

All calculations use a single driver value:

```
W = max(grossVehicleWeight, trailWeight)
```

The **greater of the two values is always used** as a deliberate safety measure — it
yields a more conservative (higher) recovery-force estimate so you don't under-spec
your recovery gear. Because of this, the per-vehicle cards don't display `W`; just
enter both weights you know and the larger one drives the math automatically.

## Calculation engine

Every resistance value is the driver weight multiplied by a fixed factor:

```
value = W * factor
```

### Factors

**If Mired**

| Scenario           | Factor |
| ------------------ | ------ |
| Tire Depth (75%)   | 0.75   |
| Wheel Depth (100%) | 1.00   |
| Body Depth (150%)  | 1.50   |

**If NOT Mired**

| Scenario               | Factor |
| ---------------------- | ------ |
| Won't Roll (66%)       | 0.66   |
| Soft Gravel/Grass (15%)| 0.15   |
| Hard Surface (10%)     | 0.10   |

**Slope Resistance** (positive = uphill adds resistance, negative = downhill subtracts)

| Slope        | Factor |
| ------------ | ------ |
| +45° (+75%)  | +0.75  |
| +30° (+50%)  | +0.50  |
| +15° (+25%)  | +0.25  |
| 0° (0%)      | 0      |
| -15° (-25%)  | -0.25  |
| -30° (-50%)  | -0.50  |
| -45° (-75%)  | -0.75  |

## Combined estimate

Pick one surface scenario and one slope. The estimate is:

```
subtotal  = surfaceValue + slopeValue
estimate  = subtotal * (1 + 0.10)   // 10% safety buffer is always applied
```

- The surface dropdown starts on **"Select one…"** and will not calculate until you
  choose a scenario.
- The slope dropdown defaults to **0° (0%)**.
- A **10% safety buffer is always included** and shown as a `+` line item.

## Sorting saved vehicles

Use the **Sort by** control in the *Saved Vehicle Resistance Details* section:

- **Manual** — keep your own order; use the ▲ / ▼ buttons on each card to reorder.
- **Name** — alphabetical by vehicle name.
- **Recency** — most recently added first.

Each vehicle card and each major section can be collapsed to keep the page short on
mobile.
