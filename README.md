Asistente de Hogar para ESP32 - Adafruit - IFTTT
Control On/Off con Google Assistant - Adaptado para ESP32
Integrantes
Rolando Aldana
Luis Alvarado
Profesor
Ignacio 
Asignatura
IOT

Requisitos de Componentes
Protoboard
ESP32
LED
Resistencias
Ejemplo de Conexión Física
Coloca el LED en la protoboard.
Conecta una resistencia a la pata más corta (cátodo) del LED.
En el otro extremo de la resistencia:
Coloca un jumper M-H y conecta el pin D23 del ESP32.
Conecta el pin GND del ESP32 a la otra pata del LED.

Configuración en Adafruit IO
Crea una cuenta gratuita en Adafruit IO.
Dirígete a Feeds y crea uno nuevo.
Coloca el nombre del feed (este nombre será el tópico).
Dirígete a Dashboard y crea uno nuevo.
En el dashboard, crea un bloque (widget) y deja las configuraciones por defecto.
Configuración en IFTTT
Crea una cuenta en IFTTT.
Crea un nuevo Applet:
Usa el servicio de Google Assistant (conéctalo y da permisos).
Agrega el servicio de Adafruit (conéctalo y da permisos también).
