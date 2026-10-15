![](../../workflows/gds/badge.svg) ![](../../workflows/docs/badge.svg)

# 5-Transistor OTA (SKY130) — Tiny Tapeout

A compact single-stage operational transconductance amplifier designed in Cadence Virtuoso for the SKY130 process and submitted to the May 2026 Tiny Tapeout shuttle. Designed by Santino Pasquale for the Cornell Custom Silicon Systems (C2S2) RFIC subteam.

| Parameter | Value (schematic simulation) |
|---|---|
| Supply | 1.8 V |
| Tail bias current | 5 µA |
| Load | 5 pF |
| DC gain | ~36 dB |
| Unity-gain frequency | ~1.4 MHz |
| Phase margin | ~90° |
| Input common-mode range | ~0.9 V – 1.8 V |
| Area | 1x2 Tiny Tapeout tile |

The compensation capacitor is placed off-chip to minimize silicon area.

- **Full design and test documentation:** [docs/info.md](docs/info.md)
- **Layout:** [`gds/tt_um_5tOTA.gds`](gds/tt_um_5tOTA.gds) · **Abstract:** [`lef/tt_um_5tOTA.lef`](lef/tt_um_5tOTA.lef)

## Pinout

| Pin | Function |
|---|---|
| `ua[0]` | Ibias — 5 µA reference current input |
| `ua[1]` | Vin1 — differential input |
| `ua[2]` | Vin2 — differential input |
| `ua[3]` | Vout — single-ended output (connect compensation capacitor here) |

## About Tiny Tapeout

[Tiny Tapeout](https://tinytapeout.com) is an educational project that makes it affordable to manufacture small designs on a real chip. See the [analog specs](https://tinytapeout.com/specs/analog/) for details on analog projects.
