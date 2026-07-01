# OneOS 2.0

**OneOS** is a from-scratch, bare-metal graphical operating system for x86 32-bit (i686). It is a complete modern rewrite of its earlier terminal-only version, now featuring a rich, software-rendered desktop environment.

Written almost entirely in **C++17** with only ~35 lines of assembly, OneOS is an educational showcase project focused on clean systems programming, kernel development, and building a full GUI stack from scratch.

It started when me and my friend Rafa decided to build an OS together because it looked like a great resume project, which quickly turned into one of those ideas that sounds simple until you actually start doing it. What we expected to be a cool side project slowly became something way more time-consuming and mentally draining than either of us planned for, still interesting, just not exactly healthy in hindsight. The only place we used AI was for the assembly code since neither of us wanted to learn assembly at the beginning.

## Key Features

### Modern Graphical Desktop
- 1024×768×32bpp VESA linear framebuffer
- Full software compositor with z-order, drop shadows, and alpha effects
- Draggable, focusable windows with macOS-style traffic-light buttons
- Top taskbar + bottom dock with hover effects
- Desktop icons and launcher menu

### Built-in Applications
- Terminal emulator with shell commands and Python integration
- Graphical file manager with sidebar and keyboard navigation
- Syntax-highlighted text editor (Ctrl+S save)
- Standalone Python REPL
- Package manager (`opkg`) with search, categories, and progress bars
- Settings panel and About dialog

### Core Capabilities
- In-memory virtual filesystem (VFS)
- Custom Python 3.12-compatible interpreter (PyRT)
- Simulated package manager with 30+ packages
- PS/2 keyboard & mouse support
- PIT timer, IDT, PIC, and debug logging

### Extremely Minimal
- ~35 lines of assembly in the entire OS (`boot.s`)
- Zero external libraries or standard C++ runtime

## Architecture

boot/boot.s → Multiboot header + stack (minimal ASM)
src/kernel.cpp → Kernel entry and main loop
src/wm.cpp → Window manager and compositor
src/desktop.cpp → Taskbar, dock, icons
src/apps.cpp → GUI applications
src/pyrt.cpp → Python interpreter
src/vfs.cpp → Virtual filesystem
src/vbe.cpp → Graphics driver
src/klog.cpp → Debug logging
src/kmalloc.cpp → Kernel heap allocator

## Major Subsystems

- Boot: GRUB2 Multiboot with VBE video mode
- Memory: Custom kmalloc (bump allocator + free list)
- Interrupts: Full IDT + 8259 PIC remapping
- Hardware Drivers: PS/2 keyboard/mouse, PIT timer, VBE framebuffer
- GUI: Software window manager + compositor
- Filesystem: In-memory VFS
- Scripting: Custom Python interpreter (PyRT)
- Debug: Serial + QEMU debugcon logging

## Tech Stack

| Layer | Technology |
|------|------------|
| Language | C++17 freestanding (no stdlib, no RTTI, no exceptions) |
| Assembly | x86 boot only |
| Bootloader | GRUB2 Multiboot v1 |
| Graphics | VESA VBE + software rendering |
| Input | PS/2 keyboard and mouse |
| Timer | 8254 PIT |
| Interrupts | Custom IDT + 8259 PIC |
| Memory | Custom heap allocator |
| Scripting | PyRT interpreter |
| Build | Makefile + build.sh |
| Toolchain | i686-linux-gnu-gcc/g++ or -m32 |
| Emulation | QEMU |

No external dependencies.

## Building & Running

### Prerequisites
sudo apt update  
sudo apt install build-essential gcc g++ grub-pc-bin grub-efi-amd64-bin xorriso mtools qemu-system-i386  

### Build
cd oneos2_polished  
chmod +x build.sh  
./build.sh  

### Run
qemu-system-i386 -cdrom oneos2.iso -m 128M -vga std  
qemu-system-i386 -cdrom oneos2.iso -m 128M -vga std -debugcon stdio  
qemu-system-i386 -cdrom oneos2.iso -m 128M -vga std -debugcon stdio -s -S  

## Project Structure

boot/boot.s — Multiboot entry point  
src/kernel.cpp — Kernel init and loop  
src/wm.cpp — Window manager  
src/desktop.cpp — UI layer  
src/apps.cpp — Applications  
src/pyrt.cpp — Python interpreter  
src/vfs.cpp — Filesystem  
src/vbe.cpp — Graphics driver  
src/klog.cpp — Logging  
src/kmalloc.cpp — Memory allocator  

## Credits

Educational operating system project focused on low-level systems programming and GUI architecture from scratch. Inspired by hobby OS development and minimal kernel design.
