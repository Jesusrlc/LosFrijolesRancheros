# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/5bacbb13-79a1-4587-9b27-0f562258d38a)
## La practica 0 en un principio fue el aprender sobre el sistema de wokwi el cual es una herramienta consistente en un simulador de proyectos Arduino, que también sirve para microcontroladores ESP32 y STM32.
## En esta practica nos familiarizamos tanto con wokwi como con la ide de Arduino con la cual hicimos que un led parpadeara.
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/9be3d65e-46a1-4fd6-91ac-0f7ad55923f4)

![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/b9e21891-867d-475e-9a65-af20b4f483ad)

*Este es el codigo para que parpadee un led*
```cpp
/*Problema:
Hacer que un led parpadeara
Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales
Autor: Almeida Valles Jose de Jesus @Nickname JoseAlmeida21
Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
  delay(1000);                      // wait for a second
  digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
  delay(1000);                      // wait for a second
}

