# Comunicación I2C entre 4 Arduinos

Sistema de comunicación por bus I2C entre un Arduino maestro y tres Arduinos esclavos, que comparten las líneas SDA y SCL y se identifican con direcciones distintas (`0x08`, `0x09` y `0x0A`), simulado en Tinkercad y armado de forma física.

## Descripción

Este proyecto implementa una arquitectura de comunicación distribuida basada en el bus I2C (Inter-Integrated Circuit) utilizando el modo de direccionamiento de 7 bits. Un microcontrolador principal configurado como **Maestro** (Controller) gestiona la topología del bus y coordina el tráfico de datos bidireccional hacia tres nodos **Esclavos** (Targets) identificados en las direcciones hexadecimales `0x08`, `0x09` y `0x0A`:

* **Esclavo 1 (`0x08`) - Actuador Digital:** Controla la conmutación de estado de un LED mediante la recepción de comandos discretos transmitidos por el Maestro.
* **Esclavo 2 (`0x09`) - Actuador PWM/Servo:** Recibe tramas numéricas de ángulo (0° a 180°) enviadas por el Maestro e implementa el control de posición del servomotor mediante modulación por ancho de pulsos (PWM).
* **Esclavo 3 (`0x0A`) - Sensor Analógico:** Adquiere lecturas mediante un conversor analógico-digital (ADC) de 10 bits conectado a un potenciómetro y transmite el valor bruto al Maestro tras recibir una petición de lectura (`requestFrom`).

### Dinámica del sistema y tolerancia a fallos
1. **Muestreo temporizado no bloqueante:** Cada 500 ms (gestionados con un temporizador por software mediante `millis()`), el Maestro solicita el dato del ADC al Esclavo 3, realiza un mapeo proporcional (0–1023 a 0°–180°) y transmite el valor calculado al Esclavo 2 para posicionar el servo.
2. **Puente UART a I2C:** El Maestro procesa entradas asíncronas por interfaz UART (Monitor Serie) enviando comandos para conmutar el estado del Esclavo 1.
3. **Gestión de bus y estado:** El Maestro evalúa continuamente las respuestas ACK/NACK en las operaciones `endTransmission()` y `requestFrom()`, notificando en tiempo real si algún nodo no responde y previniendo bloqueos en el flujo de ejecución principal.

## Objetivos de aprendizaje

Comprender el funcionamiento del bus I2C mediante la comunicación entre un maestro y varios esclavos que comparten las mismas líneas de datos, aprendiendo a asignar direcciones distintas a cada dispositivo, a programar el envío y la recepción de datos con la librería Wire, y a detectar fallas de comunicación revisando el resultado de `endTransmission()` y `requestFrom()` en lugar de bloquear el programa.

## Herramientas y material utilizado

* 4 Arduino Uno R4 WiFi
* 1 protoboard (Breadboard Small)
* 1 LED
* 1 resistencia de 470 Ω (LED) y 2 resistencias de 4.7 kΩ (pull-up en SDA y SCL)
* 1 micro servomotor
* 1 potenciómetro
* Cables de conexión
* Librerías Wire y Servo, y Monitor serie del Arduino IDE

## Diagrama del circuito

<img src="Diagrama/Protocolo l2C%20Comunicación%20I2C%20entre%204%20Arduinos.jpeg" alt="Diagrama del circuito" width="100%">

## Montaje físico

<img src="Diagrama/Armado.jpg" alt="Montaje físico del circuito" width="100%">

## Código

* [Maestro.ino](<Codigo/Maestro.ino>)
* [Esclavo1(LED).ino](<Codigo/Esclavo1(LED).ino>)
* [Esclavo2(servo).ino](<Codigo/Esclavo2(servo).ino>)
* [Esclavo3(potenciómetro).ino](<Codigo/Esclavo3(potenciómetro).ino>)

## Reporte

* 📄 [Descargar Reporte (PDF)](<Reporte/Reporte.pdf>)

## Video del funcionamiento

[![Ver Video de Funcionamiento](https://img.youtube.com/vi/xtoZlY5BY0I/hqdefault.jpg)](https://youtu.be/xtoZlY5BY0I)

👉 [**Haz clic aquí para ver el video del funcionamiento en YouTube**](https://youtu.be/xtoZlY5BY0I)

## Resultados

Durante las pruebas, los tres esclavos respondieron correctamente a sus respectivas direcciones sin interferir entre sí: el LED cambió de estado al escribir 1 o 0 en el Monitor serie, el servomotor siguió de forma consistente los cambios del potenciómetro, y el maestro detectó y reportó correctamente cuando algún esclavo dejó de responder (NACK), sin bloquearse en ningún momento gracias al uso de `millis()` en lugar de `delay()`.

## Conclusiones

El bus I2C permite comunicar varios dispositivos usando solo dos líneas (SDA y SCL) más tierra común; agregar un esclavo no requiere pines adicionales, solo una dirección diferente. Cada esclavo debe tener una dirección única, ya que si dos comparten la misma, ambos responden a la vez y los datos se corrompen. El maestro controla la comunicación completa: decide con quién habla, cuándo y si pide o manda datos, mientras el esclavo solo responde cuando se le llama. Revisar el resultado de `endTransmission()` permite detectar cuando un esclavo no contesta (NACK) en lugar de que el sistema se congele, y usar `millis()` en vez de `delay()` en el maestro permite atender el Monitor serie y consultar el potenciómetro sin bloquearse.
