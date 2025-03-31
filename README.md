Given constraints, this script finds CR/CFGR configuration for STM32F103x8/STM32F103xB (or proves that none could satisty them) using [Z3](https://github.com/Z3Prover/z3).

Example:

```
$ ./solver --help
...

Oscillators:
  --hse FREQ            restrict HSE (use 0 to avoid external oscillator)
  --lse {0,1}           restrict LSE (0 to avoid it, 1 to enforce its presence)

Peripherals:
  --adc FREQ            restrict ADCCLK
  --flash FREQ          restrict FLITFCLK
  ...
  --usb                 enforce valid USBCLK

...
```

```
$ ./solver --usb --adc ..10k --hse 48M --lse 0 
Impossible combination
```

```
$ ./solver --usb --adc ..10k --hse 0 --lse 0 
Oscillators:
        HSE                0 
        LSE                0 
Selectors:
        RTCSEL     LSI
        MCO        SYS
        SW         HSI
        PLLSRC     HSE
Prescalers:
        APB1DIV            1 
        APB2DIV            1 
        ADCDIV             2 
        AHBDIV           509 
        PLLXTPRE           0 
        PLLMUL            12 
        TIM1MUL            1 
        TIMXMUL            1 
        USBDIV             1 
Intermediate:
        SYSCLK           8.0M
        PLLCLK          48.0M
Cortex:
        SysTick         1964 
        HCLK           15717 
        FCLK           15717 
Peripherals:
        ADCCLK          7858 
        FLITFCLK         8.0M
        MCO              8.0M
        PCLK1          15717 
        PCLK2          15717 
        RTCCLK          40.0k
        TIM1CLK        15717 
        TIMXCLK        15717 
        USBCLK          48.0M
```

```
$ ./solver --usb --adc 10k --hse 0 --lse 0 
Oscillators:
        HSE                0 
        LSE                0 
Selectors:
        RTCSEL     LSI
        MCO        PLC
        SW         HSI
        PLLSRC     HSE
Prescalers:
        APB1DIV            2 
        APB2DIV            1 
        ADCDIV             2 
        AHBDIV           0.4k
        PLLXTPRE           0 
        PLLMUL            12 
        TIM1MUL            1 
        TIMXMUL            2 
        USBDIV             1 
Intermediate:
        SYSCLK           8.0M
        PLLCLK          48.0M
Cortex:
        SysTick          2.5k
        HCLK            20.0k
        FCLK            20.0k
Peripherals:
        ADCCLK          10.0k
        FLITFCLK         8.0M
        MCO             24.0M
        PCLK1           10.0k
        PCLK2           20.0k
        RTCCLK          40.0k
        TIM1CLK         20.0k
        TIMXCLK         20.0k
        USBCLK          48.0M
```
