# Semáforo Vehicular y Peatonal con Máquina de Estados Finitos

Sistema de semáforo con luces vehiculares (rojo, amarillo, verde), luces peatonales (rojo, verde) y un botón de solicitud de cruce, controlado mediante una máquina de estados finitos (FSM) en Arduino UNO R4 WiFi.

## Descripción

Este proyecto implementa el control de un semáforo vehicular y peatonal utilizando una máquina de estados finitos programada en Arduino. El sistema alterna automáticamente entre los estados de un semáforo convencional y atiende, de forma segura, la solicitud de cruce peatonal hecha mediante un botón. Toda la temporización se maneja con `millis()`, sin usar `delay()` en ningún punto del programa.

## Objetivos de aprendizaje

Modelar un sistema secuencial como una máquina de estados finitos (FSM) utilizando `enum class`, implementando transiciones controladas por tiempo y por eventos externos (botón), con antirrebote por software y reglas de prioridad para atender la solicitud peatonal sin interrumpir de forma insegura el ciclo vehicular.

## Herramientas y material utilizado

* Arduino UNO R4 WiFi
* Arduino IDE
* Protoboard
* LEDs: rojo, amarillo y verde (vehicular) y rojo/verde (peatonal)
* Resistencias limitadoras de corriente
* Pulsador (botón) para la solicitud peatonal
* Cables de conexión (jumpers)

## Diagrama del circuito

![Diagrama del circuito](Diagrama/Diagrama%20Semaforo.jpeg)

### Montaje físico

<img src="Diagrama/Semaforo.jpeg" width="600">

## Código

[Semaforo.ino](https://github.com/VHellsings/Sistemas-Programables/blob/main/Semaforo/codigo/Semaforo.ino)

## Reporte

[Reporte_Semaforo_v2.pdf](https://github.com/VHellsings/Sistemas-Programables/blob/main/Semaforo/Reporte/Reporte_Semaforo.pdf)

## Video del funcionamiento

[Ver video](https://youtu.be/K3NXqwSUx-Y)

## Conclusiones

Esta práctica permitió aplicar el patrón de máquina de estados finitos a un problema real de control secuencial, combinando temporización no bloqueante con `millis()` y eventos externos asíncronos (el botón peatonal). Se reforzó la separación de responsabilidades entre lectura de entradas, lógica de transición y aplicación de salidas, así como el diseño de reglas de seguridad y prioridad dentro de una FSM.
