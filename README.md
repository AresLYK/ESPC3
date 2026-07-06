# ESP32-C3 LED Blink (IO12 & IO13)

一个简单的 ESP32-C3 示例程序，通过循环交替翻转 GPIO12 和 GPIO13 上的 LED 状态。

## 硬件连接

| LED  | ESP32-C3 引脚 |
| ---- | ------------- |
| LED1 | **GPIO12**    |
| LED2 | **GPIO13**    |

- 建议串联 **220Ω~330Ω** 限流电阻后接入 LED 正极，负极接 GND。
- 若使用板载 LED，请确认对应引脚（此例为外接 LED）。

## 功能说明

- 程序上电后，两个 LED 以 **500ms** 间隔交替闪烁（即 IO12 亮时 IO13 灭，反之亦然）。
- 循环周期为 1 秒。

## 开发环境

- **芯片**：ESP32-C3（RISC-V 架构）
- **框架**：ESP-IDF v5.4.3 
- **烧录工具**：`esptool.py` 或 IDE 自带烧录功能

## 快速开始

 

```c
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"

#define GPIO_12 GPIO_NUM_12
#define GPIO_13 GPIO_NUM_13

void app_main(void)
{
    gpio_config_t io_conf = {
        .pin_bit_mask = (1ULL << GPIO_12) | (1ULL << GPIO_13),
        .mode = GPIO_MODE_OUTPUT,
        .pull_up_en = GPIO_PULLUP_DISABLE,
        .pull_down_en = GPIO_PULLDOWN_DISABLE,
        .intr_type = GPIO_INTR_DISABLE};

    gpio_config(&io_conf);

    int level = 0;
    while (1)
    {
        gpio_set_level(GPIO_12, level);
        gpio_set_level(GPIO_13, level);
        level = !level;
        vTaskDelay(pdMS_TO_TICKS(1000));
    }
}
```



### 1. 克隆仓库
```bash
git clone https://github.com/yourusername/your-repo.git
cd your-repo
```