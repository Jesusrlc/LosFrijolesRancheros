![image](https://gist.github.com/assets/105743061/821180ab-b75f-4b7d-8e2a-8b342a1a012b)


---
### Desplegar Mensaje "Hola Mundo!" desde simulador ConsolaSerial

#### Miembros
* Alemida Valles José de Jesús - 21211908
* López Contreras Jesús Rafael - 21211977
* Nuño Reyes Gerardo - 21212008

---
### Código a Compilar y Ejecutar
```cpp
/*
  Programa: Control de Raspberry Pi Pico W
  Autor: [PicoWCierreSemestre]
  Fecha: [30 - 04 - 2024]
  Descripción:
  Este programa inicializa la comunicación serial en una Raspberry Pi Pico W y envía un mensaje de bienvenida.
  Posteriormente, entra en un bucle infinito donde puede agregar más funcionalidades.
  Licencia: []
*/
#include <Arduino.h>

void setup() {
  // Inicializa la comunicación serial a 115200 baudios.
  Serial.begin(9600);
  
  Serial.println("Hola Mundo!");
  // Configuraciones adicionales aquí.
}

void loop() {
  // Código principal que se ejecuta repetidamente.

  // Por ejemplo, podrías agregar lógica para leer sensores, controlar actuadores, etc.
  // Envía un mensaje de bienvenida.
  
  // Retraso mínimo para evitar saturar el bucle.
  delay(2000);
}
```

---
### Prueba Código

![image](https://gist.github.com/assets/105743061/e5369cff-8e0a-4f27-b389-db4ab9dc94f3)

---
