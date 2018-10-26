An 'actually different' system

That is:

No C, no verilog, no x86/arm, no posix...

Design flows...

Specific constraints for the system I am working on:

No d$/i$, no TLB, no DMA/MMU/owned pages etc in the current style, channel IO etc instead, NoC...

Otherwise fairly conventional. 

Firm piece of ground
Able to change - test out ideas - fast bringup/change things at many levels

Regenerateable seeds at each different level...

Hw/sw/system codesign
bootstrapping

Solution:

ISA design
VM emulating chip
assembler 'written' in machine code - initially ...
hll written in assembler
chip design tool written in hll -> generate bitstream for fpga + gerbers&bom for boards etc
run in fpgas
write os/system
virtual physical eg co-simulation via partial emulation
