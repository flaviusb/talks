An 'actually different' system

No C, no verilog, no x86/arm, no posix...

Firm piece of ground
Able to change - test out ideas - fast bringup/change things at many levels

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
