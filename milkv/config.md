# config

VSYS：2V~5V电源输入引脚。
VBUS：从Micro USB (Type-C)接口获得的5V电源输出，可供电给需要5V电源的电子元件。
3V3：3V电源，与Pico W的工作电压相同。
3V3_EN：使能或禁止电源；使能或禁止Pico W以及3V3引脚的电源输出。
RUN/RESET：启用或停用Pico∕重置，输入低电平将使Pico W停止运行。
GP0-GP28：GPIO（通用输入/输出）引脚，板载LED与WL_GPIO0相连。
ADC0 ~ ADC2：具备模拟输入功能的GPIO引脚，可当作模拟输入或数字输入/输出引脚
AGND: AGND模拟地专门用于GPIO26-GPIO29信号的参考接地；单独的模拟接地层连接至AGND引脚。若不使用ADC或ADC性能不是特别关键，则可将该引脚直接接到数字地GND。
ADC_VREF: ADC_VREF是ADC电源(和基准)电压，通过对3.3V电源进行滤波在树莓派Pico上产生。若需要更好的ADC性能，则可将该引脚与外部基准一起配合使用。
RUN: RUN是RP2040 MCU芯片使能引脚，它有一个连接到3.3V的内部(片上)上拉电阻，该电阻为50kΩ。将此引脚与低电平短路将对RP2040 MCU进行复位

1. 属于GPIOA下的 为 linux组号为480，基址为0x3020000，gpio编号为 linux组号+GPIOA后面的数字
2. 属于GPIOC下的 为 linux组号为416，基址为0x3022000，gpio编号为 linux组号+GPIOC后面的数字
3. 属于GPIO下的 为 linux组号为352，基址为0x5021000，gpio编号为 linux组号+GPIO后面的数字
