# superio-FPGA-IP
A VHDL FPGA IP core to replace obsolete Super-IO chips for computer motherboard design

To download: Click the zip file, on the next page click "view raw" to download it.

originally hosted (in 2019) at: 
https://opencores.org/projects/sio
Since the OpenCores.org admins have abandoned their website, and don't allow new users to register and download old projects (like this one), I, Istvan Nagy, the sole author, decided to re-release the project on GitHub. The new release is with a new, less restrictive license: "BSD zero clause", instead of the old LGPL. This will allow companies to use it in their product without restrictions or obligations.
The BSD 0-clause license: Copyright (C) 2024 by Istvan Nagy buenoshun@gmail.com 
Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted. THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE.

SIO is a typical legacy function on x86 computer motherboards, typically implemented as a dedicated chip like the Microchip SCH3227 device. SIO provides UART serial port and PS/2 keyboard+mouse interfaces, while accessible to the x86 chipset via an LPC bus. This core relies on other opencores projects also:

https://opencores.org/projects/uart16550
https://opencores.org/projects/wb_lpc
http://www.opencores.org/projects/ps2/ You have to copy these files onto your computer together with the 3 files provided here, before compilation.

Address range:
COM1: 3F8-3FFh
COM2: 2F8-2FFh
COM3: 3E8-3EFh
COM4: 2E8-2EFh
PS2: 60h AND 64h
post-code: 80h AND 81h
Custom board logic registers: 200h...207h (r/w regs connect in/out outside, ro regs out NC)
Some adjustments are needed for the re-used cores:

UART below this module: https://opencores.org/projects/uart16550 For the UART, use the 33MHz compliant version regs file: uart_regs_33m.v In uart_defines.v uncomment the "`define DATA_BUS_WIDTH_8"
PS2 below this module: http://www.opencores.org/projects/ps2/ In the ps2_defines, uncomment `define PS2_AUX to enble the keyboard
LPC slave: https://opencores.org/projects/wb_lpc Use these files: wb_lpc_periph.v, wb_lpc_defines.v, serirq_defines.v, serirq_slave.v Some of the files had references, that needs rewriting to remove relative path: `include "wb_lpc_defines.v" In wb_lpc_periph.v change a line: always @(posedge clk_i or negedge nrst_i) ===> always @(posedge clk_i)
Detailed Logic Resource Usage on Microsemi Igloo2: 3768 4LUT, 2050 DFF

STATUS:
As of November 2019, the code was synthesized on Xilinx Vivado and Microsemi Libero, but was not tested on a prototype. Sililar logic functions were previously tested on a prototype board, but these source files with this exact implementation was not.
