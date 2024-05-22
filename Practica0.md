# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/5bacbb13-79a1-4587-9b27-0f562258d38a)
## Objetivo

El objetivo de este proyecto es desarrollar una aplicación en C/C++ que permita encender un dispositivo Raspberry Pi y desplegar un mensaje en él.

## En esta practica nos familiarizamos tanto con wokwi como con la ide de Arduino con la cual hicimos que un led parpadeara.
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/9be3d65e-46a1-4fd6-91ac-0f7ad55923f4)

![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/b9e21891-867d-475e-9a65-af20b4f483ad)

*Este es el codigo para que parpadee un led*
```cpp
/*
Programa: Control de Raspberry Pi Pico W
Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales

  Autor: 
- Almeida Valles Jose de Jesus @Nickname JoseAlmeida21
- Lopez Contreras Jesús Rafael @Nickname Jesusrlc
- Nuño Reyes Gerardo @Nickname GeraNuno

Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/

void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);

  // Inicializa la comunicación serial a 115200 baudios.
  Serial.begin(9600);
  
  Serial.println("Hola Mundo!");
  // Configuraciones adicionales aquí
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
  delay(1000);                      // wait for a second
  digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
  delay(1000);                      // wait for a second
}

```

### Prueba Código

![image](https://gist.github.com/assets/105743061/e5369cff-8e0a-4f27-b389-db4ab9dc94f3)

---
