[English](/README.md) | [ 简体中文](/README_zh-Hans.md) | [繁體中文](/README_zh-Hant.md) | [日本語](/README_ja.md) | [Deutsch](/README_de.md) | [한국어](/README_ko.md)

<div align=center>
<img src="/doc/image/logo.svg" width="400" height="150"/>
</div>

## LibDriver EM4095

[![MISRA](https://img.shields.io/badge/misra-compliant-brightgreen.svg)](/misra/README.md) [![API](https://img.shields.io/badge/api-reference-blue.svg)](https://www.libdriver.com/docs/em4095/index.html) [![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](/LICENSE)

The EM4095 (previously named P4095) chip is a CMOS integrated transceiver circuit intended for use in an RFID basestation to perform the following functions:
- antenna driving with carrier frequency.
- AM modulation of the field for writable transponder.
- AM demodulation of the antenna signal modulation induced by the transponder communicate with a microprocessor via simple interface.

LibDriver EM4095 is a full-featured driver of EM4095 launched by LibDriver.It provides the function of RF reading, RF writing and other functions. LibDriver is MISRA compliant.

### Table of Contents

  - [Instruction](#Instruction)
  - [Install](#Install)
  - [Usage](#Usage)
    - [example basic](#example-basic)
  - [Document](#Document)
  - [Contributing](#Contributing)
  - [License](#License)
  - [Contact Us](#Contact-Us)

### Instruction

/src includes LibDriver EM4095 source files.

/interface includes LibDriver EM4095 GPIO platform independent template.

/test includes LibDriver EM4095 driver test code and this code can test the chip necessary function simply.

/example includes LibDriver EM4095 sample code.

/doc includes LibDriver EM4095 offline document.

/datasheet includes EM4095 datasheet.

/project includes the common Linux and MCU development board sample code. All projects use the shell script to debug the driver and the detail instruction can be found in each project's README.md.

/misra includes the LibDriver MISRA code scanning results.

### Install

Reference /interface GPIO platform independent template and finish your platform GPIO driver.

Add the /src directory, the interface driver for your platform, and your own drivers to your project, if you want to use the default example drivers, add the /example directory to your project.

### Usage

You can refer to the examples in the /example directory to complete your own driver. If you want to use the default programming examples, here's how to use them.

#### example basic

```C
#include "driver_em4095_basic.h"

uint8_t res;
uint32_t i;
uint8_t g_rx_buf[256];
uint32_t length = 256;

static void a_receive_callback(em4095_mode_t mode, em4095_decode_t *buf, uint16_t len)
{
    switch (mode)
    {
        case EM4095_MODE_READ :
        {
            em4095_interface_debug_print("em4095: irq read done.\n");

            break;
        }
        case EM4095_MODE_WRITE :
        {
            em4095_interface_debug_print("em4095: irq write done.\n");

            break;
        }
        default :
        {
            em4095_interface_debug_print("em4095: irq unknown mode.\n");

            break;
        }
    }
}

...
    
/* gpio init */
res = gpio_interrupt_init();
if (res != 0)
{
    return 1;
}

/* set the irq */
g_gpio_irq = em4095_basic_irq_handler;

...
    
/* basic init */
res = em4095_basic_init(a_receive_callback);
if (res != 0)
{
    (void)gpio_interrupt_deinit();
    g_gpio_irq = NULL;

    return 1;
}

...
    
/* read data */
res = em4095_basic_read(g_rx_buf, length);
if (res != 0)
{
    (void)gpio_interrupt_deinit();
    (void)em4095_basic_deinit();
    g_gpio_irq = NULL;

    return 1;
}

em4095_interface_debug_print("read data: ");
for (i = 0; i < length; i++)
{
    em4095_interface_debug_print("0x%02X ", g_rx_buf[i]);
}
em4095_interface_debug_print(".\n");

...
    
em4095_interface_debug_print("write data: ");
for (i = 0; i < length; i++)
{
    em4095_interface_debug_print("0x%02X ", g_rx_buf[i]);
}
em4095_interface_debug_print(".\n");

/* write data */
res = em4095_basic_write(g_rx_buf, length);
if (res != 0)
{
    (void)gpio_interrupt_deinit();
    (void)em4095_basic_deinit();
    g_gpio_irq = NULL;

    return 1;
} 

...
    
/* basic deinit */
(void)em4095_basic_deinit();

/* gpio deinit */
(void)gpio_interrupt_deinit();
g_gpio_irq = NULL;

return 0;
```

### Document

Online documents: [https://www.libdriver.com/docs/em4095/index.html](https://www.libdriver.com/docs/em4095/index.html).

Offline documents: /doc/html/index.html.

### Contributing

Please refer to CONTRIBUTING.md.

### License

Copyright (c) 2015 - present LibDriver All rights reserved



The MIT License (MIT) 



Permission is hereby granted, free of charge, to any person obtaining a copy

of this software and associated documentation files (the "Software"), to deal

in the Software without restriction, including without limitation the rights

to use, copy, modify, merge, publish, distribute, sublicense, and/or sell

copies of the Software, and to permit persons to whom the Software is

furnished to do so, subject to the following conditions: 



The above copyright notice and this permission notice shall be included in all

copies or substantial portions of the Software. 



THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR

IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,

FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE

AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER

LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,

OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE

SOFTWARE. 

### Contact Us

Please send an e-mail to lishifenging@outlook.com.