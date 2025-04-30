# PJON Project Self-Report

## Overview

PJON (Protocol for JSON Over Network) is an open-source communication framework designed to connect up to 255 Arduino boards over a single or dual-wire connection, capable of data transmission speeds ranging from 1.81 kB/s to 48.000 kBd.

This report outlines the development, deployment, and usage instructions for PJON, focusing on its core functionalities and integration into various projects.

## Features

- **Multi-Master Communication**: PJON supports multiple devices communicating over a single bus.
- **High Speed Data Transfer**: Capable of transferring data at speeds up to 48.000 kBd using software bit-banging.
- **Synchronous Acknowledgment**: Supports both synchronous and asynchronous communication modes, enhancing reliability.
- **Flexible Error Handling**: Includes robust error handling mechanisms to manage connection losses and buffer overflows.

## Deployment

### Hardware Requirements
- Arduino boards (Uno, Mega, ESP8266, ESP32, etc.)
- Breadboard and jumper wires for connecting the devices.
- Power supply for multiple Arduino boards if needed.

### Software Requirements
- Arduino IDE: Version 1.8.x or later.
- PJON Library: Download from GitHub repository or install via the Arduino Library Manager.

## Installation

1. **Install PJON Library**:
   - Open your Arduino IDE.
   - Go to `Sketch` > `Include Library` > `Manage Libraries...`.
   - Search for "PJON" in the library manager and install it.

2. **Set Up Hardware Connections**:
   - Connect multiple Arduino boards using a single wire for data transfer or dual wires for higher speeds.
   - Ensure correct pin configuration as per PJON documentation.

## Basic Usage

### Example Code

Below is a simple example demonstrating how to use PJON in a basic communication scenario:

```cpp
#include <PJON.h>

PJON<SoftwareBitBang> bus(0); // Instantiate a PJON instance with device ID 0
void setup() {
  bus.set_pin(13); // Set the pin connected to TX
  bus.begin();    // Initialize PJON bus communication
}

void loop() {
  uint8_t packet[] = {'H', 'e', 'l', 'l', 'o'};
  bus.send(255, packet, sizeof(packet)); // Send a packet to all devices on the bus
}
```

### Explanation

1. **Include PJON Library**:
   - `#include <PJON.h>` includes the PJON library in your project.

2. **Initialize PJON Instance**:
   - `PJON<SoftwareBitBang> bus(0);` creates a new PJON instance with device ID 0 using software bit-banging for communication.

3. **Set Pin Configuration**:
   - `bus.set_pin(13);` configures the pin connected to TX (transmit) line of the Arduino.

4. **Initialize Communication**:
   - `bus.begin();` initializes the PJON bus communication.

5. **Send Data**:
   - `bus.send(255, packet, sizeof(packet));` sends a packet containing "Hello" to all devices on the bus (ID 255 represents broadcast).

## Deployment

### Configuration of Multiple Devices

1. **Assign Unique IDs**: Assign unique device IDs from 0 to 254 to each Arduino board.
   ```cpp
   PJON<SoftwareBitBang> bus(1); // For a device with ID 1
   ```

2. **Connect Devices**: Connect the devices using a single or dual-wire setup as per hardware requirements.

### Handling Multiple Packets

- **Buffer Management**: PJON automatically manages packet buffers, ensuring that no more than `MAX_PACKETS` packets are stored at any time.
- **Error Handling**: Implement error handling functions to manage connection losses and buffer overflows.

## Advanced Usage

### Custom Headers

Custom headers can be used to include additional information in the packet header:
```cpp
uint8_t custom_header = (SIMPLEX | SENDER_INFO_BIT);
bus.send(255, packet, sizeof(packet), custom_header);
```

### Repeated Transmission

- **Repeated Sending**: PJON allows repeated sending of packets at specified intervals using `send_repeatedly`.

## Conclusion

PJON is a versatile and powerful communication framework that simplifies multi-device communication in Arduino projects. By following the deployment guide and utilizing the provided example code, you can effectively implement PJON in your projects for reliable data transfer.

For more advanced features and customizations, refer to the PJON documentation available on GitHub.