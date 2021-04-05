---
layout: default
title: Projects
lastupdate: 2021-04-02
---

# Projects

## SeqStk

A virtual stack machine whose input and output is through...more stacks! Trying to keep this one minimal and flexible, so writing in Rust and avoiding any external dependencies.

### Some quirks:

* All data is 16.16 bit fixed point two's complement. If used as an address, the bottom 16 bits are ignored.
* Memory is 2^15 bytes, byte addressible. Why 2^15 and not 2^16? To enable negative offsets in particular addressing modes (we'll see if this is a good idea).
* All input/output will be triggered through eight "ports."
  * Each port is...a stack! 
  * Entities external to the VM may only push/pop with a particular port.
* Interrupts allow code to be executed at a particular time. Speaking of time....
* No internal timing.
  * You `load()` your binary code, and either `cycle_all()`, `cycle()` once, or `trigger_interrupt()`s at your pleasure.
  * Hoping this keeps scope manageable, and the VM useable in many different contexts.
  * Plus, who doesn't want to have control over their timing????

### ISA

#### Adressing Modes

Where supported, addressing modes are represented by two bits in the instruction byte.

* 11 - Immediate (Read value from next 2-4 bytes in memory)
* 10 - Index Stack (Read value from next 2 bytes in memory, treat as memory address, use stack top as offset from that address)
* 01 - Index Immediate (Read offset from next 2 bytes in memory, address from top of stack, fractional portion ignored)
* 00 - Stack (Read value from top of stack)

#### Instruction List and Format

* 111 - Stack Manipulation
  * Adressing Modes: Immediate, Index Stack, Index Immediate, Stack. Stack treats the top of the stack as a memory address (ignores fractional portion).
    * 111 - Push
    * 000 - Store
  * Adressing Modes: Stack.
    * 110 - Pop
    * 101 - Dup
    * 100 - Rot
    * 011 - Swap
    * 010 - MovToRts
    * 001 - MovFromRts
* 110 - Arithmetic
  * Adressing Modes: Stack only.
    * 11 - Add
    * 10 - Sub
    * 01 - Mul
    * 00 - Div
* 101 - Bit Manipulation
  * Adressing Modes: Stack only.
    * 111 - Shl
    * 110 - Shr
    * 101 - Rol
    * 100 - Ror
    * 011 - And
    * 010 - Or
    * 001 - Xor
    * 000 - Not
* 100 - Port Ops
  * Adressing Modes: Stack only. The remaining three bits determine which port the operation corresponds to.
    * 11 - PortPush
    * 10 - PortPop
    * 01 - PortGet
    * 00 - PortClear
* 011 - Control Flow
  * Adressing Modes: Immediate, Index Stack, Index Immediate, Stack. For comparison instructions, the top two values on the stack are compared. If the instruction jumps, the third value on the stack is used (if that adressing mode uses the stack).
    * 111 - JMP
    * 110 - CALL
    * 101 - RET
    * 100 - BEQ
    * 011 - BNEQ
    * 010 - BGT
    * 001 - BLT
* 010 - Interrupt
  * SETI - set interrupt (one for each port??)
* 001 - Misc.
  * 1 - BRK
    * Stop execution.
  * 0 - NOP
    * Do nothing.
