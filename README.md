Given constraints, this script finds CR/CFGR configuration for STM32F103x8/STM32F103xB (or proves that none could satisty them) using [Z3](https://github.com/Z3Prover/z3).

Example:

```
$ ./solver --help
...

Oscillators:
  --hse FREQ            enforce presence of HSE (use ".." to accept any valid frequency)
  --lse                 enforce presence of LSE

Peripherals:
  --adc FREQ            restrict ADCCLK
  --flash FREQ          restrict FLITFCLK
  ...
  --usb                 enforce valid USBCLK

...
```

```
$ ./solver --usb --adc ..10k --hse 48M
Impossible combination
```

```
$ ./solver --usb --adc ..10k
Oscillators:
        HSI              8.0M
        HSE                0 
        LSI             40.0k
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
        PLLX               0 
        PLL              4.0M
        PLLCLK          48.0M
        SYSCLK           8.0M
        AHB            15717 
        APB1           15717 
        APB2           15717 
Cortex:
        SysTick         1964 
        HCLK           15717 
        FCLK           15717 
Peripherals:
        ADCCLK          7858 
        FLITFCLK         8.0M
        MCO              8.0M
        USBCLK          48.0M
```

```
$ ./solver --usb --adc 10k
Oscillators:
        HSI              8.0M
        HSE                0 
        LSI             40.0k
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
        PLLX               0 
        PLL              4.0M
        PLLCLK          48.0M
        SYSCLK           8.0M
        AHB             20.0k
        APB1            10.0k
        APB2            20.0k
Cortex:
        SysTick          2.5k
        HCLK            20.0k
        FCLK            20.0k
Peripherals:
        ADCCLK          10.0k
        FLITFCLK         8.0M
        MCO             24.0M
        USBCLK          48.0M
```
