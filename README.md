# E-MU Emulator X3 Z-Plane Filter Research

Unofficial reverse-engineering research and independent re-implementation of the **E-MU Emulator X3 Z-plane filter engine**.

This repository focuses on reproducing the filter behavior found in Emulator X3 and validating the recovered implementation against the original software.

## NOTICE - SLOP CODE
I got GPT 6.1 Sol to mess around with the Emulator X3 application and stuff and it built a test harness and I provided recordings of a saw through all 55 filters.
I'd do all of this by myself but I have no clue how Ghidra or IDA Pro works LOL.

Anyway, it's a Z-Plane filter, what's more to love?

## Accuracy

The recovered implementation covers all **55 Emulator X3 filter types**:

- **17 standard filters**
- **33 factory Z-plane filters**
- **5 programmable filters**

The strongest validation was performed with a native comparison harness that directly compared the independently written implementation against the original Emulator X3 filter routines.

### Native validation result

Across all 55 filters:

```
7,040 test sequences
119,680 processing blocks
17,793,600 output float samples
35,607,968 total checked values
0 bit differences
```

The comparison included:

| Compared data | Values checked | Bit differences |
|---|---:|---:|
| Setup frames | 10,771,200 | 0 |
| Gain | 119,680 | 0 |
| Coefficients | 2,662,880 | 0 |
| Coefficient increments | 2,662,880 | 0 |
| Audio output | 17,793,600 | 0 |
| Carried filter state | 1,597,728 | 0 |

A deliberate one-bit modification was also detected correctly by the validation system.

Within the tested filter-core paths, the recovered implementation therefore reproduced the original Emulator X3 routines **bit-for-bit**.

## What was recovered

The research covers the main filter-processing behavior required by the 55-filter set, including:

- packed filter-table decoding
- coefficient conversion
- packed-space interpolation
- multi-section filter processing
- coefficient ramps
- Morph/Frequency control behavior
- Resonance/Q behavior
- programmable filter handling
- persistent filter state
- native filter banks for 44.1, 48, 96 and 192 kHz

The recovered implementation preserves the observed floating-point operation order because changing the arithmetic order can change the resulting bits.

## Filter families

### Standard filters — 17

Includes:

- 2 / 4 / 6 Pole Lowpass
- 2 / 4 Pole Highpass
- 2 / 4 Pole Bandpass
- Contrary Bandpass
- Swept EQ variants
- Phaser 1 / 2
- Bat Phaser
- Flanger Lite
- Vocal Ah-Ay-Ee
- Vocal Oo-Ah

### Factory Z-plane filters — 33

Includes filters such as:

- Ace of Bass
- MegaSweepz
- BassBox 303
- TB or Not TB
- Ooh to Eee
- Multi Q Vox
- Talking Hedz
- Zoom Peaks
- Bass Tracer
- Radio Craze
- Deep Bouche
- Acid Ravage
- Lucifer's Q
- Ear Bender
- Klang Kling

and the rest of the factory X3 Z-plane set.

### Programmable filters — 5

- Dual EQ Morph
- Dual EQ + LP Morph
- Dual EQ Morph/Expression
- Peak/Shelf Morph
- Morph Designer

## Methodology

The implementation was recovered and validated using several complementary methods:

1. Static inspection of the relevant Emulator X3 filter code and data.
2. Recovery of packed coefficient tables and conversion behavior.
3. Isolated execution of original filter routines on controlled data.
4. Independent C++ and Python re-implementation.
5. Exhaustive coefficient-decoder testing across all 65,536 packed 16-bit values.
6. Multi-rate DSP validation.
7. Direct native comparison against the original X3 filter routines.

The final native comparison avoided WAV capture, resampling and host-rendering differences by comparing the filter routines directly.

## Scope of the accuracy claim

The zero-bit-difference result applies to the **tested Emulator X3 filter-core paths**.

It does **not** mean that the complete Emulator X3 sampler or original E-MU hardware has been reproduced bit-perfectly.

This repository does not claim complete equivalence for:

- sample playback
- envelopes
- full preset routing
- voice allocation
- effects outside the recovered filter path
- every host/control scheduling path
- original E-MU hardware or H-chip execution

The accurate claim is:

> The recovered implementation produced zero bit differences against the original Emulator X3 filter-core routines across 7,040 tested sequences and 17,793,600 compared output samples covering all 55 filter types.

## Repository contents

The repository contains:

- independently written filter source code
- research and validation code
- test harnesses
- validation reports
- coefficient/filter library support
- locally generated filter-bank/library data used by the recovered implementation

Some extracted data may originate from a local Emulator X3 installation and should be treated separately from the newly written source code.

## Provenance

This is an independent research project and is not affiliated with or endorsed by E-MU Systems, Creative Technology, or Rossum Electro-Music.

E-MU, Emulator, Emulator X3, and related product/filter names belong to their respective owners.

The purpose of this project is technical research, preservation, interoperability and reproducible analysis of the Emulator X3 filter engine.
