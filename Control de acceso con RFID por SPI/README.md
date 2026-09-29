# Control de acceso con RFID por SPI

Sistema de control de acceso mediante un lector RFID RC522 comunicado por el bus SPI con un Arduino UNO R4 WiFi. El programa lee el UID de una tarjeta o llavero, lo muestra en el Monitor Serie y determina si el acceso es permitido o denegado comparándolo contra un UID autorizado.

## Descripción

Este proyecto utiliza el lector RC522 para leer el número único (UID) de una tarjeta RFID mediante el bus SPI. Al acercar una tarjeta, el Arduino compara su UID contra uno previamente autorizado y guardado en el código: si coincide, enciende un LED verde durante 2 segundos; si no coincide, enciende un LED rojo por el mismo tiempo. Todo el control de tiempo se maneja con `millis()`, permitiendo que el sistema siga leyendo tarjetas nuevas incluso mientras un LED está encendido, sin quedar bloqueado.

## Objetivos de aprendizaje

Comprender el funcionamiento del bus SPI mediante la comunicación entre un Arduino y un módulo lector RFID RC522, leyendo e identificando el UID de distintas tarjetas, e implementando un control de acceso simple con temporización no bloqueante basada en `millis()`.

## Herramientas y material utilizado

* Arduino UNO R4 WiFi
* Arduino IDE
* Librería MFRC522 (Library Manager)
* Módulo lector RFID RC522
* Tarjeta y llavero RFID
* 2 LEDs (verde y rojo)
* 2 resistencias de 220 Ω
* Protoboard
* Cables de conexión (jumpers)

## Diagrama del circuito

Conexiones del RC522 al Arduino (alimentado con **3.3V**, no 5V):

* SDA (SS) → D10
* SCK → D13
* MOSI → D11
* MISO → D12
* RST → D9
* 3.3V → 3.3V
* GND → GND
* LED verde → D7 (con resistencia de 220 Ω a GND)
* LED rojo → D6 (con resistencia de 220 Ω a GND)

![Diagrama del circuito](Diagrama/Diagrama%20de%20control%20de%20acceso%20RFID%20por%20SPI.jpeg)

### Montaje físico

Prueba con tarjeta no autorizada (LED rojo):

<img src="Diagrama/Foto%20luz%20roja%20Control%20de%20acceso%20con%20RFID%20por%20SPI.jpeg" width="600">

Prueba con tarjeta autorizada (LED verde):

<img src="Diagrama/Foto%20luz%20verde%20Control%20de%20acceso%20con%20RFID%20por%20SPI.jpeg" width="600">

### Evidencia del Monitor Serie

<img src="Diagrama/Serial%20terminal.jpeg" width="600">

## Código

[Control_Acceso_RFID.ino](https://github.com/VHellsings/Sistemas-Programables/blob/main/Control%20de%20acceso%20con%20RFID%20por%20SPI/Codigo/Control_Acceso_RFID.ino)

## Reporte

[Reporte_RFID.pdf](https://github.com/VHellsings/Sistemas-Programables/blob/main/Control%20de%20acceso%20con%20RFID%20por%20SPI/Reporte/Reporte_RFID.pdf)

## Resultados

Durante las pruebas, el lector RC522 fue detectado correctamente al iniciar (comunicación SPI confirmada mediante la lectura del registro de versión del chip). Al acercar la tarjeta autorizada, el sistema mostró "ACCESO PERMITIDO" en el Monitor Serie y el LED verde se encendió, apagándose automáticamente a los 2 segundos sin necesidad de bloquear el programa. Al acercar una tarjeta no autorizada, se mostró "ACCESO DENEGADO" y el LED rojo se encendió por el mismo tiempo. Se comprobó que, incluso con el LED verde encendido tras una lectura válida, el sistema seguía leyendo nuevas tarjetas sin quedarse esperando. Al desconectar el cable MISO (D12) y reiniciar el Arduino, el programa detectó correctamente la falta de comunicación con el lector, mostrando un mensaje de error en lugar de quedarse bloqueado.

## Video del funcionamiento

[Ver video](https://youtu.be/7c_o7p3uvKs)

## Conclusiones

Esta práctica permitió comprender el funcionamiento del bus SPI a través de la comunicación con un módulo lector RFID RC522, identificando cómo cada línea (SCK, MOSI, MISO, SS) cumple un rol distinto en la transferencia de datos. Se reforzó el uso de `millis()` para evitar bloqueos mientras se controla el tiempo de encendido de los LEDs, y se comprobó experimentalmente el efecto de desconectar una línea de datos (MISO), evidenciando la importancia de cada conexión física para el correcto funcionamiento del bus.
