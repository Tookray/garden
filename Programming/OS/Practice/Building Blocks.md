Writing a x86 OS block-by-block.

---

To begin, we need an understanding of what BIOS (basic input-output software) and boot sector are. A <mark style="background: #FFB86CA6;">BIOS</mark> is a collection of routines that provides detection and basic control of the computer's essential devices such as display, hard disks, etc. The BIOS does the following:

1. Loops through each storage device
2. Reads the boot sector into memory
3. Instructs the CPU to being executing the first boot sector that it finds

The <mark style="background: #FFB86CA6;">boot sector</mark> is a 512 bytes long sector on the disk that the BIOS searches for to find the OS. The last two bytes of the sector are the magic number `0xaa55`. A basic boot sector might look like:

```assembly
loop:
	jmp loop

times 510-($-$$) db 0

dw 0xaa55
```

When we compile (assemble?) it, and we inspect the binary using `hexdump` we get:

```
0000000 feeb 0000 0000 0000 0000 0000 0000 0000
0000010 0000 0000 0000 0000 0000 0000 0000 0000
*
00001f0 0000 0000 0000 0000 0000 0000 0000 aa55
0000200
```

>[!note] Little-endian and big-endian
>The <mark style="background: #FFB86CA6;">x86 architecture uses little-endian</mark>, but the `hexdump` tool outputs using big-endian for 16-bit words. So if we output the binary again using the `-C` flag, you will find a `55 aa` sequence!

When we run it using `qemu`:

![[basic-boot-sector.png]]

To print something, anything to the screen, we need to understand interrupts and registers. An <mark style="background: #FFB86CA6;">interrupt</mark> is a mechanism that allows the CPU to halt whatever it's doing and run some other, high-priority instructions before resuming. Interrupts can be raised by a software instruction (like `int 0x10`) or by a hardware device. Each interrupt can be uniquely identified by an index in the <mark style="background: #FFB86CA6;">interrupt vector</mark> which points to an <mark style="background: #FFB86CA6;">ISR</mark> (interrupt service routine). To save space, the BIOS multiplexes the ISRs based on the general purpose register `ax`.

```assembly
mov ah, 0x0e

mov al, 'H'
int 0x10

mov al, 'e'
int 0x10

mov al, 'l'
int 0x10

mov al, 'l'
int 0x10

mov al, 'o'
int 0x10

jmp $

times 510-($-$$) db 0

dw 0xaa55
```

Assembling the snippet above, we get:

![[hello-boot-sector.png]]

We know that the boot sector is loaded into memory, but where? As it turns out, the BIOS loads the boot sector to address `0x7c00` (BIOS routines, interrupt vector, etc. are loaded somewhere in memory and takes up space).

---

An aside.

When the processor goes through the binary, it isn't able to determine if a byte represents an instruction or data. Because of this limitation, <mark style="background: #FFB86CA6;">the processor will default to assuming that the byte is a sequence of an opcode</mark>.

The consequence of the behaviour is as follows:

```assembly
org 0x7c00

mark db 'x'

mov ah, 0x0e
mov al, [mark]
int 0x10

jmp $

times (512 - 2) - ($ - $$) db 0
dw 0xaa55
```

The binary is:

```
00000000  78 b4 0e a0 00 7c cd 10  eb fe 00 00 00 00 00 00  |x....|..........|
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa  |..............U.|
00000200
```

and it can be interpreted as:

```
78 --- mark db 'x'

b4 +-- mov ah, 0x0e
0e |

a0 +-- mov al, [mark] (expanded as 0x7c00)
00 |
7c |

cd +-- int 0x10
10 |

eb +-- jmp $
fe |
```

But there is a *slight* problem, `78` is a `jump short if sign (SF = 1)` instruction, and the opcode is `78 <byte>`. This means that our binary instead gets interpreted as:

```
78 +-- js 0xb4
b4 |

0e --- push cs

...
```

Well this isn't good, the code isn't working as intended! One way to solve this problem is to add a `0x00` after `"x"` and clear `SF`, but clearly this is not the *right* approach. This is why we <mark style="background: #FFB86CA6;">separate the data and the code into different sections</mark>.
