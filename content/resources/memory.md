---
title: Memory
---

## Table of Contents
- [Basics](#basics)
- [ICs](#ics)
  - [DDR5 ICs](#ddr5-ics)
  - [DDR4 ICs](#ddr4-ics)
- [Architecture Specific Overclocking](#architecture-specific-overclocking)
  - [Alderlake DDR4 Overclocking](#alderlake-ddr4-overclocking)
  - [Zen2 DDR4 Overclocking](#zen2-ddr4-overclocking)
  - [Zen DDR4 Overclocking](#zen-ddr4-overclocking)
- [Lantency In-Depth](#latency-in-depth)
  - [tRAS](#tras)
  - [tRC](#trc)
  - [First Word Latency](#first-word-latency)

## Basics

#### [Understanding Frequency and Timings](https://www.tomshardware.com/reviews/pc-memory-ram-frequency-timings,6328.html)

#### [DDR4 Memory Gear-Down Mode Explained](https://www.linkedin.com/pulse/what-ddr4-memory-gear-down-mode-barbara-aichinger)

## ICs

#### DDR5 ICs

##### DDR5 IC Tier List
| Tier | ICs | Description |
| :-:  | :-: | :--:        |
| S | Hynix A-Die | Best DDR5 IC known (however, worse than M-Die on Zen4). |
| A | Hynix M-Die | High performing IC, can still be overclocked very far. |
| B |  |  |
| C |  |  |
| D |  |  |
| F |  |  |

#### DDR4 ICs

##### DDR4 IC Tier List from [MemTestHelper](github.com/integralfx/MemTestHelper/blob/oc-guide/DDR4%20OC%20Guide.md)

| Tier | ICs | Description |
| :-:  | :-: | :--:        |
| S | S8B | Best DDR4 IC for all-around performance |
| A | H8D, M8E, M16B | Top Performing ICs. Known not to clock wall and generally scale with voltage. |
| B | H8C, N8B, S4E | High-end ICs with the ability to run high frequencies with good timings. |
| C | H8J, H16M, H16C, M16E, S8D | Decent ICs with good performance and decent frequency scaling. |
| D | H8A, M8B, S8C, S4D | Low-end ICs commonly found in average cheap kits. Most are EOL and no longer relevant. |
| F | H8M, M4A, S4S, N8C | Terrible ICs unable to reliably attain even the highest standard of the base JEDEC Specification. |

##### Maximum Recommended Daily Voltage from [MemTestHelper](github.com/integralfx/MemTestHelper/blob/oc-guide/DDR4%20OC%20Guide.md)

| IC                                   | Daily Voltage (V) |	Extreme Voltage (V) |
| :-:                                  | :-:               | :--:                 |
| H8D, H16A, M8E, M16B, S4D, S4E, S8B  | Up to 1.55        | Above 1.55           |
| H4A, H8A, H8C , H16C, N8B            | Up to 1.45	       | Above 1.45           |
| S8C	                                 | Up to 1.35        | N/A                  |

##### [Samsung B-Die Finder](https://benzhaomin.github.io/bdiefinder/)

## Architecture Specific Overclocking

#### Alderlake DDR4 Overclocking

- Alderlake's DDR4 IMC is similar to Rocketlake's but slightly better in G1 but worse in G2, which isn't very good compared to Cometlake
- A tight `4000MHz G1` OC on 2x8/2x16 is considered as pretty good as it is quite a lot harder to push further than that
- [AsRock Timing Configurator v4.0.13](https://drive.google.com/file/d/11A2CCcXbvAFLVNHPVP9EtZ4hwmsn2yFt/edit)
  - Most reliable on Alderlake
- [MSI Dragon Ball](https://drive.google.com/file/d/1XmKv13D0MgC9fPaA91535wCe9ztoeaHV/view?usp=sharing)
  - Tends to miss/be inaccurate with the RTLs, also do not use this software to change the timings
- E-Cores must be kept on and CPU Ring Ratio set to `33x` in order to achieve the best memory OC possible, since memory should be OC'd before the CPU
- SA should first be set to `1.32` if using 2x8GB, or `1.35v` if using 2x16GB/4x8GB, as the higher memory ranks puts more load on the IMC, there requiring more SA voltage
- VDDQ is complicated and should be left at `1.35v`
- `1T` can give a noticable performace uplift and should always be aimed for
- Alderlake is able to have a higher tREFI than previous generations as it can reach around `262K`, but gets too thermally sensitive past `65535` and should be kept that way for daily OCs, so should be set to `65534` or `65535` depending on the board
- RTLs should be close together, since being far apart can sometimes indicate instability
- Some boards aren't great at RTL training as it takes a few tries to get them to train properly
- RTTs should be at `80/60/80` for 2x8GB, or `80/60/120` for 4x8GB/2x16GB

#### Zen2 DDR4 Overclocking

[Memory Overclocking on Zen2](https://www.tomshardware.com/reviews/amd-ryzen-3000-best-memory-timings,6310-2.html)

#### Zen DDR4 Overclocking

[Memory Overclocking on Zen](https://www.reddit.com/r/overclocking/comments/ahs5a2/demystifying_memory_overclocking_on_ryzen_oc/)

## Latency In-Depth

(Credit to alatron)

#### tRAS

tRAS is the minimum command period between the activate and precharge commands. In simple terms it is the minimum number of clock cycles between these two commands. This delay is a minimum delay and thus is an 'extension timing' meaning that it extends a command period, and it does not dictate anything directly. An extension timing does not dictate a command period, it just dictates the minimum clock cycles for a command period. tRAS as a timing has no performance impact when an activate to activate command period is extended by the tRC timing. This concept is later explained in the tRC section as it is crucial to first have an understanding of the tRC timing. Common myths: There are many different common myths for a 'minimum' tRAS value or a value to set tRAS to based off other timings, and none of these rules are correct. As mentioned before tRAS is a minimum command period delay, meaning that the ACT to PRE command period can go over this value without a problem, it just cannot take less clocks then tRAS. It is not a fixed value that dictates a fixed time period like many have been led to believe. Common 'rules' for a minimum tRAS I have heard are the following:
CL + tRCD = tRAS
CL + tRCD + 2 = tRAS
CL + tRCD + tRP = tRAS
CL + tRCD + tRTP = tRAS
tRAS + CL + tRCD = tRAS
CL + TRCD + TRRD + TRP = tRAS

These are just a few examples of the many incorrect rules people have made up, usually from misunderstanding the basics of this timing. The objective fact is every single one of these 'rules' for what you should either set tRAS to or use as a minimum tRAS value are entirely false and hold no truth to them at all. What is the actual minimum value for the tRAS timing? As tRAS itself is a minimum command period delay, there is no 'minimum' value for tRAS unlike some other timings. If your system allows you to set it to something very low such as 2 or 3 and you do not need a tRAS limit for stability, those values will work just fine. They will not be ignored; those values will be used for tRAS. There is no minimum 'electrical' value like people have been misled to believe, this belief is just false. tRAS is a minimum command period delay, there is no minimum electrical value that this timing must be set above. However, whilst there is no minimum value for tRAS, there is a point at which lowering tRAS will no longer do anything at all. This is the point where tRAS no longer extends any command delays relative to the actual delays for command periods. The minimum the activate to precharge delay for read operations is: tRCD + tRTP The tRCD value used when tRCDWR and tRCDRD timings are independently defined is tRCDRD (This is not a Jedec specification however some platforms have both these timings, such as Ryzen and newer Intel (RKL)). This can be easily determined from the diagram above where tRCD = activate command to read command delay, and tRTP = read command to precharge command delay. Since tRAS is the minimum activate to precharge delay, if less than or equal to the sum of these values tRAS does nothing for reads. This concept is rather simple, and obvious for why the rules previously defined are incorrect. Therefore, setting tRAS to or below this value causes tRAS to have absolutely no effect on a memory read operation. However, it can still limit performance under a write operation.

The minimum activate to precharge delay for writing is: tRCD + tCWL + BC + tWR The tRCD value used when tRCDWR and tRCDRD timings are independently defined is tRCDWR. BC on DDR4 is 2 clock cycles, on DDR5 it is 4 clock cycles. Like the formula for reads can be easily determined from the diagram below where tRCD is the activate command to write command delay, tCWL is the write command to first data delay, BC is the first data to last data clock gap when burst chop is enabled. And finally, tWR is last data to the precharge command. Thus, if tRAS is below or equal to tRCDWR + tCWL + BC + tWR or tRCD + tRTP (whichever is highest) the tRAS timing has absolutely no effect on anything. It does not cause issues, or a performance regression, it simply does nothing at all. Whether you set tRAS to this value or the register minimum value does not matter at all. Why does the write have to include the CAS command and the burst? The write must include the tCWL timing and the burst due to the prefetch architecture which was explained early. With a read the data is in the memory and gets transferred to the prefetch buffer after 1 physically memory core clock cycle. This means that what ever happens to the data in the physical memory past this point does not matter. The row can be closed without problems as the data is already in the prefetch buffer. Though however with writes the data is being moved into the memory, so the data has to go into the memory before the row can be closed. Thus writing needs extra steps before the row can be closed.

#### tRC

It is recommended to read the tRAS section before reading about tRC. tRC is the minimum command period between two activate commands and an activate and refresh command for the same bank of memory, this delay like tRAS is a minimum delay and thus it is an 'extension timing'.

tRC and tRAS relation: tRC and tRAS are two very related timings, and as mentioned in the tRAS section, when tRC extends a command period tRAS is not relevant to performance at all and does nothing. This is shown in the following diagram. As shown by this diagram, when tRC has an affect tRAS does nothing to the total command period time. And as a tRP always follows a tRAS which goes into the final clocks of the tRC this is always the case. As shown for tRAS to have an affect it must extend past the point where tRC would usually end and allow for another activate. This means that if you are always tRC limited when tRAS limited, setting tRAS from 40 to 21 will do nothing even if tRAS has an effect at 40, but not 21. Lowering tRAS can in cases allow for a lower tRC to be stable though this is uncommon. On Intel you do not have to worry about tRC as tRC isn't a timing, and the 'effective' tRC is just the value at which tRC does nothing. At what value does tRC start to do nothing? tRC like tRAS has a value at which the timing stops doing anything. This value for tRC is: tRAS + tRP If tRAS is limiting a command period or: tRCDWR + tCWL + BC + tWR or tRCD + tRTP (highest) + tRP These formulae may look familiar as they calculate the minimum ACT to PRE command periods for reads and writes assuming that tRAS does not limit these periods. These formulae are derived from tRC being the ACT to ACT command period. Anything below these values causes tRC to do nothing at all.

#### First Word Latency

Recently I noticed the addition of the 'First Word Latency' sorting category. Now all I really have to say is that this sorting category in it's current from is misnamed, what this category actually does is sorts the memory by the CAS latency in nanoseconds, which can easily be found by doing tCL/(effective transfer rate/2000). Example: 5000 cl18 = 18 / (5000/2000) = 7.2ns CAS latency. Now, for this next portion I'll be talking about reads and pulling data out of the ram, not writes. Since writes are based of tCWL which is much harder to find for different sticks. Actual first word latency how ever is not just CAS latency. To get a word out of the memory from idle the desired bank the data is in must be opened then the read command can be enacted which induces the burst. This entire sequence of events takes tRCD + tCL + BL clock cycles. tRCD = ACT to internal read or write delay time tCL = Internal read command to first data BL = Burst length, Burst length on DDR4 is always 8 bits of data, which takes 4 physical clock cycles due to DDR4's double data rate I/O bus. Example: https://imgur.com/a/mzHnX3b In this example tRCD and tCL are both 16 with the burst taking 4 clock cycles at 2400mt/s. Due to this, the first word latency is: First word latency = (tRCD + tCL + BL)/(effective transfer rate/2000) First word latency = (16 + 16 + 4)/(2400/2000) = 30ns This is much higher then CAS latency for this situation which is 13.33ns. Just wanted to let you guys know about this error, whether or not you guys fix it is not my call. What could be done to fix this error is to rename this sorting category to something such as 'CAS latency', 'Cas latency in time', etc. Or, you guys could change the equation to (tRCD + tCL + 4)/(effective transfer rate/2000). ~ Alatron
