**JSON Schema — Rocktron Voodu Valve Preset**

This schema is designed so that:

- You can **validate** presets
- You can **serialize/deserialize** them
- You can **convert** between SysEx ↔ JSON
- You can **build an editor UI** directly from it

I’ve kept it practical, readable, and structured exactly like the device’s internal memory.



**What this schema represents**

✔ `name`

Plain text name (the device stores it as UTF‑16‑LE, but JSON uses normal strings).

✔ `parameters`

A dictionary of **0x00–0x7F** parameter IDs → values.

Example:

> "parameters": {
> "07": 120, // Amp Gain
> "10": 64, // Bass
> "11": 70 // Mid
> }

✔ `effects`

The 8‑byte effect parameter blocks you saw in the SysEx:

> 0B 00 4A 00 01 00 00 00

Becomes:

> {
> "id": 11,
> "value": 74,
> "enabled": true
> }

✔ `controllers`

The CC assignment table at the end of the preset.

✔ `checksum`

The final SysEx checksum byte.
