---
layout: post
title: Folder structure of the embedded project
blogid: personal
sticky: false
published: true
tags: [HAL, Cube MX, folder, structure, c]
---
In this short post I want to discuss organisation of the code for STM CUBE MX HAL generated projects.

Why do wee need folder structure at all? 
Certainly, one can put all source files in one folder. For small projects it will work. But with growing complexity single folder mess becomes troublesome. 
It is much better idea to have separate containers(folders in eclipse) for code on similar abstraction level, as well as functionality.
The modular approach to the embedded project keeps code organized and manageable.
I am trying to keep alike structure for embedded projects at works and home.

General idea is to have abstraction hierarchical levels:

![Hierarchical structure]({{"/images/img/2019-06-07/Vsod4b4.png"|relative_url}}){: .left-image }

1. On the bottom hardware (HW) layer is located. The level with processor registers access, memory, IRQ, peripherals. CMSIS connects HW with CUBE MX library by providing definitions and human readable names for registers memory mapping.
2. CUBE MX HAL implements universal API for usage of processor peripherals. 
3. On the next level Board Support Package (BSP) defines interfaces between Application (APP) and HAL layers. BSP can combine several peripherals in one functional block. E.g. if two frequency measurements channels need to be provided for the app, bsp_frequency module may use two hardware timers configured via HAL and output measured frequency. The responsibility of BSP is to encapsulate HAL calls.
4. Final level is application level. APP implements functionality of the device, logic. 

In practice, the following unified folder structure was shaped to reflect hierarchical dependency(from eclipse project explorer):

![Folder structure]({{"/images/img/2019-06-07/general_architecture.PNG"|relative_url}}){: .left-image }

- Application folder contains code for device logic.
- BSP or board support package, with all bsp interfaces.
- Core, Drivers, Sturtup folders are created by CubeMX. Core - prerpherials initialization, main.c, interrupts.
- Drivers CMSIS, HAL drivers. 
- lib contains all the libraries used by application.
- libmodel provides adjustments of the library for the project (e.g. implementation of interfaces)
- libdef configures library, e.g. defines.

Such way of code organisation is easy to navigate, expand and understand.
Definition of the project structure is important point for clean code standard.
