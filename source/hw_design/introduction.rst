Introduction & Design Goals
============================

Overview
--------

This document provides hardware design guidelines for the Realtek AmebaPro2 platform.

It serves as a reference for schematic design, PCB layout implementation, and hardware review prior to mass production.

The goal of this guide is to help hardware engineers:

- Design stable and reliable boards based on AmebaPro2
- Reduce board bring-up time
- Avoid common schematic and layout mistakes
- Improve EMC and signal integrity performance
- Increase first-pass design success rate

Target Audience
---------------

This guide is intended for:

- Hardware design engineers
- PCB layout engineers
- System integrators
- Technical reviewers performing schematic or layout checks

It assumes readers have basic knowledge of:

- Power supply design
- High-speed signal routing
- PCB stack-up and impedance control
- Embedded system development

Hardware Design Philosophy
--------------------------

The AmebaPro2 platform integrates high-speed interfaces, multimedia inputs, and switching power circuits into a compact system-on-chip (SoC).

Proper hardware implementation is critical to ensure:

- Stable boot behavior
- Reliable firmware download and debug access
- Clean and stable power delivery
- Controlled EMI emission
- Robust high-speed communication performance

This guide separates design recommendations into two major phases:

1. **Schematic Design Guidelines**

   Focused on correct circuit implementation, signal configuration, and power architecture.

2. **PCB Layout Guidelines**

   Focused on component placement, routing strategy, grounding, impedance control, and noise reduction.

Design Flow Overview
--------------------

A recommended hardware development flow is:

1. Review the datasheet and reference design.
2. Complete schematic design following this guide.
3. Perform schematic self-check using the checklist.
4. Implement PCB layout according to the layout guidelines.
5. Conduct layout review (power, high-speed signals, grounding).
6. Perform board bring-up and validation testing.

Following this structured flow significantly reduces hardware iteration cycles and improves overall product reliability.

Reference Materials
-------------------

For complete technical specifications, please also refer to:

- `RTL8735Bx Series HDK Collection <https://www.realmcu.com/en/Resources/hardware/RTL8735B-Series>`_

.. note::

   The EVB schematic is optimized for the evaluation board design only.
   It should be used as a reference example.
   Actual product designs may require additional considerations.
   Do not directly copy the EVB schematic without proper review and validation.

- `SDK Open-Source GitHub Repository <https://github.com/Ameba-AIoT/ameba-rtos-pro2>`_
