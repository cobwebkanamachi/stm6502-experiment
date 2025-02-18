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
 -------   ------- back view(pins)
 |7   5     3   1|      
 |8   6     4   2|
 |----------------
  disco      stlink(101)
  5. GND-------------GND(6pin)
  6. [5V]--------------5V(7pin)
  7. NRST--------------1pin
  8. PA12--------------2pin
  9. PA13--------------4pin
  10.GND---------------6pin

</PRE>
  
