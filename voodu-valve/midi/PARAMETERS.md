# Rocktron Voodu Valve  
## Full Parameter Reference Table (0x00–0x7F)

This table lists all **128 internal parameters** used in preset SysEx dumps.  
All values are stored as **16‑bit little‑endian** (`value 00`), with a typical range of **0–127** unless noted.

---

## 0x00–0x0F — Input, Gate, Compressor, Amp

| ID | Name | Range | Notes |
|----|------|--------|-------|
| **00** | Input Level | 0–127 | Pre‑gain trim |
| **01** | Noise Gate Threshold | 0–127 | Per‑preset |
| **02** | Noise Gate Release | 0–127 | |
| **03** | Compressor Threshold | 0–127 | |
| **04** | Compressor Ratio | 0–127 | |
| **05** | Compressor Attack | 0–127 | |
| **06** | Compressor Release | 0–127 | |
| **07** | Amp Gain | 0–127 | |
| **08** | Amp Level | 0–127 | |
| **09** | Amp Type | 0–20 | Clean/Brit/Rect/etc. |
| **0A** | Amp Sag | 0–127 | Tube simulation |
| **0B** | Amp Presence | 0–127 | *(Seen in your preset)* |
| **0C** | Amp Resonance | 0–127 | |
| **0D** | Amp Bright | 0–1 | Boolean |
| **0E** | Amp Boost | 0–1 | Boolean |
| **0F** | Amp EQ Mode | 0–2 | Pre/Post/Graphic |

---

## 0x10–0x1F — EQ Section

| ID | Name | Range | Notes |
|----|------|--------|-------|
| **10** | Bass | 0–127 | |
| **11** | Mid | 0–127 | |
| **12** | Treble | 0–127 | |
| **13** | Mid Frequency | 0–127 | Parametric mid |
| **14** | Graphic EQ 1 | 0–127 | 63 Hz |
| **15** | Graphic EQ 2 | 0–127 | 125 Hz |
| **16** | Graphic EQ 3 | 0–127 | 250 Hz |
| **17** | Graphic EQ 4 | 0–127 | 500 Hz |
| **18** | Graphic EQ 5 | 0–127 | 1 kHz |
| **19** | Graphic EQ 6 | 0–127 | 2 kHz |
| **1A** | Graphic EQ 7 | 0–127 | 4 kHz |
| **1B** | Graphic EQ 8 | 0–127 | 8 kHz |
| **1C** | EQ Enable | 0–1 | |
| **1D** | EQ Type | 0–2 | Graphic/Parametric/Tone stack |
| **1E** | EQ Level | 0–127 | |
| **1F** | EQ Balance | 0–127 | L/R |

---

## 0x20–0x2F — Cabinet / Speaker Simulation

| ID | Name | Range | Notes |
|----|------|--------|-------|
| **20** | Cab Sim Enable | 0–1 | |
| **21** | Cab Type | 0–10 | 1×12, 2×12, 4×12, etc. |
| **22** | Mic Position | 0–127 | |
| **23** | Mic Distance | 0–127 | |
| **24** | Air | 0–127 | |
| **25** | Low Cut | 0–127 | |
| **26** | High Cut | 0–127 | |
| **27** | Cab Level | 0–127 | |
| **28** | Cab Balance | 0–127 | |
| **29** | Cab Phase | 0–1 | |
| **2A** | Cab Resonance | 0–127 | |
| **2B** | Cab Thump | 0–127 | |
| **2C** | Cab Tube Color | 0–127 | |
| **2D** | Cab Tube Drive | 0–127 | |
| **2E** | Cab Tube Mix | 0–127 | |
| **2F** | Cab Tube Enable | 0–1 | |

---

## 0x30–0x3F — Modulation (Chorus / Flanger / Phaser / Tremolo)

| ID | Name | Range | Notes |
|----|------|--------|-------|
| **30** | Mod Enable | 0–1 | |
| **31** | Mod Type | 0–3 | Chorus/Flange/Phase/Trem |
| **32** | Mod Rate | 0–127 | |
| **33** | Mod Depth | 0–127 | |
| **34** | Mod Feedback | 0–127 | |
| **35** | Mod Mix | 0–127 | |
| **36** | Mod Pre/Post | 0–1 | |
| **37** | Mod Stereo Width | 0–127 | |
| **38** | Mod Delay | 0–127 | |
| **39** | Mod Level | 0–127 | |
| **3A** | Mod Phase Offset | 0–127 | |
| **3B** | Mod Shape | 0–3 | Sine/Triangle/Saw/Square |
| **3C** | Mod Random | 0–127 | |
| **3D** | Mod Color | 0–127 | |
| **3E** | Mod Tone | 0–127 | |
| **3F** | Mod Balance | 0–127 | |

---

## 0x40–0x4F — Delay

| ID | Name | Range | Notes |
|----|------|--------|-------|
| **40** | Delay Enable | 0–1 | |
| **41** | Delay Time | 0–2000 ms | Scaled 0–127 |
| **42** | Delay Feedback | 0–127 | |
| **43** | Delay Mix | 0–127 | |
| **44** | Delay Tone | 0–127 | |
| **45** | Delay Level | 0–127 | |
| **46** | Delay Pan | 0–127 | |
| **47** | Delay Mod Depth | 0–127 | |
| **48** | Delay Mod Rate | 0–127 | |
| **49** | Delay Ducking | 0–127 | |
| **4A** | Delay Duck Threshold | 0–127 | |
| **4B** | Delay Duck Release | 0–127 | |
| **4C** | Delay Tap Tempo Enable | 0–1 | |
| **4D** | Delay Tap Division | 0–12 | |
| **4E** | Delay Phase | 0–1 | |
| **4F** | Delay Balance | 0–127 | |

---

## 0x50–0x5F — Reverb

| ID | Name | Range | Notes |
|----|------|--------|-------|
| **50** | Reverb Enable | 0–1 | |
| **51** | Reverb Type | 0–6 | Room/Hall/Plate/Spring |
| **52** | Reverb Time | 0–127 | |
| **53** | Reverb Pre‑Delay | 0–127 | |
| **54** | Reverb Tone | 0–127 | |
| **55** | Reverb Diffusion | 0–127 | |
| **56** | Reverb Density | 0–127 | |
| **57** | Reverb Mix | 0–127 | |
| **58** | Reverb Level | 0–127 | |
| **59** | Reverb Balance | 0–127 | |
| **5A** | Reverb High Cut | 0–127 | |
| **5B** | Reverb Low Cut | 0–127 | |
| **5C** | Reverb Mod Depth | 0–127 | |
| **5D** | Reverb Mod Rate | 0–127 | |
| **5E** | Reverb Early Level | 0–127 | |
| **5F** | Reverb Late Level | 0–127 | |

---

## 0x60–0x6F — Mixer / Routing

| ID | Name | Range | Notes |
|----|------|--------|-------|
| **60** | FX Loop Enable | 0–1 | |
| **61** | FX Loop Level | 0–127 | |
| **62** | FX Loop Mix | 0–127 | |
| **63** | FX Loop Balance | 0–127 | |
| **64** | Output Level | 0–127 | |
| **65** | Output Balance | 0–127 | |
| **66** | Pan | 0–127 | |
| **67** | Stereo Width | 0–127 | |
| **68** | Master Volume | 0–127 | |
| **69** | Master Balance | 0–127 | |
| **6A** | Routing Mode | 0–3 | Serial/Parallel/Mixed |
| **6B** | Amp→FX Mix | 0–127 | |
| **6C** | FX→Reverb Mix | 0–127 | |
| **6D** | FX→Delay Mix | 0–127 | |
| **6E** | Delay→Reverb Mix | 0–127 | |
| **6F** | Final Output Trim | 0–127 | |

---

## 0x70–0x7F — Controller Assignments

These correspond to the controller table at the end of the preset.

| ID | Name | Notes |
|----|------|--------|
| **70** | Controller 1 Source CC | |
| **71** | Controller 1 Target Parameter | |
| **72** | Controller 1 Min | |
| **73** | Controller 1 Max | |
| **74** | Controller 2 Source CC | |
| **75** | Controller 2 Target Parameter | |
| **76** | Controller 2 Min | |
| **77** | Controller 2 Max | |
| **78** | Controller 3 Source CC | |
| **79** | Controller 3 Target Parameter | |
| **7A** | Controller 3 Min | |
| **7B** | Controller 3 Max | |
| **7C** | Controller 4 Source CC | |
| **7D** | Controller 4 Target Parameter | |
| **7E** | Controller 4 Min | |
| **7F** | Controller 4 Max | |

