# One-Step-Writer
This repository contains the official source code pertaining to the GPIO writer optimizations for one-step.

## Installation:
> $ sudo apt install ./wiringpi_3.14_arm64.deb

## Compilation:
The following code can be compiled into a shared library by using the following command in a Linux terminal.
> $ gcc -fPIC -shared -o one-step-writer.so one-step-writer.c -lm -l wiringPi

This code is not meant to be executed as a lone-program, and is instead a major component in [one-step-joint](https://www.github.com/whs-one-step/one-step-joint)

## Preview:
```c
/**
    @brief Writes pulse modulation to GPIO pins to represent binary.

    @param pulse_modulation The pulse modulation to represent as binary.
*/
void write_pulse_modulation(int pulse_modulation) {
    // Variables (Assignment):
    // Binary:
    char* binary = number_to_binary((unsigned char) pulse_modulation);

    // Logic:
    digitalWrite(STOP_PIN, LOW);

    for (int iteration = 7; iteration >= 0; iteration--) {
        if (binary[iteration] == '1' && status[iteration] == 0) {
            digitalWrite(pins[iteration], HIGH);
            status[iteration] = 1;

            continue;
        } else if (binary[iteration] == '0' && status[iteration] == 1) {
            digitalWrite(pins[iteration], LOW);
            status[iteration] = 0;
        }
    }

    free(binary);
}

/**
    @brief Activates the STOP pin and clears the other pins.
*/
void write_stop_pin() {
    for (int iteration = 0; iteration < 8; iteration++) {
        digitalWrite(pins[iteration], LOW);
    }

    digitalWrite(STOP_PIN, HIGH);
}
```

## Frameworks:
No frameworks were used in these writing optimizations, however a major library named [wiringPi](https://www.github.com/WiringPi/WiringPi) was used, providing low level access.
- Additionally, **gcc**, a C compiler, was used to compile the code into a shared library for [one-step-joint](https://www.github.com/whs-one-step/one-step-joint) to use.

## Language:
- **This repository is programmed in C, a low level language permitting access to hardware and memory. This was used for extreme performance in a project where performance is important, potentially leading to injury in cases where performance lacks.**
