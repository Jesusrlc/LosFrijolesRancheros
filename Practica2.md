
# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/58d2c889-b52c-43a7-bcbe-d7803e0c97b4)

## La actividad 2 consto en conectar un display a la Raspberry pico w para mostrar un mensaje, primeramente se realizo la simulacion en el programa de wokwi.

*Codigo para el sketch.ino*
```CC
/*Problema:
Mostrar un mensaje en un display conectado a la Pico W
Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales
Autor: Almeida Valles Jose de Jesus @Nickname JoseAlmeida21
Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>


#define SCREEN_WIDTH 128 // Ancho de la pantalla OLED
#define SCREEN_HEIGHT 64 // Altura de la pantalla OLED


// Declaración del objeto display
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire);


void setup() {
  // Iniciar la pantalla OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {  // Asegúrate de que la dirección I2C es correcta (0x3C o 0x3D)
    Serial.println(F("SSD1306 allocation failed"));
    for (;;); // Bucle infinito si falla la pantalla
  }
 
  display.clearDisplay();


  // Dibuja un texto
  display.setTextSize(1);             // Tamaño del texto
  display.setTextColor(SSD1306_WHITE); // Color del texto
  display.setCursor(0,0);             // Posición inicial del texto
  display.println(F("Hola puto"));
  display.display(); // Mostrar todo en la pantalla
}


void loop() {
  // Aquí podrías agregar código para actualizar la pantalla continuamente
}
```
--- 
*Codigo para el diagram.json*
```json
/*Problema:
Mostrar un mensaje en un display conectado a la Pico W
Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales
Autor: Lopez Contreras Jesus Rafael @Nickname Jesusrlc
Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/
{
  "version": 1,
  "author": "Anonymous maker",
  "editor": "wokwi",
  "parts": [
    { "type": "board-pi-pico-w", "id": "pico", "top": 0, "left": 0, "attrs": {} },
    {
      "type": "board-ssd1306",
      "id": "oled1",
      "top": 79.94,
      "left": -153.37,
      "attrs": { "i2cAddress": "0x3c" }
    }
  ],
  "connections": [
    [ "pico:GP0", "$serialMonitor:RX", "", [] ],
    [ "pico:GP1", "$serialMonitor:TX", "", [] ],
    [ "oled1:GND", "pico:GND.1", "black", [ "v0" ] ],
    [ "oled1:VCC", "pico:3V3", "red", [ "v0" ] ],
    [ "oled1:SCL", "pico:GP5", "green", [ "v0" ] ],
    [ "oled1:SDA", "pico:GP4", "green", [ "v0" ] ]
  ],
  "dependencies": {}
}
```
--- 
### Estas fueron las librerias utilizadas
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/ee751196-fa3e-4615-9531-b3c9658621d3)
--- 
### Y este fue el programa simulado.
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/564fd2ed-8a32-42fc-a195-c1aaad944038)

## Una vez realizada la simulacion pasamos a la parte fisica.
*Codigo para arduino*
```CC
/*Problema:
Mostrar un mensaje en un display conectado a la Pico W
Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales
Autor: Nuño Reyes Gerardo @Nickname GeraNuno
Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/
#include <Adafruit_SSD1306.h>
#include <Adafruit_GFX.h>


Adafruit_SSD1306 display(128, 64);


void setup() {
  // Initialize the display
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);  // Address 0x3C for 128x32
  // Clear the display
  display.clearDisplay();
  display.setTextColor(WHITE);
  display.setTextSize(2);
  display.setCursor(0, 0);
  display.println("Raspberry Pi Pico W");
  display.println("hola mundo");
  display.display();
}


void loop() {
  // Nothing to do here, just display the static text
}
```
## Resultado de la actividad 2
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/3d54a296-bc5e-4dee-aa05-ec70a63060b6)
