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
  --usb                 enforce valid USBCLK

...

Selectors:
  ...
  --pllsrc {hse,hsi}    set PLLSRC (PLL source)
```

```
$ ./solver --usb --hse 8M --pllsrc hse --rtc 16384
Impossible combination
```

```
$ ./solver --usb --hse 8M --pllsrc hse --rtc 16384.. # note the difference in rtc: this time it's "16384 or more"
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
$ ./solver --usb --hse 8M --pllsrc hse --rtc 16384.. --adc 1k
Oscillators:
	HSI              8.0M
	HSE              8.0M
	LSI             40.0k
	LSE                0 
Selectors:
	RTCSEL     HSE
	MCO        SYS
	SW         HSI
	PLLSRC     HSE
Prescalers:
	APB1DIV            8 
	APB2DIV            8 
	ADCDIV             8 
	AHBDIV           125 
	PLLXTPRE           0 
	PLLMUL            12 
	TIM1MUL            2 
	TIMXMUL            2 
	USBDIV             1 
Intermediate:
	PLLX             8.0M
	PLL              4.0M
	PLLCLK          48.0M
	SYSCLK           8.0M
	AHB             64.0k
	APB1             8.0k
	APB2             8.0k
Cortex:
	SysTick          8.0k
	HCLK            64.0k
	FCLK            64.0k
Peripherals:
	ADCCLK           1.0k
	FLITFCLK         8.0M
	MCO              8.0M
	RTCCLK          62.5k
	USBCLK          48.0M
```
