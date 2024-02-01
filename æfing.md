# Tímaverkefni 8

## Stýripinni

Tengdu stýripinnann þinn við ESP-inn á eftirfarandi hátt.
1. GND tengist í GND
2. :warning: 5V tengist í 3,3V :warning:
3. X(VRX) tengist í pinna 9
4. Y(VRY) tengist í pinna 8
5. B(SW) tengist í pinna 7

Settu svo upp eftirfarandi kóða:
```python
from machine import Pin, ADC
from time import sleep_ms

x_as = ADC(Pin(9), atten=ADC.ATTN_11DB)
y_as = ADC(Pin(8), atten=ADC.ATTN_11DB)
takki = Pin(7, Pin.IN, Pin.PULL_UP)

while True:
    print(f"X: {x_as.read()}, Y: {y_as.read()}, Takki: {takki.value()}")
    sleep_ms(500)
```

Skoðaðu vel hvaða gildi þú ert að fá meðan þú hreyfir stýripinnann. Hvaða gildi lestu þegar stýripinninn er í miðjunni, en lengst til hægri eða upp.

Þegar þú hefur áttað þig á hvernig stýripinninn virkar með kóðanum skaltu leysa eftirfarandi verkefni.

### Verkefni 1 (10%)

Settu upp eina LED peru (ásamt viðnámi) og stjórnaðu birtumagni ([PWM](https://github.com/VESM2VT/ESP32/blob/main/kennsluefni/analog.md#skrifað-á-pinna)) hennar með upp/niður hreyfingu á stýripinnanum. 

### Verkefni 2 (10%)

Bættu virkni á skottakkann. Takkinn á að virka sem [rofi](https://github.com/VESM2VT/ESP32/blob/main/kennsluefni/digital.md#rofar) á LED peruna í verkefninu hér að ofan. Bættu svo [debounce](https://github.com/VESM2VT/ESP32/blob/main/kennsluefni/digital.md#debounce) við.

### Verkefni 3 (20%)

1. Settu upp fjórar LEDs (ásamt viðeigandi viðnámum) á brauðbretti og raðaðu þeim í tígul. Stýrðu með stýripinnanum  (upp, niður, vinstri, hægri) hvaða LED er kveikt á hverju sinni, það á bara vera kveikt á einni LED peru í einu. Ekki á nota PWM í þessu verkefni.

## ESPnow

Kynntu þér ESPnow með því að lesa þessa [grein](https://dronebotworkshop.com/esp-now/) (lestu að kaflanum "MAC Address Sketch"). Hafðu eftirfarandi í huga meðan þú lest greinina:
- Hvernig er ESPnow ólíkt "venjulegu" WIFI eins og t.d. á fartölvunni þinni?
- Hversu stóra pakka (í bætum) er hægt að senda með ESPnow?
- Hversu mörg tæki er hægt að láta tala saman með ESPnow?
- Hvað er MAC vistfang (e. address)?

Til að geta notað ESPnow þarf að setja inn sérstaka útgáfu af python á ESP-inn. Eini munurinn á þessari útgáfu og þeirri sem á ESP-unum þínum núna er að þessi útgáfa styður ESPnow og að ekki er hægt að nota pinna 11 - 18 með [ADC](https://github.com/VESM2VT/ESP32/blob/main/kennsluefni/analog.md#lesið-frá-pinna), pinnarnir virka þó fyrir alla aðra virkni (Pin inn og út og PWM).

### ESPnow python sett á ESP32-S3

:warning: Áður en þú setur nýtt python á ESP-ana þína skaltu passa að taka afrit af öllum python skrám sem eru á þeim. Þeim verður eytt í ferlinu hér á eftir. :warning:

1. Byrjaðu á að sækja [þessa](https://github.com/Freenove/Freenove_ESP32_S3_WROOM_Board.git) github geymslu (e. repository). Smella á græna "<> Code" takkann og velja "Download ZIP" og **afþjappaðu** (e. extract) svo ZIP skránni. 
2. Opnaðu afþjöppuðu möppuna í File Explorer og farðu í Python möppuna sem þú sér þar, þaðan í Python_Firmware möppuna.
3. Sæktu svo [þessa](https://github.com/glenn20/micropython-espnow-images/blob/main/20220709_espnow-g20-v1.19.1-espnow-6-g44f65965b/firmware-esp32-GENERIC_S3.bin) skrá.
    ![firmware](https://raw.githubusercontent.com/VESM2VT/ESP32/main/myndir/saekja_firmware.png)
4. Næst þarftu að afrita bin skrána í Python_Firmware möppuna úr lið 2.
5. Í Python_Firmware möppunni skaltu opna .py skrána sem heitir eftir stýrikerfinu þínu með uppáhalds ritlinum (e. editor) þínum.
6. Breyttu línu 7 í .py skránni þannig að í stað `GENERIC_S3-20220618-v1.19.1.bin` komi `firmware-esp32-GENERIC_S3.bin`.
7. Opnaðu svo skipanalínu (e. comman line) í Python_Firmware möppunni og keyrðu eftirfarandi (passaðu að Thonny sé ekki keyrandi):
```bash
python3 NAFNIÐ_Á_STÝRIKERFINU_ÞÍNU.py
# eða
py NAFNIÐ_Á_STÝRIKERFINU_ÞÍNU.py
# eða
python NAFNIÐ_Á_STÝRIKERFINU_ÞÍNU.py
```

Endurtaktu svo lið 7 fyrir hinn ESP-inn þinn.

### Hafa tvo Thonny glugga opna í einu

Í Thonny skaltu fara í Tools->Options og taka hakið úr *Allow only single Thonny instance*. Þá getur þú opnað tvo Thonny glugga. Á Mac þarf að opna glugga nr. 2 með því að fara í terminal og skrifa eftirfarandi: `open -n -a Thonny.app`

### Finna MAC addressu á ESP32

Til að geta átt samskipti milli ESP-anna þarftu að finna MAC addressuna á þeim báðum. Farðu í REPL (Shell) hluta Thonny og sláðu inn eftirfarandi:
```python
import machine
machine.unique_id()
# strengurinn sem birtist er á forminu b'4\x85\x18n\x03\xf0'
```
Þá birtist MAC addressa ESP. Skráðu MAC addressu strengina hjá þér.

### Hello World eða 123 halló?

Settu eftirfarandi kóða á annan ESP-inn þinn. Þessi kóði er fyrir sendingu á gögnum.
```python
# Kóðinn á ESP-inn sem sendir skilaboð
from network import WLAN, STA_IF
from espnow import ESPNow
from time import sleep_ms

# Virkja þráðlausa netið
sta = WLAN(STA_IF)
sta.active(True)

sendir = ESPNow()
sendir.active(True)
hinn_esp_inn = b'4\x85\x18m\xc3\xd0'   # MAC address-an á hinum ESP-inum (móttakaranum)
sendir.add_peer(hinn_esp_inn)

teljari = 0

while True:
    # skilaboðin eru alltaf send sem strengur (eða bytestring) en við getum notum streng í þessum áfanga
    skilabod = f"{teljari} halló"
    sendir.send(hinn_esp_inn, skilabod, True)
    teljari += 1
    sleep_ms(500)
  
```
Hér er svo kóðinn fyrir ESP-inn sem tekur á móti gögnunum.

```python
# Kóðinn fyrir ESP-inn sem móttekur skilaboðin
from network import WLAN, STA_IF
from espnow import ESPNow

# Virkja þráðlausa netið
sta = WLAN(STA_IF)
sta.active(True)

mottakari = ESPNow()
mottakari.active(True)
hinn_esp_inn = b'4\x85\x18n\x03\xf0'   # MAC address-an á hinum ESP-inum (sendananum)
mottakari.add_peer(hinn_esp_inn)

while True:
    sendandi, skilabod = mottakari.recv()
    if skilabod: # ef einhver skilaboð bárust
        # skilaboðin eru á forminu "tala texti"
        tala, texti = skilabod.split()
        # þurfum því að gera ráðstafanir til að geta notað töluna sem tölu
        tala = int(tala)
        print(f"{sendandi} sendi {tala + 10} {texti} sem má decoda í {texti.decode()}" )
```

### Verkefni 4 (10%)

Breyttu kóðanum hér fyrir ofan þannig að sendandinn sendir tvær handahófsvaldar (e. random) heiltölur á bilinu 1 til 100. Móttakarinn tekur svo við þeim, birtir báðar tölurnar, leggur þær saman og birtir niðurstöðuna.

### Verkefni 5 (40%)

Kynntu þér hvernig DHT11 (hita og rakamælir) virkar með því að skoða kafla 24 í "Bókinni" sem þú finnur undir Efni á Innu. Þú finnur **dht** klasann [hér](https://github.com/Freenove/Freenove_Ultimate_Starter_Kit_for_ESP32_S3/blob/main/Python/Python_Libraries/dht.py).

Tengdu svo DHT11 við sendi ESP-inn og sendu svo mælingarnar yfir á hinn. Tengdu LCD skjáinn sem þú notaðir í [Tímaverkefni 7](https://github.com/VESM2VT/ESP32/blob/main/verkefni/T%C3%ADmaverkefni7.md) við móttöku ESP-inn og birtu mælingarnar frá DHT-11 á honum. Ef LCD skjárinn var með leiðindi við þig í verkefni 7, birtu mælingarnar þá bara á tölvuskjánum þínum.

