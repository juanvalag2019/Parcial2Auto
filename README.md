# Automatización Liquidos 1A S.A.

## Contenido
  - [Diseño](#diseño)
  - [Implementación en OpenPLC](#implementación-en-openplc)
  - [Implementación en Arduino](#implementación-en-arduino)

## Diseño
Para la automatización del proceso dentro de la planta se analizó el sistema teniendo en cuenta las condiciones iniciales y el proceso en diferentes estados, planteando el sistema en la imagen 1.

![Diagrama Función Secuencial](analisis.jpg)

Dentro de la imagen en la parte superior derecha se definió los componentes del programa necesarios y sus estados posibles. También, en la parte media izquierda, se planteó el uso de una escala de $\frac{1}{10}$ para los verdaderos valores de los tiempos y flujos en la planta, y para la simulación en Codesys y OpenPLC una escala de 1, lo que implica que si en las simulaciones hay un flujo de 100 litros por segundo el flujo de la planta sería de 10 litros/segundo.

Para el planteamiento del proceso en la planta se propuso que la salida para las motobombas fuera de 10 litros/segundo, el encendido de estas bombas se realiza al mismo tiempo con el fin de sincronizar las dos ramas del proceso y reducir la complejidad, esta salida se usó como base para definir los otros parametros de la planta, por lo tanto, el tiempo de llenado de los dos tanques es de 100 segundos (en 10 segundos 100 litros, 10 veces 10 segundos 1000 litros), luego debido a que las valvulas de los dos tanques de 1000 litros se deben abrir simultaneamente se opta por un flujo de 20 litros por segundo en las dos valvulas, por lo que después de abiertas el contenedor de 2000 litros se llena en 50 segundos (40 litros/segundo en 5 segundos 200 litros, en 50 segundos 2000 litros), al llenarse el contenedor de 2000 litros se inicia el rotor de la mezcladora por 100 segundos, seguidamente se abre la valvula de este contenedor que tiene también un flujo de 20 litros por segundo lo que permite unificar la salida de los 3 contenedores, por lo que el contenedor tarda en desocuparse 100 segundos, (100 litros en 5 segundos, 1000 litros en 50 segundos, 2000 litros en 100 segundos) después de estos 100 segundos el sistema reinicia el proceso y se repite 10 veces porque en cada iteración se mezclan 2000 litros de detergente.

El tiempo del proceso se acumula desde el inicio de las motobombas primero 100 segundos para llenar los dos contenedores de 1000 litros, luego 50 segundos para el llenado del contenedor de 2000 litros, seguidamente el rotor de la mezcladora que dura 100 segundos y por último se desocupa el contenedor de 2000 litros en un lapso de 100 segundos, por lo que el tiempo final del proceso alcanza los 350 segundos.

![Variables OpenPLC](diagramaSeq.png)

## Implementación en OpenPLC

Para la implementación en OpenPLC se plantearon las siguientes variables 

![Variables OpenPLC](VarsOpenPLC.jpeg)

Primero se realizó una configuración Latch para mantener la entrada de Start después de que se presiona el botón start. Luego se añadio un contacto normalmente cerrado para el stop, luego un internal relay (IR1) para el reinicio del proceso cuando se  abre la última valvula, se hace un reset a todas las variables (motores, valvula, mezcladora, IR_STOP) y se establece un internal relay para comenzar a correr los motores (IR2). Luego de que se activa IR2 se da un delay de 500 milisegundos para el arranque de los motores, estos por medio de un on-delay timer (TON) se mantienen encendidos durante 10 segundos simulando la entrada de liquido a los dos contenedores de 1000 Litros, se apagan los motores y se activan las valvulas por medio de otro TON a los 100 milisegundos. Estas valvulas se mantienen abiertas por 5 segundos que simulan el llenado del tanque de 2000 litros, luego se enciende la helice mezcladora y se mantiene durante 10 segundos encendida por medio de otro TON, seguidamente se apaga la helice y se abre la valvula del contenedor de 2000 litros. Finalmente, esta valvula se mantiene encendida por 10 segundos, luego se espera 100 milisegundos para reiniciar el sistema por medio del IR1, cada vez que se abre la última valvula la señal va a un contador que tiene programado 10 ciclos, cuando se llega al decimo se cierran las valvulas, se reinicia el estado de start y stop y se reinicia el contador de ciclos, para quedar listo para otro uso.

Se implementó el diseño en el editor de OpenPLC:

![Ladder OpenPLC](Ladder_Prototipo_Arduino_page-0001.jpg)

## Implementación en Arduino

Para la implementación en Arduino se hizo uso de las 4 salidas digitales que puede utilizar el firmware de OpenPLC, se utilizó dos entradas digitales en los pines 2 y 3 para los botones de inicio y parada respectivamente, con una ubicación en OpenPLC de %IX100.0 y %IX100.1 respectivamente, luego los pines de salida fueron el pin 7 para las motobombas (se activan al mismo tiempo), el pin 8 para la helice de mezclado, el pin 12 para las valvulas de los dos contenedores de 1000 litros y el pin 13 para el contenedor de 2000 litros con una ubicación en OpenPLC de %QX100.0, %QX100.1, %QX100.2 y %QX100.3 respectivamente.

Cada salida se conecto a un LED  con una resistencia de 220$\Omega$, la salida de la helice de mezclado se conectó a un módulo L298N puente H para controlar el motor de la helice.

![Diagrama Circuito](Prototipo%20Arduino%20Dise%C3%B1o%20Circuito%20Electrico_page-0001.jpg)

Esto se implementó en físico teniendo en cuenta el siguiente diagrama: 

![Diagrama TinkerCAD](arduinoTinker.jpeg)
