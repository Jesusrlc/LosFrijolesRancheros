# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/cffe8fdc-38a3-442c-8e5d-97a25d06d977)

### Captura del Momento: Tomando una Foto con el Equipo usando Arduino Tiny Machine Learning Kit (detección e imagen)

## Objetivos
### NOTA: Puede implementarlo con C, CSharp, Python que invoque la camara para que tome la foto.

## General
### Utilizar el Arduino Tiny Machine Learning Kit para integrar habilidades técnicas y colaborativas, culminando en la captura de una foto del equipo como cierre simbólico del semestre.

## Específicos
### Configurar el Arduino Tiny Machine Learning Kit para controlar una cámara.
### Programar el Arduino para que, mediante un botón o sensor, se active la cámara y se tome una foto.
### Demostrar la colaboración y el aprendizaje conjunto a través de una actividad de equipo.

```CC
/*Problema:
 Tomando una Foto con el Equipo usando Arduino Tiny Machine Learning Kit.

Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales

  Autor: 
- Almeida Valles Jose de Jesus @Nickname JoseAlmeida21
- Lopez Contreras Jesús Rafael @Nickname Jesusrlc
- Nuño Reyes Gerardo @Nickname GeraNuno

Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/

/*
  OV767X - Camera Capture Raw Bytes

  This sketch reads a frame from the OmniVision OV7670 camera
  and writes the bytes to the Serial port. Use the Procesing
  sketch in the extras folder to visualize the camera output.

  Circuit:
    - Arduino Nano 33 BLE board
    - OV7670 camera module:
      - 3.3 connected to 3.3
      - GND connected GND
      - SIOC connected to A5
      - SIOD connected to A4
      - VSYNC connected to 8
      - HREF connected to A1
      - PCLK connected to A0
      - XCLK connected to 9
      - D7 connected to 4
      - D6 connected to 6
      - D5 connected to 5
      - D4 connected to 3
      - D3 connected to 2
      - D2 connected to 0 / RX
      - D1 connected to 1 / TX
      - D0 connected to 10

  This example code is in the public domain.
*/

#include <Arduino_OV767X.h>

int bytesPerFrame;

byte data[176 * 144 * 2]; // QVGA: 320x240 X 2 bytes per pixel (RGB565)

void setup() {
  Serial.begin(9600);
  while (!Serial);

  if (!Camera.begin(QCIF, RGB565, 1)) {
    Serial.println("Failed to initialize camera!");
    while (1);
  }

  bytesPerFrame = Camera.width() * Camera.height() * Camera.bytesPerPixel();

  // Optionally, enable the test pattern for testing
   //Camera.testPattern();
}

void loop() {
  Camera.readFrame(data);

  Serial.write(data, bytesPerFrame);
}

```
