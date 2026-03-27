# Rocktron Voodu Valve  
## Developer MIDI Guide  
### Complete MIDI Implementation + SysEx Specification + Parameter Reference

This document provides a **complete, developer‑grade** description of the Rocktron Voodu Valve’s MIDI and SysEx behavior.  
It is reconstructed from a full memory dump and verified against hardware.

Rocktron never published this information — this is the definitive reference.

---

# 1. MIDI Implementation Chart

| Function | Supported | Notes |
|---------|-----------|-------|
| **MIDI Channels** | 1–16 | User‑selectable (Global Settings) |
| **Program Change** | Yes | Selects presets 0–255 |
| **Control Change (CC)** | Yes | Fully assignable |
| **Pitch Bend** | Yes | Assignable |
| **Channel Aftertouch** | Yes | Assignable |
| **Poly Aftertouch** | No | — |
| **Bank Select** | No | Linear preset numbering only |
| **MIDI Clock** | Yes | Tempo sync for delay/mod |
| **SysEx** | Yes | Full preset/global dump & load |
| **Active Sensing** | Yes | Passed through |
| **System Reset** | No | Ignored |

---

# 2. Control Change (CC) Behavior

### Default CC Assignments
| CC | Function | Notes |
|----|----------|-------|
| **7** | Volume | Default assignment |
| **5** | Wah | Common assignment |
| **1, 4, 64, etc.** | Assignable | Any CC 0–127 |

### Controller Assignment Table
The Voodu supports **4 controller mappings**, each containing:

- Source CC  
- Target parameter ID (0x00–0x7F)  
- Min value  
- Max value  

These appear at the end of each preset SysEx.

---

# 3. Program Change

| Feature | Supported | Notes |
|---------|-----------|-------|
| **Program Change** | Yes | Selects presets 0–255 |
| **Program Change Enable** | Yes | Can be disabled globally |
| **Program Mapping** | No | No remap table |

---

# 4. SysEx Overview

All SysEx messages follow this structure:

```
F0 00 00 29 07 <type> <subtype?> <data...> <checksum> F7
```

| Field | Meaning |
|-------|---------|
| `00 00 29` | Rocktron Manufacturer ID |
| `07` | Device family (Voodu/Chameleon) |
| `<type>` | Message type |
| `<subtype>` | Only for global messages |
| `<checksum>` | 7‑bit sum of data bytes |

---

# 5. SysEx Message Types

| Type | Meaning | Notes |
|------|---------|-------|
| **2A** | Preset Dump | One SysEx per preset |
| **2B 00** | Global Parameter Index Table | Sent once |
| **2B 01** | Global Settings Block | Sent once |

---

# 6. Preset SysEx Format

### 6.1 Header

```
F0
00 00 29        ; Rocktron
07              ; Device family
2A              ; Preset dump
vv vv           ; Format version (usually 06 00)
```

### 6.2 Parameter Block (0x00–0x7F)

- 128 parameters  
- Each stored as **16‑bit little‑endian**  
- Total size: **256 bytes**

Example:

```
78 00 = 120
3D 00 = 61
```

### 6.3 Preset Name (UTF‑16‑LE)

- 11 characters  
- UTF‑16‑LE  
- Space‑padded  
- Total size: **22 bytes**

### 6.4 Effect Parameter Table

Each effect parameter is stored as an 8‑byte structure:

```
ii 00 vv 00 ee 00 00 00
```

| Field | Meaning |
|--------|---------|
| `ii` | Parameter ID |
| `vv` | Value |
| `ee` | Enable flag (0/1) |
| `00 00` | Reserved |

### 6.5 Controller Assignment Table

Up to **4 entries**, each 8 bytes:

```
cc 00 tt 00 mn 00 mx 00
```

| Field | Meaning |
|--------|---------|
| `cc` | MIDI CC number |
| `tt` | Target parameter ID |
| `mn` | Minimum value |
| `mx` | Maximum value |

### 6.6 Checksum

```
checksum = (sum of all data bytes) & 0x7F
```

### 6.7 End of Message

```
F7
```

---

# 7. Global SysEx Blocks

## 7.1 Global Block 0 — Parameter Index Table

```
F0 00 00 29 07 2B 00
00 01 02 03 ... 7F 00
<checksum>
F7
```

A simple enumeration of parameter IDs.

## 7.2 Global Block 1 — Global Settings

```
F0 00 00 29 07 2B 01
<data...>
<checksum>
F7
```

Contains:

- MIDI channel  
- Program change enable  
- Output mode  
- Tuner settings  
- Global EQ / cab sim  
- Noise gate defaults  

All values are 16‑bit LE.

---

# 8. Bulk Dump / Load Behavior

### Bulk Dump Order
1. Preset 0  
2. Preset 1  
3. …  
4. Preset 255  
5. Global Block 0  
6. Global Block 1  

### Bulk Load Behavior
- The **first** preset SysEx received is written to **preset 0**  
- The **second** to **preset 1**  
- …  
- Preset numbers are **not stored** inside SysEx  

Order determines destination.

---

# 9. Full Parameter Reference Table (0x00–0x7F)

*(See full table in `PARAMETERS.md` — included separately for clarity.)*

This table defines:

- Parameter ID  
- Human‑readable name  
- Range  
- Notes  

Covers:

- Input / Gate / Compressor  
- Amp  
- EQ  
- Cabinet  
- Modulation  
- Delay  
- Reverb  
- Mixer / Routing  
- Controller assignments  

---

# 10. Example Preset SysEx (Annotated)

```
F0
00 00 29        ; Rocktron
07              ; Device family
2A              ; Preset dump
06 00           ; Format version
...             ; 128 parameters (256 bytes)
...             ; Preset name (22 bytes)
...             ; Effect parameter table
...             ; Controller table
36              ; Checksum
F7
```

---

# 11. Notes for Developers

- All values are **16‑bit little‑endian**, even when range is 0–127  
- Names are UTF‑16‑LE  
- Effect table is sparse — only active blocks appear  
- Controller table is fixed at 4 entries  
- Checksum must be correct or the unit ignores the message  
- Preset numbers are **implicit**, not encoded  

---

# 12. Revision History

- **2025‑03‑24** — Initial complete reverse‑engineered specification  
- Verified against full SysEx dump and hardware behavior  

