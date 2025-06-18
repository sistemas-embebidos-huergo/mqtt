# Simulación de Smart Home con ESP32 y MQTT

## Objetivo
Simular un sistema de iluminación inteligente en el hogar utilizando dos placas ESP32 conectadas a una red WiFi y comunicadas mediante el protocolo MQTT. Una placa funcionará como panel de control inteligente (pulsadores y sensor de luz) y la otra como controlador de luces (con LEDs).

## Requisitos
### Hardware
- 2 placas ESP32
- 4 pulsadores con resistencias (pull-down o pull-up)
- 4 LEDs con resistencias
- 1 sensor de luz (LDR + resistencia)
- Jumpers, protoboard
- Fuente de alimentación (USB o fuente externa)

### Software
- Arduino IDE
- Librerías:
  - WiFi.h
  - PubSubClient.h
- Broker MQTT en la nube (ej. HiveMQ, Adafruit IO)

## Circuito 1: Panel de Control Inteligente
Este circuito simula un dispositivo de control con teclas inteligentes y detección de oscuridad.

### Componentes y funciones:
- 4 pulsadores: cada uno publica en un tópico distinto, simulando interruptores inteligentes.
- 1 LDR (sensor de luz): si detecta poca luz, publica un mensaje para encender todas las luces.

### Tópicos sugeridos para publicar:
- casa/luz/living
- casa/luz/cocina
- casa/luz/dormitorio
- casa/luz/baño
- casa/luz/todas

### Pasos:
1. Configurar WiFi y MQTT.
2. Configurar pulsadores con `digitalRead()`. Cuando se presione un botón, publicar `"ON"`/`"OFF"` en el tópico correspondiente.
3. Leer el valor del LDR usando `analogRead()`. Si el valor cae por debajo de cierto umbral (ej. < 500), publicar `"ON"`/`"OFF"` en el tópico casa/luz/todas.

> Ayuda: Es probable que se necesite mantener el estado de cada luz para saber que significa cada pulsación, es decir: si pulso el boton y anteriormente estaba prendida, quiere decir que debo enviar un `"OFF"` caso contrario un `"ON"`. Por eso se sugiere tener un struct global para ir modificando y saber cual es el estado actual.
> 


## Circuito 2: Controlador de Luces
Este circuito simula la parte de la casa que enciende o apaga luces físicas (LEDs) según los mensajes que recibe.

### Componentes:
- 4 LEDs conectados a salidas digitales del ESP32.
- Tópicos a escuchar (subscribirse):
  - casa/luz/living
  - casa/luz/cocina
  - casa/luz/dormitorio
  - casa/luz/baño
  - casa/luz/todas

### Pasos:
1. Configurar WiFi y MQTT.
2. Subscribirse a los tópicos anteriores.
3. En el callback del MQTT, verificar el tópico recibido y encender el LED correspondiente si el mensaje es `"ON"` / `"OFF"`
4. Si se recibe el mensaje `"ON"` en casa/luz/todas, encender todos los LEDs, de lo contrario encender la correspondiente. Mismo concepto para apagar.
