# Practical 2: Arithmetic and Logical Operations in 8086 Assembly (MASM)

## Aim

To perform addition, subtraction, multiplication, division, AND, OR, XOR, and NOT operations using 8086 assembly instructions, and inspect the results in the DOS DEBUG utility.

## Requirements

- DOSBox with MASM, LINK, and DEBUG available
- A text editor to save each operation as a separate `.ASM` file

## How to Run Any Program

1. Save each code block below using its stated filename (for example, `ADD.ASM`). GitHub provides a **Copy** button on every code block.
2. Put your `.ASM` files, `MASM.EXE`, `LINK.EXE`, and `DEBUG.EXE` in `C:\8086` (or use your existing DOSBox work folder).
3. Open DOSBox and mount that folder:

```dos
MOUNT C C:\8086
C:
```

4. Assemble, link, and debug a program. For example, for addition:

```dos
MASM ADD.ASM;
LINK ADD.OBJ;
DEBUG ADD.EXE
```

5. Inside DEBUG, enter these commands **one at a time**:

```text
g
r
q
```

- `g` runs the program until its `INT 3` breakpoint.
- `r` displays the registers; check `AX`, `BX`, and (where relevant) `DX`.
- `q` quits DEBUG.

For the other operations, replace `ADD` with `SUB`, `MUL`, `DIV`, `AND`, `OR`, `XOR`, or `NOT`.

> The examples include a small data declaration and a DOS exit after `INT 3` so they assemble consistently and can terminate if execution is continued. Inspect the registers at the `INT 3` breakpoint.

## 1. Addition

**File:** `ADD.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 5169H
    MOV BX, 1000H
    ADD AX, BX

    INT 3                   ; Breakpoint: inspect AX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `5169H + 1000H = 6169H`

**Expected registers:** `AX = 6169H`, `BX = 1000H`.

## 2. Subtraction

**File:** `SUB.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 5169H
    MOV BX, 1000H
    SUB AX, BX              ; AX = AX - BX

    INT 3                   ; Breakpoint: inspect AX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `5169H - 1000H = 4169H`

**Expected registers:** `AX = 4169H`, `BX = 1000H`.

## 3. Multiplication

**File:** `MUL.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 05H
    MOV BX, 03H
    MUL BX                  ; DX:AX = AX * BX

    INT 3                   ; Breakpoint: inspect DX:AX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `05H * 03H = 0FH` (15 in decimal)

**Expected registers:** `AX = 000FH`, `BX = 0003H`, `DX = 0000H`.

## 4. Division

**File:** `DIV.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 0200H           ; Dividend = 512
    MOV BX, 0100H           ; Divisor = 256
    XOR DX, DX              ; Clear high word of dividend
    DIV BX                  ; AX = quotient, DX = remainder

    INT 3                   ; Breakpoint: inspect AX and DX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `0200H / 0100H = 2`, remainder `0`.

**Expected registers:** `AX = 0002H`, `BX = 0100H`, `DX = 0000H`.

> **Important:** `DIV BX` divides the 32-bit value `DX:AX` by the 16-bit value in `BX`. Clear `DX` before dividing a 16-bit number held in `AX`, when the high word should be zero.

## 5. AND Operation

**File:** `AND.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 05H
    MOV BX, 03H
    AND AX, BX

    INT 3                   ; Breakpoint: inspect AX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `0101 AND 0011 = 0001`

**Expected registers:** `AX = 0001H`, `BX = 0003H`.

## 6. OR Operation

**File:** `OR.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 05H
    MOV BX, 03H
    OR AX, BX

    INT 3                   ; Breakpoint: inspect AX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `0101 OR 0011 = 0111`

**Expected registers:** `AX = 0007H`, `BX = 0003H`.

## 7. XOR Operation

**File:** `XOR.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 03H
    MOV BX, 02H
    XOR AX, BX

    INT 3                   ; Breakpoint: inspect AX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `0011 XOR 0010 = 0001`

**Expected registers:** `AX = 0001H`, `BX = 0002H`.

## 8. NOT Operation

**File:** `NOT.ASM`

```asm
.MODEL SMALL
.STACK 100H
.DATA
DUMMY DB 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 05H
    NOT AX                  ; Invert all 16 bits of AX

    INT 3                   ; Breakpoint: inspect AX
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Calculation:** `0005H = 0000 0000 0000 0101` becomes `1111 1111 1111 1010`.

**Expected register:** `AX = FFFAH`.

## Summary of Results

| Operation | Instruction | Expected result |
| :-- | :-- | :-- |
| Addition | `ADD AX, BX` | `AX = 6169H` |
| Subtraction | `SUB AX, BX` | `AX = 4169H` |
| Multiplication | `MUL BX` | `DX:AX = 0000:000FH` |
| Division | `DIV BX` | `AX = 0002H`, `DX = 0000H` |
| AND | `AND AX, BX` | `AX = 0001H` |
| OR | `OR AX, BX` | `AX = 0007H` |
| XOR | `XOR AX, BX` | `AX = 0001H` |
| NOT | `NOT AX` | `AX = FFFAH` |

## Useful DEBUG Commands

| Command | Purpose |
| :-- | :-- |
| `g` | Run until a breakpoint |
| `r` | Show or edit registers |
| `d` | Display memory |
| `u` | Disassemble machine instructions |
| `t` | Trace one instruction |
| `e` | Edit memory |
| `q` | Quit DEBUG |

## Theory

The 8086 microprocessor provides **arithmetic instructions** (`ADD`, `SUB`, `MUL`, `DIV`) for numerical operations and **logical instructions** (`AND`, `OR`, `XOR`, `NOT`) for bit manipulation. Arithmetic and logical results are stored in registers; many operations also update the CPU flags. `MUL` returns a 16-bit-by-16-bit unsigned product in `DX:AX`, while `DIV` places the quotient in `AX` and the remainder in `DX` for a 16-bit divisor. The `INT 3` instruction creates a breakpoint so results can be examined in DEBUG.

## Conclusion

All eight arithmetic and logical operations were demonstrated using 8086 assembly language. The results can be verified by inspecting the registers at the breakpoint with DOS DEBUG.
