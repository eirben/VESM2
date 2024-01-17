```python
from machine import Timer, Pin
import random
import time
# bý til led og buzzer
myLED = Pin(14, Pin.OUT)
myLED2 = Pin(13, Pin.OUT)
myBuzzer = Pin(12, Pin.OUT)

# Bý til timera
myTimer = Timer(0)
myTimer1 = Timer(1)
myTimer2 = Timer(2)

# Bý til föll sem tímer kallar á eftir ákveðin tíma
# Þessi föll sjá um Led og Buzzer
def toggle_led(timer):
    myLED.value(not myLED.value())
    
def toggle_led2(timer):
    myLED2.value(not myLED2.value())
    
def toggle_speaker(timer):
    myBuzzer.value(not mySpeaker.value())
    

# frumstilli tímera á tima sem við viljum 1000, 500 og 100, tímerar eru stilltir sem reglubundið kall (PERIODIC)
def startTimers():
    myTimer.init(period=1000, mode=Timer.PERIODIC, callback=toggle_led)
    myTimer1.init(period=500, mode=Timer.PERIODIC, callback=toggle_led2)
    myTimer2.init(period=100, mode=Timer.PERIODIC, callback=toggle_speaker)

while True:
    test = random.randint(0,9)
    print(test)
    if test >= 5:
        startTimers()# starta tímera annars keyra þeir endalaust 
    else:
# stoppa tímera og Buzzer
        myTimer.deinit()
        myTimer1.deinit()
        myTimer2.deinit()
        mySpeaker.value(0)
    time.sleep(2)# hægi á keyrslu til að debuga, óþarfi í raun
```
