# Arduino Client for MQTT

This library provides a client for doing simple publish/subscribe messaging with
a server that supports MQTT.
Full API documentation is available here: http://pubsubclient.knolleary.net

#Actualizaciones sobre la versión original de Nicholas O'Leary
http://knolleary.net/arduino-client-for-mqtt/

Implementación de la conexión con clean Session=0.

El principal motivo por el que no se implementa en esta librería el Clean Session = 0 es porque el cliente no tiene forma de almacenar temporalmente aquellos paquetes enviados con QOS >0 que no han sido entregados al servidor.
Según la especificación del protocolo un cliente que inicie una sesión persistente debe de ser capaz de almacenar los paquetes no entregados.

Sin embargo, hay una situación especial como la que se describe en este issue https://github.com/knolleary/pubsubclient/issues/86, un dispositivo con una conexión de red poco fiable en la que se asuma que en los momentos de desconexión los paquetes que envia el dispositivo se pueden perder
pero que queramos poder asegurar que las actualizaciones que envia el servidor van a llegar al dispositivo. 

las funciones de conexión ahora devuelven un int, porque hay varios estados posibles. Según  la especificación del protocolo MQTT3.1.1 cuando se establece una sesión con clean session=0 el broker en el paquete CONNACK informa al cliente si la sesión existe en el servidor. 

Si se inicia una conexión con Clean Session=0 (se ha añadido un último parámetro booleano a la función original) si la función devuelve 0x00 significa que ha conectado correctamente, pero que no hay sesión preexistente en el servidor, por lo que hay que suscribirse de nuevo a los topics.
Si la función devuleve -1, significa que hay sesión en el servidor y no es necesario suscribirse a los topics de nuevo.

para las suscipciones con qos>0 en el momento de la conexión se recibirán las actualizaciones enviadas con qos>0 mientras el dispositivo no estaba conectado.
 

