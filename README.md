# Información del Proyecto

**Nombre del Proyecto:** Emulador de Mouse Bluetooth HID con ESP32  

**Autores:**  
- **[2061110]** Erick Alejandro Francisco Baltazar  
- **[2057902]** Omar Santiago De León Salinas  
- **[2056820]** Walter Giovani Benavente Tapia  

**Docente:** M.C. Héctor Hugo Flores Moreno  
**Equipo:** 10  
**Materia:** Laboratorio de Controladores y Microcontroladores Programables  

---

## Descripción

Este proyecto consiste en el diseño, desarrollo e implementación de un dispositivo periférico inalámbrico basado en el microcontrolador **ESP32**, capaz de actuar como un dispositivo de interfaz humana (**HID**).

El objetivo principal es emular las funciones de un mouse convencional utilizando componentes electrónicos externos, permitiendo que el usuario controle computadoras, tabletas o teléfonos inteligentes mediante **Bluetooth Low Energy (BLE)**. Al configurarse como un dispositivo HID, el sistema es reconocido automáticamente por distintos sistemas operativos sin necesidad de instalar controladores adicionales.

La arquitectura del sistema combina entradas analógicas y digitales para recrear una experiencia de navegación inalámbrica. Un joystick analógico controla el movimiento del cursor y varios pulsadores ejecutan acciones como clic izquierdo, clic derecho, retroceso y avance.

---

## Estructura del Proyecto

```text
├── README.md
├── codigo
│   └── codigo_fuente.ino
├── diagramas
│   ├── diagrama_pictorico.png
│   ├── diagrama_bloques.png
│   └── diagrama_esquematico.png
└── actividades
    ├── actividad_6.pdf
    ├── actividad_7.pdf
    └── actividad_8.pdf

# Representación Gráfica

## Act. 6: Diagrama Pictórico
Este diagrama muestra la conexión física real de los componentes.

<img width="1536" height="1024" alt="Diagrama Pictorico" src="https://github.com/user-attachments/assets/2d741794-3795-4a26-8a4d-2a879ee10a10" />

Descripción: Este diagrama muestra la conexión física de los pulsadores y el joystick a los pines GPIO del ESP32 sobre una protoboard, funcionando como una guía visual del montaje del hardware. En él se puede observar la distribución de los componentes, así como las conexiones de alimentación y de datos necesarias para que el sistema opere correctamente. Además, facilita la comprensión de cómo interactúan físicamente el ESP32 y los dispositivos de entrada dentro del proyecto.


## Act. 7: Diagrama de Bloques

Este diagrama es una representación gráfica de lo que se realizó en nuestro proyecto.

<img width="1023" height="355" alt="Diagrama de Bloques" src="https://github.com/user-attachments/assets/5733e63f-9be8-498b-8942-b3c1a4d8b6ed" />

Descripción: Este diagrama representa la arquitectura lógica del sistema y el flujo de información entre sus módulos principales: entrada, procesamiento y salida. Muestra cómo las señales generadas por el joystick y los botones son procesadas por el ESP32 y posteriormente enviadas mediante Bluetooth HID al dispositivo conectado.

## Act. 8: Diagrama Esquemático
Representación técnica y simbólica del circuito electrónico.

<img width="552" height="306" alt="Diagrama Esquematico" src="https://github.com/user-attachments/assets/1ab6e1da-a60a-4d7f-bc35-cce8c62191df" />

Descripción: Este diagrama representa de forma técnica las conexiones eléctricas entre el ESP32, el joystick y los pulsadores mediante simbología electrónica. Permite identificar cómo se distribuyen las señales de entrada y cómo se interconectan los componentes principales del sistema, sirviendo como referencia para comprender el funcionamiento eléctrico del emulador y facilitar su correcta implementación.
---

## ¿Qué hace el proyecto?

**Contexto:**
Este proyecto surge en el entorno del laboratorio de microcontroladores como una aplicación práctica para explorar las capacidades de conectividad inalámbrica y la emulación de periféricos de hardware modernos.

**Propósito:**

El propósito principal es diseñar y construir un dispositivo periférico inalámbrico autónomo que utilice el microcontrolador ESP32 para emular un ratón (Human Interface Device, HID) a través de Bluetooth.

**¿Qué problemas resuelve?**

El dispositivo ofrece una solución versátil y portátil para controlar sistemas computacionales sin cables y sin depender de drivers propietarios, abordando principalmente:

* **Control Remoto:** Permite al usuario interactuar con una PC, laptop o smartphone (como la HP Pavilion x360 mostrada) a distancia, ideal para presentaciones o navegación multimedia.

* **Integración Multiplataforma:** Soluciona la necesidad de instalar software especial. Al emular el estándar HID universal, es reconocido instantáneamente por Windows, macOS, Linux, Android y iOS.

* **Interfaz Personalizada:** Proporciona un control del cursor mucho más intuitivo que las teclas de flecha y asigna funciones comunes (como "siguiente diapositiva" o clics) a botones físicos claros de colores.

**Alcance:**

El proyecto abarca el diseño físico del hardware sobre protoboard, el desarrollo del firmware en C++ (Arduino IDE) para procesar las señales analógicas y digitales, y la implementación de la pila de Bluetooth Low Energy (BLE) para la comunicación. El resultado es un prototipo funcional que puede realizar movimientos precisos del cursor, clics (izquierdo/derecho) y emular pulsaciones de teclas específicas para controlar diapositivas o reproducir multimedia.

---

## ¿Cómo funciona internamente?

**Descripción Técnica:**

La imagen de arriba muestra la implementación física real del proyecto sobre una laptop. El prototipo consiste en un microcontrolador ESP32 montado en una protoboard estándar, interconectado con un módulo de joystick analógico de 5 pines y una matriz lineal de cuatro pulsadores táctiles de colores. Un cable de datos USB-C (en la parte inferior) provee la alimentación inicial de 5V al ESP32 para su funcionamiento y programación.

**Arquitectura General y Flujo de Datos:**

La arquitectura interna del proyecto se basa en un flujo de control "Adquisición-Procesamiento-Emisión":

1.  **Adquisición (Capa de Hardware):**
    * **Joystick:** Este componente actúa como un dispositivo de entrada analógico de dos ejes. Sus potenciómetros internos generan voltajes variables (entre 0V y 3.3V) según la posición de la palanca. Estas variaciones son leídas por los pines GPIO analógicos (ADC) del ESP32.

    * **Pulsadores (Botones de Colores):** Estos son entradas digitales simples. Cuando el usuario presiona un botón de color (rojo, amarillo, negro), se cierra el circuito y un voltaje sólido (3.3V) viaja a los pines GPIO digitales configurados como entrada.

2.  **Procesamiento (Capa de Firmware):**
    * **Mapeo del Joystick:** El firmware en C++ lee constantemente los valores del ADC y aplica una función de mapeo para convertir los voltajes leídos en un rango de desplazamiento negativo/positivo que representa los píxeles que debe mover el cursor en pantalla. Se implementa una "zona muerta" para evitar movimientos falsos cuando el joystick está en reposo.

   * **Lógica de Botones y Debounce:** El código supervisa los pines digitales y, al detectar un estado alto, asigna y emite el comando HID correspondiente (ej: MOUSE\_LEFT para el clic izquierdo o KEY\_RIGHT\_ARROW para pasar una diapositiva). Se incluye lógica de anti-rebote (debouncing) para asegurar que una sola pulsación física no se interprete como múltiples acciones.

3.  **Emisión (Capa de Comunicación):**
    * **Pila de Protocolos BLE HID:** El ESP32 se anuncia ante la PC (en este caso, la laptop HP Pavilion x360 visible) como un "ESP32 Mouse BT". Utiliza la pila de protocolos de Bluetooth Low Energy para emular la estructura de datos estándar de un "Descriptor de Reporte HID", que es el lenguaje universal que los sistemas operativos entienden para interactuar con ratones y teclados. Los datos procesados se envían de forma inalámbrica en tramas compatibles con BLE HID.
---

# Instalación y Uso

## Requisitos

### Hardware
- ESP32
- Joystick analógico
- 4 pulsadores
- Protoboard
- Cables Dupont
- Cable USB para programación y alimentación

### Software
- Arduino IDE
- Paquete de placas ESP32 instalado en Arduino IDE
- Librerías:
  - BleMouse.h
  - BleConnectionStatus.h

# Pasos para ponerlo en marcha

1.  Debemos de clonar este repositorio creado por nosotros: `git clone https://github.com/OmarDeLeon1910/Acts-6-7-8-Lab.Microcontroladores.git`
2.  Hay que conectar los componentes según el **Diagrama Esquemático**.
3.  Cargar el código de la carpeta `/codigo` a tu ESP32.
4.  Debemos emparejar el dispositivo vía Bluetooth con el nombre "ESP32 Mouse BT".

## Dependencias y Librerías Utilizadas

- BleMouse.h
- BleConnectionStatus.h
- Arduino IDE
- ESP32
- Bluetooth HID

## Resumen Técnico

El proyecto utiliza un ESP32 programado en Arduino IDE para emular un mouse Bluetooth HID mediante las librerías **BleMouse.h** y **BleConnectionStatus.h**. Un joystick analógico conectado a los GPIO 34 y 35 controla el movimiento del cursor, mientras que cuatro botones conectados a los GPIO 32, 33, 25 y 26 permiten realizar clic izquierdo, clic derecho, retroceso y avance.

El sistema calibra la posición central del joystick, aplica una zona muerta para evitar movimientos no deseados y envía las acciones al dispositivo conectado mediante Bluetooth, permitiendo que el ESP32 funcione como un mouse inalámbrico.
---

## FAQ - Preguntas Frecuentes

**1. ¿Qué hago si el dispositivo Bluetooth no aparece?**  
Verifica que el ESP32 esté correctamente alimentado y que el código haya sido cargado correctamente.

**2. ¿Es posible agregar más botones al sistema?** 
Sí, se pueden añadir más botones utilizando pines GPIO disponibles en el ESP32 y modificando el código para asignar nuevas funciones.

**3. ¿Es necesario mantener el ESP32 conectado por USB todo el tiempo?** 
No necesariamente. Después de programarlo, el ESP32 puede alimentarse con una fuente externa de 5V siempre que reciba la energía adecuada.

**4. ¿Qué pasa si el joystick no mueve el cursor correctamente?**  
Es necesario revisar las conexiones del joystick y verificar que los pines GPIO configurados en el código coincidan con las conexiones físicas.

**5. ¿Se puede modificar la función de los botones?**  
Sí. Las funciones de los botones pueden cambiarse modificando el código fuente en Arduino IDE.
