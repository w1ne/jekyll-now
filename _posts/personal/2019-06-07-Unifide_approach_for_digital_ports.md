---
layout: post
title: Unifide approach for digital ports
blogid: personal
sticky: false
published: false
---
In recent design there was a requirement for hundreds of GPIO pins on the MCU.
STM32F373 in 100-pin LQFP processor used for GPIO, analogue and resistance measurements is perfect for the task due to Sigma Delta ADC and rich peripherals. But it lacks required count of GPIO ports. In the same time the bigger processor is prohibited by mechanical limitations of the board, production process and logistical limitations.

The HW solution is to use port expanders. 
The SW challenge is to integrate shift register port expanders with clear separation on BSP and application layers. bsp_port module unifies access to GPIO and SIPO pins. 

There are two possibilities to toggle a GPIO pin:
     via SIPO registers, using bsp module responsible for data transfer via shift registers.
     via MCU GPIO, using interfaces provided by CubeMX.

The structure to combine SIPO and GPIO pins looks as follows:
```c
typedef struct port_conf
{
     /* for "traditional" MCU pins */
     const uint16_t GPIOPin; /*!< GPIO Pin */
     const GPIO_TypeDef *GPIOPort; /*!< Port of the Pin */
     /* for SIPO pins */
     const uint8_t SIPOBit; /*!< Bit location on SIPO */
     const uint8_t SIPOPortByte; /*!< Port byte in SIPO buffer */
}port_conf_t;
```
In (ANSI) C99, you can use a designated initializer to initialize a structure:
type_t MyStruct = { .IntValue = 123, .Color = green_e, SomeBool. = true };
Not explicitely initialized members are initialized as zero.
As stated in C11 paragraph "6.7.9 Initialization":
"If there are fewer initializers in a brace-enclosed list than there are elements or members of an aggregate, or fewer characters in a string literal used to initialize an array of known size than there are elements in the array, the remainder of the aggregate shall be initialized implicitly the same as objects that have static storage duration."

Therefore SIPO pin 

```c
static const port_conf_t PinConf[] =
{
     /* SIPO Leds */
     [ LedPort0_pin2 ] = { .SIPOBit = SIPO_BIT_POS_LED_P2, .SIPOPortByte = bsp_ports_Port0, .GPIOPin = PORT_NA, .GPIOPort = PORT_NA },
     [ LedPort0_pin4 ] = { .SIPOBit = SIPO_BIT_POS_LED_P4, .SIPOPortByte = bsp_ports_Port0, .GPIOPin = PORT_NA, .GPIOPort = PORT_NA },

     /* GPIO Leds */
     [ LedUAL ] = { .GPIOPin = LED_L_Pin, .GPIOPort = LED_L_GPIO_Port, .SIPOBit = PORT_NA, .SIPOPortByte = PORT_NA },
     [ LedUAR ] = { .GPIOPin = LED_R_Pin, .GPIOPort = LED_R_GPIO_Port, .SIPOBit = PORT_NA, .SIPOPortByte = PORT_NA },
};
```

Advantages of the solution:
+ All GPIO ports are visible to application layer as same GPIO objects. 
HW differences dosn't obscure application interfaces.
+ Universal interface for all GPIO.
+ Self explanatory, readable, easy port configuration
+ Such initialization is compatible with MISRA rule 9.2. 
Disadvantages:
- Excessive memory use. Not initialized members just stay in memory. 
Not really a problem, as long as table is located in flash.
- Code is only compatible with C99 standard and higher.

Conclusion
