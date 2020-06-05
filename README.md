# Configuración de relojes - STM32L476

## ¿Qué es un reloj y para qué se necesita en un microcontrolador (uC)?

En pocas palabras, el reloj es lo que marca el ritmo de toda la ejecución de código dentro del procesador. 

Un uC es, básicamente, un autómata que evoluciona al ritmo de una señal cuadrada o “reloj”. Cada vez que la señal del reloj transita, hace que cambien los flip flops y otros elementos del circuito, haciendo que la máquina avance al siguiente estado. Para tener reloj, la mayoría de los uC modernos suelen incorporar uno o varios osciladores internos para tener parte del oscilador externamente mediante redes tipo R-C o cristales de cuarzo-cerámicos-sintéticos. Es habitual que coexistan varios osciladores simultáneos, por ejemplo, un oscilador para la CPU y un oscilador para el reloj de tiempo real con un cristal de 32 kHz. O también un oscilador de baja precisión interno (alrededor del 2 %) y uno de buena exactitud externo con cristal de cuarzo.

## ¿Por qué aprender a configurar los relojes?

El control de los relojes es primordial para poder controlar el ritmo al que se ejecuntal los programas, la velocidad de los temporizadores, la velocidad de las transmisiones, etc. Además del reloj en sí, esto implica conocer y controlar distintos tipos de osciladores. 

Desde el punto de vista energético, a mayor velocidad y mayor precision habrá mayor consumo y se requerirá más tiempo para despertar el oscilador (la puesta en marcha), mientras que relojes de baja velocidad y poca precisión incorporados requerirán menos energía y permiten un despertar rápido (una consideración importante la aplicación que se desarrolla funciona con baterías).

## Manos a la obra 

En esta práctica se propone probar el efecto del uso de distintos modos de reloj del sistema y oscilador en un microcontrolador de ultra-bajo consumo.

El STM32L476 Nucleo-64 cuenta con 3 relojes internos y 2 externos (de los cuales uno NO viene soldado en la Nucleo). Los relojes externos tienden a ser más precisos que los internos pero ocupan más espacio.

Tiene 4 fuentes de reloj que se pueden utilizar para controlar el reloj maestro del sistema (SYSCLK): 

 * HSI -> High Speed Internal de 16MHz (±1%).
 * MSI -> Multi-Speed Internal de valores predeterminados desde 100kHz hasta 48MHz (±0.25).
 * HSE -> High Speed External (no está disponible en la Nucleo) de 4-48 MHz.
 * PLL -> Phase Locked Loop. Es un sistema de control que genera una señal de salida cuya fase está relacionada con la fase de una señal de "referencia" de entrada. Esto permite que ciertas secciones del microcontrolador funcionen más rápido que otras, o que el microcontrolador funcione a una frecuencia de reloj más rápida que el oscilador mismo.
 
Adicionalmente, cuenta con 2 fuentes de reloj de ultra baja potencia que se pueden usar para manejar los controladores LCD y relojes en tiempo real:

* Cristal externo de baja velocidad (LSE) de 32.768 kHz.
* RC interno de baja velocidad (LSI) de 32 kHz, también utilizado para controlar el watchdog (perro guardián) independiente (±5%).

La frecuencia máxima del reloj del sistema es de 80 MHz. Después de reiniciar el sistema, el oscilador MSI, en 4 MHz, se selecciona como reloj del sistema.

### ¿Cómo lo hago?

En este pequeño ejercicio se mostrará cómo seleccionar MSI a 8MHz como reloj maestro del sistema.



