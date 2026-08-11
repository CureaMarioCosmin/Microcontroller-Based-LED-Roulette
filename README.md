# Microcontroller-Based LED Roulette — Hardware & Firmware Design (STM32)

 * [![Project Showcase](https://img.shields.io/badge/Project_Showcase-blue?style=for-the-badge)](https://cureamariocosmin.github.io/Microcontroller-Based-LED-Roulette/)
 * Complete end-to-end implementation of an electronic roulette game using an STM32 microcontroller and shift registers.

> **Tools:** STM32CubeMX · HAL Library · UART | **Author:** Mario-Cosmin Curea

---

## Overview

This project focuses on designing and programming an electronic roulette game implemented on an STM32F103CBTx microcontroller. The work covers both hardware assembly and firmware development: utilizing 74HC595 shift registers to control 24 LEDs with minimal pins, handling user inputs via tactile buttons, generating pseudo-random outcomes using an ADC, and providing serial communication for debugging and game status updates.

## Design Specifications

* **Microcontroller:** STM32F103CBTx operating at a 72 MHz system clock.
* **Display:** 24 LEDs (12 green, 12 yellow) driven by three cascaded 74HC595 shift registers.
* **User Interface:** 3 normally open tactile buttons (6x6x6 mm) configured for Select, Confirm, and Start actions.
* **Communication:** USB-TTL converter (CH340 / CP2102) for UART programming and debugging at 9600 bps.
* **Timing:** Timer2 configured to generate interrupts every 10 ms.
* **Randomness:** Pseudo-random seed generation utilizing the microcontroller's internal 10-bit ADC.

## Circuit Architecture

* **Display Network:** The shift registers receive serial data through GPIO pins PA1 (DS), PA2 (SHCP), and PA3 (STCP). Each register outputs to 8 LEDs through 220 Ω resistors.
* **Input Controls:** The three tactile buttons are connected to GPIO input pins PA4, PA5, and PA6, utilizing internal pull-up resistors.
* **Power Supply:** The logic and logic components are powered via a 3.3V supply regulated by an LM11175.
* **Memory Footprint:** The compiled firmware (Test1.elf) utilizes 26.8 KB of Flash memory and 7.1 KB of RAM.

## What I Did

* Generated the initial hardware configuration code (GPIO, TIM2, UART1, RCC) using STM32CubeMX.
* Developed the main application logic structured around a state machine (`SM_State`) using the STM32 HAL libraries.
* Implemented the `selectBet()` function to allow users to navigate and select a position from 0 to 23, updating the LED display in real-time via `shiftOut()` and `displayLED()` routines.
* Built the `rouletteGame(uint8_t bet)` function to simulate a spinning wheel with progressively slowing LED sequences, outputting win/loss messages via UART.
* Managed asynchronous events using hardware interrupts, including `HAL_UART_RxCpltCallback()` for serial commands and `HAL_TIM_PeriodElapsedCallback()` for RGB LED pulsing.
* Engineered a software debounce function to filter signal fluctuations from the physical buttons.

## Key Results

* Successfully controlled an array of 24 LEDs using only 3 digital pins by efficiently cascading the 74HC595 shift registers.
* Achieved stable and reliable user input by implementing a robust software debounce mechanism.
* Ensured unpredictable roulette outcomes by extracting a pseudo-random seed from the 10-bit ADC modulo 41.
* Delivered a fully functional, interactive real-time game integrated across multiple MCU peripherals (GPIO, UART, TIM).


