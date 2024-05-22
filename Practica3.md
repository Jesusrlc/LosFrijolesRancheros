# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/54bc15c3-80d2-42b2-b536-01eb3b7e7a1d)

## Objetivo
Desarrollar un servidor web utilizando el microcontrolador Raspberry Pi Pico W, programado en C/C++, que permita controlar el estado de un LED a través de una interfaz web.

## Requisitos del Proyecto
* **Configuración del Hardware:** Leer documentacion de Pico W del LED interno (no requiere uno externo).
* **Configuración de Red:** Establecer una conexión WiFi con el Pico W para permitir el acceso remoto.
* **Servidor Web:** Implementar un servidor web en el Pico W que pueda recibir comandos a través de HTTP para controlar el LED.
* **Interfaz de Usuario Web:** Crear una página web sencilla con botones para "Encender" y "Apagar" el LED.
* **Control del LED:** Programar la lógica en C/C++ para que el Pico W pueda encender y apagar el LED basándose en las solicitudes recibidas del servidor web.

```CC
/*Problema:
Controlar el estado de un LED a través de una interfaz web.

Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales

  Autor: 
- Almeida Valles Jose de Jesus @Nickname JoseAlmeida21
- Lopez Contreras Jesús Rafael @Nickname Jesusrlc
- Nuño Reyes Gerardo @Nickname GeraNuno

Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/
#include <WiFi.h>

const char* ssid = "SSID_WIFI";             // Reemplaza con tus credenciales de red
const char* password = "CONTRASEÑA";        // Reemplaza con tu contraseña
int Led = 2;

String header;                                      // Variable para almacenar la solicitud HTTP
String estadoPicoLED = "apagado";                        // Variable para almacenar el estado del LED incorporado
unsigned long tiempoActual = millis();               // Tiempo actual
unsigned long tiempoPrevio = 0;                     // Tiempo anterior
const long tiempoEspera = 2000;
WiFiServer servidor(80);                              // Establecer el número de puerto del servidor web en 80

void setup() {
  Serial.begin(115200);                              // Iniciar el Monitor Serial
  pinMode(LED_BUILTIN, OUTPUT);                           // Inicializar el LED como salida
  digitalWrite(LED_BUILTIN, LOW);                           // Apagar el LED
  WiFi.begin(ssid, password);                       // Conectar a la red Wi-Fi con SSID y contraseña
  while (WiFi.status() != WL_CONNECTED) {             // Mostrar progreso en el monitor Serial
    delay(500);
    Serial.print(".");
  }
  Serial.println("");                               // Imprimir dirección IP local y comenzar el servidor web
  Serial.print("WiFi conectado a la dirección IP ");
  Serial.println(WiFi.localIP());
  servidor.begin();                                   // Iniciar el servidor
}

void loop() {
  WiFiClient cliente = servidor.available();   // Escuchar clientes entrantes
  if (cliente) {   // Si un nuevo cliente se conecta
    tiempoActual = millis();
    tiempoPrevio = tiempoActual;
    Serial.println("Nuevo Cliente.");                                                                                            // imprimir un mensaje en el puerto serial
    String lineaActual = "";                                                                                                  // crear una cadena para almacenar los datos entrantes del cliente
    while (cliente.connected() && tiempoActual - tiempoPrevio <= tiempoEspera) { // bucle mientras el cliente esté conectado
      tiempoActual = millis();
      if (cliente.available()) {   // si hay bytes para leer del cliente
        char c = cliente.read();                                                                                           // leer un byte, luego
        Serial.write(c);                                                                                                  // imprimirlo en el monitor serial
        header += c;
        if (c == '\n') {   // si el byte es un carácter de nueva línea
          // si la línea actual está en blanco, tienes dos caracteres de nueva línea seguidos.
          // ese es el final de la solicitud HTTP del cliente, así que envía una respuesta:
          if (lineaActual.length() == 0) { // Las cabeceras HTTP siempre comienzan con un código de respuesta (por ejemplo, HTTP/1.1 200 OK)
            // y un tipo de contenido para que el cliente sepa qué viene, luego una línea en blanco:
            cliente.println("HTTP/1.1 200 OK");
            cliente.println("Content-type:text/html");
            cliente.println("Connection: close");
            cliente.println();
            // Cambiar el LED encendido y apagado
            if (header.indexOf("GET /led/on") >= 0) {
              Serial.println("LED encendido");
              estadoPicoLED = "encendido";
              digitalWrite(LED_BUILTIN, HIGH);
            } else if (header.indexOf("GET /led/off") >= 0) {
              Serial.println("LED apagado");
              estadoPicoLED = "apagado";
              digitalWrite(LED_BUILTIN, LOW);
            }
            // Mostrar la página web HTML
            cliente.println("<!DOCTYPE html><html>");
            cliente.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
            cliente.println("<link rel=\"icon\" href=\"data:,\">");
            // CSS para estilizar los botones de encendido/apagado
            cliente.println("<style>html { font-family: Arial; display: inline-block; margin: 0px auto; text-align: center;}");
            cliente.println(".button { background-color: #800080; border: none; color: white; padding: 16px 40px;");
            cliente.println("text-decoration: none; font-size: 30px; margin: 2px; cursor: pointer;}");
            cliente.println(".button2 {background-color: #000000;}</style></head>");
            // Encabezado de la página web
            cliente.println("<body><h1>Interruptor de Control Web LED Pico W</h1>");
            // Mostrar estado actual y botones ON/OFF para el LED incorporado
            cliente.println("<p>El estado del LED es " + estadoPicoLED + "</p>");
            // Configurar botones
            if (estadoPicoLED == "apagado") {
              cliente.println("<p><a href=\"/led/on\"><button class=\"button\">ENCENDER</button></a></p>");}            // estadoPicoLED está apagado, mostrar el botón ENCENDER
            else {
              cliente.println("<p><a href=\"/led/off\"><button class=\"button button2\">APAGAR</button></a></p>");}  // estadoPicoLED está encendido, mostrar el botón APAGAR
            cliente.println("</body></html>");
            cliente.println();                                                                                     // La respuesta HTTP termina con otra línea en blanco
            break;                                                                                                // Salir del bucle while
          } else {
            lineaActual = "";                                                                                   // si obtuviste una nueva línea, luego borra líneaActual
          }
        } else if (c != '\r') { // si obtuviste algo más que un carácter de retorno de carro,
          lineaActual += c;                                                                                              // agrégalo al final de lineaActual
        }
      }
    }
    header = "";                                                    // Limpiar la variable de encabezado
    cliente.stop();                                                  // Cerrar la conexión
    Serial.println("Cliente desconectado.");                        
    Serial.println("");
  }
}
```
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/3b098e39-78ef-48d5-82e9-ddaac8f6db7c)

![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/65e93bf9-ee3a-47b0-9853-80ebf73eb4de)

