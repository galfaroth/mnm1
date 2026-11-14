# MNM-1

[![License: GPL-3.0-only](https://img.shields.io/badge/license-GPL--3.0--only-blue.svg)](LICENSE)

**A six-track synth firmware for the M-VAVE FM-1.** Six monophonic tracks of synth and FX machines,
pages of eight knobs, a resonant HP/LP filter and a tape delay on every track, three LFOs a track,
parameter locks and parameter slide, all on the FM-1's own keys, knobs and screen.

MNM-1 is inspired by and forked from [Hortator](https://deadactive.github.io/hortator/) by DEADACTIVE (0.23.3), which
is built on [Felucca](https://github.com/hugelton/Felucca) by Leo Kuroshita (Hügelton Instruments). Hortator's
sequencer, project storage, screen, PERFORM effects and compressor carry over; the tracks, machines and pages are
MNM-1's own. Calibrated audio algorithms and 120 complete sounds are ported from
[MONO2K](https://github.com/InigoHernandez/mono2k) by Inigo Hernandez (GPL-3.0-only).

- **Play it in your browser:** <https://galfaroth.github.io/mnm1/>. The page runs the real firmware, compiled to
  WebAssembly, on an FM-1 you can play with the mouse, a touch screen or the computer keyboard.
- **Install it on your FM-1:** <https://galfaroth.github.io/mnm1/webapp/installer/>, in Chrome or Edge, over the
  FM-1's USB cable. Or from a terminal: `python3 tools/fm1_install.py mnm1-1.0.1.fwsc --port FM-1 --yes`
  (needs `mido` and `python-rtmidi`).
- **What comes next:** [docs/ROADMAP.md](docs/ROADMAP.md).
- **What changed:** [CHANGELOG.md](CHANGELOG.md).

**Beta.** MNM-1 has been tested on one FM-1 only. You install it at your own risk: the author accepts no
responsibility if your FM-1 is damaged or misbehaves. M-VAVE's own updater takes you back to the official firmware
([below](#back-to-the-official-firmware)).

## What it does

- **Six tracks, one machine each.** Synth machines: SWSAW, SWPLS (SuperWave), SID, FM+, DPRO, GND, VO-6. FX machines:
  THRU, REVRB, CHRS, DYNMX, RING, PHASR, FLANG. Any machine on any track.
- **Calibrated machines:** G-SIN, GPRC, SID2, FMST, FMDYN, SAW2, ENS, WAVE, VOX2, FMPAR, PUL2, CHO2,
  RING2, DYN2, NOISE, DKICK, DSNAR, DHAT and DCYMB. These keep their own complete audio chain and parameter laws;
  the original models remain available for saved projects. VOX consonant/switch controls marked `n/a` are not implemented.
- **More effects:** STRNG, PIPE, CHORD and MODAL machines; the LOFI page adds tape, adaptive crunch, hiss/dust and
  drive. REVERB TYPE adds STEREO. Calibrated tracks also expose DESTROY and MOD FX pages.
- **Routing.** An FX machine takes the previous track (NEIB) or a bus (AB, CD, EF) as its input,
  as an insert or a send and return. Each track's OUT picks its bus, or none.
- **8-knob pages.** SYNTH, AMP (ATK HOLD DEC REL / DIST LVL PAN PORT), FILTER (BASE WDTH HPQ LPQ / ATK DEC BOFS WOFS),
  EFFECTS (EQF EQG SRR DTIM / DSND DFB DBAS DWID), LFO 1-3 (PAGE DEST TRIG WAVE / MULT SPD INTL DPTH). Two rows of
  four on the FM-1's four knobs.
- **Filter:** a resonant high-pass into a low-pass, each with its own Q, tracking the note, with an AD envelope.
- **Tape delay on every track:** tempo-synced, ping-pong or stereo, feedback up to self-oscillation.
- **Three LFOs a track:** any knob of the track, another LFO, or pitch; FREE, TRIG, HOLD, ONE, HALF; eleven waves.
- **Parameter locks:** a lock lasts until the next trig; up to 240 a project, recorded live or set in
  LOCK mode; **parameter slide** glides locked values to the next lock.
- **Presets:** 330 factory sounds: 210 existing/extended presets, 24 complete sounds in SOURCE and 96 in ARCHIVE.
  The PRESETS knob steps through them; hold EDIT for the list. Audition/cancel restores the complete sound.
  Calibrated sounds have separate AMP VOL and mixer LEVEL, and LFO target/phase controls. The existing 128 USER
  slots store complete sounds after expanded-library migration; unmigrated storage retains its legacy format.
- **Calibrated live input:** channels 1–6 play the corresponding calibrated track chromatically; held keys feed its
  arpeggiator. The original drum-channel mapping stays available.
- **From Hortator:** up to 64 steps per track with its own length, PROB and A/B conditions, ratchets, accents,
  a per-track arpeggiator, live recording, mutes, SONG, the COMP sidechain compressor, PERFORM (live master effects),
  MIDI and clock over USB and TRS, MIDI LEARN, and compatible legacy project loading.
- **Expanded library:** eight patterns, eight independent kits and 128 full user sounds after explicit migration.
  The library replaces sample slots 2 and 3; sample slot 1 remains. See [the storage plan](docs/PORT_PLAN.md).
- **Sequencing:** independent AMP/FILTER/LFO trig lanes, note-off and trigless events; swing masks,
  fractional speeds, KEY/SID/ADD arpeggiation with a 16-step rhythm, and track/pattern transpose.

## On the panel

| Button | What it does |
| --- | --- |
| **EDIT** | SYNTH (the machine's 8 knobs), then AMP, then TRACK (MODEL, NOTE, OUT, IN, GATE, sends). Press again for the next row or page |
| **ENV** | FILTER |
| **FX** | EFFECTS, SLICER, REVERB, CHORUS, LOFI; calibrated tracks also DESTROY and MOD FX. Hold for PERFORM |
| **LFO** | LFO 1, 2, 3 |
| **ARP** | existing ARP, then ARP MODE, ARP TRIG and ARP RHYTHM |
| **SEQ** | STEP, PATTERN, MOTION, SONG, SWING MASK and TRANSPOSE |
| **HOME** | a tap: TRACKS (levels, lengths, pans) or HOME (the scope; SELECT on to COMP). Hold: the menu |
| **GLO** | a tap: global settings; held: the white keys mute tracks |
| **SAVE** | patterns (save, load, name), tools and independent KIT slots |
| **SCL** (SEL) | KEYS on / off: the keys play the selected track (on at power-on); OCT- / OCT+ set the octave |
| **REC** | arm recording; on STEP, stopped, with KEYS on: step record |
| **PLAY** | start / stop |

**ALGORITHM** picks the track. **SELECT** changes the track's machine on the 8-knob pages, turns pages elsewhere, and
sets the tempo on TRACKS. **KNOB 1-4** edit the lit row.

### Steps and locks

- On **STEP**, tap a white key to turn a step on or off; OCT- / OCT+ change the bank of 16.
- STEP knob 2 chooses ALL, AMP, FILTER or LFO. Hold a step and turn SELECT to choose its trig type.
- Hold a step: KNOB 1 its chance (PROB), KNOB 2 ratchets, KNOB 3 accent, KNOB 4 SLIDE.
- **LOCK mode:** hold a step and press EDIT, ENV, FX or LFO. The page opens with the keys still the steps; hold any
  step and turn a knob to lock that value there (inverted on screen, `LOCK S5`). Hold OCT- and turn a knob to clear only its lock; tap OCT- to clear the held step's locks on release.
  SEQ leaves LOCK mode.
- Press REC while playing and turn knobs to record locks live.

### Timing and live arpeggiation

ARP MODE selects LEGACY (the existing ARP page), KEY, SID, ADD or OFF. PLAY chooses
TRUE, UP, DOWN, CYCLE or RND; SPD 6 is a sixteenth note, RNGE is one to four octaves.
ARP TRIG controls OJMP and the AMP/FLT/LFO switches for subsequent notes; the first
audible note triggers all three. ARP RHYTHM chooses length, step, enable and note offset.

SWING MASK sets the pattern amount (50–80), step number and that track's enable bit.
A value of 0 retains the old global/track swing controls. TRANSPOSE sets track amount,
scale, key and the pattern amount. FIX ignores the pattern transpose; MAJ and MIN
choose the nearest scale note, ties downward. Stored step notes remain unchanged.

## Building

The firmware builds with JieLi's pi32v2 toolchain and the AC79 SDK (Linux x86-64), see [BUILDING.md](BUILDING.md):

```sh
JIELI_TOOLCHAIN=... AC79_SDK=... DRUM_PACKAGE=1 ./build.sh
```

The host tests run on macOS or Linux with any C compiler: `tests/run_drum_tests.sh`.

The web simulator and site ([docs/SIMULATOR.md](docs/SIMULATOR.md)) need Python 3 with Pillow, Node 18+ and
emscripten (a native `emcc`, e.g. `brew install emscripten`, or Docker):

```sh
tools/build_sim.sh --preview --package dist/mnm1-1.0.1.fwsc
cd build/sim/preview && python3 -m http.server 8001
```

## Back to the official firmware

M-VAVE's updater, M-UPGRADE, puts it back: from [m-vave.com/download](https://www.m-vave.com/download), Downloads →
PC Software → M-UPGRADE, with PC Firmware → FM-1 V15. If the FM-1 stops answering, see
[docs/RECOVERY.md](docs/RECOVERY.md).

## Licence and credits

MNM-1's code is GPL-3.0-only, as Hortator's and Felucca's; see [LICENSE](LICENSE) and [LICENSING.md](LICENSING.md)
for the assets and third-party parts (Mutable Instruments' Plaits, Streams and Grids ports, DaisySP, CC0 samples, the
Terminus font, the JieLi SDK files in the package under Apache-2.0).

- **Hortator** by DEADACTIVE: the firmware this is inspired by and forked from
  (<https://deadactive.github.io/hortator/>).
- **Felucca** by Leo Kuroshita (@kurogedelic), Hügelton Instruments.
- M-VAVE and FM-1 are trademarks of their owners; MNM-1 is not affiliated with M-VAVE.

## Firmware identity and releases

MNM-1 1.0.1 and later use the stable package/USB identity `FM-1_61000100`.
The firmware version is reported separately in ABOUT and the USB console.
Older MNM-1 packages used the shared `FM-1_900`; the ordinary installer upgrades
those devices too. The unchanged, pinned update loader still identifies itself
as `ota-FM-1_900` while writing; this is not the running firmware's identity.

Download the `.fwsc` from [GitHub Releases](https://github.com/galfaroth/mnm1/releases/latest).
[Screen capture, 240 × 240 PNG](https://raw.githubusercontent.com/galfaroth/mnm1/main/docs/mnm1-screen.png)
shows FM+ playing with its audio waveform in the firmware oscilloscope.

This identity-only update keeps MNM-1 1.0 project/settings formats. Compatibility
with arbitrary projects from other firmware is not guaranteed. It does not
perform the optional expanded-library migration or replace user samples.

You can also try MNM-1 in the browser, no flashing needed, in the free community [FM-1 Emulator](https://fm1-emulator.atst.tools/).
