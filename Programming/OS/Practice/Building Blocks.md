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
