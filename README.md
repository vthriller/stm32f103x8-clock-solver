Given constraints, this script finds CR/CFGR configuration for STM32F103x8/STM32F103xB (or proves that none could satisty them) using [Z3](https://github.com/Z3Prover/z3).

Example:

```
$ ./solver --help
...

Oscillators:
  --hse FREQ            enforce presence of HSE (use ".." to accept any valid frequency)
  ...

Peripherals:
  --adc FREQ            restrict ADCCLK
  ...
  --rtc FREQ            restrict RTCCLK
  ...
  --usb                 enforce valid USBCLK; implies --hse .. --pllsrc hse

...

Selectors:
  ...
  --pllsrc {hse,hsi}    set PLLSRC (PLL source)
```

```
$ ./solver --usb --hse 8M --rtc 16384
Impossible combination
```

```
$ ./solver --usb --hse 8M --rtc 16384.. # note the difference in rtc: this time it's "16384 or more"
Oscillators:
	HSI              8.0M
	HSE              8.0M
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
	AHBDIV           512 
	PLLXTPRE           0 
	PLLMUL            12 
	TIM1MUL            1 
	TIMXMUL            1 
	USBDIV             1 
Intermediate:
	PLLX             8.0M
	PLL              4.0M
	PLLCLK          48.0M
	SYSCLK           8.0M
	AHB            15625 
	APB1           15625 
	APB2           15625 
Cortex:
	SysTick         1953 
	HCLK           15625 
	FCLK           15625 
Peripherals:
	FLITFCLK         8.0M
	MCO              8.0M
	RTCCLK          40.0k
	USBCLK          48.0M
```

```
$ ./solver --usb --hse 8M --rtc 16384.. --adc 1M
Oscillators:
	HSI              8.0M
	HSE              8.0M
	LSI             40.0k
	LSE                0 
Selectors:
	RTCSEL     HSE
	MCO        SYS
	SW         HSE
	PLLSRC     HSE
Prescalers:
	APB1DIV            1 
	APB2DIV            1 
	ADCDIV             2 
	AHBDIV             4 
	PLLXTPRE           0 
	PLLMUL            12 
	TIM1MUL            1 
	TIMXMUL            1 
	USBDIV             1 
Intermediate:
	PLLX             8.0M
	PLL              4.0M
	PLLCLK          48.0M
	SYSCLK           8.0M
	AHB              2.0M
	APB1             2.0M
	APB2             2.0M
Cortex:
	SysTick        250.0k
	HCLK             2.0M
	FCLK             2.0M
Peripherals:
	ADCCLK           1.0M
	FLITFCLK         8.0M
	MCO              8.0M
	RTCCLK          62.5k
	USBCLK          48.0M
```
