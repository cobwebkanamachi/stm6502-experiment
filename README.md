# stm6502-experiment
clone of https://github.com/BigEd/stm6502 and experiment with applying openocd to it.
<PRE>
1. prep.
1.1 openocd windows binary 
   https://sysprogs.com/files/auxiliary/openocd/com.sysprogs.arm.openocd/openocd-20240916.7z
   also you should use 7zip.
1.2 extract all to windows folder you choiced.
1.3 hardware prep.
1.3.1 stlink
1.3.2 ft232rl
1.3.3 target board (stm32f4 discovery)
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
 please notice pin assign of some differents of stlink (	F101 vs F103, this article intended F101 use.)bellow.<\PRE>
 https://denshikousakusenka.jimdofree.com/%E9%96%8B%E7%99%BA%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89/stm32/st-link-v2-clone%E3%81%ABreset/
<PRE>   
   disco      stlink(101)
  5. GND---------------GND(6pin)
  6. [5V]--------------5V(9pin)
  7. NRST--------------1pin
  8. PA12--------------2pin
  9. PA13--------------4pin
  10.GND---------------6pin
</PRE>
  
