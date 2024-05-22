![image](https://p84.cooltext.com/Rendered/Cool%20Text%20-%20Prctica%205%20458936622821649.png)

## Práctica: Conexión de Raspberry Pi Pico W con ChatGPT y Visualización en OLED

## Objetivo 
Implementar un programa en el Raspberry Pi Pico W que establezca una conexión con ChatGPT, envíe un prompt definido, reciba la respuesta y la muestre. Adicionalmente, el LED del Pico W deberá parpadear cada vez que se reciban tokens de la respuesta.

## Requisitos
* **Configuración del Pico W:** Configurar el Raspberry Pi Pico W para conectarse a Internet.
* **Conexión con ChatGPT:** Establecer una conexión API con ChatGPT para enviar y recibir datos.
* **LED Interactivo:** Programar un LED para que parpadee en sincronización con la llegada de cada token de la respuesta de ChatGPT.

## Código Utilizado
```cpp
#include <WiFi.h>
#include <WiFiClientSecure.h>
#include <ArduinoJson.h>

// Reemplazar con las credenciales de tu red
const char* ssid = "SSID WIFI";
const char* password = "PASSWORD";
// Reemplazar con tu API Key e ID de modelo
const char* api_key = "API KEY";
const char* model_id = "gpt-3.5-turbo-0125";
// Establecer el nombre de host y la URL para la API
const char* host = "api.openai.com";
const char* url = "/v1/chat/completions";

void setup() {
  Serial.begin(115200);
  delay(10);
  
  // Conectar a WiFi
  Serial.println();
  Serial.print("Conectando a ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi conectado");

  // Configurar cliente seguro
  WiFiClientSecure client;
  client.setInsecure();
  client.setTimeout(10000);
  
  // Conectar a la API de ChatGPT
  Serial.print("Conectando a la API...");
  if (!client.connect(host, 443)) {
    Serial.println("falló");
    return;
  }
  Serial.println("conectado");

  // Construir carga útil de la solicitud
  String payload = "{";
  payload += "\"model\": \"gpt-3.5-turbo-0125\",";
  payload += "\"messages\": [";
  payload += "{\"role\": \"user\",";
  payload += "\"content\": \"Quien es monsta x?\"}";
  payload += "],";
  payload += "\"temperature\": 0.7,";
  payload += "\"max_tokens\": 50,";
  payload += "\"n\": 1,";
  payload += "\"stop\": \"\\n\"}";

  // Construir solicitud HTTP
  String request = "POST ";
  request += url;
  request += " HTTP/1.1\r\n";
  request += "Host: ";
  request += host;
  request += "\r\n";
  request += "Authorization: Bearer ";
  request += api_key;
  request += "\r\n";
  request += "Content-Type: application/json\r\n";
  request += "Content-Length: ";
  request += payload.length();
  request += "\r\n\r\n";
  request += payload;

  // Enviar solicitud HTTP
  Serial.println("Enviando solicitud...");
  client.print(request);

  // Esperar respuesta de la API de OpenAI
  Serial.println("Esperando respuesta...");
  while (client.connected()) {
    if (client.available()) {
      String response = client.readString();

      // Convertir respuesta a JSON y extraer contenido de la respuesta generada por ChatGPT
      const char* r = response.c_str();
      JsonDocument respuesta;
      deserializeJson(respuesta, response);
      const char* content = respuesta["choices"][0];

      // Imprimir contenido de la respuesta generada por ChatGPT
      Serial.print(content);
      Serial.print(response);
    }
  }
}

void loop() {
  // Nada que hacer aquí en el bucle principal
}
```

## Resultados
![ChatGPT](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/105743061/cb29555b-3b72-4ac4-864f-cbd1137f6547)
