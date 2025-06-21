
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
![[image-1.png]]

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


![[image-2.png]]

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
## Timer Group – ESP32 (General Purpose Timers)
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
# Aula 6

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
		
	I2C:
		
8. What voltage levels are used (for each logic level)?
	RS232:
		
	SPI:
		
	I2C:
		
9. How is a data frame organized? How are the bits arranged in a transmission?
	RS232:
		
	SPI:
		
	I2C:
		
10. How is synchronization achieved in data transfer? What type of clock is used?
	RS232:
		
	SPI:
		
	I2C:
		
11. What configuration parameters need to be programmed?
	RS232:
		
	SPI:
		
	I2C:
		
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


















