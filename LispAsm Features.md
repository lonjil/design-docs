# LispAsm overview

## Bytecode & VM (may be changed later)
* Register machine
* 256 general registers
* 32-bit for now
* Instructions are 4 bytes wide, 8 bits for the op, and 8 for each operand. For some ops, you'll have one big operand, or one medium and one small.


## Assembly
The 8 bit operands are X, Y, and Z. A `$` indicates a register, otherwise it is a immediate value.
Any combination such as YZ or XYZ indicates a larger immediate value taking up 16 or 24 bits.

The mnemonics follow a certain pattern. V stands for an immediate value, R stands for a register, I stands for signed, and U stands for unsigned.

* Op table:

| Op    | Operands    | Description |
|-------|-------------|-------------|
| LDVI  | $X, YZ      | Loads signed value YZ into $X, performing sign extension |
| LDVU  | $X, YZ      | Loads unsigned value YZ into $X |
| ADVI  | $X, $Y, Z   | Adds $Y to Z and stores it in $X (signed) |
| ADRI  | $X, $Y, $Z  | Adds $Y to $Z and stores it in $X (signed) |
| ADVU  | $X, $Y, Z   | Adds Y to Z and stores it in $X (unsigned) |
| ADRU  | $X, $Y, $Z  | Adds $Y to $Z and stores it in $X (unsigned) |
| SBVI  | $X, $Y, Z   | Subtracts Z from $Y and stores it in $X (signed) |
| SBRI  | $X, $Y, $Z  | Subtracts $Z from $Y and stores it in $X (signed) |
| SBVU  | $X, $Y, Z   | Subtracts Z from $Y and stores it in $X (unsigned) |
| SBRU  | $X, $Y, $Z  | Subtracts $Z from $Y and stores it in $X (unsigned) |
| LDBI  | $X, $Y, $Y  | Sets $X to the byte pointed to by $Y + $Z, performing sign extension |
| LDBU  | $X, $Y, $Z  | Sets $X to the byte pointed to by $Y + $Z |
| LDWI  | $X, $Y, $Y  | Sets $X to the wyde pointed to by $Y + $Z, performing sign extension |
| LDWU  | $X, $Y, $Z  | Sets $X to the wyde pointed to by $Y + $Z |
| LDTI  | $X, $Y, $Y  | Sets $X to the tetra pointed to by $Y + $Z, performing sign extension |
| LDTU  | $X, $Y, $Z  | Sets $X to the tetra pointed to by $Y + $Z |
| LDLB   | $X, *Label* | Same as the first LDTU, except the assembler finds an appropriate static register and value for the address of *Label* (Error if no valid combination can be found) |
| JMPF  | *Label*     | Jumps to 26-bit relative address *Label* (forwards) |
| JMPB  | *Label*     | Jumps to 26-bit relative address *Label* (backwards) |
| CMPI  | $X, $Y, $Z  | $Y < $Z -> -1, $Y = $Z -> 0, $Y > $Z -> 1 |
| CMPU  | $X, $Y, $Y  | Same but unsigned |
| BZRO  | $X, *Label* | Jumps to 18-bit relative address *Label* if $X is 0 |
| BNZO  | $X, *Label* | Jumps to 18-bit relative address *Label* if $X is not 0 |
| BPOS  | $X, *Label* | Jumps to 18-bit relative address *Label* if $X is positive |
| BNPO  | $X, *Label* | Jumps to 18-bit relative address *Label* if $X is not positive |
| BNEG  | $X, *Label* | Jumps to 18-bit relative address *Label* if $X is is negative |
| BNNE  | $X, *Label* | Jumps to 18-bit relative address *Label* if $X is not negative |
| SHLV  | $X, $Y, Z   | Shift $Y left by Z, filling with 0 and storing it in $X |
| SHLR  | $X, $Y, $Z  | Shift $Y left by $Z, filling with 0 and storing it in $X |
| SLAV  | $X, $Y, Z   | Shift $Y left by Z, filling with 0 and storing it in $X |
| SLAR  | $X, $Y, $Z  | Shift $Y left by $Z, filling with 0 and storing it in $X |
| SHRV  | $X, $Y, Z   | Shift $Y right by Z, filling with 0 and storing it in $X |
| SHRR  | $X, $Y, $Z  | Shift $Y right by $Z, filling with 0 and storing it in $X |
| SRAV  | $X, $Y, Z   | Shift $Y right by Z, filling with the leftmost bit and storing it in $X |
| SRAV  | $X, $Y, $Z  | Shift $Y right by $Z, filling with the leftmost bit and storing it in $X |

## Opcode Format

Here is an initial proposal:

|   | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | A    | B    | C    | D    | E    | F    |
|---|------|------|------|------|------|------|------|------|------|------|------|------|------|------|------|------|
| 0 | LDVI | LDVU | LDBI | LDBU | LDWI | LDWU | LDTI | LDTU |      |      |      |      |      |      |      |      |
| 1 | ADVI | ADRI | ADVU | ADRU | SBVI | SBRI | SBVU | SBRU | MLVI | MLRI | MLVU | MLRU | DVVI | DVRI | DVVU | DVRU |
| 2 | JMPF | JMPB | CMPI | CMPU | BZRO | BNZO | BPOS | BNPO | BNEG | BNNE |      |      |      |      |      |      |
| 3 | SHLV | SHLR | SLAV | SLAR | SHRV | SHRR | SRAV | SRAR |      |      |      |      |      |      |      |      |
| 4 |      |      |      |      |      |      |      |      |      |      |      |      |      |      |      |      |

...


|   | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | A    | B    | C    | D    | E    | F    |
|---|------|------|------|------|------|------|------|------|------|------|------|------|------|------|------|------|
| E |      |      |      |      |      |      |      |      |      |      |      |      |      |      |      |      |
| F |      |      |      |      |      |      |      |      |      |      |      |      |      |      |      | NOOP |
