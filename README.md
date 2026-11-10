# ioBroker.historian (ioBroker adapter `historian`)

History of ioBroker states for the SCADA: a **native adapter** (RFC-008) whose engine is written in Rust
(built from a private engine repository). It reads the Valkey stream that
scada.js-controller writes in the same step as each state (RFC-006), filters as the history adapters do
(`sql`, `influxdb`, `history`: same custom settings, checked against `iobroker.influxdb` 5.1.8 by a
differential test), and stores in **TimescaleDB**, **ClickHouse** or **InfluxDB 2**.

- Needs scada.js-controller with native adapters and the history stream (v2).
- `common.platform: "Rust_Native"`: the controller starts `bin/<os>-<arch>/historian[.exe]`.
- Admin: instance settings (`admin/jsonConfig.json`) and the per-state custom dialog (`admin/jsonCustom.json`).
- `getHistory` through `sendTo`, for admin, mywebui and the Grafana gateway.
- Our licence (scada-core, product `scada-historian`): 14 days without a key, then the history stops completely
  until a key is entered; the admin creates the activation request for the vendor.

## Build

The binaries are not in git. They are built from the engine repository and copied in:

    cargo build --release -p history-engine          # the licence check is a default feature
    # bin/win-x64/historian.exe, bin/linux-x64/historian, bin/linux-arm64/…, bin/osx-arm64/…, bin/osx-x64/…

then `npm pack`.
