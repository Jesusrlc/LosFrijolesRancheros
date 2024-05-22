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

const char* ssid = "WIFI SSID";             // Replace with your network credentials
const char* password = "PASSWORD";          // Replace with your password
int Led=2;

String header;                                      // Variable to store the HTTP request
String picoLEDState = "off";                        // Variable to store onboard LED state
unsigned long currentTime = millis();               // Current time
unsigned long previousTime = 0;                     // Previous time
const long timeoutTime = 2000;
WiFiServer server(80);                              // Set web server port number to 80
void setup()
{
  Serial.begin(115200);                              // Start Serial Monitor
  pinMode(LED_BUILTIN, OUTPUT);                           // Initialize the LED as an output
  digitalWrite(LED_BUILTIN, LOW);                           // Set LED off
  WiFi.begin(ssid, password);                       // Connect to Wi-Fi network with SSID and password
  while (WiFi.status() != WL_CONNECTED)             // Display progress on Serial monitor
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");                               // Print local IP address and start web server
  Serial.print("WiFi connected at IP Address ");
  Serial.println(WiFi.localIP());
  server.begin();                                   // Start Server
}

void loop()
{
  WiFiClient client = server.available();   // Listen for incoming clients
  if (client)
    {   // If a new client connects,
        currentTime = millis();
        previousTime = currentTime;
        Serial.println("New Client.");                                                                                            // print a message out in the serial port
        String currentLine = "";                                                                                                  // make a String to hold incoming data from the client
        while (client.connected() && currentTime - previousTime <= timeoutTime)
            { // loop while the client's connected
              currentTime = millis();
              if (client.available())
                {   // if there's bytes to read from the client,
                    char c = client.read();                                                                                           // read a byte, then
                    Serial.write(c);                                                                                                  // print it out the serial monitor
                    header += c;
                    if (c == '\n')
                      {   // if the byte is a newline character
                          // if the current line is blank, you got two newline characters in a row.
                          // that's the end of the client HTTP request, so send a response:
                          if (currentLine.length() == 0)
                              { // HTTP headers always start with a response code (e.g. HTTP/1.1 200 OK)
                                // and a content-type so the client knows what's coming, then a blank line:
                                client.println("HTTP/1.1 200 OK");
                                client.println("Content-type:text/html");
                                client.println("Connection: close");
                                client.println();
                                // Switch the LED on and off
                                if (header.indexOf("GET /led/on") >= 0)
                                  { Serial.println("LED on");
                                    picoLEDState = "on";
                                    digitalWrite(LED_BUILTIN, HIGH);}
                                else if (header.indexOf("GET /led/off") >= 0)
                                  { Serial.println("LED off");
                                    picoLEDState = "off";
                                    digitalWrite(LED_BUILTIN, LOW);}
                                // Display the HTML web page
                                client.println("<!DOCTYPE html><html>");
                                client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
                                client.println("<link rel=\"icon\" href=\"data:,\">");
                                // CSS to style the on/off buttons
                                client.println("<style>html { font-family: Arial; display: inline-block; margin: 0px auto; text-align: center;}");
                                client.println(".button { background-color: #800080; border: none; color: white; padding: 16px 40px;");
                                client.println("text-decoration: none; font-size: 30px; margin: 2px; cursor: pointer;}");
                                client.println(".button2 {background-color: #000000;}</style></head>");
                                // Web Page Heading
                                client.println("<body><h1>Pico W LED Web Control Switch</h1>");
                                // Display current state, and ON/OFF buttons for Onboard LED
                                client.println("<p>LED state is " + picoLEDState + "</p>");
                                // Set buttons
                                if (picoLEDState == "off")
                                  { client.println("<p><a href=\"/led/on\"><button class=\"button\">ON</button></a></p>");}            //picoLEDState is off, display the ON button
                                else
                                  { client.println("<p><a href=\"/led/off\"><button class=\"button button2\">OFF</button></a></p>");}  //picoLEDState is on, display the OFF button
                                client.println("</body></html>");
                                client.println();                                                                                     // The HTTP response ends with another blank line
                                break;                                                                                                // Break out of the while loop
                             }
                          else
                              { currentLine = "";}                                                                                   // if you got a newline, then clear currentLine          
                      }
                    else if (c != '\r')
                      { // if you got anything else but a carriage return character,
                        currentLine += c; }                                                                                            // add it to the end of the currentLine
                }
           }
          header = "";                                                    // Clear the header variable
          client.stop();                                                  // Close the connection
          Serial.println("Client disconnected.");                        
          Serial.println("");
      }
}

```
