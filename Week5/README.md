# Week 5 - OpenROAD Installation Report

## Terminal Screenshots with Commands

### 1. Dependency Installation
```bash
sudo ./setup.sh
```
![Dependency Installation](Lab_Images/setup.jpg)

---

### 2. OpenROAD Build
```bash
rm -rf tools/OpenROAD/build
./build_openroad.sh --local --threads 1 --openroad-args "-DENABLE_DIST_TESTS=OFF"
```
![OpenROAD Build](Lab_Images/build.jpg)

---

### 3. Environment Setup & Verification
```bash
source ./env.sh
openroad -help
yosys -help
```
![Environment Verification](Lab_Images/installation_verification)

---

### 4. Design Flow Execution
```bash
cd ~/OpenROAD-flow-scripts/flow
make
```
![Design Flow](installation_verification/flow.jpg)

---

### 5. GUI Generation
```bash
make gui_final
```
![GUI Generation](Lab_Images/gui_final)

---

### 6. Directory Verification
```bash
ls OpenROAD-flow-scripts
ls flow
```
![Directory Structure](Lab_Images/directory_verification)

---

## Generated Output Files
```bash
cd OpenROAD-flow-scripts/flow/results/nangate45/gcd/base
```
![Output](Lab_Images/output_files.jpg)

---

## Summary

**Steps Followed:**
Installed OpenROAD in WSL Ubuntu 22.04 by running dependency setup script, building from source with single-threaded compilation to optimize memory usage, sourcing environment variables, and executing the complete RTL-to-GDSII design flow on the default GCD design.

**Challenges & Solutions:**
Initial build failures were caused by gtest linking errors and out-of-memory issues with 8 threads; resolved by using `-DENABLE_DIST_TESTS=OFF` flag to skip test compilation and reducing threads to 1 for stable memory management in WSL environment.

**Username:** `bnk@Nithishkumar`  
**System:** WSL Ubuntu 22.04 LTS  
**Status:** ✅ Installation & Flow Execution Complete
