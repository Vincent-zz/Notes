# PLD

- PLD（Programmable Logic Devices）
  - SPLD（Simple PLD）
  - CPLD（Complex PLD）
  - FPGA（Field Programmable Gate Array）
- ASIC（Application Specific Integrated Circuit）

## Technologies

- fuse-based：仅支持一次编程
  - fuse
  - anti-fuse
- ROM：断电保持，有限次重写，小体积，波动性小
  - structure
    - fixed AND array
    - programmable OR array
  - type
    - EPROM
    - EEPROM
    - flash
    - ...
- PAL（Programmable Array Logic）
  - structure
    - programmable AND array
    - fixed OR array
- PLA（Programmable Logic Array）
  - structure
    - programmable AND array
    - programmable OR array
- GAL（Generic Array Logic）
- SRAM：断电不保持，几乎无限重写，大体积，波动性大，CMOS工艺

**examples**：

- truth table -> ROM => any logic gate
- D-latch + ROM => FSM

## FPGA

a reconfigurable semiconductor integrated circuit

内部组成

- Configurable Logic Blocks (the fundamental piece)
  - LUT（Look Up Table）：储存器，储存“真值表”，修改“真值表”即等效于设置组合逻辑
  - Flip-Flop：实现时序逻辑
- Interconnects：导线
- IO Blocks
- ...
