# RasberryPicoW

## Departamento de Sistemas y Computación
## Ing. En Sistemas Computacionales
## Sistemas Programables 23a

### Autor (es): Hernández Sáenz Sara Jazmín
### Fecha de revisión: 05/Octubre/2023

**_Objetivo_**
Aprender más de la Rasberry Pico w



## Practica de Prender el LED de la Pico W

### CÓDIGO
```python
## Autor: Hernandez Saenz  Sara Jazmín
## Fecha de revisión:   05/Oct/2023

from machine import Pin
from utime import sleep

led = machine.Pin("LED", machine.Pin.OUT)

while True:

    led.toggle()
    sleep(0.5)
##
```

### Evidencia de la práctica realizada

Apagado

![](20231005_135808.jpg)

Prendido

![](20231005_135807.jpg)


## Practica de Mostrar "Hola mundo" en el OLED y Pico W

### CÓDIGO
```python
## Autor: Hernandez Saenz  Sara Jazmín
## Fecha de revisión:   09/Oct/2023

import machine
import ssd1306

# Configura los pines SDA y SCL para la comunicación I2C
i2c = machine.I2C(0, sda=machine.Pin(8), scl=machine.Pin(9))

# Configura el objeto SSD1306 para la pantalla OLED
oled = ssd1306.SSD1306_I2C(128, 64, i2c)

# Limpia la pantalla
oled.fill(0)
oled.show()

# Dibuja "Hola Mundo" en la pantalla
oled.text("Hola Mundo", 0, 0)
oled.text("Soy Sara", 0, 10)

# Actualiza la pantalla para mostrar el texto
oled.show()
##
```

### Evidencia de la práctica realizada

Circuito

![](OLED3.png)

Corriendo en el OLED

![](OLED1.jpg)

![](OLED2.jpg)



## Practica de desplegar en el OLED y Pico W por medio de internet

### CÓDIGO
```python
## Autor: Hernandez Saenz  Sara Jazmín
## Fecha de revisión:   18/Oct/2023

import time
import machine
import ssd1306
import utime
import network
from machine import Pin, I2C #configuración de pines para su comunicación
from ssd1306 import SSD1306_I2C #comunicación con la pantalla OLED
import framebuf, sys

#bloque de código para la conexion del raspberry a internet
print("¡Conectando a Wi-Fi! ", end="")
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect("Pulga","0505pulga")
while not wlan.isconnected():
  print(".", end="")
  time.sleep(0.1)
print("¡Conexión con éxito!")
print(wlan.ifconfig())

#Verificación de dispositivo
def init_i2c(scl_pin, sda_pin):
    i2c_dev = I2C(1, scl=Pin(scl_pin), sda=Pin(sda_pin), freq=200000)
    i2c_addr = [hex(ii) for ii in i2c_dev.scan()]
    
    if not i2c_addr:
        print('Pantalla I2C no encontrada')
        sys.exit()
    else:
        print("I2C Address : {}".format(i2c_addr[0]))
        print("I2C Configuration: {}".format(i2c_dev))
        
    return i2c_dev

i2c = machine.I2C(0, sda=machine.Pin(8), scl=machine.Pin(9), freq=400000)
oled = ssd1306.SSD1306_I2C(128, 64, i2c)

import ntptime
ntptime.settime()

while True:
    hr = utime.localtime() #hora local
    #despliegue de hora de internet
    print("{:02}:{:02}:{:02}".format(hr[3], hr[4], hr[5]))
    utime.sleep(1)
    oled.fill(0)
    oled.text("Hora:", 0, 0)
    oled.text("{:02d}:{:02d}:{:02d}".format(hr[3], hr[4], hr[5]), 0, 16)
    oled.show()
    time.sleep(1)
##
```

### Evidencia de la práctica realizada

Circuito

![](HoraCirc.png)

Corriendo en el OLED

![](Hora1.jpg)

![](Hora2.jpg)
