# Step 0 — Toolchain Only, No Code Yet

## Goal

Be able to assemble, link, convert, and flash a binary to the ATtiny416 entirely from the command line. No IDE. No code yet. Just infrastructure.

You are proving that:

- You control the build pipeline
- You understand what each tool does
- You can flash reliably
- You can debug at the lowest level

---

## What You Need Installed

You need four pieces:

1. **Assembler / Compiler toolchain**
   `avr-gcc` (includes assembler and linker)

2. **Binary conversion tool**
   `avr-objcopy`

3. **Programmer tool**
   `pymcuprog` (recommended for UPDI Nano boards)

4. **USB access working**
   Your ATtiny416 Xplained Nano should appear as a USB device.

---

## What Each Tool Actually Does

### avr-gcc

Even if you write assembly only, `avr-gcc` acts as:

- Assembler
- Linker
- Toolchain driver

It understands:

```
-mmcu=attiny416
```

That flag is critical. It:

- Selects correct instruction set
- Selects correct memory layout
- Selects correct linker script

### avr-objcopy

Converts:

**ELF → Intel HEX**

The chip cannot flash ELF. It needs a HEX file.

### pymcuprog

Talks to the onboard debugger via USB.

Uses UPDI protocol to:

- Erase
- Flash
- Verify

No GUI required.

---

## What Success Looks Like

Before writing any code, verify:

1. Board connects
2. Tool can detect device
3. Tool can erase device

Run:

```bash
pymcuprog ping
```

You should see the ATtiny416 detected.

Then try:

```bash
pymcuprog erase -d attiny416
```

If that works, your flashing path is confirmed.

---

## Directory Layout

Create a clean project folder:

```
tiny416-groundstate/
    main.S
    Makefile
```

Even before writing code, create an empty `main.S` file.

---

## The First Build Test

Create the smallest possible assembly file — just a reset vector and infinite loop.

Build:

```bash
avr-gcc -mmcu=attiny416 -nostartfiles -o main.elf main.S
```

Convert:

```bash
avr-objcopy -O ihex main.elf main.hex
```

Flash:

```bash
pymcuprog write -d attiny416 -f main.hex
```

No blinking yet. Just prove the pipeline works.

---

## What You Should Understand At This Stage

You should know:

- What an ELF file is
- What a HEX file is
- What linking does
- Why `-mmcu` matters
- What UPDI is
- How the USB debugger bridges to UPDI

If any part feels magical, pause and inspect it.

---

## Why This Step Matters

Most people start writing code immediately.

You are building **mechanical understanding first**.

Once flashing is frictionless, you can focus on:

- Registers
- Clocks
- Vectors
- Timing

Without build system confusion.

---

## Next

When you are ready, we move to **Step 1** — Minimal reset vector and LED toggle, explained instruction by instruction.
