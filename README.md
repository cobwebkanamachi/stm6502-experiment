# stm6502-experiment
clone of https://github.com/BigEd/stm6502 and experiment with applying openocd to it.<BR>
Sorry NO WARRANTY. Your experiment is at your own risk.
<img src="https://github.com/cobwebkanamachi/stm6502-experiment/blob/main/splash.png">splash</IMG>
<PRE>
1. prep.
1.1 openocd windows binary 
   https://sysprogs.com/files/auxiliary/openocd/com.sysprogs.arm.openocd/openocd-20240916.7z
   also you should use 7zip.
1.2 extract all to windows folder you choiced.
1.3 hardware prep.
1.3.1 stlink(clone)
like bellow, but you check that is F101 or F103 misc attributes(not marked on package or body).If you done check it, choice connetion wire pin by F101 or F103.</PRE>
https://www.amazon.co.jp/YOUMILE-BG-YM-973898-Youmile-ST-LINK-V2%E3%82%A8%E3%83%9F%E3%83%A5%E3%83%AC%E3%83%BC%E3%82%BF%E3%83%BC%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%80%E3%83%BC%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9E%E3%83%BC%E3%82%B5%E3%83%9D%E3%83%BC%E3%83%88%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E3%83%A6%E3%83%8B%E3%83%83%E3%83%88STM8/dp/B084VMYNF7/ref=asc_df_B084VMYNF7?mcid=23eedf02bc1337aebf79e5d56d641719&tag=jpgo-22&linkCode=df0&hvadid=707442675357&hvpos=&hvnetw=g&hvrand=347283457245925512&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1009292&hvtargid=pla-1660779238061&psc=1&gad_source=1
<PRE>
1.3.2 ft232rl
like bellow.</PRE>
https://www.amazon.co.jp/Rasbee-FTDI232-FT232RL-USB%EF%BC%8DTTL-Arduino%E3%81%AB%E5%AF%BE%E5%BF%9C/dp/B09184LV3V/ref=asc_df_B09184LV3V?mcid=a1be1273696e33d9a52510bd64f20568&tag=jpgo-22&linkCode=df0&hvadid=707563100591&hvpos=&hvnetw=g&hvrand=11645460739277464130&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1009292&hvtargid=pla-1456684805499&psc=1&gad_source=1
<PRE>
1.3.3 target board (stm32f4 discovery)
Please remove stlink jumpers(two jumper near ST logo). If not removed, stlink of disco's upper side works.
You could buy disco like bellow.</PRE>
https://www.marutsu.co.jp/pc/i/14305647/
<PRE>
1.3.4 some jumper wire (female-female x 8 or above)
 disco              ft232rl
 1. PA3(RX)---------TX
 2. PA2(TX)---------RX
 3. GND-------------GND
 disco to stlink connection with jumper wires(bellow).
 --------   -------- back view(pins)
 |9   7   5   3   1|      
 |10  8   6   4   2|
 |------------------
</PRE>
 please notice pin assign of some differents of stlink (	F101 vs F103, this article intended F101 use.)bellow.<BR>
 https://denshikousakusenka.jimdofree.com/%E9%96%8B%E7%99%BA%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89/stm32/st-link-v2-clone%E3%81%ABreset/
<PRE>   
   disco      stlink(101)
  5. GND---------------GND(6pin)
  6. [5V]--------------5V(9pin)
  7. NRST--------------1pin
  8. PA12--------------2pin
  9. PA13--------------4pin
  10.GND---------------6pin
2. openocd setting
2.1 make my.cfg file with notepad or you prefer one on openocd bin dir.
https://github.com/marshall/target-stm32f407-disco-gcc/blob/master/openocd/stm32f407g-disc1.cfg
use this as basis. if you prefer another taste, you change this.
I use bellow.
source [find interface/stlink-v2-1.cfg]
source [find board/stm32f4discovery.cfg]

reset_config trst_only

gdb_memory_map disable

proc stm32f4_unlock () {
  stm32f2x unlock 0
  mww 0x40023C08 0x08192A3B; mww 0x40023C08 0x4C5D6E7F
  mww 0x40023C14 0x0fffaaed
}

proc stm32f4_flash {file_to_flash} {
    init; sleep 200
    reset halt; wait_halt
    stm32f4_unlock ()
    flash write_image erase "$file_to_flash"
    reset run; sleep 10
    shutdown
}

proc stm_run () {
    init; sleep 200
    reset halt; wait_halt
    reset run; sleep 10
    shutdown
}
   
2.2 invoke openocd with my.cfg
open cmd.exe
change directory you extracted openocd binary folder.
openocd -f my.cfg
then shown like bellow.
-------------------------------
Licensed under GNU GPL v2
libusb1 d52e355daa09f17ce64819122cb067b8a2ee0d4b
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
WARNING: interface/stlink-v2-1.cfg is deprecated, please switch to interface/stl
ink.cfg
Warn : Interface already configured, ignoring
Error: already specified hl_layout stlink
Info : The selected transport took over low-level target control. The results mi
ght differ compared to plain JTAG/SWD
DEPRECATED! use 'gdb memory_map', not 'gdb_memory_map'
stm_run
Info : Listening on port 6666 for tcl connections
Info : Listening on port 4444 for telnet connections
Info : clock speed 2000 kHz
Info : STLINK V2J37S7 (API v2) VID:PID 0483:3748
Info : Target voltage: 3.245563
Info : [stm32f4x.cpu] Cortex-M4 r0p1 processor detected
Info : [stm32f4x.cpu] target has 6 breakpoints, 4 watchpoints
Info : [stm32f4x.cpu] Examination succeed
Info : [stm32f4x.cpu] starting gdb server on 3333
Info : Listening on port 3333 for gdb connections
[stm32f4x.cpu] halted due to breakpoint, current mode: Thread
xPSR: 0x61000000 pc: 0x08001cb8 msp: 0x2001ff68
-------------------------------
ctrl+C or close window.

2.3 Download and extract GNU Arm Embedded Toolchain.
I use this GNU Arm Embedded Toolchain(10 2021.10). You would use more newer.
This is for 32bit environment.</PRE>
https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-win32.zip?rev=8f4a92e2ec2040f89912f372a55d8cf3&hash=8A9EAF77EF1957B779C59EADDBF2DAC118170BBF
<PRE>
2.4 connect gdb to openocd
open another cmd.exe
invoke arm-none-eabi-gdb
then shown like bellow.
------------------------------
Copyright (C) 2021 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "--host=i686-w64-mingw32 --target=arm-none-eabi".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb)

2.5 type into gdb shown bellow.
target extended-remote localhost:3333
then like shown bellow.
(gdb) target extended-remote localhost:3333
Remote debugging using localhost:3333
warning: No executable has been specified and target does not support
determining executable automatically.  Try using the "file" command.
0x08001cb8 in ?? ()
(gdb)

2.6 open telnet localhost:4444
I use teraterm.
then shown like bellow.
Open On-Chip Debugger
>

2.7 type into teraterm or you prefered terminal bwllow.
> stm32f4_unlock ()
device id = 0x10016413
flash size = 1024 KiB
>
notice:This makes stm32f4 disco's flash unlock without stlink utility.

2.8 type into teraterm or you prefered terminal bellow.
> stm32f4_flash stm6502.elf
notice: stm6502.elf is bellow.Please download this on your PC</PRE>
https://github.com/BigEd/stm6502/blob/master/testing-binaries/stm6502.elf
<PRE>
notice: if downloaded then set file into the path of (1) GDB bin path.
I set this file several path(same file).
</PRE>
<PRE>
2.9 check connect disco with ft232rl and ft232rl with your PC.
2.10 type into gdb RUN.
2.11 open teraterm with serial connection.
115200, 7bits, even, stopbit 1.
encoding is sjis/sjis.
you would some 6502 monitor output.-)
------------------------------------
*** STM32F4-Disco 6502 Simulator ***

B*
   PC SR AC XR YR SP
;F80F 94 F8 FF 00 FF
------------------------------------
   
2.12 invoke Eh Basic 
notice: basic.o is this.</PRE>
https://github.com/BigEd/stm6502/blob/master/testing-binaries/basic.o
<PRE>
I set https://github.com/BigEd/stm6502/tree/master/testing-binaries 's several files
into under 6502code/ folder. this is set under GDB bin folder. 

type into GDB shown bellow, if running, stop it CTRL+C then type.
load 6502code/basic.o 0x20000000
then type into GDB run.
(GDB) run (enter).
shown like bellow.
------------------------------------
(gdb) load 6502code/basic.o 0x20000000
Loading section .data, size 0x10000 lma 0x20000000
Start address 0x00000000, load size 65536
Transfer rate: 85 KB/sec, 13107 bytes/write.
(gdb) run
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program:
warning: No executable has been specified and target does not support
determining executable automatically.  Try using the "file" command.
------------------------------------
   
   
then change window to supermon (teraterm connected with disco of serial) .
type shown bellow.
g c800
voila. shown like bellow.
------------------------------------
G C800
Derived from EhBASIC
[C]old/[W]arm?
48382 Bytes free
Enhanced BASIC 2.22

Ready
PRINT(1/3)
 .333333

Ready
PRINT(1/3*3)
 1
Ready
------------------------------------
Now you have 6502 with supermon and EhBasic Machine on your hand.
Enjoy!   
</PRE>
