# MiniDos-utils  
A set of simple utility commands for the Mini/Dos operating system.  These commands were all assembled into 1802 binary files using the 
[Asm/02 1802 Assembler](https://github.com/fourstix/Asm-02) by Mike Riley.

XSB and XRB Utilities  
---------------------
If you are looking for the latest version of the XSB utility, it can be found [here](https://github.com/fourstix/MiniDos-utils/blob/main/bin/file/xsb.bin). If you are looking for the updated version of the XRB utility, it can be found [here](https://github.com/fourstix/MiniDos-utils/blob/main/bin/file/xrb.bin).

Platform  
--------
These commands were written to run on a [Pico/Elf](http://www.elf-emulation.com/picoelf.html) with the Spare Time Gizmos [STG RTC/NVR/UART expansion card](http://www.elf-emulation.com/hardware.html) and the [STG Pico/Elf EPROM v1.12](https://groups.io/g/cosmacelf/files/STG%20Elf2K/Elf2K%20and%20PicoElf%20EPROM%20v112%20BIOS%201.0.9.zip) written by Bob Armstrong. These commands have also been tested on the [1802-Mini](https://github.com/dmadole/1802-Mini) by David Madole. A lot of information and software for the Pico/Elf and the 1802-Mini can be found on the [Elf-Emulation](http://www.elf-emulation.com/) website and in the [COSMAC ELF Group](https://groups.io/g/cosmacelf) at groups.io.

Mini/DOS File Utility Commands
-------------------------------------

## cmd
**Usage:** cmd [-e] [*filename*, default = start.cmd]    
Run commands from the file *filename*, or the file start.cmd if not specified. Each
line of the file contains a separate command. The option -e will echo the commands before they are executed.
 
**Note:** 
To use *cmd* as the Mini/DOS init program, copy this file as an executable file named *init* in the /bin directory. Mini/DOS will then execute the commands contained in the start.cmd file in the root / directory when the Mini/DOS boots. *Press and hold Input /EF4 to skip the execution of start.cmd and auto-baud during boot-up.* 

**Note:**
The cmd program occupies memory from $5000 to $6000.  Programs up to 12K in size that load at $2000 can be run from a command file.  If a program allocates memory so that the heap goes below $6000, the command interpreter will exit with an 'Out of Memory' error. 

## flags
**Usage:** flags *filename*    
Show the Mini/DOS flags associated with the file. Display 'd' for a directory file, 'x' for an executable file, 'h' for a hidden file and 'w' for write-protected file.  If a flag is not set, then a dot '.' is displayed instead. The string '. . . .' means no flags are set for the file.

## header
**Usage:** header *filename*    
Show the executable header information for a file to display the program load address, program size and the program execution address.

## scpy
**Usage:** scpy [-y] *source* *dest*    
Safely copy the file from *source* to the destination file *dest*.  The scpy command does not over-write directories and will prompt before over-writing an existing destination file.  The -y option will over-write an existing file without the prompt. *Copy or rename 'scpy' to 'copy' to replace default Mini/DOS command in the /bin directory.*

## xtrim
**Usage:** xtrim *filename*, where *filename* is an executable file.  
Trim the executable file *filename* to the runtime size in its header, and save with the .tr extension.  *Useful to remove padding bytes added by an XMODEM transfer* 

## xsb
**Usage:** xsb *filename*    
XModem Send command that uses the hardware UART from am expansion card instead of the bit banged serial routines to send the file named *filename*.  This command is the compliment to the **xrb** [XModem receive command](https://github.com/rileym65/Elf-Elfos-xr), and can be used to send binary files from the Pico/Elf to another computer via the STG NVR/RTC/UART expansion card's UART serial interface, or from the 1802-Mini to another computer via the [1854 Serial](https://github.com/dmadole/1802-Mini-1854-Serial) card's UART serial or FTDI interface. Xsb uses Mini/DOS Kernel API and is compatible with Mini/DOS UART drivers such as the [Mini/DOS Studio](https://github.com/dmadole/Elfos-studio) driver. 


## xrb
**Usage:** xrb *filename*    
XModem Receive command that uses the hardware UART from am expansion card instead of the bit banged serial routines to receive the file named *filename*.  This command is an updated version of the [XModem receive command](https://github.com/rileym65/Elf-Elfos-xr) that uses the Mini/DOS Kernel API, to be compatible with Mini/DOS UART drivers such as the [Mini/DOS Studio 1854 UART](https://github.com/dmadole/Elfos-studio) driver. Xrb can be used to receive binary files from another computer to the Pico/Elf via the STG NVR/RTC/UART expansion card's UART serial interface, or from another computer to the 1802-Mini via the [1854 Serial](https://github.com/dmadole/1802-Mini-1854-Serial) card's UART serial or FTDI interface.

Mini/DOS System Utility Commands
-------------------------------------

## int
**Usage:** int [-d|-e]  
Display the interrupt status and value of the IE flag.  The option -d will disable interrupts by 
setting the IE flag false.  The option -e will enable interrupts by setting the IE flag true.

## malloc
**Usage:** malloc [-f *hh*] *size*    
Allocate a block of memory of *size* bytes on the heap. The -f option will fill the memory with the *hh* hex byte value. *Useful for testing low memory conditions.*

## mfree
**Usage:** mfree *hhhh*    
Free a block of memory allocated at the hex address *hhhh* on the heap.

## req  
**Usage:** req  
Reset Q.  This command turns the Q bit off. (Q = 0)

## seq  
**Usage:** seq  
Set Q.  This command turns the Q bit on. (Q = 1)

## stack
**Usage:** stack    
Print the value of the Mini/DOS stack pointer.


Mini/DOS I/O Utility Commands
-------------------------------------

## about
**Usage:** about    
Show information about the current drive. Write the contents of the file /cfg/about.nfo to the output
to show information about the disk in the current drive.

## clr
**Usage:** clr    
Clear the screen. Clears both ANSI and non-ANSI displays. *Copy or rename to 'cls' to replace default Mini/DOS command in the /bin directory.*

## drive
**Usage:** drive    
Write the current drive number to the output.

## input
**Usage:** input  
Input and display data read from Port 4

## output
**Usage:** output *hh*     
Send the hex value *hh* out to Port 4 *(where hh ranges in value from 00 to FF)*

## nop
**Usage:** nop    
No Operation, a simple program that does nothing. *Can be copied or renamed to 'rem' in the /bin directory and used for comments in command files*

## pause
**Usage:** pause [-0|-1|-2|-3|-4, default = -4]  
Display a prompt *Press Input to continue...* and wait for Input to return.  The options -1,-2,-3 or -4 will wait for input on the /EFn line.  The option -0 will wait for serial input. The default is to wait for Input on /EF4.

## pwd
**Usage:** pwd    
Print Working Directory, write the current directory to the output.

## say
**Usage:** say *text*      
Print the string *text* to the output. *Useful for printing text output in command files*

## up
**Usage:** up    
Move up to the Parent Directory, write the new current directory to the output.

STG EPROM Utility Commands  
----------------------------

## stg
**Usage:** stg    
Jump to the STG Pico/Elf EPROM v1.12 menu.  This command is the same as *Exec 8003*. Use *CALL 0303* to execute a Warm Boot to return to the Mini/DOS from the EPROM menu.

## visualstg
**Usage:** visualstg  
Run Visual02 from the STG Pico/Elf EPROM v1.12 code. This command replaces the Mini/DOS visual02 command to correctly invoke the visual02 code in the STG Pico/Elf EPROM.  *Rename or copy to visual02 in the /bin directory to replace the original command.*

1861 Pixie Video Demo and Utility Commands
------------------------------------------
## spaceship
**Usage:** spaceship *(Press Input /EF4 to exit program.)*    
Joseph A Weisbecker's Pixie Graphic Demo program modified to 
run under Mini/DOS. 

## dma_test
**Usage:** dma_test *(Press Input /EF4 to exit program.)*    
Tom Pittman's Video DMA program modified to run under Mini/DOS.

## tvclock
**Usage:** tvclock *(Press Input /EF4 to exit program.)*    
Tom Pittman's TV Clock program modified to run under Mini/DOS.

## voff
**Usage:** voff  
Video Off. Output Port 1 and Disable interrupts. This command 
is useful when debugging or writing pixie video programs to turn off a 1861 video display.

Library Files
-------------
The command files are grouped into three Mini/DOS library files that can be unpacked with the Mini/DOS lbr command using the e option to *extract* files.
* file_utils.lbr - Library file for Mini/DOS file utilities containing the cmd, flags, header, scpy, xtrim xrb and xsb commands. Extract these files with the Mini/DOS command *lbr e file_utils*
* sys_utils.lbr - Library file for Mini/DOS system utilities containing the int, malloc, mfree, req, seq and stack commands. Extract these files with the Mini/DOS command *lbr e sys_utils*
* io_utils.lbr - Library file for Mini/DOS I/O utilities containing the about, clr, drive, input, nop, output, pause, pwd, say and up commands. Extract these files with the Mini/DOS command *lbr e io_utils*
* pixie_utils.lbr - Library file for Mini/DOS 1861 Pixie Video utilities contains the spaceship, dma_test, tvclock and voff commands. Extract these files with the Mini/DOS command *lbr e pixie_utils*
* stg_utils.lbr - Library file for the STG EPROM utilities containing the stg and videostg commands. Extract these files with the Mini/DOS command *lbr e stg_utils*

Help Files
-------------
The utils.lbr file provides help information for the Mini/DOS utilities.  Although this file has the 
same format as an Mini/DOS library, do not use the lbr command to unpack it.  Instead copy this file,
as is, into the /hlp directory with the other help libraries.  The *help* command will extract information from this file.

The help information for an individual command can be displayed by typing *help utils:name*, where name is the name of the utility command.  For example to show the help information for *cmd*, type in *help utils:cmd*.

The command *help utils:* (note that it ends with a colon) will list all the utility programs with help information.  

Repository Contents
-------------------
* **/src/**  -- Common source files for assembling Mini/DOS utilities.
  * asm.bat - Windows batch file to assemble source file with Asm/02 to create binary file. Use the command *asm xxx.asm* to assemble the xxx.asm file.
  * ops.inc - Opcode definitions for Asm/02.
  * bios.inc - Bios definitions from Mini/DOS
  * kernel.inc - Kernel definitions from Mini/DOS
* **/src/file/**  -- Source files for Mini/DOS file utilities.
  * cmd.asm - Run commands from a file.
  * flags.asm - Show Mini/DOS flags associated with a file.
  * header.asm - Show the executable header information for a file.
  * scpy.asm - Safely copy a file.
  * xtrim.asm - Trim an executable file to its runtime size.
  * xrb.asm - XMODEM Receive using the hardware UART and Mini/DOS Kernel API.  
  * xsb.asm - XMODEM Send using the hardware UART and Mini/DOS Kernel API.
* **/src/io/**  -- Source files for Mini/DOS I/O utilities.  
  * about.asm - Show information about the current drive
  * clr.asm - Clear the screen
  * drive.asm - Print the current drive number
  * input.asm - Input and display data read from Port 4
  * nop.asm - No Operation - simple program that does nothing
  * output.asm - Output hh - send the hex value 'hh' out to Port 4
  * pause.asm - Display a prompt and wait for input
  * pwd.asm - Print Working Directory - prints the current directory
  * say.asm - Say 'text' - write the text string back to the output
  * up.asm - Move Up to the parent directory
* **/src/pixie/**  -- Source files for 1861 Pixie Video utilities and demo programs
  * spaceship - Joseph A Weisbecker's Pixie Graphic Demo program (Press Input /EF4 to exit)
  * dma_test -Tom Pittman's Video DMA program (Press Input /EF4 to exit)
  * tvclock - Tom Pittman's TV Clock Demo program (Press Input /EF4 to exit)
  * voff - Turn 1861 Pixie Video Off (OUT 1 and disable interrupts)    
* **/src/stg/**  -- Source files for STG NVR/RTC/UART and STG EPROM utilities.  
  * stg.asm - jump to the STG v1.12 EPROM menu
  * visualstg.asm - Run Visual02 from the STG v1.12 EPROM code (replaces visual02 command)
* **/src/sys/**  -- Source files for Mini/DOS system utilities.
  * int.asm - Display interrupt status
  * malloc.asm - Allocate block of memory on the heap.
  * mfree.asm - Free a block of memory on the heap.
  * req.asm - Reset Q. (The Q bit is available when using the STG Expansion card UART)
  * seq.asm - Set Q. (The Q bit is available when using the STG Expansion card UART)
  * stack.asm - Print the value of the Mini/DOS stack pointer  
* **/bin/file/**  -- Binary files for Mini/DOS file utilities.
* **/bin/io/**  -- Binary files for Mini/DOS I/O utilities.  
* **/bin/stg/**  -- Binary files for STG EPROM utilities.
* **/bin/sys/**  -- Binary files for Mini/DOS System utilities. 
* **/bin/pixie/**  -- Binary files for 1861 Pixie Video utilities and demo programs  
* **/lbr/**  -- Library files for Mini/DOS utilities. (Unpack with Mini/DOS lbr command)
  * file_utils.lbr - Library file for Mini/DOS file utilities.
  * io_utils.lbr - Library file for Mini/DOS I/O utilities.
  * pixie_utils.lbr - Library file for Mini/DOS 1861 Pixie Video utilities.  
  * stg_utils.lbr - Library file for STG EPROM utilities.
  * sys_utils.lbr - Library file for Mini/DOS system utilities.
* **/hlp/**  -- Help file for Mini/DOS utilities. (Used with Mini/DOS help command)
  * utils.lbr - Help file for Mini/DOS file utilities. (Do not unpack with lbr, instead copy into /hlp directory.)  
* **/docs/**  -- Other Mini/DOS documentation.
  * elf-emulation.com.zip - Zip archive file for Elf-Emulation.com website.
  
  License Information
  -------------------
  
  This code is public domain under the MIT License, but please buy me a beverage
  if you use this and we meet someday (Beerware).
  
  References to any products, programs or services do not imply
  that they will be available in all countries in which their respective owner operates.
  
  Any company, product, or services names may be trademarks or services marks of others.
  
  All libraries used in this code are copyright their respective authors.
  
  This code is based on a Mini/DOS code libraries written by David Madole and assembled with the Asm/02 assembler also written by Mike Riley.
  
  Mini/DOS
  Copyright (c) 2024-2025 by David Madole
  
  Elf/OS 
  Copyright (c) 2004-2025 by Mike Riley
  
  Asm/02 1802 Assembler
  Copyright (c) 2004-2025 by Mike Riley
  
  Mini/DOS Init Program 
  Copyright (c) 2022-2025 by David Madole
  
  Mini/DOS 1854 UART Studio Driver Program 
  Copyright (c) 2021-2025 by David Madole
    
  The Pico/Elf Microcomputer Hardware
  Copyright (c) 2020-2025 by Mike Riley
   
  The STG Pico/Elf EPROM v1.12
  Copyright (c) 2004-2025 by Spare Time Gizmos.
  
  STG NVR/RTC/UART Pico/Elf Expansion Card hardware
  Copyright (c) 2020-2025 by Spare Time Gizmos.
  
  The 1802-Mini Microcomputer Hardware
  Copyright (c) 2020-2025 by David Madole
  
  Many thanks to the original authors for making their designs and code available as open source.
   
  This code, firmware, and software is released under the [MIT License](http://opensource.org/licenses/MIT).
  
  The MIT License (MIT)
  
  Copyright (c) 2025 by Gaston Williams
  
  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files (the "Software"), to deal
  in the Software without restriction, including without limitation the rights
  to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
  copies of the Software, and to permit persons to whom the Software is
  furnished to do so, subject to the following conditions:
  
  The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the Software.
  
  **THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
  FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
  AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
  LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
  SOFTWARE.**
