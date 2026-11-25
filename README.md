# RX Switch PTT — RF-sensing TX/RX antenna switch

A KiCad 9 project for a through-hole TX/RX antenna switch for amateur radio, **1.8–50 MHz, 100 W**.
The radio normally connects to a separate **RX antenna**; on PTT or detected RF it switches to the
**TX antenna** using PIN-style diodes, with a relay bypass for low-loss transmit.

## How it works

- **RF path** — Series/shunt diode switch (D1–D3, 1N4007 as PIN substitutes) with ~120 mA bias,
  RF chokes L1–L4 (~47 µH on FT50-43) and 1 kV MKP/C0G coupling capacitors.
- **PTT** — PTT IN (five RCA inputs in parallel) drives K1 (audio mute + dry-contact PTT OUT)
  and K2, a G2RL-2 relay that bypasses the diode TX path. K2 is always cold-switched because the
  diode path is already conducting.
- **RF sense** — Fast detector (LM311, < 1 µs) forces TX the moment RF appears. RF without PTT is
  treated as a fault: TX is held ~10 s after the last RF and the **RF SENSED** LED lights.
- **RX diode bias** — D3 is reverse-biased from an RF-derived high-voltage doubler
  (HT ≈ 2 × RF peak, clamped at ~220 V), so no separate HV supply is needed.
- **Fail-safe** — K3 (G2RL-2 DPDT) connects RADIO straight to the TX antenna whenever 12 V is
  absent or the RX ANT switch (SW1) is off, fully isolating the switch.
- **Power** — 12 V DC via a 2.1 × 5.5 mm barrel jack (center positive).

> ⚠️ **High voltage:** up to ~220 V DC is present on the HT net during transmit.

## Board

- Two-layer, all through-hole, intended for CNC milling.
- Layout rev 2.0: hybrid layout with 3 mm RF clearances and the RF block at the antenna end.
- Coax (RG58/RG316) solders directly to the board; panel parts (SW1, TX and RF SENSED LEDs) connect via 2.54 mm headers.

## Files

| Path | Contents |
|------|----------|
| `rx_switch_ptt/rx_switch_ptt.kicad_pro` | KiCad project (open this) |
| `rx_switch_ptt/rx_switch_ptt.kicad_sch` | Schematic, with detailed design notes |
| `rx_switch_ptt/rx_switch_ptt.kicad_pcb` | PCB layout |
| `rx_switch_ptt/rx_switch_ptt.kicad_sym` | Project symbol library |
| `rx_switch_ptt/rx_switch_ptt.pretty/` | Custom footprints (coax pads, RCA jacks, T50 toroid, PIN diodes, …) |

The schematic text notes cover part choices, bias calculations, relay pinouts and test
procedures. Read them before building.

## Before use

- Verify relay pinouts and RCA jack footprints against datasheets and real parts.
- Set the radio's TX delay to ≥ 10 ms so K2 closes before full power.
- Test a 10 s RF-sense hold at 100 W into a dummy load, checking diode temperature and SWR,
  including on 160 m.

73 de W1TOY
