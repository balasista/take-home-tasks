# Data Dictionary

All sensor data comes from a SCADA historian covering a fleet of motor-driven
centrifugal pumps. One row is one **operating cycle** — roughly one hour of
aggregated operation.

> This is real-world plant data exported from a historian. Treat it as you
> would any production dataset: inspect quality before you trust a channel.

---

## `data/train.csv`

Run-to-failure (and censored) history for the **80 training pumps**, stacked in
long format. Sorted by `unit_id`, then `cycle`.

| Column | Type | Unit | Description |
|---|---|---|---|
| `unit_id` | string | — | Unique pump identifier, e.g. `PUMP_001`. |
| `cycle` | int | cycles | Operating-cycle index within the unit, 1-based. One cycle ≈ 1 hour of aggregated operation. Monotonic per unit. |
| `timestamp` | datetime | — | Wall-clock time of the cycle (`YYYY-MM-DD HH:MM:SS`). |
| `op_regime` | int | — | Operating regime / load class set by the plant controller: `0` = low load, `1` = medium, `2` = high. |
| `op_setpoint_flow` | float | m³/h | Commanded flow setpoint issued to the pump's variable-frequency drive. |
| `ambient_temp_c` | float | °C | Ambient air temperature at the pump skid. |
| `vibration_rms_mms` | float | mm/s | Broadband RMS vibration velocity at the pump bearing housing. |
| `bearing_temp_c` | float | °C | Pump-end bearing temperature. |
| `casing_temp_c` | float | °C | Pump casing surface temperature. |
| `motor_current_a` | float | A | Motor line current. |
| `discharge_pressure_bar` | float | bar | Pump discharge (outlet) pressure. |
| `suction_pressure_bar` | float | bar | Pump suction (inlet) pressure. |
| `flow_rate_m3h` | float | m³/h | Measured process flow rate through the pump. |
| `motor_winding_temp_c` | float | °C | Motor stator winding temperature. |
| `power_kw` | float | kW | Electrical power draw of the motor. |
| `oil_particle_count` | float | ppm | Lubricating-oil particle contamination, from periodic oil lab sampling. Not recorded every cycle. |
| `coolant_flow_lpm` | float | L/min | Flow through the motor cooling jacket. |
| `ambient_humidity_pct` | float | % | Relative humidity at the pump skid. |
| `pump_speed_rpm` | float | rpm | Pump shaft rotational speed. |
| `gearbox_temp_c` | float | °C | Gearbox housing temperature. |

---

## `data/train_units.csv`

One row per training pump — unit-level metadata. Join to `train.csv` on `unit_id`.

| Column | Type | Description |
|---|---|---|
| `unit_id` | string | Pump identifier; joins to `train.csv`. |
| `commissioned_date` | date | Date the pump entered service. |
| `n_cycles` | int | Number of cycles recorded for this unit (= row count in `train.csv`). |
| `event` | int | Failure indicator — **read this carefully:** |

**`event` semantics**

- **`event = 1`** — the pump **ran to failure**. Its **last recorded cycle is
  the failure point**. There are 72 such units.
- **`event = 0`** — monitoring **stopped while the pump was still healthy**
  (right-censored). The true failure cycle is *unknown* and lies somewhere
  *after* `n_cycles`. There are 8 such units. The last cycle of these units is
  **not** a failure — treating it as one will corrupt any time-to-failure label.

---

## `data/sample_test_input.csv`

A small, illustrative example (2 units, relabelled `SAMPLE_01` / `SAMPLE_02`)
showing the **exact schema your inference code will receive** at the live test.
It has the **same per-row columns as `train.csv`**. There is no `event` column
and no failure point — these are pumps still in service. Use it to build and
smoke-test your `predict` interface. See `README.md` for the live-test contract.
