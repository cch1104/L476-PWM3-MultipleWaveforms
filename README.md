# L476-PWM3-MultipleWaveforms

## 📌 Overview

This project demonstrates how to generate **PWM (Pulse Width Modulation)** signals using **TIM2** on an STM32 microcontroller.
Three channels are configured to output different duty cycles:

* Channel 1 → 25%
* Channel 2 → 50%
* Channel 3 → 75%

The project uses the **HAL (Hardware Abstraction Layer)** library.

---

## ⚙️ Features

* Multi-channel PWM output (3 channels)
* Adjustable duty cycle via function
* Simple and modular structure
* Suitable for:

  * LED brightness control
  * Motor speed control
  * Signal generation

---

## 🧠 PWM Concept

PWM controls output power by adjusting the **duty cycle**:

[
Duty\ Cycle = \frac{CCR}{ARR} \times 100%
]

Where:

* `ARR` → Auto Reload Register (period)
* `CCR` → Compare Register (pulse width)

---

## 🔧 Configuration

### Timer (TIM2)

| Parameter    | Value      |
| ------------ | ---------- |
| Prescaler    | 80         |
| Period (ARR) | 99         |
| Mode         | PWM Mode 1 |

### PWM Frequency

Assuming system clock = 80 MHz:

```
f_PWM = 80MHz / ((PSC+1) × (ARR+1))
      ≈ 9.8 kHz
```

---

## 📍 GPIO Configuration

Make sure GPIO is set to **Alternate Function Push-Pull**:

```c
GPIO_InitStruct.Pin = GPIO_PIN_0 | GPIO_PIN_1 | GPIO_PIN_2;
GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
GPIO_InitStruct.Pull = GPIO_NOPULL;
GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
GPIO_InitStruct.Alternate = GPIO_AF1_TIM2;
```

### Pin Mapping

| Channel  | Pin |
| -------- | --- |
| TIM2_CH1 | PA0 |
| TIM2_CH2 | PA1 |
| TIM2_CH3 | PA2 |

---

## 🚀 Usage

### Start PWM

```c
HAL_TIM_PWM_Start(&htim2, TIM_CHANNEL_1);
HAL_TIM_PWM_Start(&htim2, TIM_CHANNEL_2);
HAL_TIM_PWM_Start(&htim2, TIM_CHANNEL_3);
```

### Set Duty Cycle

```c
DutyCycle(25, TIM_CHANNEL_1);
DutyCycle(50, TIM_CHANNEL_2);
DutyCycle(75, TIM_CHANNEL_3);
```

---

## 🔑 Key Function

```c
void DutyCycle(float Duty, int Channel){
    uint16_t AutoReload, SetDutyCycle;
    AutoReload = __HAL_TIM_GET_AUTORELOAD(&htim2);
    SetDutyCycle = AutoReload * Duty / 100.0;
    __HAL_TIM_SET_COMPARE(&htim2, Channel, SetDutyCycle);
}
```


### Tips:

* Ensure GND is connected
* Use proper trigger settings
* Check probe attenuation

### Demo Video
https://youtu.be/3cer_WQ9OAM

