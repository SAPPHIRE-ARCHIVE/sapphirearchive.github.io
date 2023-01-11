---
title: Memory
---

## Basics

[Understanding Frequency and Timings](https://www.tomshardware.com/reviews/pc-memory-ram-frequency-timings,6328.html)

[DDR4 Memory Gear-Down Mode Explained](https://www.linkedin.com/pulse/what-ddr4-memory-gear-down-mode-barbara-aichinger)

## ICs

#### DDR4 IC Tier List from [MemTestHelper](github.com/integralfx/MemTestHelper/blob/oc-guide/DDR4%20OC%20Guide.md)
| Tier | ICs | Description |
| :-:  | :-: | :--:        |
| S | S8B | Best DDR4 IC for all-around performance |
| A | H8D, M8E<sup>1</sup>, M16B | Top Performing ICs. Known not to clock wall and generally scale with voltage. |
| B | H8C, N8B, S4E | High-end ICs with the ability to run high frequencies with good timings. |
| C | H8J, H16M, H16C, M16E, S8D | Decent ICs with good performance and decent frequency scaling. |
| D | H8A, M8B, S8C, S4D | Low-end ICs commonly found in average cheap kits. Most are EOL and no longer relevant. |
| F | H8M, M4A, S4S, N8C | Terrible ICs unable to reliably attain even the highest standard of the base JEDEC Specification. |

[B-Die Finder](https://benzhaomin.github.io/bdiefinder/)

## Architecture Specific
[Alderlake DDR4](#alderlake-ddr4)

## Alderlake DDR4

##### Alderlake's IMC
- Alderlake's DDR4 IMC is similar to Rocketlake's but slightly better in G1 but worse in G2, which isn't very good compared to Cometlake
- A tight `4000MHz G1` OC on 2x8/2x16 is considered as pretty good as it is quite a lot harder to push further than that

##### Timing Software
- [AsRock Timing Configurator v4.0.13](https://drive.google.com/file/d/11A2CCcXbvAFLVNHPVP9EtZ4hwmsn2yFt/edit)
  - Most reliable on Alderlake
- [MSI Dragon Ball](https://drive.google.com/file/d/1XmKv13D0MgC9fPaA91535wCe9ztoeaHV/view?usp=sharing)
  - Tends to miss/be inaccurate with the RTLs, also do not use this software to change the timings

#### CPU Settings
- E-Cores must be kept on and CPU Ring Ratio set to `33x` in order to achieve the best memory OC possible, since memory should be OC'd before the CPU

#### SA/VDDQ Voltages
- SA should first be set to `1.32` if using 2x8GB, or `1.35v` if using 2x16GB/4x8GB, as the higher memory ranks puts more load on the IMC, there requiring more SA voltage
- VDDQ is complicated and should be left at `1.35v`

#### Command Rate
- `1T` can give a noticable performace uplift and should always be aimed for

#### tREFI
- Alderlake is able to have a higher tREFI than previous generations as it can reach around `262K`, but gets too thermally sensitive past `65535` and should be kept that way for daily OCs, so should be set to `65534` or `65535` depending on the board

#### RTLs
- RTLs should be close together, since being far apart can sometimes indicate instability
- Some boards aren't great at RTL training as it takes a few tries to get them to train properly

#### RTTs
- RTTs should be at `80/60/80` for 2x8GB, or `80/60/120` for 4x8GB/2x16GB
