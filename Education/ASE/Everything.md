# General Concepts

## Microcontroller vs Microprocessor

### **Microcontroller (MCU)**
A **microcontroller** is a compact, integrated chip designed to perform **specific control tasks**. It combines a **CPU**, **memory** (RAM and flash), and **peripherals** (timers, UART, GPIO, ADC, etc.) all within a **single chip**.
- **Designed for embedded applications** (e.g., appliances, sensors, automation)
- Often runs a single-purpose program
- Optimized for **low power consumption** and **real-time control**
- Examples: **AVR (ATmega328)**, **PIC**, **STM32**, **ESP32**

**Used in:**
- IoT devices
- Home automation
- Robotics and sensors
- Consumer electronics

### **Microprocessor (MPU)**
A **microprocessor** is a general-purpose CPU chip that requires **external components** such as RAM, ROM, and I/O controllers to function. It is designed for **higher performance and flexibility**, capable of running complex operating systems like Linux or Windows.
- Used in **computers, smartphones, and high-level computing devices**
- Focuses on **processing power**, not integrated peripherals
- Supports multitasking, virtual memory, complex software stacks
- Examples: **Intel x86**, **AMD Ryzen**, **ARM Cortex-A**

**Used in:**
- Desktop and laptop computers
- Mobile phones
- Embedded Linux systems
- Industrial control systems with complex user interfaces

## GPIO

GPIOs are one of the most fundamental interfaces in embedded systems. They allow the microcontroller to **interact with the outside world** by configuring individual pins as either **inputs** (to read the state of buttons, sensors, etc.) or **outputs** (to control LEDs, relays, etc.). Each GPIO pin can be configured in various electrical modes, such as pull-up, pull-down, or open-drain, depending on the circuit’s needs. The simplicity of GPIO makes it a foundational element in nearly every embedded application.
### GPIO Electrical Configurations: Pull-Up, Pull-Down, and Open-Drain
When configuring a GPIO pin as **input** or **output**, it’s important to understand how its **electrical state** behaves. That’s where these terms come in:
#### **Pull-Up Resistor**
A **pull-up** resistor connects the input pin to **Vcc (logic high)** through a high resistance (internally or externally). It ensures that the pin reads **HIGH (1)** when nothing else is actively driving it.
- Prevents **floating inputs** (which could randomly read 0 or 1)
- Common in button circuits, where pressing the button pulls the pin to GND

#### **Pull-Down Resistor**
A **pull-down** resistor connects the input pin to **GND (logic low)** through a high resistance. It ensures the pin reads **LOW (0)** when not actively driven.
- Less common than pull-up in microcontrollers
- Ensures a known state when no signal is applied

#### **Open-Drain (or Open-Collector)**
In **open-drain mode**, the GPIO **can only pull the line LOW** — it cannot drive it HIGH. To allow the line to go high, an **external pull-up resistor** is used.
- Useful for **bus sharing** (e.g., I2C lines), where multiple devices can pull the line low without conflicting
- Allows **safe multi-device communication**
- Can be used for **wired-AND** logic

## PWM

**Pulse Width Modulation** is a technique used to simulate **analog output** using a digital signal. It works by rapidly toggling a pin between HIGH and LOW states, where the **duty cycle**—the proportion of time the signal is HIGH in one cycle—determines the average voltage. By adjusting this duty cycle, PWM can be used to control devices that respond to varying power levels.

**Used in:**
- LED brightness control
- Motor speed and direction control
- Generating analog-like signals for DAC-less audio or sensor actuation

Most microcontrollers have dedicated **PWM hardware timers**, and some offer features like **automatic fading** or **phase adjustment** for more advanced applications.

## Macros

Macros are **preprocessor directives** used to define constants, reusable code snippets, or hardware-specific register manipulations. They are processed by the compiler before actual compilation starts, meaning macros don’t consume memory or execution time themselves.

They are especially useful in embedded systems to define register addresses, bit positions, or operations that are reused frequently. For example:

```c
define LED_PIN 13
define TOGGLE_BIT(PORT, BIT) ((PORT) ^= (1 << BIT))
```

While macros can simplify code and make it more readable, they lack type checking and can lead to unexpected behavior if not carefully used. Unlike functions, macros don’t generate real code—they just perform textual substitution.

**Macros** in C/C++ are handled entirely by the **preprocessor**, **before** the actual compilation of your code so they are **not stored in the memory**. 
## UART

UART is a widely used **asynchronous serial communication protocol**, ideal for low-speed data exchange between a microcontroller and peripherals like GPS modules, Bluetooth modules, or computers. It requires only two main lines: **TX (transmit)** and **RX (receive)**.

UART sends data in structured frames containing a **start bit**, **data bits**, an optional **parity bit**, and one or more **stop bits**. Since it is asynchronous, both sides must agree on a **baud rate** (e.g., 9600, 115200 bps).

It is a popular choice for:
- Debugging via serial monitors
- Communication with GPS, GSM, or Wi-Fi modules
- Bootloaders and firmware flashing

Many microcontrollers offer multiple UARTs with configurable options such as hardware flow control and DMA support for high-speed transfers.
## DMA

**DMA (Direct Memory Access)** is a hardware feature that allows data to be transferred between **memory and peripherals** (or between two memory locations) **without CPU involvement**. This is especially important in high-performance or real-time applications where the CPU must remain free to handle critical tasks.

For example, a DMA controller can automatically move a large block of data from an ADC into a memory buffer without the CPU needing to copy each byte. This drastically reduces CPU overhead and improves data throughput.

**Used in:**
- Transferring audio samples
- Capturing sensor data streams
- Sending large data blocks via SPI, I2C, or UART

DMA also supports interrupts to signal transfer completion, allowing the CPU to respond only when needed.
## DMAC

The **DMA Controller (DMAC)** is the logic unit responsible for managing **multiple DMA channels**. It handles the setup of data transfers, including:
- Source and destination addresses
- Transfer size
- Data width
- Direction (read/write)

It also manages **channel priority** when multiple transfers are requested simultaneously. Advanced DMACs can chain multiple transfers or trigger actions on peripheral events (e.g., start DMA transfer when a timer overflows).

In many microcontrollers, the DMAC operates semi-autonomously and can be configured via register programming or hardware abstraction layers (HAL).
## EEPROM

EEPROM is a type of **non-volatile memory** that retains data even when power is lost. It is often used in embedded systems to store **configuration settings**, **calibration data**, **serial numbers**, or **user preferences** that need to persist between resets or power cycles.

EEPROM allows **byte-level read and write**, unlike flash memory which requires block-level erasure. However, it has a limited number of **write cycles** (typically 100,000 or fewer), so frequent updates should be carefully managed to avoid premature wear.

**Used in:**
- Saving device state or settings across restarts
- Storing unique identifiers or product information
- Logging small amounts of data over time

Some microcontrollers include internal EEPROM, while others require external EEPROM chips via I2C or SPI.
# Aula 2

## What is the purpose of the CMake tool?

Basic Workflow of CMake
1. Write a `CMakeLists.txt` file that defines the project, source files, dependencies, and build settings.
2. Run `cmake` to generate the build system files:
$$ cmake -S . -B build $$
3. Use the generated build system to compile the project:
$$ cmake --build build $$

CMake is widely used in modern C++ projects due to its flexibility, portability, and ease of integration with various tools and compilers.

CMake is an open-source tool designed to manage the build process of software in a platform-independent manner. It is primarily used for generating build files that can be used with different native build systems such as Makefiles, Ninja, Visual Studio project files, Xcode project files, and others.

**Purpose of CMake:**
1. **Cross-Platform Build Configuration**
    - CMake allows developers to write build scripts that work across different operating systems (Windows, macOS, Linux) and compilers (GCC, Clang, MSVC).
2. **Automatic Build System Generation**
    - Instead of manually writing complex Makefiles or project files, CMake generates them based on a simple CMake configuration file (`CMakeLists.txt`).
3. **Dependency Management**
    - It can find and configure external libraries, ensuring that all required dependencies are correctly located and linked.
4. **Out-of-Source Builds**
    - Supports building software in a separate directory from the source code, keeping the source clean and preventing conflicts.
5. **Integration with IDEs**
    - Works with popular Integrated Development Environments (IDEs) like Visual Studio, CLion, and Xcode.
6. **Build Customization**
    - Allows defining custom build options and configurations using `CMakeLists.txt` and `CMakeCache.txt`.
7. **Support for Multiple Build Types**
    - Easily configure different build types such as Debug, Release, RelWithDebInfo, and MinSizeRel.
## What is the purpose of the Ninja tool? 

**Purpose of the Ninja Build System** Ninja is a small, fast build system designed to efficiently handle incremental builds. It is primarily used to speed up the compilation of large projects by minimizing unnecessary work and maximizing parallelism.

#### **Key Features and Purpose of Ninja:**
1. **Optimized for Speed**
    - Ninja is significantly faster than traditional build systems like Make because it avoids unnecessary file checks and only rebuilds what is required.
2. **Incremental Builds**
    - It tracks dependencies efficiently, ensuring that only modified files are recompiled, reducing build times dramatically.
3. **Parallel Execution**
    - Ninja automatically maximizes CPU usage, making it much faster than `make -j` in large projects.
4. **Minimal Overhead**
    - The Ninja build system is lightweight and has fewer features than Make, focusing only on executing build commands as quickly as possible.
5. **Integration with CMake and Other Generators**
    - Unlike Make, Ninja does not have its own build configuration language. Instead, CMake, GN (Google's build system), or Meson are often used to generate Ninja build files.
6. **Cross-Platform Support**
    - Works on Windows, Linux, and macOS, making it a portable solution for fast builds.
**Ninja vs. Other Build Systems**
- **Make:** Ninja is significantly faster and better at handling dependencies.
- **CMake:** CMake is not a build system but a **build system generator**. It can generate Ninja files for fast compilation.
- **Bazel/Meson:** Ninja is simpler but does not include higher-level dependency management.
#### **Basic Usage of Ninja**
1. **Generating Ninja Build Files using CMake**
$$ cmake -G Ninja -S . -B build $$
2. **Building the Project with Ninja**
$$ ninja -C build $$
Ninja is widely used in modern development workflows, especially in projects where **fast incremental builds** are essential, such as **Chromium, LLVM, and Android development**. If your project is large and relies on CMake, switching to Ninja can significantly improve build performance.

# Aula 3

## C/C++ Compilation Flow
![[image-1 3.png]]

## C/C++ Primitives Types

- **Integer Types:** `int`, `short`, `long`, `long long`
- **Floating-Point Types:** `float`, `double`, `long double`
- **Character Types:** `char` (maybe signed or unsigned)
	- `wchar_t`, `char16_t` and `char32_t` for wider character sets
- **Void Type:** `void`, represents the absence of type, for functions that do not return value or a generic pointer
- **Boolean Type (C++ only):** `bool`, values being `true` or `false`

## C/C++ Derived Types

- **Arrays:** Collections of elements of a specified type (int arr[10])
- **Pointers:** Variables that hold memory addresses
- **Functions:** While not a data type, functions can be pointed to, making them a derived type
- **References (C++ only):** Provide an alternative name for an existing variable (`int& ref = someInt;`)

## C/C++ User-defined Types

- **Structures (struct):** Group together related variables
- **Unions (union):** Similar to structs but will all members sharing the same memory location
- **Enumerations (enum):** can have one of a few discrete values
- **Classes (C++ only):** Classes can encapsulate data and functions together


![[image-2 2.png]]

```c
void f()
{
	int i;
	static int j = 0;
}
```

**For `int i`**:
- **Visibility:** Local variable inside function `f()`. 
- **Lifetime:** it's created when function is called and ceases when it exists
- **Allocation:** allocated in the stack

**For static int j;**
- **Visibility:** Local even thought its static
- **Lifetime:** static lifetime, program lifetime. Even if function `f()` is called multiple times, `j` will not be re-initialized
- **Allocation:** Allocated in the static/data segment. To **BSS** if its uninitialized, but here its initialized to 0 so its in the **data**

```c 
void f()
{
    int* p;
    p = malloc(N * sizeof(int));
    // ...
    free(p);
}
```

### Where the pointer “p” resides?
Local variables are typically stored on the **Stack**. Therefore, the pointer `p` itself resides on the **Stack**.
However, the memory that `p` _points to_ (allocated by `malloc(N * sizeof(int))`) resides on the **Heap**.
### What is the purpose of the “extern” keyword?
The `extern` keyword serves as a **promise to the compiler** that a definition for the declared entity (variable or function) will be provided by another compilation unit (source file) at link time.

**File1.c**
```c
int global_variable = 10;
```

**File2.c**
```c
extern int global_variable; // Declaration: "trust me, global_variable exists elsewhere" 

void some_function() { 
	// You can now use global_variable in File2.c 
	printf("%d\n", global_variable); // Will print 10
}
```

# Aula 4
## System Timer @ ESP32
- Contains **two 52-bit counters** and **three 52-bit comparators**
- **Registers accessed via APB_CLK**; **counting uses CNT_CLK** (≈16 MHz avg)
- **CNT_CLK** is derived from a **40 MHz XTAL_CLK**
- Supports:
    - **52-bit alarm values** (`t`)
    - **26-bit alarm periods** (`δt`)
- **Alarm modes**:
    - **Target mode**: one-time alarm at `t`
    - **Period mode**: recurring alarms every `δt`
- **Three comparators** generate **independent interrupts** based on `t` or `δt`
## Timer Group – ESP32 GPT (General Purpose Timers)
- **16-bit clock prescaler**: configurable from **2 to 65,536**
- **54-bit time-base counter**: supports **incrementing or decrementing**
- **Real-time counter readout** available
- **Start/stop control**: counter can be halted and resumed
- **Programmable alarms**
- **Auto-reload support**:
    - Reload on alarm
    - Manual reload via software
- **Level interrupt generation**
## Watchdog Timers – ESP32
- **Four-stage timer**, each with a **programmable timeout**
    - Each stage can be **independently configured and enabled/disabled**
- **Timeout actions**:
    - **MWDT**: interrupt, CPU reset, or core reset
    - **RWDT**: interrupt, CPU reset, core reset, or full **system reset**
- **32-bit expiry counter**
- **Write protection**: prevents accidental changes to MWDT/RWDT configuration
- **Flash boot protection**: restarts the system if **SPI flash boot** doesn't complete in time
## PWM Generators (LED Controllers) – ESP32
- **6 independent PWM channels**
- **4 independent timers** with **fractional clock division**
- **Automatic duty cycle fading**:
    - Smooth transitions without CPU involvement
    - **Interrupts** triggered on fade completion
- **Adjustable phase** of PWM output
- Supports **PWM output in low-power (light-sleep) mode**
- **Maximum resolution**: **14 bits**

# Aula 5
- **Polling**:  
    The processor **repeatedly checks** (polls) a peripheral to see if it’s ready for data transfer.
    - Simple, but **wastes CPU cycles**.
    - Best for **short or predictable tasks**.
- **Interrupts**:  
    The peripheral **notifies the CPU** when it’s ready via an **interrupt signal**.
    - CPU can perform other tasks meanwhile.
    - More **efficient** than polling.
- **Memory Copy (DMA / memcpy)**:  
    Data is **transferred directly** between memory and peripherals (e.g., using **DMA**) or via `memcpy` in software.
    - **Fast and efficient**, especially for **large data blocks**.
    - Frees the CPU from handling each byte.


### 1. **Polling**
**Usage:**
- Used when the processor repeatedly checks the status of a peripheral (e.g., checking if new data is available from UART).

**Programming:**
- Implemented using `while` or `if` statements that read status flags from registers.
```c
while (!(UART->STATUS & RX_READY)) {
    // Wait until data is ready
}
char data = UART->DATA;
```
**Pros:**
- Simple to implement
- Predictable behavior
**Cons:**
- CPU is **blocked** while waiting
- **Inefficient** for infrequent or slow events
- Wastes power and processing time

### 2. **Interrupts**
**Usage:**
- Peripheral **alerts the CPU** only when it needs attention (e.g., a character received via UART).
**Programming:**
- Set up interrupt service routines (ISRs) and enable interrupt sources in peripheral and processor.
```c
void UART_ISR() {
    char data = UART->DATA;
    // Handle received data
}
```
**Pros:**
- **Efficient use of CPU** (can perform other tasks while waiting)
- **Responsive** to asynchronous events
**Cons:**
- Requires more complex programming (e.g., ISR design)
- Improper ISR handling can lead to missed or delayed responses
- Can be hard to debug

### 3. **Direct Memory Access (DMA)**
**Usage:**
- Transfers data between memory and peripherals **without CPU involvement**, ideal for large or high-speed transfers (e.g., SPI or ADC to memory).
**Programming:**
- Set up source, destination, transfer size, and control parameters in DMA controller.
```c
DMA->SRC = (uint32_t)&ADC->RESULT;
DMA->DEST = (uint32_t)buffer;
DMA->SIZE = 256;
DMA->CTRL = START;
```
**Pros:**
- **Very efficient** for large data blocks
- **Frees CPU** for other tasks
- Reduces latency and improves throughput
**Cons:**
- **More complex** configuration
- Not all peripherals support DMA
- Debugging DMA issues can be difficult

|Method|CPU Involvement|Use Case|Pros|Cons|
|---|---|---|---|---|
|Polling|High|Simple, predictable tasks|Easy to implement|CPU wasted in waiting loop|
|Interrupt|Medium|Asynchronous events|Efficient, responsive|Requires careful ISR handling|
|DMA|Low|Large data transfers|High performance, low CPU load|Complex setup, limited support|
# Aula 6 & 7

## Autonomous Study Questions

1. What distinguishes serial and parallel interfaces in a computer system?
	**R:** In **Serial interface** data is transmitted one bit at a time over a single data line, in Parallel interfaces multiple bits are transmitted simultaneously, each on its own data line. Serial is simpler and more efficient over long distances, while parallel offers faster transfer over short distances.
2. What are the advantages of serial interfaces?
	**R:** Fewer wires, reducing cost and complexity. Lower electromagnetic interference, better suited for long-distance communication. Can support high data rates with proper protocols. 
**For RS232, SPI, and I2C serial interfaces (separately):**
3. What is the purpose / preferred use?
	**R:** 
	RS232:
		General purpose point-to-point communication
	SPI:
		Fast data transfer between microcontrollers and peripherals
	I2C:
		Communication with multiple low-speed peripherals
4. What signals make up the interface?
	**R:**
	RS232:
		TX (transmit)
		RX (Receive)
		RTS/CTS (flow control, optional)
	SPI:
		MOSI
		MISO
		SCLK
		SS/CS
	I2C:
		SDA
		SCL
		
5. How is it characterized regarding communication (bi)directionality (simplex, half-duplex, full-duplex)?
	**R:**
	RS232:
		Full-duplex
	SPI:
		Full-duplex
	I2C:
		Half-duplex
6. Is there symmetry between the communication ends, or is it a Master/Slave configuration?
	RS232:
		Symetrical, no master slave roles
	SPI:
		Master/Slave Config
	I2C:
		Master/Slave Config and can support multi master
7. What is the (typical) maximum transmission rate (bits per second)?
	RS232:
		1 Mbps
	SPI:
		50Mbps
	I2C:
		3.4Mbps
8. What voltage levels are used (for each logic level)?
	RS232:
		Logic 1: -3V to -15V
		Logic 0: 3V to 15V
	SPI:
		3.3V or 5V
	I2C:
		3.3V or 5V
9. How is a data frame organized? How are the bits arranged in a transmission?
	RS232:
	- **Start bit**
	- **Data bits (5–8)**
	- **Optional parity bit**
	- **Stop bit(s)**
	SPI:
	- Continuous bitstream (no start/stop bits)
	- Frame size varies (commonly 8, 16, or 32 bits)
	- MSB or LSB first (configurable)
	I2C:
	- **Start condition**
	- **7/10-bit address**
	- **Read/Write bit**
	- **ACK/NACK**
	- **Data bytes**
	- **Stop condition**
10. How is synchronization achieved in data transfer? What type of clock is used?
	RS232:
		Each end uses **its own internal clock**
	SPI:
		Synchronous, clock provided by master on SCLK
	I2C:
		**Asynchronous** (no clock line); uses start/stop bits
11. What configuration parameters need to be programmed?
	RS232:
		- Baud rate
		- Parity
		- Stop bits
		- Word length (data bits)
	SPI:
		- Clock polarity (CPOL)
		- Clock phase (CPHA)
		- Bit order (MSB/LSB)
		- Frame size
		- Clock frequency
	I2C:
		- Slave address
		- Speed mode (standard, fast, etc.)
		- Acknowledgment handling
## SPI (Serial Peripheral Interface)
- **Type**: Synchronous, bidirectional, full-duplex master–slave bus, point-to-point
	- Clock is generated by the master and makes it available for all the slaves
- **Wiring**: 4 lines (**MISO** (Master in Slave out), **MOSI** (Master out salve in), **SCLK**, **SS/CS** (Slave Select)) 
- **Speed**: Up to tens of Mbps (device-dependent)
- **Direction**: Full-duplex
- **Multiple devices**: Supported via multiple CS (chip select) lines
- **Use cases**: Fast communication with sensors, flash memory, displays
- **Pros**:
    - High-speed
    - Simple protocol, easy to implement
- **Cons**:
    - Needs more wires (per slave)
    - No standard protocol for device addressing

To debug on oscilloscope, place the trigger in the **descending flange** in the **slave select**.

## I²C (Inter-Integrated Circuit)
- **Type**: Synchronous, half-duplex multi-master bus
- **Wiring**: 2 lines (**SDA** for data, **SCL** for clock)
- **Speed**: Standard (100 kbps), Fast (400 kbps), Fast+ (1 Mbps), High-speed (3.4 Mbps)
- **Direction**: Half-duplex, bidirectional on shared data line
- **Multiple devices**: Addressed via 7 or 10-bit addresses
- **Use cases**: Low-speed peripherals like EEPROMs, RTCs, sensors
- **Pros**:
    - Only 2 wires regardless of number of devices
    - Supports multiple masters and slaves
- **Cons**:
    - Slower than SPI
    - More complex protocol and timing requirements

## RS232 (Recommended Standard 232)
- **Type**: Point-to-point, asynchronous serial communication
- **Wiring**: 2 lines minimum (TX, RX), optional RTS/CTS for hardware flow control
- **Speed**: Typically up to ~1 Mbps
- **Voltage levels**: ±12V (logic 1 = -12V, logic 0 = +12V)
- **Direction**: Full-duplex
- **Use cases**: Legacy devices, serial consoles, modems
- **Pros**:
    - Simple, mature, widely supported
- **Cons**:
    - Long cables and high voltage levels
    - Limited to **1-to-1 communication**

| Feature          | RS232          | SPI            | I²C                 |
| ---------------- | -------------- | -------------- | ------------------- |
| Bus Type         | Point-to-point | Master–Slave   | Multi-master/slave  |
| Wires            | 2–4            | 4 (min)        | 2                   |
| Speed            | ~1 Mbps        | Up to 50+ Mbps | Up to 3.4 Mbps      |
| Duplex           | Full           | Full           | Half                |
| Multiple Devices | No             | Yes (via CS)   | Yes (via addresses) |
| Complexity       | Low            | Low            | Medium              |


# Aula 8

## Questions

• What is an ADC?
**R:** Analog to Digital Converter, converts a **continuous analog signal** into a **discrete digital value**.
• What is a DAC?
**R:** Digital to Analog Converter, performs the inverse operation of an ADC.
• What is a continuous signal?
**R:** A signal with a continuous range of amplitudes, where its samples have an infinite number of amplitude levels. 
• What is a discrete signal?
**R:** A signal with values defined at specific time intervals, where it's a finite value. (1 or 0 clock)
• What is the sampling process in analog-to-digital conversion?
**R:** Sampling process is the process of measuring the amplitude of a continuous signal at **regular time intervals**.
• What is the quantization process in analog-to-digital conversion?
**R:** Quantization maps each sampled analog value to the nearest level within a finite set of discrete values. (**bit resolution**)
• What is the reconstruction process in digital-to-analog conversion?
**R:** Reconstruction converts discrete digital samples back into a **continuous analog waveform**, using **zero-order hold** or a filter. 
• What is the sampling rate of ADCs/DACs?
**R:** The sampling rate is how many samples per second the converter can process, usually in Hz or MS/s
• What is the bit resolution of ADCs/DACs?
**R:** **Bit Resolution** defines the **number of levels** the signal can be quantized into, an **n-bit ADC** can represent $2^n$ levels. 
• What is the Nyquist frequency? What is Aliasing?
**R:** **Nyquist frequency** is always half the sampling rate. **Aliasing** occurs when the signal contains frequencies **above the Nyquist limit**, causing them to be **misinterpreted** as lower frequencies in the digital domain. Using antialiasing filters before the ADC will remove the high-frequency components.
• What is the reference voltage of ADCs and DACs?
**R:** **Reference Voltage** defines the input range of an ADC or output range of a DAC. For ADC, analog input is mapped to digital values between 0 and V<sub>ref</sub> (voltage of the ADC). For DAC, digital input is converted to analog voltage between 0 and V<sub>ref</sub>.
Example: A 3.3V V<sub>ref</sub> with 10-bit ADC gives ≈ **3.22 mV per step**.
• How does a Successive Approximation Register (SAR) ADC work?
**R:** **SAR ADC** converts an analog signal to digital by **binary search**:
1. Compares input voltage with a **DAC-generated reference**.
2. Starts from the **most significant bit (MSB)**.
3. Sets each bit based on comparison, updating DAC value at each step.
4. After **n steps**, the n-bit digital value is obtained.

## ESP32-C3 Analog-to-Digital Converters (ADCs)

The ESP32-C3 microcontroller incorporates Analog-to-Digital Converters (ADCs) to convert continuous analog signals into discrete digital values
- **Number of ADCs and Channels:** The ESP32-C3 implements **multiple ADCs** and provides **several ADC input channels**
- **Operating Modes:** ESP32 ADCs support two main modes of operation
	- **One-shot mode:** This mode generates a **one-time alarm** based on a configured alarm value (t). This is used for single acquisitions, such as reading a potentiometer value or an analog temperature sensor. 
	- **Continuous mode:** This mode generates **periodic alarms** based on an alarm period (δt). It is suitable for acquiring continuous signals, like a sinusoidal signal, and sending "chunks" of samples for plotting.
- **Voltage Determination:** To determine the **voltage applied to the ADC from the digital sample value**, it's crucial to understand the reference voltage (Vref) of the ADC. For an **n-bit ADC**, the analog input is mapped to digital values between 0 and Vref. For example, a 3.3V Vref with a 10-bit ADC provides approximately **3.22 mV per step**.

## ESP32-C3 Digital-to-Analog Converter (DAC) Functionality

**The ESP32-C3 does not provide native DACs**. However, similar functionality can be implemented using other peripherals:
- **GPIOs with Delta-Sigma Modulator:** One method to achieve DAC-like functionality is by utilizing General Purpose Input/Output (GPIO) pins in conjunction with a Delta-Sigma Modulator.
- **PWM Generators:** A common and effective way to simulate DAC functionality on the ESP32-C3 is by using its **Pulse Width Modulation (PWM) generators**.
	- The ESP32 has **six independent PWM generators** (channels) and **four independent timers** that support fractional clock division
	- These PWM generators allow for **automatic duty cycle fading**, enabling smooth transitions without direct CPU involvement, and can trigger interrupts upon fade completion
	- The **adjustable phase of the PWM signal** output and support for **PWM output in low-power** **(Light-sleep) mode** are also notable features
	- The **maximum PWM** resolution is 14 bits

# Aula 9 & 10

## Questions

• What are real-time systems? Are “real-time” and “fast” synonymous?
**R:** A **real-time system** is one in which the **correctness of operations depends not only on logical results** but also on **timing constraints**. These systems must **guarantee a response within specific time bounds** to external stimuli or events.

• What is a deadline, and a deadline miss, in real-time systems?
**R:** 
- A **deadline** is the maximum time allowed for a task or system to complete a specific operation.
- A **deadline miss** occurs when a system fails to complete the task within that timeframe.

• What is the difference between hard and soft real-time systems?
**R:** 
- A **hard real-time system** is one where **missing a deadline is considered a system failure**. The timing constraints must always be met, without exception.
- A **soft real-time system** allows for **occasional deadline misses**. The system can tolerate some delays without failing, though performance may degrade

• How severe is missing a deadline in a hard real-time system? And in a soft real-time system?
**R:** 
- In a **hard real-time system**, missing a deadline can have **catastrophic consequences**, such as system failure or safety risks.
- In a **soft real-time system**, missing a deadline typically results in **reduced performance or degraded quality of service**, but the system continues to function.

• What is a Real-time Operating System (RTOS)?
**R:** An **RTOS** is an operating system specifically designed to run **tasks within predictable timing constraints**. It provides mechanisms for **scheduling**, **synchronization**, **inter-task communication**, and **resource sharing**, all optimized for time determinism.

• What distinguishes a RTOS from a general-purpose operating system?
**R:** A **Real-Time Operating System (RTOS)** differs from a **General-Purpose Operating System (GPOS)** in terms of **predictability**, **timing guarantees**, and **system behavior**.
1. **Determinism**
    - An RTOS provides **predictable and bounded response times** to external events.
    - A GPOS focuses on maximizing average throughput or resource usage, but without guaranteeing how long a task will take to start or finish.
2. **Task Scheduling**
    - RTOS uses **priority-based preemptive scheduling**, ensuring high-priority tasks execute as soon as they're ready.
    - GPOS typically uses **fair-share scheduling** (e.g., round-robin, multi-level queues) to balance responsiveness across users and tasks.
3. **Latency and Jitter**
    - RTOS is optimized for **low latency** and **minimal jitter** (timing variation).
    - GPOS may have high or unpredictable latency, due to background services, user processes, and I/O buffering.
4. **Resource Usage**
    - RTOS is **lightweight**, designed to run on **resource-constrained systems** (e.g., microcontrollers).
    - GPOS (like Linux or Windows) requires **more memory and processing power**, and is often unsuitable for real-time constraints.
5. **Interrupt Handling**
    - In RTOS, **interrupts are handled quickly** and can often trigger real-time tasks directly.
    - In GPOS, interrupt response is delayed by scheduling policies and system overhead.
6. **Use Case Orientation**
    - RTOS is built for **embedded systems**, **safety-critical**, or **time-sensitive applications** (e.g., robotics, automotive, avionics).
    - GPOS is built for **general computing** needs (e.g., desktop applications, file systems, multitasking environments).

• Which services and abstractions are typically provided by a RTOS?
**R:**
- **Task/thread creation and management**
- **Preemptive scheduler**
- **Task priorities**
- **Timers and tick control**
- **Semaphores and mutexes**
- **Queues/message passing**
- **Event flags**
- **Memory management**

• What are the pros and cons of an RTOS-based system (compared to a standalone/bare-metal system)?
**R:**
#### Pros:
- Deterministic behavior
- Efficient multitasking
- Clean separation of tasks
- Built-in synchronization and communication mechanisms
####  Cons:
- More complex than bare-metal
- Requires more memory and CPU
- Can introduce bugs like race conditions or deadlocks if misused

• Name (list) a few examples of RTOSs (free, academic, commercial).
**R:** 
- **Free (Open-source):**
    - FreeRTOS
    - Zephyr RTOS
    - RIOT OS
- **Academic:**
    - ChibiOS
    - Erika Enterprise
- **Commercial:**
    - VxWorks
    - ThreadX (Azure RTOS)
    - QNX

• What are RTOS tasks?
**R:** **Tasks** (or threads) are independent sequences of execution managed by the RTOS. Each task typically performs one logical function of the system.

• What is the role of RTOS scheduler?
**R:** The **scheduler** decides which task to run next based on:
- Task **priority**
- Task **state** (ready, blocked, etc.)
- Scheduling policy (preemptive, round-robin, etc.)

• What are the typical task states in a RTOS?
**R:** 
- **Ready:** Can run when given CPU time
- **Running:** Currently executing
- **Blocked:** Waiting for an event (e.g., delay, semaphore)
- **Suspended:** Inactive until resumed

• What is the purpose of task priorities?
**R:** Task **priorities** allow more critical tasks to **preempt** lower-priority ones. Ensures that **time-sensitive operations** are handled first.

• What is the RTOS tick?
**R:** The **tick** is a periodic interrupt (e.g., every 1 ms) that drives the RTOS timekeeping. It's used for:
- Task delays (e.g., `vTaskDelay()`)
- Timeouts
- Scheduling decisions

• What are RTOS timers?
**R:** **RTOS timers** allow scheduling actions in the future without blocking tasks. Types:
- **One-shot:** Trigger once after delay
- **Periodic:** Trigger repeatedly at fixed intervals
Useful for:
- Sensor sampling
- LED blinking
- Timeout handling

• What is a shared variable?
**R:** A **shared variable** is a memory location accessed by multiple tasks or ISRs. Example: a shared counter, or sensor data buffer.

• Why is mutual exclusion of access to shared variables important?
**R:** Without **mutual exclusion**, simultaneous access to shared variables can cause **race conditions**, leading to corrupted or inconsistent data.

• What are critical sections (in the code)?
**R:** **Critical sections** are code regions that access shared resources and **must not be interrupted** or accessed by multiple tasks simultaneously. Protected using:
- **Disabling interrupts**
- **Semaphores or mutexes**

• What are queues?
**R:** **Queues** allow tasks to safely exchange data. Data is sent (enqueued) by one task and received (dequeued) by another, ensuring **safe communication** between tasks or from ISRs.

• What are semaphores and mutexes?
**R:** 
#### **Semaphores**
- Used for **synchronization** (e.g., signaling between tasks or from ISR to task)
- Types:
    - **Binary semaphore:** Acts like a flag
    - **Counting semaphore:** Can track multiple events
#### **Mutexes**
- Used for **mutual exclusion**
- Only one task can hold the mutex at a time
- Prevents race conditions
- Often supports **priority inheritance** to avoid **priority inversion**

## FreeRTOS functions 
### **xTaskCreate(...)**
Creates a new task and schedules it for execution.
```c
BaseType_t xTaskCreate(
    TaskFunction_t pvTaskCode,    // Task function (void(void*))
    const char * const pcName,    // Task name (for debugging)
    uint16_t usStackDepth,        // Stack size in words
    void *pvParameters,           // Parameter passed to the task
    UBaseType_t uxPriority,       // Task priority
    TaskHandle_t *pxCreatedTask   // (Optional) Task handle
);
```
- **allocates** a Task Control Block and stack from the heap
- Use `xTaskCreateStatic()` to avoid dynamic allocation
### **vTaskDelay(...)**
Delays the calling task for a specified number of tick periods.
```c
void vTaskDelay(TickType_t xTicksToDelay);
```
- The task is **blocked** for `xTicksToDelay` ticks
- Use `pdMS_TO_TICKS(ms)` to convert milliseconds to ticks

### **xTaskDelayUntil(...)**
Provides periodic task execution based on a fixed time interval.
```c
BaseType_t xTaskDelayUntil(
    TickType_t *pxPreviousWakeTime,
    TickType_t xTimeIncrement
);
```
- Ensures **consistent periodicity**, compensating for previous delays

### **xTimerCreate(...)**
Creates a software timer.
```c
TimerHandle_t xTimerCreate(
    const char * const pcTimerName,
    TickType_t xTimerPeriod,
    UBaseType_t uxAutoReload,
    void *pvTimerID,
    TimerCallbackFunction_t pxCallbackFunction
);
```
- Allocates memory for timer and control structure

### **xTimerStart(...)**
Starts or restarts a software timer.
```c
BaseType_t xTimerStart(
    TimerHandle_t xTimer,
    TickType_t xTicksToWait
);
```
- Must be called before the timer can run

### **xQueueCreate(...)**
Creates a queue to hold a fixed number of items of fixed size.
```c
QueueHandle_t xQueueCreate(
    UBaseType_t uxQueueLength,
    UBaseType_t uxItemSize
);
```
- Two memory blocks are allocated: control structure and storage area

### **xQueueSend(...)**
Sends (copies) an item to the **back** of the queue.
```c
BaseType_t xQueueSend(
    QueueHandle_t xQueue,
    const void *pvItemToQueue,
    TickType_t xTicksToWait
);
```
- Blocks for space if the queue is full
- Use `xQueueSendFromISR()` inside ISRs

### **xQueueReceive(...)**
Receives (copies) an item from the **front** of the queue.
```c
BaseType_t xQueueReceive(
    QueueHandle_t xQueue,
    void *pvBuffer,
    TickType_t xTicksToWait
);
```
- Removes the item from the queue
- Use `xQueueReceiveFromISR()` in ISRs

### **vSemaphoreCreateBinary(...)**
Creates a binary semaphore.
```c
SemaphoreHandle_t xSemaphoreCreateBinary(void);
```
- Allocates memory automatically
- Useful for simple signaling between tasks or from ISRs
### **xSemaphoreTake(...)**
Attempts to acquire the semaphore.
```c
BaseType_t xSemaphoreTake(
    SemaphoreHandle_t xSemaphore,
    TickType_t xTicksToWait
);
```
- Can block the calling task until the semaphore becomes available

### **xSemaphoreGive(...)**
Releases the semaphore.
```c
BaseType_t xSemaphoreGive(SemaphoreHandle_t xSemaphore);
```
- Typically unblocks a task waiting on the semaphore


| Function                 | Purpose                        |
| ------------------------ | ------------------------------ |
| `xTaskCreate`            | Create and schedule a new task |
| `vTaskDelay`             | Delay a task by ticks          |
| `xTaskDelayUntil`        | Periodic task delay            |
| `xTimerCreate`           | Create a software timer        |
| `xTimerStart`            | Start/restart the timer        |
| `xQueueCreate`           | Create a message queue         |
| `xQueueSend`             | Send an item to a queue        |
| `xQueueReceive`          | Receive an item from a queue   |
| `vSemaphoreCreateBinary` | Create binary semaphore        |
| `xSemaphoreTake`         | Acquire semaphore              |
| `xSemaphoreGive`         | Release semaphore              |

## ## Basics of Real-Time Systems
### What is a Real-Time System?
A **real-time system** is a system that must respond to inputs or events **within a guaranteed time limit**. The correctness depends not only on **logical results**, but also on **timing**.
- **Hard real-time**: Missing a deadline leads to system failure (e.g., medical devices, automotive braking).
- **Soft real-time**: Occasional missed deadlines are tolerable (e.g., audio streaming, robotics).

## RTOS: Real-Time Operating System
An **RTOS** is an operating system designed to manage tasks with **strict timing constraints** and **predictable scheduling**.
### Main Features
- **Preemptive multitasking** with priority-based scheduling
- **Deterministic behavior** (predictable task switching)
- **Task management** (create, delete, suspend, resume tasks)
- **Inter-task communication** (queues, semaphores, mutexes)
- **Timers and delays**
- **ISR (interrupt service routine) integration**
### Advantages
- Precise timing and responsiveness
- Clean and modular application structure
- Scalability: from small MCUs to complex applications
- Easier debugging and maintenance with task isolation
### Limitations
- More **RAM/ROM usage** than bare-metal code
- Steeper learning curve than simple loops or interrupts
- Requires careful design to avoid **deadlocks**, **priority inversion**, or **race conditions**

## FreeRTOS: Capabilities and Programming
**FreeRTOS** is a widely used, open-source RTOS designed for **microcontrollers and small embedded systems**. It is lightweight, portable, and supports many MCU architectures.
### Capabilities
- **Multitasking**: tasks with priorities, blocking, and cooperative scheduling
- **Queues & Semaphores**: for synchronization and communication
- **Software timers**: schedule future actions
- **Tickless idle**: power-saving mode
- **ISR-safe functions** for real-time interaction
- Modular and portable (used in STM32, ESP32, ARM Cortex, etc.)
### Typical FreeRTOS Programming Flow
1. **Create tasks** using `xTaskCreate()`
2. Use **queues**, **semaphores**, or **mutexes** to share data
3. Implement delays with `vTaskDelay()` or `xTaskDelayUntil()`
4. Handle external events via **interrupts**
5. Start the kernel with `vTaskStartScheduler()`

## Using FreeRTOS in Sensor-Based and Interactive Systems
FreeRTOS is ideal for systems that need to **read sensors**, **respond to user input**, and **manage multiple operations in parallel**. Here's how it's typically applied:
### Sensors
- A **dedicated task** periodically reads sensors (e.g., temperature, motion)
- Task uses `vTaskDelay()` or `xTaskDelayUntil()` for consistent sampling
- Data is sent to other tasks via **queues**
### User Interaction
- Another task handles **input (e.g., buttons, touchscreen)** or **outputs (e.g., displays)**
- Interrupts (e.g., button press) can **give semaphores** to wake tasks immediately
- Tasks manage user feedback (e.g., updating LEDs, displays)
### Example System
- **Task 1:** Reads temperature sensor every 500ms
- **Task 2:** Monitors a push-button and sends commands via queue
- **Task 3:** Updates an LCD display with the latest reading
- **Task 4:** Communicates via UART/WiFi (e.g., ESP32 + FreeRTOS)

Each task operates independently, and communication between tasks is **safe, non-blocking, and prioritized**.


# Aula 11

## Questions

• Why is power management critical in modern computing and embedded systems?
• How is the power consumption affected by the supply voltage and operating frequency of the system?
• Which power modes are predefined in ESP32 and the corresponding features?
• How is a reduced power management mode entered in ESP32?
• What are the typical wake-up sources in ESP32?

## Microcontroller Power Management Basics
### Why Power Management?
Power management is crucial in embedded systems, especially for **battery-powered** or **energy-constrained** devices. Efficient power usage extends battery life, reduces heat, and enables always-on features with minimal drain.
Common techniques include:
- **Dynamic frequency scaling**
- **Peripheral and clock gating**
- **Sleep/standby modes**
- **Disabling unused modules (ADC, UART, etc.)**
- **Using interrupts instead of polling**

## ESP32 Power Modes
The **ESP32** is a dual-core MCU with integrated Wi-Fi and Bluetooth, and it offers several power modes to balance performance and energy efficiency. These modes allow the chip to **reduce consumption drastically** when full processing power is not needed.
### 1. **Active Mode**
- CPU(s) fully running
- Wi-Fi/Bluetooth on or off depending on task
- Power usage: ~160–260 mA (depending on use)
### 2. **Modem Sleep**
- CPU active
- Wi-Fi and Bluetooth are off or in standby
- Used during tasks that don’t require network access
**Power consumption:** ~3–20 mA  
**Ideal for:** periodic Wi-Fi transmission with idle computation in between
### 3. **Light Sleep**
- CPU paused
- Most peripherals paused
- RTC controller and memory remain powered
- Wake-up via timer, GPIO, touch sensor, or ULP (Ultra Low Power) core
**Power consumption:** ~0.8–2 mA  
**Use case:** periodic tasks with fast wake-up and low latency requirements
### 4. **Deep Sleep**
- CPU and RAM are powered off
- RTC memory and ULP core are active
- Only RTC peripherals (e.g., timer, GPIO, touch) can trigger wake-up
- Boot from scratch after waking
**Power consumption:** ~10–150 µA  
**Ideal for:** sensors that report data every few minutes/hours
Wake-up sources:
- Timer
- GPIO (external interrupts)
- ULP
- Capacitive touch
### 5. **Hibernation**
- Everything except RTC timer is powered off
- Only timer wake-up possible
- Longest battery life, but full reboot on wake
**Power consumption:** ~2–8 µA  
**Best for:** ultra-low-power long-sleep applications (e.g., environmental monitoring)

# Aula 12

## Questions

• How are WiFi networks organized? What is the “anatomy” of a WiFi network?
• What are the frequency bands used in WiFi?
• What are the capabilities of each WiFi version?
• What are the purposes, advantages and constraints of ISM bands?
• What are the protocols/services/applications that share the RF spectrum bands with WiFi?
• Is WiFi adequate for safety-critical applications?
• What WiFi version is supported by ESP32-C3?
• Which WiFi networking APIs are provided with ESP/IDF?
• What are the restrictions on using WiFi and BLE simultaneously in an ESP32- C3 SoC?

## Wi-Fi-Based Concepts (Revisited)
### What is Wi-Fi?
**Wi-Fi** is a wireless communication standard based on **IEEE 802.11**, commonly used for local area networking (WLAN). It enables devices to exchange data over radio waves, typically in the **2.4 GHz or 5 GHz bands**.
### Key Concepts:
- **SSID (Service Set Identifier):** The name of the wireless network.
- **BSSID:** The MAC address of the Access Point (AP).
- **Modes of Operation:**
    - **Station (STA):** Connects to an Access Point.
    - **Access Point (AP):** Creates its own Wi-Fi network.
    - **SoftAP + STA:** ESP acts as both an access point and station.
- **Security Protocols:** WPA2, WPA3, WEP
- **IP Assignment:**
    - **Static IP**
    - **DHCP (Dynamic Host Configuration Protocol)**
### Typical Wi-Fi Use in Embedded Devices:
- Connecting to the internet to send sensor data
- Hosting a local web interface for configuration
- Receiving remote commands from a mobile app or server
## Wi-Fi Capabilities of ESP32-C3

The **ESP32-C3** is a low-power, low-cost Wi-Fi & Bluetooth LE microcontroller from Espressif, based on a **single-core RISC-V processor**. It is optimized for secure IoT and supports **Wi-Fi in the 2.4 GHz band**.
### Core Wi-Fi Features:
- **IEEE 802.11 b/g/n support (2.4 GHz)**
- Up to **150 Mbps** PHY throughput
- Supports both **Station (STA)** and **Soft Access Point (SoftAP)** modes
- **Simultaneous SoftAP + STA mode** supported (ideal for device provisioning)
- **Wi-Fi Protected Access (WPA/WPA2/WPA3-Personal)** security support
- **TCP/IP stack** fully integrated (via LwIP)
- **IPv4 and IPv6** support
- **DHCP client and server**
- **mDNS**, **HTTP/HTTPS**, **MQTT**, **WebSocket**, etc., supported via software libraries
### Additional Features:
- **Power-Saving Modes:**
    - **Modem sleep** for low-power networking
    - Wake-up on Wi-Fi events (e.g., beacon receive)
- **Wi-Fi provisioning via Bluetooth LE** also supported
- **TLS/SSL support** for secure communication (via mbedTLS)
- Fully compatible with **ESP-IDF** and **Arduino** environments
### Use Cases with ESP32-C3:
- IoT sensor nodes connecting to the cloud
- Smart home devices with a web interface
- Wi-Fi-controlled actuators (e.g., smart relays)
- Devices acting as a Wi-Fi access point for configuration or communication with nearby clients (e.g., phones)