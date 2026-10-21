# ALU-Arithmetic-Logic-Unit-

# 🧮 ALU (Arithmetic Logic Unit) — Project Overview 

## 📌 1. Project Title
✅ **Parameterized N-bit ALU with Arithmetic, Logic, Shift, Compare Operations + Status Flags**

---

## 📝 2. Description
This project implements a **combinational ALU** in Verilog that supports multiple operation types using a **4-bit opcode**. It is **parameterized** (default **8-bit**) and generates common CPU-style flags:

- 🧾 **carry**
- 🎯 **zero**
- ➖ **negative**
- ⚠️ **overflow**

It also provides a dedicated **2N-bit `product` output** for multiplication.

---

## ⭐ 3. Key Features
- ⚙️ **Parameterized bit-width** using `parameter N = 8`
- 🧮 Supports:
  - ➕ Arithmetic: `ADD SUB MUL DIV MOD INC DEC`
  - 🔐 Logical: `AND OR XOR NOT`
  - 🔁 Shift: `LEFT RIGHT`
  - 🧾 Compare: `EQUAL LESS_EQ`
- 🚩 Flag generation: `carry`, `zero`, `neg`, `overflow`
- 🧱 Pure combinational logic: `always @(*)`

---

## 🔌 4. Module Interface

### 📄 Module Declaration
```verilog
module ALU(a,b,opcode,result,carry,zero,neg,overflow,product);
```

### 🧾 Ports
| Signal     | Dir | Width     | Description |
|-----------|-----|-----------|-------------|
| `a`       | In  | `N`       | Operand A |
| `b`       | In  | `N`       | Operand B |
| `opcode`  | In  | 4         | Operation select |
| `result`  | Out | `N`       | Main ALU output |
| `product` | Out | `2*N`     | Multiplication output (valid for MUL) |
| `carry`   | Out | 1         | Carry-out (used in ADD) |
| `zero`    | Out | 1         | Set when output is 0 |
| `neg`     | Out | 1         | Sign bit of output |
| `overflow`| Out | 1         | Signed overflow (ADD/SUB) |

---

## 🎛️ 5. Opcode Map (Operations)
| Operation | Parameter Name | Opcode (4-bit) |
|----------|-----------------|----------------|
| ADD      | `ADD`           | `0000` |
| SUB      | `SUB`           | `0001` |
| MUL      | `MUL`           | `0010` |
| DIV      | `DIV`           | `0011` |
| MOD      | `MOD`           | `0100` |
| XOR      | `XOR`           | `0101` |
| NOT      | `NOT`           | `0110` |
| AND      | `AND`           | `0111` |
| OR       | `OR`            | `1000` |
| Shift L  | `LOG_LEFT_SHIFT`| `1001` |
| Shift R  | `LOG_RIGHT_SHIFT`| `1010` |
| INC      | `INC`           | `1011` |
| DEC      | `DEC`           | `1100` |
| EQUAL    | `EQUAL`         | `1101` |
| LESS_EQ  | `LESS_EQ`       | `1110` |

---

## 🚩 6. Flag Behavior
### 🎯 Zero Flag (`zero`)
- For `MUL`: `zero = (product == 0)`
- Otherwise: `zero = (result == 0)`

### ➖ Negative Flag (`neg`)
- For `MUL`: `neg = product[2*N-1]`
- Otherwise: `neg = result[N-1]`

### 🧾 Carry Flag (`carry`)
- Meaningful for **ADD** only:
  - Computed using an extra-bit temporary sum: `temp = a + b`
  - `carry = temp[N]`

### ⚠️ Overflow Flag (`overflow`)
- **ADD overflow** (signed):
  - When signs of `a` and `b` are same, but sign of `result` differs
- **SUB overflow** (signed):
  - When signs of `a` and `b` differ, and sign of `result` differs from `a`

---

## 🛡️ 7. Special Cases
- ➗ **DIV by zero**: returns `result = 0`
- 🧮 **MOD by zero**: returns `result = 0`
- `product` is only used for `MUL`; otherwise it is cleared to `0`

---

## ⚙️ 8. Parameterization Example
To build a 16-bit ALU:
```verilog
ALU #(.N(16)) u_alu (
  .a(a),
  .b(b),
  .opcode(opcode),
  .result(result),
  .carry(carry),
  .zero(zero),
  .neg(neg),
  .overflow(overflow),
  .product(product)
);
```

---

## 📁 9. Files
- 📄 `ALU.v` — ALU module (combinational)

---

If you want, I can also create 🧪 a complete **testbench** (covers all opcodes + edge cases like overflow, DIV0, MOD0) and a 📊 opcode/flag waveform reference.
