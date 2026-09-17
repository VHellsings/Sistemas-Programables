# Interrupciones Externas y Buffer Circular con Arduino UNO R4 WiFi

Sistema de detección de eventos mediante un pulsador (simulando el sensor de piezas de una banda transportadora), utilizando una interrupción externa, un buffer circular de tamaño fijo y una animación no bloqueante en la matriz de LEDs integrada del Arduino UNO R4 WiFi.

## Descripción

Este proyecto implementa el registro de eventos generados de manera rápida e impredecible mediante un pulsador conectado al pin 2 del Arduino, configurado como interrupción externa. Cada evento detectado se almacena en un buffer circular de 16 posiciones, evitando pérdidas de información mientras el programa principal mantiene, de forma simultánea y sin bloquear la ejecución, una animación en la matriz de LEDs integrada. Toda la temporización se maneja con millis() y micros(), sin usar delay() dentro de la lógica de detección o animación.

## Objetivos de aprendizaje

Comprender el funcionamiento de las interrupciones externas en Arduino, utilizando un pulsador como simulación de un sensor de piezas para registrar eventos mediante una rutina de interrupción (ISR), implementando un buffer circular de tamaño fijo con filtro de rebote por software, y aplicando millis() para ejecutar una animación no bloqueante, comprendiendo el uso de contadores de escritura y lectura para diferenciar entre un buffer vacío y uno lleno.

## Herramientas y material utilizado

* Arduino UNO R4 WiFi
* Arduino IDE
* Matriz de LEDs integrada en el Arduino UNO R4 WiFi
* Protoboard
* Pulsador (botón) para la simulación del sensor de piezas
* Cables de conexión (jumpers)

## Diagrama del circuito

![Diagrama del circuito](Diagrama/Diagrama%20buffer.jpeg)

## Montaje físico

![Armado](Diagrama/Foto%20buffer.jpeg)

## Evidencia del Monitor Serial

![Terminal](Diagrama/Terminal%20Buffer.jpeg)

## Código

[Codigo Buffers circulares.ino](https://github.com/VHellsings/Sistemas-Programables/blob/main/Buffers%20circulares/Codigo/Codigo%20Buffers%20circulares.ino)

## Reporte

[Reporte.pdf](https://github.com/VHellsings/Sistemas-Programables/blob/main/Buffers%20circulares/Reporte/Reporte.pdf)

## Resultados

Durante las pruebas, el sistema cumplió con el comportamiento esperado: el pulsador generó una interrupción externa al detectarse el flanco de bajada correspondiente, registrando de inmediato el instante del evento (micros()/millis()) en el buffer circular sin necesidad de detener la animación de la matriz de LEDs; el filtro de rebote de 50 ms evitó registros múltiples por una sola pulsación mecánica; los contadores escritos y leídos permitieron distinguir correctamente entre un buffer vacío y uno lleno, y el mecanismo de detección de desbordamiento identificó y notificó por el monitor serie los casos en que se generaron más eventos de los que podían almacenarse antes de ser procesados. La animación de la matriz continuó ejecutándose de forma ininterrumpida durante todas las pruebas, validando así el funcionamiento no bloqueante del sistema.

## Video del funcionamiento

[Ver video](https://youtu.be/YXpjNMaJI1U)

## Conclusiones

Esta práctica permitió comprender cómo realizar varias tareas al mismo tiempo utilizando una programación no bloqueante. La animación de la matriz continúa funcionando mientras el Arduino detecta y procesa las pulsaciones del botón, mostrando cómo las interrupciones, millis() y el buffer circular pueden trabajar juntos para responder a eventos sin detener el programa.
