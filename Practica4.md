![image](https://p84.cooltext.com/Rendered/Cool%20Text%20-%20Prctica%204%20458936176718201.png)

# Control de LED Interno en Pico W mediante Bluetooth y Smartphone

## Objetivo
Desarrollar un programa en C/C++ que permita controlar el LED interno de una Raspberry Pi Pico W a través de conexiones Bluetooth utilizando un smartphone como interfaz de control.

## Requisitos
* **Configuración de Bluetooth:** Establecer y configurar la conexión Bluetooth en el Pico W para permitir la comunicación con el smartphone.
* **Aplicación Smartphone:** Utilizar una aplicación Google Play o AppStore de Apple en smartphone que pueda enviar comandos a través de Bluetooth para controlar el LED.
* **Control del LED:** Implementar la funcionalidad en el Pico W para encender y apagar el LED interno basado en los comandos recibidos del smartphone.
* **Documentación:** Documentar el código fuente para explicar la configuración de Bluetooth y el manejo de eventos de control del LED.

## Código Utilizado
```cpp
/*
Programa: Control de Raspberry Pi Pico W
Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales

  Autor: 
- Almeida Valles Jose de Jesus @Nickname JoseAlmeida21
- Lopez Contreras Jesús Rafael @Nickname Jesusrlc
- Nuño Reyes Gerardo @Nickname GeraNuno

Fecha: 22/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/

#include <BTstackLib.h>  // Biblioteca Bluetooth
#include <SPI.h>  // Biblioteca SPI

static char characteristic_data = 'H';  

void setup(void) {
  Serial.begin(9600);
  pinMode(LED_BUILTIN, OUTPUT);  // LED DE SALIDA

  // Funciones de callback
  BTstack.setBLEDeviceConnectedCallback(deviceConnectedCallback);
  BTstack.setBLEDeviceDisconnectedCallback(deviceDisconnectedCallback);
  BTstack.setGATTCharacteristicRead(gattReadCallback);
  BTstack.setGATTCharacteristicWrite(gattWriteCallback);

  // Base de datos GATT
  BTstack.addGATTService(new UUID("B8E06067-62AD-41BA-9231-206AE80AB551"));  // Se añade un servicio GATT
  BTstack.addGATTCharacteristic(new UUID("f897177b-aee8-4767-8ecc-cc694fd5fcef"), ATT_PROPERTY_READ, "This is a String!");  // Se añade una característica GATT
  BTstack.addGATTCharacteristicDynamic(new UUID("f897177b-aee8-4767-8ecc-cc694fd5fce0"), ATT_PROPERTY_READ | ATT_PROPERTY_WRITE | ATT_PROPERTY_NOTIFY, 0);  // Se añade una característica GATT dinámica

  // Inicia Bluetooth
  BTstack.setup();
  BTstack.startAdvertising();
}

void loop(void) {
  BTstack.loop();
}

void deviceConnectedCallback(BLEStatus status, BLEDevice *device) {
  (void) device;  
  switch (status) {
    case BLE_STATUS_OK:
      Serial.println("Device connected!");  // Indica si el dispositivo esta conectado
      break;
    default:
      break;
  }
}

void deviceDisconnectedCallback(BLEDevice * device) {
  (void) device;  
  Serial.println("Disconnected.");  // Indica si el dispositivo se ha desconectado
}

uint16_t gattReadCallback(uint16_t value_handle, uint8_t * buffer, uint16_t buffer_size) {
  (void) value_handle;  
  (void) buffer_size;  
  if (buffer) {
    Serial.print("gattReadCallback, value: ");
    Serial.println(characteristic_data, HEX);  // Valor de la caractistica Hexadecimal
    buffer[0] = characteristic_data;  

    // Control del LED
    if (characteristic_data == '1')
      digitalWrite(LED_BUILTIN, HIGH);  // LED ENCENDIDO
    else if (characteristic_data == '0')
      digitalWrite(LED_BUILTIN, LOW);   // LED APAGADO
  }
  return 1;  
}

int gattWriteCallback(uint16_t value_handle, uint8_t *buffer, uint16_t size) {
  (void) value_handle;  
  (void) size;  
  characteristic_data = buffer[0];
 
  // Controla del LED
  if (characteristic_data == '1')
    digitalWrite(LED_BUILTIN, HIGH);  // LED ENCENDIDO
  else if (characteristic_data == '0')
    digitalWrite(LED_BUILTIN, LOW);   // LED APAGADO

  Serial.print("gattWriteCallback , value ");
  Serial.println(characteristic_data, HEX);  // Valor de la caractistica Hexadecimal
  return 0;
}
```

## Resultados
![Bluethoot1](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/105743061/67ffd2af-a5d1-4bb3-8ad9-f14ddc456946)

---

![Bluethoot2](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/105743061/482772b6-6b64-4a4f-b153-94a2c0610062)
