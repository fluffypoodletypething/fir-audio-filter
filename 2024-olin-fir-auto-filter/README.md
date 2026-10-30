# FPGA FIR Filter

Names redacted, but this was originally a project from my time at Olin College
of Engineering before I transferred to UMass Amhest. The original team name was
Team25, because we were all supposed to graduate in 2025.

# Setup

The setup is designed for a
[Digilent CMOD A7](https://digilent.com/reference/programmable-logic/cmod-a7/start)
FPGA development board with a
[Digilent I2S PMOD](https://digilent.com/reference/pmod/pmodi2s2/start) board
attached to it. It should work with other Xilinx FPGA boards, but you will need
to provide your own XDC.

# `sim.py`


`sim.py` is a Python wrapper around simulation and synthesis tools. It uses
`iverilog` as the simulator and `vivado` for synthesis. It has been confirmed to
work with iverilog 14.0 as of October 30, 2026.

I no longer have access to the Xilinx FPGA dev board, so I am not able to confirm if
the latest Vivado works. It worked at the start of 2024.

## Simulating a Testbench with `sim.py`

You can run a testbench with `sim.py -br verif/testbench_name/`
- `-b` "build", recompile the testbench
- `-r` "run", run the simulation

For example, to test the top level system `sim.py -br verif/fir_filter/`

## Building a Bitstream with `sim.py`

To build the bitstream for the full system

```
sim.py fpga/fir_system/ -s
```

This will use `vivado` to synthesize the FPGA. Note, you must set the
environment variable `FPGA_PART` to your particular fpga. In the case of the
digilent board: `FPGA_PART=xc7a35tcpg236-1`

To flash it to the board, you can use `vivado` or `djtgcfg`. You can do this
manually or use `sim.py`

```
sim.py fpga/fir_system -w vivado   # For vivado
sim.py fpga/fir_system -w digilent # For djtgcfg
```

The whole process can be combined together with

```
sim.py fpga/fir_system -s -w your-choice-of-programmer
```

`fir_system` should now synthesize and flash to a Cmod-A7 plugged into your
computer.

Button 0 resets the system, and button 1 switches between a loopback test and
the FIR filter.

The implemented filter is a band-pass filter with cutoffs of 50Hz and 8kHz.
