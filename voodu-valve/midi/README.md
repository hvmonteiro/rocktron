# Rocktron Voodu Valve  
## MIDI Implementation Chart (Reconstructed)

| Function | Supported | Notes |
|---------|-----------|-------|
| **MIDI Channels** | 1–16 | User‑selectable (Global Settings) |
| **Poly Mode** | N/A | Not a polyphonic instrument |
| **Mono Mode** | N/A | Not a monophonic instrument |
| **Multi‑Timbral** | No | Single processing chain |

---

## 1. Channel Voice Messages

| Message | Supported | Notes |
|---------|-----------|-------|
| **Note On** | No | Ignored |
| **Note Off** | No | Ignored |
| **Poly Aftertouch** | No | — |
| **Channel Aftertouch** | Yes | Can be assigned to parameters via Controller Table |
| **Pitch Bend** | Yes | Assignable via Controller Table |
| **Control Change (CC)** | Yes | Fully supported, assignable to any parameter |
| **Program Change (PC)** | Yes | Selects presets 0–255 |
| **Bank Select (CC0/CC32)** | No | Voodu uses linear preset numbering only |

---

## 2. Control Change (CC) Messages

### Default CC Behavior
| CC | Function | Notes |
|----|----------|-------|
| **7** | Volume | Default assignment (can be remapped) |
| **1** | Mod Wheel | Assignable |
| **4** | Foot Controller | Assignable |
| **5** | Portamento / Wah | Often used for Wah |
| **64** | Sustain | Ignored unless assigned |
| **Any 0–127** | Assignable | Up to 4 simultaneous controller mappings |

### Controller Assignment Table
The Voodu Valve supports **4 controller slots**, each containing:

- Source CC  
- Target Parameter ID (0x00–0x7F)  
- Min value  
- Max value  

These appear in SysEx as 8‑byte blocks.

---

## 3. Program Change

| Feature | Supported | Notes |
|---------|-----------|-------|
| **Program Change** | Yes | Selects presets 0–255 |
| **Program Change Enable** | Yes | Can be disabled in Global Settings |
| **Program Mapping** | No | No remapping table |

---

## 4. System Common Messages

| Message | Supported | Notes |
|---------|-----------|-------|
| **MTC** | No | — |
| **Song Select** | No | — |
| **Song Position Pointer** | No | — |
| **Tune Request** | No | — |

---

## 5. System Real‑Time Messages

| Message | Supported | Notes |
|---------|-----------|-------|
| **Clock** | Yes | Used for tempo‑based effects (Delay, Modulation) |
| **Start/Stop/Continue** | No | Ignored |
| **Active Sensing** | Yes | Passed through |
| **System Reset** | No | Ignored |

---

## 6. System Exclusive (SysEx)

### Manufacturer ID
```
00 00 29   (Rocktron)
```

### Device Family
```
07   (Voodu Valve / Chameleon family)
```

### SysEx Message Types

| ID | Meaning | Notes |
|----|---------|-------|
| **2A** | Preset Dump | One SysEx per preset |
| **2B 00** | Global Parameter Index Table | Sent once in bulk dump |
| **2B 01** | Global Settings Block | Sent once in bulk dump |

### Preset Dump Structure
```
F0 00 00 29 07 2A vv vv [preset data...] cs F7
```

- `vv vv` = format version (usually `06 00`)
- Parameters stored as **16‑bit little‑endian**
- Name stored as **UTF‑16‑LE (11 chars)**
- Effect blocks stored as **8‑byte structures**
- Controller table stored at end
- `cs` = checksum

### Global Settings Structure
```
F0 00 00 29 07 2B 01 [global data...] cs F7
```

Contains:

- MIDI channel  
- Program change enable  
- Output mode  
- Tuner settings  
- Global EQ / Cab Sim  
- Checksum  

---

## 7. Device Behavior

| Feature | Behavior |
|---------|----------|
| **Preset Numbers in SysEx** | Not stored — determined by order in bulk dump |
| **Bulk Dump** | 256 preset SysEx messages + 2 global SysEx messages |
| **Bulk Load** | Must be sent in correct order |
| **Checksum** | 7‑bit sum of all data bytes |

---

## 8. MIDI Thru

| Feature | Supported | Notes |
|---------|-----------|-------|
| **MIDI Thru** | Yes | Hardware pass‑through |

---

## 9. Foot Controller Support

| Controller | Supported | Notes |
|------------|-----------|-------|
| **Rocktron All‑Access** | Yes | Full compatibility |
| **Generic MIDI Foot Controllers