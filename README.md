# Zuup Computer

An open hardware project for a small computer built from discrete transistors, with three seven-segment digits and user-added pushbuttons.

## What is designed now

This first KiCad revision is **Stage 1: a two-input transistor NOR gate**. It is the smallest reusable logic block for the computer. The schematic and routed two-layer PCB use two 2N3904 NPN transistors, resistors, and headers for power, inputs, and output. The board passes KiCad's electrical and board checks. It has not been fabricated or electrically tested yet.

The 8-bit computer and three-digit display are the project goal; they are **not implemented in this first revision**. A full discrete-transistor design needs many repeated logic cells and several rounds of testing before it is safe to order as one large PCB. See [HOW_IT_WORKS.md](HOW_IT_WORKS.md) for the beginner explanation and [ARCHITECTURE.md](ARCHITECTURE.md) for the design plan.

## Open the design

Open `Zuup_Computer.kicad_pro` in KiCad 10. The project schematic is `Zuup_Computer.kicad_sch`, and the routed board is `Zuup_Computer.kicad_pcb`.

The NOR-gate board's Gerbers and drill file are in `fabrication/NOR-v0.1/`. The PDF schematic and 3D board preview are included for quick review.

## Stage 1: NOR gate

The circuit implements:

```text
Y = NOT (A OR B)
```

| A | B | Y |
|---:|---:|---:|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

Use a regulated **5 V DC** supply. Inputs A and B are logic levels: connect an input to GND for 0 or +5 V for 1. R4 and R5 pull the transistor bases low when the input is open, so a pushbutton can connect each input to +5 V. J1 pin order is `+5V, GND, A, B`; J2 is `Y, GND`.

The schematic uses a 1 kΩ collector pull-up, 10 kΩ base resistors, and 100 kΩ base pull-downs. A 2N3904 footprint is assigned. Check the exact transistor manufacturer's pinout against the board footprint before assembling; different TO-92 parts can have different lead orders.

## Open hardware license

The hardware design files are licensed under **CERN-OHL-S-2.0** so people can study and modify the design, with changes to shared hardware designs kept under the same license. The official text is linked in [LICENSE](LICENSE).

## First-stage parts

See [BOM.csv](BOM.csv) for the part list. Use through-hole parts and 2.54 mm headers so the first prototype is easy to inspect and modify.

## Building toward the computer

1. Assemble and verify the NOR gate at 5 V.
2. Combine NOR gates into a one-bit half adder and then a full adder.
3. Chain eight full-adder cells for an 8-bit addition unit, with a ninth carry output.
4. Add input storage and a result latch so the buttons can enter two numbers and hold the answer.
5. Add a binary-to-decimal conversion stage and three seven-segment driver stages for results from 000 to 255.

The pushbuttons can be added as the front-panel controls in a later revision. Their contacts should connect to defined logic levels, with pull-down resistors so open buttons do not leave inputs floating.

## Safety and build notes

- Keep this low-voltage: use 5 V DC and do not connect mains power.
- Do not order a later computer PCB from this Stage 1 project; only the NOR gate has a component-level schematic here.
- Prototype each logic cell first. Discrete resistor-transistor logic has limited drive strength, so a cell's output fan-out must be checked before it drives several following gates.
# zuupcomputer
