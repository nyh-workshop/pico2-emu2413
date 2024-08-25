# Raspberry Pico 2 EMU2413

This is the YM2413 emulator by Mitsutaka Okazaki and Rupert Carmichael (https://github.com/digital-sound-antiques/emu2413) ported to the Raspberry Pico RP2350.

The program opens up the Miditones song file in the memory and play it with the EMU2413. 

## Requirements:
- Raspberry Pico SDK (that supports RP2350, SDK 2.0.0)
- Pico-Extras (https://github.com/raspberrypi/pico-extras)
- I2S DAC (PCM5102)
- UART0 at GP0 and GP1 of the Raspberry Pico
- EMU2413 version 1.5.7
- **Binary is compiled with ARM Cortex-M33!** (RISC-V Hazard3 not tested yet!)

## Limitations:
- Some of the EMU2413 code has been changed to fit into that embedded system. Means that there are no memory allocations and a number of the tables (especially the large ones like tll_table) are precalculated with constexpr.
- The RP2350 has a double-precision coprocessor (DCP) other than having an FPU - the calculations with doubles are utilizing this one. However, there are still many overheads, and a speed boost to 270MHz is only way to mitigate them.
- Some instruments such as "Synthesizer" and "Violin" can stutter at the note end.
- The percussions are not tested yet.
- There are clicking noises between note switches - this is mitigated by using an older version of the Miditones (v1.12) where there are note stops before the note change happens. This note stops allow the brief release of the note in the envelope generator and significantly minimizes the unpleasent noise.
- For best performance with minimal stuttering: ***use frequency of 270MHz, optimization O3 and buffer size 1024***. 

## Extra info:
- New wrapper functions for Pico Extras Audio I2S for easy integration (refer to picoI2sAudio.h)
- New wrapper functions for Miditones v1.12 (refer to the "static compilation virtual" of Miditones.h)
- These audio_i2s and Miditones folder libraries are now decoupled and can be used in other projects
- The Pico Extras Audio I2S driver, Miditones and the EMU2413 are combined together for ease of use (refer to emu2413_picoI2sAudioDriver.h)