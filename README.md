Aquí tienes el contenido en formato Markdown:

---

# Asistente de Hogar para ESP32 - Adafruit - IFTTT

### Control On/Off con Google Assistant - Adaptado para ESP32

---

## Requisitos de Componentes

- Protoboard
- ESP32
- LED
- Resistencias

## Ejemplo de Conexión Física

1. Coloca el **LED** en la protoboard.
2. Conecta una **resistencia** a la pata más corta (cátodo) del LED.
3. En el otro extremo de la resistencia:
   - Coloca un **jumper M-H** y conecta el pin **D23** del ESP32.
4. Conecta el pin **GND** del ESP32 a la otra pata del LED.

---

## Código

```cpp
/*
 * Control On/Off con Google Assistant - Adaptado para ESP32
 */

#include <WiFi.h>                   // Librería Wi-Fi para ESP32
#include "Adafruit_MQTT.h"
#include "Adafruit_MQTT_Client.h"

// Configuración de Wi-Fi
#define WIFI_SSID "nombre_del_wifi"   // Nombre del WiFi
#define WIFI_PASS "clave_del_wifi"    // Clave del WiFi

// Configuración de Adafruit IO MQTT
#define MQTT_SERV "io.adafruit.com"
#define MQTT_PORT 1883
#define MQTT_NAME "nombre_usuario_adafruit"   // Nombre de usuario de Adafruit IO
#define MQTT_PASS "clave_adafruit_io"         // Clave de Adafruit IO, obtenida en la llave de la esquina derecha de la página

WiFiClient client;
Adafruit_MQTT_Client mqtt(&client, MQTT_SERV, MQTT_PORT, MQTT_NAME, MQTT_PASS);

// Suscripción al tópico "onoff" en Adafruit IO
Adafruit_MQTT_Subscribe onoff = Adafruit_MQTT_Subscribe(&mqtt, MQTT_NAME "/feeds/nombredelfeed"); // Tópico del feed

// Configuración del pin para la luz
#define LIGHT_PIN 23  // Pin D23 en el ESP32

void setup()
{
  Serial.begin(9600);

  // Conexión a la red Wi-Fi
  Serial.print("\n\nConectando a WiFi... ");
  WiFi.begin(WIFI_SSID, WIFI_PASS);

  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }

  Serial.println(" Conectado a WiFi!");

  // Suscribirse al tópico "onoff" para recibir mensajes
  mqtt.subscribe(&onoff);

  pinMode(LIGHT_PIN, OUTPUT);  // Configuración del pin de la luz como salida
}

void loop()
{
  // Conectar/reconectar a MQTT si es necesario
  MQTT_connect();

  // Leer los mensajes de la cola de suscripciones (hasta que se agoten o se esperen 5 segundos)
  Adafruit_MQTT_Subscribe *subscription;
  while ((subscription = mqtt.readSubscription(5000)))
  {
    if (!strcmp((char*) onoff.lastread, "ON"))
    {
      digitalWrite(LIGHT_PIN, HIGH);  // Enciende la luz (lógica activa baja)
      Serial.println("Encendiendo la luz...");
      Serial.println(digitalRead(LIGHT_PIN) == LOW ? "Pin en estado LOW" : "Pin en estado HIGH");
    }
    else
    {
      digitalWrite(LIGHT_PIN, LOW);  // Apaga la luz
      Serial.println("Apagando la luz...");
      Serial.println(digitalRead(LIGHT_PIN) == HIGH ? "Pin en estado HIGH" : "Pin en estado LOW");
    }
  }

  // Mantener viva la conexión MQTT
  if (!mqtt.ping())
  {
    mqtt.disconnect();
  }
}

/* Función para gestionar la conexión MQTT */
void MQTT_connect() 
{
  int8_t ret;

  // Si ya está conectado, salir de la función
  if (mqtt.connected())
  {
    return;
  }

  Serial.print("Conectando a MQTT... ");

  uint8_t retries = 3;
  while ((ret = mqtt.connect()) != 0)  // connect retorna 0 si la conexión fue exitosa
  { 
    Serial.println(mqtt.connectErrorString(ret));
    Serial.println("Reintentando conexión MQTT en 5 segundos...");
    mqtt.disconnect();
    delay(5000);  // Espera 5 segundos antes de reintentar
    retries--;
    if (retries == 0) 
    {
      // Si se acaban los intentos, espera a que el sistema se reinicie
      while (1);
    }
  }

  Serial.println("Conectado a MQTT!");
}
```

---

## Configuración en Adafruit IO

1. Crea una cuenta gratuita en [Adafruit IO](https://io.adafruit.com/).
2. Dirígete a **Feeds** y crea uno nuevo.
   - Coloca el nombre del feed (este nombre será el tópico).
3. Dirígete a **Dashboard** y crea uno nuevo.
4. En el dashboard, crea un bloque (widget) y deja las configuraciones por defecto.

---

## Configuración en IFTTT

1. Crea una cuenta en [IFTTT](https://ifttt.com/).
2. Crea un nuevo **Applet**:
   - Usa el servicio de **Google Assistant** (conéctalo y da permisos).
   - Agrega el servicio de **Adafruit** (conéctalo y da permisos también).

---
