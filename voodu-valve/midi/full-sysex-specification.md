# Rocktron Voodu Valve  
## Full System Exclusive (SysEx) Specification  
*(Reverse‑engineered from hardware behavior and full memory dump)*

---

# 1. Overview

The Rocktron Voodu Valve uses SysEx for:

- Preset dump (one SysEx per preset)
- Global settings dump (two SysEx blocks)
- Bulk dump (256 presets + 2 globals)
- Bulk load (same order as dump)

The device **does not store preset numbers** inside SysEx.  
Preset order is determined **by the order of messages** during transmission.

All SysEx messages follow this structure:

```
F0 00 00 29 07 <type> <subtype?> <data...> <checksum> F7
```

---

# 2. Manufacturer & Device IDs

| Field | Value | Meaning |
|-------|--------|---------|
| Manufacturer ID | `00 00 29` | Rocktron |
| Device Family | `07` | Voodu Valve / Chameleon family |

---

# 3. SysEx Message Types

| Type | Meaning | Notes |
|------|---------|-------|
| `2A` | **Preset Dump** | One per preset |
| `2B 00` | **Global Parameter Index Table** | Sent once in bulk dump |
| `2B 01` | **Global Settings Block** | Sent once in bulk dump |

---

# 4. Preset Dump Format

### 4.1 Header

```
F0
00 00 29        ; Rocktron Manufacturer ID
07              ; Device family
2A              ; Message type = Preset Dump
vv vv           ; Format version (usually 06 00)
```

### 4.2 Preset Data Layout

All preset data is stored as **16‑bit little‑endian** values:

```
<value> 00
```

### 4.3 Preset Name (UTF‑16‑LE)

- 11 characters  
- UTF‑16‑LE encoding  
- Space‑padded  
- Total size: **22 bytes**

Example:

```
41 00 43 00 4F 00 55 00 53 00 54 00 49 00 43 00 20 00 20 00 20 00
```

→ “ACOUSTIC     ”

### 4.4 Parameter Block (0x00–0x7F)

The Voodu stores **128 parameters**, each 16‑bit LE:

```
[param00] [00] [param01] [00] ... [param7F] [00]
```

### 4.5 Effect Parameter Table

After the main parameter block, the Voodu stores effect‑specific parameters in **8‑byte structures**:

```
ii 00 vv 00 ee 00 00 00
```

Where:

| Byte(s) | Meaning |
|---------|---------|
| `ii` | Parameter ID |
| `vv` | Value |
| `ee` | Enable flag (0/1) |
| `00 00` | Reserved |

Example:

```
0B 00 4A 00 01 00 00 00
```

### 4.6 Controller Assignment Table

Up to **4 controller mappings**, each 8 bytes:

```
cc 00 tt 00 mn 00 mx 00
```

| Field | Meaning |
|--------|---------|
| `cc` | MIDI CC number |
| `tt` | Target parameter ID |
| `mn` | Minimum value |
| `mx` | Maximum value |

Example:

```
07 00 05 00 78 00 00 00
```

→ CC7 → parameter 5, range 0–120

### 4.7 Checksum

The checksum is a **7‑bit sum** of all data bytes between:

```
after <type> … before F7
```

Formula:

```
checksum = (sum of all data bytes) & 0x7F
```

### 4.8 End of Message

```
F7
```

---

# 5. Global SysEx Blocks

## 5.1 Global Block 0 — Parameter Index Table

```
F0 00 00 29 07 2B 00
00 01 02 03 ... 7F 00
<checksum>
F7
```

This is a simple enumeration of parameter IDs.

## 5.2 Global Block 1 — Global Settings

```
F0 00 00 29 07 2B 01
<data...>
<checksum>
F7
```

Fields include:

- MIDI channel  
- Program change enable  
- Output mode  
- Tuner settings  
- Global EQ / cab sim  
- Noise gate defaults  

All values are 16‑bit LE.

---

# 6. Bulk Dump Format

A full memory dump consists of:

1. **256 Preset SysEx messages**  
2. **Global Block 0**  
3. **Global Block 1**

Order is critical for bulk load.

---

# 7. Bulk Load Behavior

- The first preset SysEx received is written to **preset 0**
- The second to **preset 1**
- …
- The 256th to **preset 255**
- Then global blocks overwrite global memory

Preset numbers are **not stored** inside SysEx.

---

# 8. Example Preset SysEx (Annotated)

```
F0
00 00 29        ; Rocktron
07              ; Device family
2A              ; Preset dump
06 00           ; Format version
...             ; Parameter block (128 × 2 bytes)
...             ; Preset name (22 bytes)
...             ; Effect parameter table (N × 8 bytes)
...             ; Controller table (4 × 8 bytes)
36              ; Checksum
F7
```

---

# 9. Notes for Developers

- All values are **16‑bit little‑endian**, even when the range is 0–127  
- Preset numbers are **implicit**, not encoded  
- UTF‑16‑LE is used for names  
- The effect table is sparse — only active blocks appear  
- Controller table is fixed at 4 entries  
- Checksum must be correct or the unit ignores the message  

---

# 10. Revision History

- **2025‑03‑24** — Initial full reverse‑engineered specification  
- Based on complete SysEx dump from hardware  
- Verified against multiple presets and global blocks  

