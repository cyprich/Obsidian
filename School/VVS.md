# VVS

## Hardware Terminologia

Procesor - jadro, ktore vykonava instrukcie, spracovava data  
Mikropocitac/mikrokontroler - procesor + pamate + periferie  
Cip - integrovany obvod  
System on Chip (SoC) - vacsina casti pocitaca je na cipe  
Modul - viac cipov na jednej doske  
System on Module - pocitac na module  
Kit, DevKit - modul + doplnky (napajanie, tlacidla, LED, ...)

## ESP32 serie

- Seria S2
  - 2019
  - Single core 32 bit CPU
  - 320 kB RAM, 4MB flash (1GB externa)
  - Len WiFi
- Seria S3
  - 2020
  - Dvojjadrovy 32 bit CPU
  - 512 kB RAM, 8MB flash (1GB externa)
  - Bluetooth 5 (BLE)
- Seria C3
  - 2020
  - 400kB SRAM, 4MB flash (1GB externa)
  - More suitable for IoT
- Seria C6
  - 2021
  - WiFi 6 (2.4GHz), Bluetooth 5.3, IEEE 802.15.4 (ZigBee)
- Seria C5
  - 2022
  - Aj 5GHz WiFi
- Seria H2
  - 2021
  - Thread, ZigBee, Bez WiFi
  - Ultra nizka spotreba

## GPIO

General Purpose Input-Output  
Vstupno-vystupny pin pre vseobecne pouzitie  
Digitalne vs. Analogove

ESP32-C6 pinout

![ESP32-C6 pinout](../others/images/vvs-esp-pinout.png)

24-26, 28-30 vyhradene pre externu Flash, nepouzivat  
6, 7, 12, 13, 16, 17 - pouzivat opartne (USB, JTAG, UART)  
4, 5, 8, 9, 15 - Strapping GPIO - config pri bootovani

> `Vdd` - vstupne napatie = `3.3V`

Vystup

- Logicka 0: `0` az `0.1 x Vdd`
- Logicka 1: `0.8 x Vdd` az `Vdd`

Vstup

- Logicka 0: `-0.3` az `0.25 x Vdd`
- Logicka 1: `0.75 x Vdd` az `Vdd + 0.3`

### Vystupy

> Logicka 1 `> 2.64V`  
> Logicka 0 `< 0.33V`

Max. zatazitelnost

- $I_{OH}$ = `40mA` - zdroj (source), default `20mA`
- $I_{OL}$ = `28mA` - spotrebic (sink)
- Celkovo vystup max `1000mA`
- Celkovo vstup max `> 500mA`

```python
# micropython
from machine import Pin

p1 = Pin(1, Pin.OUT)
p2 = Pin(1, Pin.OUT, value=1)
p3 = Pin(1, Pin.OUT, drive=Pin.DRIVE_3)  # max vykon = 40mA

# nastavenie hodnoty
p1.on()
p1.off()
p2.value(1)

# citanie hodnoty
x = p3.value()
```

```python
from machine import Pin
import time

r = Pin(25, Pin.OUT)
g = Pin(26, Pin.OUT)
b = Pin(27, Pin.OUT)

i = 0
while True:
    r.value(i&1)
    g.value(i&2)
    b.value(i&4)
    i = i+1
    time.sleep(1)
```

## PWM

Pulse Width Modulation

### Fejkovy PWM

LEDka velmo rychlo blika, co posobi ako keby menej svietila

```python
while True:
    r.on()
    time.sleep(1)

    for i in range(500):
        r.off()
        time.sleep_ms(1)
        r.on()
        time.sleep_ms(1)
```

```python
# vacsi rozdiel - viac vidno
while True:
    r.on()
    time.sleep(1)

    for i in range(100):
        r.off()
        time.sleep_ms(9)
        r.on()
        time.sleep_ms(1)
```

### Normalny PWM

$U_O = U_{max} \cdot \dfrac{T_{on}}{T}$

> $T$ = perioda  
> $T_{on}$ = kedy je zapnuta

Duty Cycle

Neda sa pouzit pri "pomalom" zariadeni (motor)

Caste spinanie = vacsie energeticke (?) straty (to nechceme)  
Idealne mimo pocutelnej oblasti (20kHz)

ESP32-C6 - 6 PWM kanalov, 4 casovace

```python
from machine import Pin, PWM

pwm = PWM(Pin(0), freq=5000, duty_u16=32768)
pwm.freq(1000)

pwm.duty(256)  # 0 - 1023
pwm.duty_u16(256)  # 0 - 65_535
pwm.duty_ns(25000)  # 0 - 1_000_000_000
pwm.deinit()
```

```python
pwm = PWM(Pin(10), freq=500, duty=100)

while True:
    time.sleep(1)
    pwm.duty(1023)
    time.sleep(1)
    pwm.duty(100)

# pwm.deinit()
```

## Vstupy

### Prerusenie od tlacidla

### Dotykovy snimac

Kondenzator - uchovavanie naboja (energie) - kapacita $C = \epsilon \cdot \dfrac{S}{d}$  
Pri dotyku (dotykovy displej?) sa meni kapacita kondenzatora

Pri dotyku sa znizuje hodnota  
Idealne kalibrovat  
Snimac by mal byt odizolovany od tela (staticka el.)

```pyton
t = TouchPad(Pin(14))

while True:
    print(t.read())
    time.sleep(1)
```

## Casovac

Vola sa callback funkcia pravidelne v danom intervale

```python
t = Timer(id, mode=Timer.PERIODIC, freq=-1, period=-1, callback=None)
# mode moze byt aj Timer.ONE_SHOT
```

```python
def MyCallback():
    pass

tim = Timer(0)
tim.init(mode=Timer.PERIODIC, freq=1000, callback=MyCallback)
tim.deinit()
```

Lambda funkcia

- mala anonymna funkcia
- vyhodnocuje jeden vyraz

```python
tim = Timer(2, freq=2, callback=lambda x: print(".", end=""))
```

Trieda

```python
class Blik:
    def __init__(self, timer, period, led):
        self.led = Pin(led, Pin.OUT)
        self.tim = Timer(timer, period=period, mode=Timer.PERIODIC, callback=self.callback)

    def callback(self, tim):
        self.led.value(not self.led.value())

red = Blik(0, 2000, 21)  # actually to bude kazdu sekundu, nie 2, nevieme preco
green = Blik(2, 4000, 11)
```

### Watchdog casovac

Strazi zariadenie predtym, aby nezamrzlo  
Ak ho nenakrmime do `timeout` milisekund, ✨zacne vyvadzat✨ a resetuje zariadenie  
Hardwarova zalezitost

Co moze resetovat

- Vyvolat prerusenie
- CPU reset (by default)
- Core reset - CPU, WDT, periferie
- System reset - vsetko = +napajanie

```python
wdt = WDT(timeout=3000)
wdt.feed()
```

## Prevodniky

- Analogovo cislocovy prevodnik - AC (ADC)
- Cislicovo analogovy prevodnik - CA (DAC)

### CA prevodnik (DAC)

Generovanie spojiteho, analogoveho vystupu  
Diskretne -> Spojite

$U_{vyst} = \dfrac{\text{vstupny kod}}{2^N - 1} \cdot U_{ref}$

Chyby - offsetu, zosielnenia, diferencialna nelinearita, integralna nelinearita, monotonnost

```python
dac = DAC(Pin(25))
dac.write(128)  # 0-255
```

> ESP32-C6 ho nema

Nejde od 0V po 3.3V, ale kusok menej

### AC prevodnik (ADC)

Analogove -> Diskretne (digitalne)  
Diskretny aj v case aj v hodnote  
Sampling - vzorkovanie - poseka sa v case (CD - ~44kHz)  
Quantization - priradenie hodnoty nejakej urovni (CD - 16bit)

ESP32-C6

- Jeden 12bit prevodnik (4096 hodnot)
- Krok - $\dfrac{3.3V}{4096} \approx 0.8mV$
- GPIO Kanaly 0 az 6

```python
adc = ADC(Pin(3))
val = adc.read()  # 12 bit
val = adc.read_u16()  # softwarovo 16 bit
val = adc.read_uv()  # mikrovolty
# ak chceme co najpresnejsie hodnoty, pouzit read_uv()

adc.width(ADC.WIDTH_9BIT)  # 9 - 12 bit

acd.atten(ADC.ATTN_11DB) # utlm - (ake napatie mozeme pripojit?)
```

## Fotorezistor

Meni svoj odpor v zavislosti od osvetlenia

- neosvetleny ~50k ohm
- osvetleny ~20 ohm

ESP32-C6 - GPIO 3
