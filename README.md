# IBEX-timing-card
Development and Testing for the NI-PCIe Timing Card used for measurement timing.

## Requirements
The timing system is required to provide multiple timing triggers to various external hardware devices with close precision.

Each trigger must be synchronized to a single start trigger. The point at which the start trigger occurs and the duration of the measurement is governed by a *Master Timer*.

## Operation of NI Timing Card.vi
DAQmx counters of the timing card provide a means of generating rising/falling edges and pulses. The counters are controlled through DAQmx Tasks.

A DAQmx task is used to control the counter used for the Master Timer: The delay between the start of the measurement and the *start trigger* (i.e. the *pre-trigger time*) is implemented as the *low time* of counter task; the measurement-duration is governed by the *high-time* of this counter task.

Subordinate Timers are realized as individual DAQmx tasks; each is configurable to provide different types of output, currently rising or falling edge steps, and at variable delays from the start-trigger. They are configured in a similar way to the Master Timer's counter except that they trigger from the 'internal' output of the Master Timer's counter and the high/low time (depending on rising/falling edge respectively) are set taking into account the measurement-duration.

The Master Timer's counter is triggered implicitly so the DAQmx Start Task vi is not called until everything is configured. Once started, the Master Timer's counter task can be polled to see when the measurement time has elapsed. After this time the Subordinate Timer Tasks are explicitly stopped to clear any which have not completed.

The code is designed for user interaction **before** the *Run VI* button is pressed. As soon as the VI is run it sets up the tasks, performs a single shot and cleans up. It is intended for testing the timing card and verifying that this mode of operating the hardware is acceptable. **It is not designed to be production code or even be converted into such**

Following successful development,hacking and testing the concepts of operation will be converted into a library with software development best practices in mind.

This VI is included in a project for the sake of code management and not for the sake of future development.

## Hardware
This code is designed to test the NI PCIe-6612 card [manual available here](http://www.ni.com/pdf/manuals/374008b.pdf).

## Requirements
* LabVIEW Development Environment 2015 SP1
* National Instruments DAQmx driver
