# Práctica Guiada: Chat entre dos ESP32

## Objetivo
Lograr que dos placas **ESP32** se comuniquen entre sí a través de un **chat básico**, usando el protocolo **MQTT**. Cada placa podrá **enviar y recibir mensajes**, simulando una conversación a través de una red Wi-Fi.

## Formato
Los mensajes enviados deben seguir la siguiente convención: `<apellido>: <mensaje>`. De esta manera quien lo reciba podrá identificar quien le está escribiendo.

## Conceptos que vas a trabajar

- Comunicación `Wi-Fi` con ESP32  
- Protocolo `MQTT ` 
- Publicación y suscripción a `topics`  
- Lectura y escritura desde el Monitor Serial  


## Pasos
### Paso 1: Preparar el entorno

1. Abrir el **Arduino IDE**.
2. Asegurarse de tener instalado:
   - La **placa ESP32** en el Administrador de placas.
   - La **librería `PubSubClient`** desde el Administrador de Bibliotecas (`Sketch > Include Library > Manage Libraries`).
3. Conectar cada ESP32 la computadora con un cable USB.



### Paso 2: Definir la red Wi-Fi y el broker MQTT

1. Buscar el nombre (SSID) y la contraseña de tu red Wi-Fi.
2. Vamos a usar un **broker público** para esta práctica:  
   - Dirección: `broker.hivemq.com`  
   - Puerto: `1883`
   - Tópico (nombre del chat): `huergo/sistemas-embebidos/<apellido>`



### Paso 3: Programar el ESP32

El ESP32 A será el **primer participante del chat**. Su tarea es:

- Conectarse al Wi-Fi.  
- Conectarse al broker MQTT.  
- **Publicar mensajes** en un topic específico.  
- **Escuchar mensajes** desde otro topic.  
- Mostrar los mensajes en el Monitor Serial.  



#### ¿Qué estructura debe tener el programa?

El programa debe tener estas partes básicas:

1. Conexión Wi-Fi  
2. Configuración del broker MQTT  
3. Suscripción a un topic  

#### 1. Conectar al Wi-Fi

Es necesario escribir una función que conecte el ESP32 a una red Wi-Fi:

```cpp
#include <WiFi.h>

// Conectarse a una red por nombre + contraseña
WiFi.begin(<ssid>,<password>);

if (WiFi.status() != WL_CONNECTED){
  // aun no esta conectado
}
// ok
```
### 2. Configurar el broker MQTT
```cpp
#include <PubSubClient.h>

WiFiClient espClient;
// Creamos un cliente de MQTT con la conexión a internet via wifi
PubSubClient client(espClient);

// Realizamos la conexión con el servidor
client.setServer(<server_de_mqtt>,<puerto_de_mqtt>);
```

### 3. Conectarse a un tópico
```cpp
client.connect(<un_id_que_nos_identifique>) // devuelve un booleano indicando si se conecto correctamente

client.connected(); // devuelve un booleano que nos permite chequear si la conexión fue exitosa

client.subscribe(<nombre_del_topico>); // seguir patron del topico a escuchar
```

### 4. Leer del monitor serial el mensaje a enviar
```cpp
if (Serial.available()) {
  String msg = Serial.readStringUntil('\n');
}
```
### 5. Enviar un mensaje a un chat
```cpp
  client.publish(<topico>, <mensaje>); // convertir el mensaje mediante .c_str()
```

### 6. Recibir mensajes
```cpp
// Callback (una función con una firma particular) que respondera ante un nuevo mensaje recibido
void callback(char* topic, byte* payload, unsigned int length) {
  // Iteramos el payload
  for (int i = 0; i < length; i++) {
    
  }
  Serial.println();
}

// Configuramos el cliente con el callback correcto
client.setCallback(callback);
```

### 7. Esuchar por nuevos mensajes
En caso de encontrar un nuevo mensaje en el topico al que estamos subscritos, ejecutará el callback que ya seteamos
```cpp
client.loop();
```
