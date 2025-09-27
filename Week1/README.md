# 📘 Week 1 – Day 1

## 🎯 Topics Covered
- Introduction to **RTL Simulation**  
- Role of a **Simulator** in design verification  
- Environment setup with **SkyWater 130nm PDK**  
- File organization for Verilog design, testbench, and libraries  
- Basics of **Synthesis flow** with Yosys  

---

## 🖥️ Why Do We Need a Simulator?
- Digital design in RTL is a **behavioral implementation of specifications**.  
- Verification ensures the design **matches expected functionality** before synthesis.  
- A **testbench** provides input stimulus and observes outputs.  
- The simulator generates a **VCD file**, which can be viewed in **GTKWave**.  

---

## 📂 Environment & File Setup
- Git clone of **SkyWater 130nm PDK** for standard cell libraries.  
- **Verilog files**:  
  - `good_mux.v` → RTL design  
  - `tb_good_mux.v` → Testbench (generates stimulus, no I/O ports).  
- **Library files**:  
  - `lib/` → Liberty files (`.lib`)  
  - `my_lib/verilog_models/` → Standard cell models (`primitives.v`, `sky130_fd_sc_hd.v`)  

---

## 🔄 RTL → Waveform Flow
1. Write RTL (`good_mux.v`) + testbench (`tb_good_mux.v`).  
2. Simulate with **Icarus Verilog**.  
3. Run executable to produce `.vcd` file.  
4. View waveforms in **GTKWave**.  

### 🧪 Simulation Commands
```bash
# Compile RTL + testbench
iverilog good_mux.v tb_good_mux.v

# Run simulation (creates good_mux.vcd)
./a.out

# Open waveform
gtkwave good_mux.vcd
```
---

## 🔄 RTL → Netlist Flow (via Yosys)

1. Launch **Yosys**.
2. Load the standard cell library (`.lib`).
3. Read the RTL design (`good_mux.v`)
4. Run synthesis (synth -top good_mux).
5. Map design to standard cells using (`abc`).
6. Write the synthesized netlist to a Verilog file.

### 🧪 Simulation Commands
```bash
# Inside yosys shell
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog good_mux_netlist.v
```
---
```gvim
# Useful gvim commands to edit/view verilog_files
gvim -p good_mux.v tb_good_mux.v   # Open multiple files in tabs
:tabn                             # Next tab
:tabp                             # Previous tab
:tabc                             # Close current tab

:i                                # Insert mode
<Esc>                             # Exit insert mode

:se nu                            # Show line numbers
:se nonu                          # Hide line numbers
:syntax on                        # Enable syntax highlighting
:syntax off                       # Disable syntax highlighting

:w                                 # Save
:wq                                # Save and quit
:q!                                # Quit without saving
:qa!                               # Quit all without saving
:vs filename.v                     # Vertical split with another file
:sp filename.v                     # Horizontal split with another file
Ctrl+w w                           # Switch between split windows
```

**For all simulation, synthesis, and waveform outputs, please refer to the Lab_Images folder in the corresponding week’s directory.**
