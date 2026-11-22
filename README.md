# Bluetooth Home Automation System

Wireless control of four appliances from an Android phone using an **STM32F407VGT6**, an **HC-05** Bluetooth module and a **4-channel relay module**.

![System architecture](docs/system_architecture.png)

## Features

- Bluetooth Classic (SPP) link between an Android phone and the STM32 through an HC-05 module
- Interrupt-driven UART reception (`HAL_UART_Receive_IT`) on USART2, 9600 baud, 8N1
- Single-character command parser that switches four relays independently
- "All OFF" command and "INVALID COMMAND" handling
- Status message sent back to the phone for every command

## Hardware

| Part | Details |
|---|---|
| MCU | STM32F407VGT6, internal HSI clock (16 MHz, no PLL) |
| Bluetooth | HC-05 on USART2 (HC-05 TX to MCU RX, HC-05 RX to MCU TX) |
| Relays | 4-channel relay module, inputs on PB0, PB1, PB2, PB10 |
| Power | 5 V for the relay module and HC-05, common ground with the STM32 |
| App | Serial Bluetooth Terminal (Android), Bluetooth Classic tab |

## Commands

| Send | Action | Pin |
|---|---|---|
| `1` / `a` | Relay 1 ON / OFF | PB0 |
| `2` / `b` | Relay 2 ON / OFF | PB1 |
| `3` / `c` | Relay 3 ON / OFF | PB2 |
| `4` / `d` | Relay 4 ON / OFF | PB10 |
| `0` | All relays OFF | all four |
| other | Replies `INVALID COMMAND` | none |

## Getting started

1. Open the project in **STM32CubeIDE** (File > Open Projects from File System).
2. Check in the `.ioc` file that **USART2 global interrupt** is enabled in NVIC.
3. Build and flash with an ST-LINK.
4. Pair the phone with the HC-05 (PIN usually `1234` or `0000`).
5. Open Serial Bluetooth Terminal, choose the **Bluetooth Classic** tab, connect to HC-05, and set the newline option to **None**.
6. Send a command from the table above. The board replies with a status message.

If your relay module is active-low, swap `RELAY_ON` and `RELAY_OFF` in `Core/Src/main.c`.

## Project structure

```
Core/            Application source (main.c with the command parser)
Drivers/         STM32 HAL and CMSIS drivers
docs/            Project report and diagrams
*.ioc            STM32CubeMX configuration
```

## Documentation

The full project report (build process, wiring, code explanation, testing and troubleshooting) is in [`docs/`](docs/).

## Safety

Test with LEDs or low-voltage loads first. Only wire mains loads if you are competent to do so, and keep the mains side insulated and separated from the logic side.

## Future improvements

- Password or key before accepting commands
- Restore relay states after a reset
- Framed command protocol
- Custom Android app
- BLE version with an HM-10 module and a GATT service

## License

Add a license of your choice (for example MIT) as a `LICENSE` file.
