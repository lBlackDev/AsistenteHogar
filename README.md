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
