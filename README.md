# UDS Protocol on STM32
## Overview

This project implements the **Unified Diagnostic Services (UDS)** protocol (ISO 14229-1) on an **STM32 Nucleo F446RE** microcontroller. The system communicates over the **CAN bus** for ECU diagnostics and utilizes **UART** for testing and debugging. The UDS services are structured into functional units, each corresponding to a specific diagnostic function.

---

## Table of Contents

* [Features](#features)
* [STM32CubeIDE Setup Guide](#stm32cubeide-setup-guide)
* [How to Use](#how-to-use)
* [Disclaimer](#disclaimer)

---

## Features

The following UDS services have been implemented and tested:

### Diagnostic Communication Management (DCM)

* 0x10: Diagnostic Session Control
* 0x11: ECU Reset
* 0x27: Security Access
* 0x28: Communication Control
* 0x3E: Tester Present
* 0x83: Access Timing Parameter
* 0x84: Secured Data Transmission
* 0x85: Control DTC Setting
* 0x86: Response on Event
* 0x87: Link Control

### Data Transmission

* 0x22: Read Data by Identifier
* 0x23: Read Memory by Address
* 0x2A: Read Data by Periodic Identifier
* 0x2C: Dynamically Define Data Identifier
* 0x2E: Write Data by Identifier

### Stored Data Transmission

* 0x14: Clear Diagnostic Information
* 0x19: Read DTC Information

### Upload/Download

* 0x34: Request Download
* 0x35: Request Upload
* 0x36: Transfer Data
* 0x37: Request Transfer Exit
* 0x38: Request File Transfer



---

## STM32CubeIDE Setup Guide

Follow the steps below to recreate the project environment using the **STM32CubeIDE GUI**.

### ✅ Prerequisites

* **STM32CubeIDE** version **1.13.2 or later**
* **STM32 Nucleo F446RE** development board
* Python 3.7+ installed for testing scripts

---

### 🔧 Step 1: Create a New Project

1. Open **STM32CubeIDE**.
2. Go to **File > New > STM32 Project**.
3. In the **Board Selector** tab:

   * Type `NUCLEO-F446RE`
   * Select the board and click **Next**.
4. Name your project (e.g., `UDS_CAN_Project`) and click **Finish**.

---

### ⚙️ Step 2: Configure Peripherals Using the GUI

After the project opens, STM32CubeMX (built into the IDE) will show the **.ioc configuration window**.

#### 🛠 CAN1 Configuration

1. Click on **PA11** and select **CAN1\_RX**.
2. Click on **PA12** and select **CAN1\_TX**.
3. In the **Pinout & Configuration** tab, click **CAN1** under **Connectivity**.

   * Set **Mode**: `Normal`
   * Set **Prescaler/Time Quantum** to result in \*\*640.0 ns\`

#### 💬 USART2 (UART for Debugging)

1. Click on **PA2** → Select **USART2\_TX**
2. Click on **PA3** → Select **USART2\_RX**
3. In the **Configuration tab** under **Connectivity > USART2**:

   * Set **Mode**: `Asynchronous`
   * Leave defaults for baud rate (115200 is typical for testing)

#### ⏱ Clock Configuration

1. Go to the **Clock Configuration** tab (top center).
2. Set the following:

   * **HSE** (High-Speed External Clock) to `8 MHz`
   * Set **PLL Source**: `HSE`
   * Set **SYSCLK**: `50 MHz` (adjust PLL settings accordingly)
3. Ensure **AHB**, **APB1**, and **APB2** clocks are adjusted accordingly.

#### 🧠 System Core Settings

* Go to **System Core > SYS**:

  * Debug: `Serial Wire`
  * Systick Timer: `Enabled`

#### ⏲ TIM6 Configuration (Optional but mentioned)

* Go to **Timers > TIM6**

  * Mode: `Internal Clock`

#### 💡 GPIO Configuration

1. **PA5** (Green LED)

   * Set as: `GPIO_Output`
   * Label it: `LD2`
2. **PC13** (User Button)

   * Set as: `GPIO_EXTI13`
   * Trigger: `Falling edge`
   * Mode: `External Interrupt`

---

### 🧰 Step 3: Generate Initialization Code

1. Click the **"Project > Generate Code"** button (gear icon on toolbar).
2. If prompted, confirm file generation with **Yes**.

---

### 📂 Step 4: Add Project Source Files

1. In the **Project Explorer**, locate the project you created.
2. Copy the files from this repository into your project directory:

   * `Inc/` → Copy to your project’s `Core/Inc/`
   * `Src/` → Copy to your project’s `Core/Src/`
   * `Test_uds_services/` → Place it in the project root or outside if preferred
3. In STM32CubeIDE:

   * Right-click the project name > **Refresh**
   * Right-click again > **Clean Project**
   * Then > **Build Project**

---

### 🧪 Step 5: Run UDS Tests via Python

1. Ensure your board is connected and flashed using the **Run** button.
2. Open a terminal or VSCode and navigate to the `Test_uds_services/` folder.
3. Run a test script (example):

```bash
python Test_Diagnostic_Communication_Management_functional_unit/Diagnostic_Session_Control.py
```

4. Ensure the UART port is correctly set in your test script (`COMx` on Windows, `/dev/ttyUSBx` on Linux/macOS).

---

## How to Use

1. **Flash the Firmware**
   Use STM32CubeIDE to flash the binary to the STM32 Nucleo F446RE board.

2. **Connect CAN and UART**

   * Connect the CAN interface to a testing node or analyzer.
   * Use ST-Link virtual COM or USB-UART adapter to communicate via USART2.

3. **Run UDS Requests**
   Launch Python scripts in `Test/` to send UDS requests and receive responses over UART for verification.



## Disclaimer

This project is intended **for educational and prototyping purposes only**. It is **not certified** for use in production-grade automotive environments or safety-critical applications.

---
