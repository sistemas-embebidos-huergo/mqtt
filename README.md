# 🌐 Introducción a MQTT

MQTT (Message Queuing Telemetry Transport) es un **protocolo de mensajería ligero**, diseñado para dispositivos con recursos limitados y redes con poco ancho de banda. Se utiliza principalmente en proyectos de **Internet de las Cosas (IoT)**.

---

## 🧩 Componentes Principales

### 1. 🗣 Publicador (Publisher)
Dispositivo que **envía un mensaje** a un tema (topic).

> Ejemplo: un sensor de temperatura que publica datos cada 5 segundos.

### 2. 👂 Suscriptor (Subscriber)
Dispositivo o software que **escucha** un tema para recibir los mensajes publicados.

> Ejemplo: una app que muestra en pantalla la temperatura recibida.

### 3. 🧠 Broker MQTT
Servidor que **recibe, filtra y distribuye** los mensajes entre publicadores y suscriptores.

> Es como un cartero: recibe un mensaje y lo entrega a quienes están esperando esa información.

---

## 🧵 ¿Qué es un Topic?

Un **topic** es un **canal de comunicación** con un nombre jerárquico, como una dirección de mensaje.

Ejemplos de topics:
- `casa/sala/temperatura`
- `sensor/luz`
- `chat/usuario1`

Un dispositivo publica en un topic, y otro se suscribe a ese mismo topic para recibir el mensaje.

---

## 📬 ¿Cómo Funciona MQTT?

1. Un dispositivo **publica** un mensaje en un **topic** a través del broker.
2. El **broker** recibe el mensaje.
3. Todos los dispositivos **suscritos** a ese topic reciben el mensaje.

Este modelo se llama **publish/subscribe (pub/sub)**.

---

## 🌍 Aplicaciones Comunes

- Domótica (hogares inteligentes)
- Sensores de temperatura, humedad, luz
- Agricultura inteligente
- Comunicación entre ESP32 o Arduinos
- Sistemas de monitoreo industrial
- Chats entre dispositivos

---

## 💡 Ventajas de MQTT

| Ventaja        | Descripción                                                  |
|----------------|--------------------------------------------------------------|
| Ligero         | Usa pocos datos, ideal para redes lentas o inestables        |
| Rápido         | Comunicación en tiempo real                                   |
| Flexible       | Fácil de integrar con muchos tipos de dispositivos            |
| Escalable      | Permite millones de dispositivos conectados                   |
| Confiable      | Diferentes niveles de entrega de mensajes                     |

---

## ⚙️ Características Técnicas Avanzadas

### 🔐 Niveles de Calidad de Servicio (QoS)

- `0`: El mensaje se entrega **una vez máximo**, sin confirmación.
- `1`: El mensaje se entrega **al menos una vez** (puede duplicarse).
- `2`: El mensaje se entrega **exactamente una vez** (más seguro).

### 🔒 Seguridad

- Autenticación: usuario y contraseña
- Cifrado: uso de TLS/SSL para conexiones seguras

### 🌐 Brokers MQTT

- **Públicos**: sin necesidad de instalar nada  
  Ej: `broker.hivemq.com`
  
- **Locales**: instalados en Raspberry Pi o PC  
  Ej: `Mosquitto`
  
- **En la nube**: servicios escalables  
  Ej: AWS IoT, Adafruit IO, Google Cloud IoT

---

## 🧪 Ejemplo de Comunicación entre ESP32

```text
ESP32 A → (publica en topic: "chat/B") → BROKER MQTT → (lo recibe ESP32 B suscrito a "chat/B")
