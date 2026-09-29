# Bottom-side JLCPCB parts — bitaxeBonanza 1004x

Catalog review: 2026-09-29. Source: saved `bitaxeBonanza.kicad_pcb` B.Cu position export joined to the schematic BOM, with the bitaxeProto-1103x BOM used for existing choices. Stock changes; recheck before placing an order.

- 209 bottom-side footprints: 180 schematic BOM parts in 56 original PARTNO groups, plus 29 test points, fiducials, mounting holes, and a net tie outside the assembly BOM.
- All 180 BOM positions have a selected JLCPCB/LCSC catalog code. These are candidate assembly selections, not an indication that all parts can be ordered today.
- 12 × D1–D12 use Preferred C7502722 (BAS516), with 33,674 available and the existing SOD-523 footprint. L1 uses exact C3911742 with 0 available and remains the stock blocker for a complete JLCPCB assembly.
- Equivalents are labeled separately from exact MPNs. Basic was selected where value, package, voltage, dielectric/tolerance or resistor rating agree. The 10 µF 25 V 0603 capacitor retains its ±10% exact part because the proto Basic choice is ±20%. Y1 uses the requested Proto Basic crystal, whose stability is ±20 ppm versus the original ±10 ppm.
- The saved schematic BOM has the selected `LCSC` code on all 180 bottom-side positions, including the manually updated reused sheets.

## Parts needing attention

| References | Selection | Finding |
| --- | --- | --- |
| D1–D12 | [C7502722](https://www.lcsc.com/product-detail/C7502722.html) | Preferred BAS516, 33,674 available; same SOD-523 footprint. It is rated 75 V versus the original 80 V. |
| L1 | [C3911742](https://www.lcsc.com/product-detail/C3911742.html) | Exact XAL1060-122MEC, 0 available. See the researched replacements below; no L1 selection has been changed in KiCad. |
| Q1 | [C7420339](https://www.lcsc.com/product-detail/C7420339.html) | Requested Proto generic BSS138 in SOT-23; this is an alternative to the original BSS138K-13 PARTNO. |
| Y2 | [C5137267](https://www.lcsc.com/product-detail/C5137267.html) | Exact 50 MHz oscillator, 10 available. This limits a multiple-board order. |
| D13 | [C24672](https://www.lcsc.com/product-detail/C24672.html) | Proto 3.3 V SOD-123 zener. Original DDZ9684-7 exact C165455 has 3 available. |
| Y1 | [C9002](https://www.lcsc.com/product-detail/C9002.html) | Requested Proto Basic 12 MHz, 20 pF, SMD3225-4P crystal; ±20 ppm stability versus the original ±10 ppm. |
| J1 | [C431092](https://www.lcsc.com/product-detail/C431092.html) | Proto XT30PW-M30.G.Y variant; confirm pin/tail geometry of the custom footprint. |

## D1–D12 replacement review

These diodes connect adjacent ASIC logic signals (TRIP, TX, RESET, and RX) with 1 kΩ resistors to the local 1.2 V rail or ground. The exported netlist indicates a low-voltage interface; the 75 V rating of BAS516 is ample for that use, although it is below the original diode's 80 V rating. The selected [BAS516 datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2311171118_hongjiacheng-BAS516_C7502722.pdf) and [original DLLFSD01T datasheet](https://www.diodes.com/datasheet/download/DLLFSD01T.pdf) give:

| Property | Original DLLFSD01T-7 | Preferred BAS516 C7502722 | Preferred 1SS400 C7502721 |
| --- | --- | --- | --- |
| Package | SOD-523 | SOD-523 | SOD-523 |
| Reverse rating | 80 V | 75 V | 80 V |
| Forward voltage at 1 mA, max | 0.700 V | 0.715 V | Not specified |
| Capacitance at 0 V, max | 2.5 pF | 1.0 pF | 3.0 pF |
| Reverse recovery, max | 4 ns | 4 ns | 4 ns |
| Reverse leakage, max | 10 nA at 5 V; 0.2 µA at 80 V | 30 nA at 25 V; 1 µA at 75 V | 0.1 µA at 80 V |

The leakage figures use different test voltages and should not be compared as if measured under one condition. BAS516 has a specified low-current forward voltage and lower capacitance, which makes it the better fit for these logic shifters. [1SS400 C7502721](https://www.lcsc.com/product-detail/C7502721.html) is a same-footprint Preferred backup. The only Basic SOD-323 switching diode found, [1N4148WS C2128](https://www.lcsc.com/product-detail/C2128.html), needs a larger footprint and has a 1 µA leakage specification at 75 V. No Basic SOD-523 switching diode was listed in the catalog search.

The exact DLLFSD01T-7 is [C460049](https://www.lcsc.com/product-detail/C460049.html), with only 5 available for the 12 positions at the time of this review.

The existing `bitaxe:D_SOD-523` pads are 0.6 × 0.7 mm with 1.4 mm center spacing, within the BAS516 datasheet's suggested 0.55–0.65 × 0.65–0.75 mm pads and 1.37–1.47 mm spacing. No PCB footprint change is needed. Verify logic levels and edge timing on the first assembled board.

KiCad's saved schematic now exports BAS516 as the Value and PARTNO for D1–D12, with the new manufacturer and datasheet. The manually added `LCSC=C7502722` is present on all twelve exported BOM positions. The saved PCB footprint Values still show DLLFSD01T-7 because the PCB editor IPC endpoint did not accept Konnect's edit call.

## L1 replacement research

The saved [WEBENCH design report](../WebBench/WBDesign-1004x.pdf) is for 11–13 V input, 2.8 V at 20 A output, and 325 kHz switching. It calculates 5.738 A peak-to-peak inductor ripple and 22.869 A peak switch current. Current through L1 is therefore about 20.07 A RMS at that reported operating point. **The report uses TPS546D24A, while the saved KiCad BOM names TPS546D24SRVFR for U2; those current and compensation results are design guidance, not validation of the actual board.** The original Coilcraft XAL1060-122MEC is 1.2 µH, 2.5 mΩ typical DCR, 43 A saturation, and 17.9/26.3 A for a 20/40 °C rise [per Coilcraft](https://www.coilcraft.com/en-us/products/power/shielded-inductors/molded-inductor/xal/xal1060/xal1060-122/).

JLCPCB stock was checked live on 2026-09-29; all viable high-current choices found are **Extended** parts. The cataloged stock is an availability snapshot, not a reservation. The selected L1 code C3911742 remains out of stock.

| Candidate | LCSC | µH | DCR typ/max | Irms | Isat | Body | Live stock | Assessment |
| --- | --- | ---: | ---: | ---: | ---: | --- | ---: | --- |
| [Bourns SRP1245A-1R2M](https://www.bourns.com/docs/product-datasheets/srp1245a.pdf) | [C3224581](https://www.lcsc.com/product-detail/C3224581.html) | 1.2 | 2.5/3.0 mΩ | 28 A | 49 A | 13.5 × 12.5 × 4.8 mm | 65 | Best verified 1.2 µH choice; requires a new footprint and nearby placement review. |
| Vishay IHLP5050FDER1R5M01, in WEBENCH list | [C1336173](https://www.lcsc.com/product-detail/C1336173.html) | 1.5 | 2.5 mΩ catalog value | 27 A | 45 A | 13.2 × 12.9 × 6.5 mm | 233 | Electrically plausible; WEBENCH accepts 1.5 µH. New footprint and height check needed. |
| [Tai-Tech TMPC1265HP-1R5MG-D](https://www.tai-tech.com.tw/tmpc1265hp) | [C357270](https://www.lcsc.com/product-detail/C357270.html) | 1.5 | 2.5/3.0 mΩ | 27 A | 45 A | 13.5 × 12.5 × 6.2 mm | 385 | More stocked 1.5 µH option; new footprint and compensation check needed. |
| Coilank APS1060M1R2F | [C49261245](https://www.lcsc.com/product-detail/C49261245.html) | 1.2 | 2.64 mΩ catalog value | 26.5 A | 45 A | 11.9 × 11 mm | 82 | Closest body size; manufacturer land pattern and current definitions still need verification. |

The existing L1 footprint has two 2.38 × 9 mm pads at ±3.325 mm and a 10 × 11.3 mm silkscreen body outline. Bourns specifies a 14.2 mm overall recommended land pattern, so its part is not a pad-compatible substitute. C17 is only about 8 mm from L1's center on the bottom side; the larger land pattern will require a clearance and routing review. Bourns defines its 49 A saturation rating at a 20% inductance drop; Coilcraft defines the original 43 A at a 30% drop, so the headline ratings are not measured to the same threshold. Do not change the L1 `PARTNO`, `LCSC`, or footprint until a candidate and land pattern are selected.

## Complete assembly selection

| References | Qty | Value | KiCad footprint | Design PARTNO | JLCPCB/LCSC | Catalog MPN | Class | Match | Source |
| --- | ---: | --- | --- | --- | --- | --- | --- | --- | --- |
| C2, C3, C4, C5, C6 | 5 | 22uF | `Capacitor_SMD:C_1206_3216Metric` | `TMK316BBJ226ML-T` | [C12891](https://www.lcsc.com/product-detail/C12891.html) | `CL31A226KAHNNNE` | basic | Equivalent | New catalog match |
| C7, C15, C19, C25, C26, C27, C28, C34, C38, C39, C68, C72 | 12 | 1uF | `Capacitor_SMD:C_0402_1005Metric` | `EMK105BJ105MV-F` | [C52923](https://www.lcsc.com/product-detail/C52923.html) | `CL05A105KA5NQNC` | basic | Equivalent | Bonanza existing |
| C9, C10 | 2 | 1uF | `Capacitor_SMD:C_0603_1608Metric` | `CL10B105KA8NNNC` | [C29936](https://www.lcsc.com/product-detail/C29936.html) | `CL10B105KA8NNNC` | extended | Exact MPN | New catalog match |
| C11 | 1 | 4.7uF | `Capacitor_SMD:C_0402_1005Metric` | `GRM155R61A475MEAAD` | [C23733](https://www.lcsc.com/product-detail/C23733.html) | `CL05A475MP5NRNC` | basic | Equivalent | Proto reused |
| C13 | 1 | 4.7nF | `Capacitor_SMD:C_0402_1005Metric` | `GRM155R71E472KA01D` | [C1538](https://www.lcsc.com/product-detail/C1538.html) | `0402B472K500NT` | basic | Equivalent | Proto reused |
| C14 | 1 | 2.2nF | `Capacitor_SMD:C_0402_1005Metric` | `GRM155R61H222KA01D` | [C1531](https://www.lcsc.com/product-detail/C1531.html) | `0402B222K500NT` | preferred | Equivalent | Proto reused |
| C16, C32, C36, C37, C40, C41, C42, C43, C44, C45, C46, C57, C58, C59, C77 | 15 | 0.1uF | `Capacitor_SMD:C_0402_1005Metric` | `KGM05AR71C104KH` | [C307331](https://www.lcsc.com/product-detail/C307331.html) | `CL05B104KB54PNC` | basic | Equivalent | Bonanza existing |
| C17 | 1 | 1000pF | `Capacitor_SMD:C_0805_2012Metric` | `CC0805KRX7R9BB102` | [C46653](https://www.lcsc.com/product-detail/C46653.html) | `CL21B102KBCNNNC` | basic | Equivalent | Proto reused |
| C18 | 1 | 100pF | `Capacitor_SMD:C_0402_1005Metric` | `CC0402JRNPO9BN101` | [C1546](https://www.lcsc.com/product-detail/C1546.html) | `0402CG101J500NT` | basic | Equivalent | Proto reused |
| C20 | 1 | 47uF | `Capacitor_SMD:C_1206_3216Metric` | `GRM31CR61A476ME15L` | [C96123](https://www.lcsc.com/product-detail/C96123.html) | `CL31A476MPHNNNE` | basic | Equivalent | New catalog match |
| C21, C22, C23, C24 | 4 | 100uF | `Capacitor_SMD:C_1206_3216Metric` | `CL31A107MQHNNNE` | [C15008](https://www.lcsc.com/product-detail/C15008.html) | `CL31A107MQHNNNE` | basic | Exact MPN | New catalog match |
| C33, C35 | 2 | 30pF | `Capacitor_SMD:C_0402_1005Metric` | `GRM1555C1H300JA01D` | [C1570](https://www.lcsc.com/product-detail/C1570.html) | `0402CG300J500NT` | preferred | Equivalent | Bonanza existing |
| C47, C76 | 2 | 10uF | `Capacitor_SMD:C_0603_1608Metric` | `GRM188R61E106KA73D` | [C344022](https://www.lcsc.com/product-detail/C344022.html) | `GRM188R61E106KA73D` | extended | Exact MPN | New catalog match |
| C48 | 1 | 12pF | `Capacitor_SMD:C_0402_1005Metric` | `CC0402JRNPO9BN120` | [C1547](https://www.lcsc.com/product-detail/C1547.html) | `0402CG120J500NT` | basic | Equivalent | Proto reused |
| C49, C50 | 2 | 10uF | `Capacitor_SMD:C_1210_3225Metric` | `CL32B106KBJNNNE` | [C138687](https://www.lcsc.com/product-detail/C138687.html) | `CL32B106KBJNNNE` | extended | Exact MPN | Bonanza existing |
| C51, C52, C55, C56, C67, C69, C70 | 7 | 0.1uF | `Capacitor_SMD:C_0402_1005Metric` | `CL05B104KB54PNC` | [C307331](https://www.lcsc.com/product-detail/C307331.html) | `CL05B104KB54PNC` | basic | Exact MPN | Bonanza existing |
| C53, C54 | 2 | 33nF | `Capacitor_SMD:C_0603_1608Metric` | `CL10B333KB8NNNC` | [C21117](https://www.lcsc.com/product-detail/C21117.html) | `CL10B333KB8NNNC` | basic | Exact MPN | Bonanza existing |
| C60, C61, C62, C63, C64 | 5 | 120pF | `Capacitor_SMD:C_0402_1005Metric` | `GCM1555C1H121FA16D` | [C126496](https://www.lcsc.com/product-detail/C126496.html) | `GCM1555C1H121FA16D` | extended | Exact MPN | New catalog match |
| C65, C66 | 2 | 22uF | `Capacitor_SMD:C_0805_2012Metric` | `CL21A226MAQNNNE` | [C45783](https://www.lcsc.com/product-detail/C45783.html) | `CL21A226MAQNNNE` | basic | Exact MPN | Bonanza existing |
| C83, C84, C85, C91, C92, C93, C99, C100, C101, C107, C108, C109, C115, C116, C117, C123, C124, C125, C131, C132, C133, C139, C140, C141 | 24 | 100uF | `Capacitor_SMD:C_0805_2012Metric` | `GRM21BR60G107ME11K` | [C7282454](https://www.lcsc.com/product-detail/C7282454.html) | `GRM21BR60G107ME11K` | extended | Exact MPN | New catalog match |
| C142, C143, C144 | 3 | 1000pF | `Capacitor_SMD:C_0402_1005Metric` | `GRM155R71H102KA01D` | [C1523](https://www.lcsc.com/product-detail/C1523.html) | `0402B102K500NT` | basic | Equivalent | New catalog match |
| D1, D2, D3, D4, D5, D6, D7, D8, D9, D10, D11, D12 | 12 | BAS516 | `bitaxe:D_SOD-523` | `BAS516` | [C7502722](https://www.lcsc.com/product-detail/C7502722.html) | `BAS516` | preferred | Exact MPN | New catalog match |
| D13 | 1 | 3.3V Zener | `Diode_SMD:D_SOD-123` | `DDZ9684-7` | [C24672](https://www.lcsc.com/product-detail/C24672.html) | `MMSZ4684T1G` | extended | Equivalent | Proto reused |
| FB1, FB2 | 2 | FerriteBead | `Inductor_SMD:L_1206_3216Metric` | `BLM31KN271SN1L` | [C703115](https://www.lcsc.com/product-detail/C703115.html) | `BLM31KN271SN1L` | extended | Exact MPN | Bonanza existing |
| J1 | 1 | XT30 | `bitaxe:XT30PW-M` | `XT30PW-M` | [C431092](https://www.lcsc.com/product-detail/C431092.html) | `XT30PW-M30.G.Y` | extended | Specified variant | Bonanza existing |
| J6 | 1 | USB_C_Receptacle_USB2.0 | `bitaxe:USB_C_Receptacle_GCT_USB4105-xx-A` | `USB4105-GF-A` | [C3020560](https://www.lcsc.com/product-detail/C3020560.html) | `USB4105-GF-A` | extended | Exact MPN | New catalog match |
| L1 | 1 | 1.2uH | `bitaxe:XAL1060122MEC` | `XAL1060-122MEC` | [C3911742](https://www.lcsc.com/product-detail/C3911742.html) | `XAL1060-122MEC` | extended | Exact MPN | New catalog match |
| L2, L3 | 2 | 1.5uH | `bitaxe:IND-SMD_L2.0-W1.6` | `FTC201612S1R5MBCA` | [C5832350](https://www.lcsc.com/product-detail/C5832350.html) | `FTC201612S1R5MBCA` | extended | Exact MPN | Bonanza existing |
| Q1 | 1 | BSS138 | `Package_TO_SOT_SMD:SOT-23` | `BSS138K-13` | [C7420339](https://www.lcsc.com/product-detail/C7420339.html) | `BSS138` | preferred | Equivalent | Proto reused |
| R1, R4, R17, R35, R36, R37, R38, R39, R40, R41, R42, R43, R44, R45, R46, R47, R48, R49, R50, R51, R52 | 21 | 1k | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-071KL` | [C11702](https://www.lcsc.com/product-detail/C11702.html) | `0402WGF1001TCE` | basic | Equivalent | Proto reused |
| R2, R3, R9, R22, R34 | 5 | 10k | `Resistor_SMD:R_0402_1005Metric` | `RC0402JR-0710KL` | [C25744](https://www.lcsc.com/product-detail/C25744.html) | `0402WGF1002TCE` | basic | Equivalent | Proto reused |
| R5 | 1 | 10 | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-0710RL` | [C25077](https://www.lcsc.com/product-detail/C25077.html) | `0402WGF100JTCE` | basic | Equivalent | Proto reused |
| R6 | 1 | 1 | `Resistor_SMD:R_0805_2012Metric` | `ESR10EZPJ1R0` | [C253344](https://www.lcsc.com/product-detail/C253344.html) | `ESR10EZPJ1R0` | extended | Exact MPN | Proto reused |
| R7, R8 | 2 | 49.9 | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-0749R9L` | [C25120](https://www.lcsc.com/product-detail/C25120.html) | `0402WGF499JTCE` | basic | Equivalent | Proto reused |
| R14 | 1 | 10k | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-0710KL` | [C25744](https://www.lcsc.com/product-detail/C25744.html) | `0402WGF1002TCE` | basic | Equivalent | Bonanza existing |
| R15, R28 | 2 | 15k | `Resistor_SMD:R_0402_1005Metric` | `0402WGF1502TCE` | [C25756](https://www.lcsc.com/product-detail/C25756.html) | `0402WGF1502TCE` | basic | Exact MPN | Bonanza existing |
| R16 | 1 | 49.9k | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-0749K9L` | [C25897](https://www.lcsc.com/product-detail/C25897.html) | `0402WGF4992TCE` | preferred | Equivalent | New catalog match |
| R18, R19, R20, R21 | 4 | 4.7k | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-074K7L` | [C25900](https://www.lcsc.com/product-detail/C25900.html) | `0402WGF4701TCE` | basic | Equivalent | Proto reused |
| R23, R24 | 2 | 5.1k | `Resistor_SMD:R_0402_1005Metric` | `RMCF0402JT5K10` | [C25905](https://www.lcsc.com/product-detail/C25905.html) | `0402WGF5101TCE` | basic | Equivalent | Bonanza existing |
| R25 | 1 | 10k | `Resistor_SMD:R_0402_1005Metric` | `0402WGF1002TCE` | [C25744](https://www.lcsc.com/product-detail/C25744.html) | `0402WGF1002TCE` | basic | Exact MPN | Bonanza existing |
| R26 | 1 | 20k | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-0720KL` | [C25765](https://www.lcsc.com/product-detail/C25765.html) | `0402WGF2002TCE` | basic | Equivalent | New catalog match |
| R27 | 1 | 47k | `Resistor_SMD:R_0402_1005Metric` | `0402WGF4702TCE` | [C25792](https://www.lcsc.com/product-detail/C25792.html) | `0402WGF4702TCE` | basic | Exact MPN | Bonanza existing |
| R29 | 1 | 27k | `Resistor_SMD:R_0402_1005Metric` | `0402WGF2702TCE` | [C25771](https://www.lcsc.com/product-detail/C25771.html) | `0402WGF2702TCE` | preferred | Exact MPN | Bonanza existing |
| R30, R31, R32, R53 | 4 | 5.1k | `Resistor_SMD:R_0402_1005Metric` | `0402WGF5101TCE` | [C25905](https://www.lcsc.com/product-detail/C25905.html) | `0402WGF5101TCE` | basic | Exact MPN | Bonanza existing |
| R33 | 1 | 5.6k | `Resistor_SMD:R_0402_1005Metric` | `RC0402FR-075K6L` | [C25908](https://www.lcsc.com/product-detail/C25908.html) | `0402WGF5601TCE` | preferred | Equivalent | Proto reused |
| SW3, SW4 | 2 | SWITCH-SPST | `bitaxe:SW-SMD_TC-S3601-3.5-260G-F0.6` | `TC-S3601-3.5-260G-F0.6` | [C49101620](https://www.lcsc.com/product-detail/C49101620.html) | `TC-S3601-3.5-260G-F0.6` | extended | Exact MPN | Bonanza existing |
| U1, U3, U4, U6 | 4 | MCP1824T-1202E/OT | `bitaxe:SOT-23-5` | `MCP1824T-1202E/OT` | [C625461](https://www.lcsc.com/product-detail/C625461.html) | `MCP1824T-1202E/OT` | extended | Exact MPN | New catalog match |
| U2 | 1 | TPS546D24SRVFR | `bitaxe:LQFN40-CLIP` | `TPS546D24SRVFR` | [C20345977](https://www.lcsc.com/product-detail/C20345977.html) | `TPS546D24SRVFR` | extended | Exact MPN | Proto reused |
| U5 | 1 | W25Q16JVUXIQ TR | `bitaxe:USON-8_UX_2x3x0p6_WIN` | `W25Q16JVUXIQ TR` | [C2843335](https://www.lcsc.com/product-detail/C2843335.html) | `W25Q16JVUXIQ` | extended | Exact MPN | Bonanza existing |
| U8 | 1 | SN74AXC4T774 | `bitaxe:BQB0016A-MFG` | `SN74AXC4T774BQBR` | [C2862903](https://www.lcsc.com/product-detail/C2862903.html) | `SN74AXC4T774BQBR` | extended | Exact MPN | New catalog match |
| U9 | 1 | TPS22919DCKR | `bitaxe:DCK0006A_N` | `TPS22919DCKR` | [C2149796](https://www.lcsc.com/product-detail/C2149796.html) | `TPS22919DCKR` | extended | Exact MPN | Proto reused |
| U11 | 1 | RP2040 | `bitaxe:RP2040-QFN-56-1EP_7x7mm_P0.4mm_EP3.2x3.2mm` | `SC0914(13)` | [C2040](https://www.lcsc.com/product-detail/C2040.html) | `RP2040` | extended | Same device | Bonanza existing |
| U12 | 1 | ESP32-S3-WROOM-1 | `bitaxe:ESP32-S3-WROOM-1` | `ESP32-S3-WROOM-1-N16R8` | [C2913202](https://www.lcsc.com/product-detail/C2913202.html) | `ESP32-S3-WROOM-1-N16R8` | extended | Exact MPN | Proto reused |
| U13, U14 | 2 | TPS62933DRLR | `bitaxe:SOT-583-8_L2.1-W1.2-P0.50-LS1.6-BR` | `TPS62933DRLR` | [C3200405](https://www.lcsc.com/product-detail/C3200405.html) | `TPS62933DRLR` | extended | Exact MPN | Bonanza existing |
| Y1 | 1 | 12MHz | `bitaxe:XTAL_ECS-120-20-33-CKM-TR3` | `ECS-120-20-33-CKM-TR3` | [C9002](https://www.lcsc.com/product-detail/C9002.html) | `X322512MSB4SI` | basic | Equivalent | Proto reused |
| Y2 | 1 | 50MHz | `bitaxe:OSC_ECS-3225MV-500-CN-TR` | `SX3M50.000E20F30THN` | [C5137267](https://www.lcsc.com/product-detail/C5137267.html) | `SX3M50.000E20F30THN` | extended | Exact MPN | Bonanza existing |

The [per-reference CSV](bottom-side-jlcpcb-1004x.csv) includes all 180 bottom-side BOM positions, current saved schematic LCSC fields, catalog stock seen during the review, and any part-specific notes. Stock counts represent catalog snapshots and are not a reservation. The 29 excluded footprint references are: FID3, FID4, H5, H6, H7, H8, NT1, TP1, TP2, TP3, TP4, TP5, TP7, TP8, TP10, TP16, TP18, TP19, TP23, TP24, TP25, TP26, TP27, TP58, TP59, TP60, TP61, TP62, TP63.
