# Diseño y construcción de interfaces UAI

Ramo de la UAI, en el repositorio se encontrata una profundización de arduino. Además, con este repositorio es posible preparse para la cerfificación de arduino. Cabe destacar que el repositorio utiliza placas UNO R3 y UNO R4 de Arduino.

Link electronica básica: <Agregar_link>

Una interfaz es un punto de interacción o conexión entre dos o más sistemas, dispositivos o componentes. En el contexto de la computación y la programación, puede referirse a:

1. **Interfaz de Usuario (UI):** Es el conjunto de elementos gráficos y de control que permite a los usuarios interactuar con un software o hardware. Esto incluye botones, menús, iconos, y otros componentes visuales.

2. **Interfaz de Programación de Aplicaciones (API)**: Es un conjunto de definiciones y protocolos que permiten que diferentes aplicaciones se comuniquen entre sí. Facilita la integración de diferentes sistemas y la utilización de funcionalidades de un software desde otro.


# Funciones de tiempo

Las funciones millis() y delay() son parte del entorno de programación de Arduino y se utilizan para controlar el tiempo en los programas.

**delay()**

Descripción: La función delay() pausa la ejecución del programa durante un tiempo especificado, en milisegundos.

Sintaxis: delay(milliseconds);

Parámetro: milliseconds: es el número de milisegundos que el programa debe detenerse. Por ejemplo, delay(1000); detendría el código durante 1 segundo.

Uso: Es útil para crear pausas en el código, como esperar antes de encender un LED o esperar entre lecturas de sensores.

Limitaciones: Durante el tiempo que el programa está en delay(), no se ejecutan otras instrucciones. Esto significa que no se pueden leer señales, no se pueden procesar entradas y la respuesta del sistema puede sentirse lenta si se usan retardos largos.

**millis()**

Descripción: La función millis() devuelve el número de milisegundos que han transcurrido desde que la placa Arduino comenzó a ejecutar el programa. Es útil para medir el tiempo sin bloquear el ciclo de ejecución del programa.

Sintaxis: unsigned long currentTime = millis();

Uso: Se usa para crear temporizadores no bloqueantes. Por ejemplo, puedes verificar cuánto tiempo ha pasado desde un evento (como un botón presionado) y tomar decisiones en función de eso sin detener el funcionamiento del programa.

Ventajas: Permite realizar múltiples tareas al mismo tiempo, como encender y apagar LEDs, leer sensores y responder a entradas del usuario sin que el programa se detenga.

Tambien existen otras funciones hour(), minute(), day(), y month() son parte de la biblioteca de tiempo de Arduino, específicamente cuando se utiliza junto con un módulo de tiempo real (RTC) como el DS1307 o el DS3231, o con la biblioteca Time, que permite trabajar con fechas y horas. Estas funciones ayudan a obtener y manejar la hora, el minuto, el día y el mes actuales respectivamente.

Ejemplo de uso (Usar placa UNO R3):

```cpp
#include <Wire.h>
#include <RTClib.h>

RTC_DS3231 rtc; // Crear un objeto RTC

void setup() {
  Serial.begin(9600);
  if (!rtc.begin()) {
    Serial.println("No se pudo encontrar un RTC");
    while (1); // Detener si no se encuentra el RTC
  }
}

void loop() {
  // Obtener la hora y fecha actual
  Serial.print("Hora: ");
  Serial.print(hour());
  Serial.print(":");
  Serial.print(minute());
  Serial.print(":");
  Serial.print(second());
  Serial.print("  Fecha: ");
  Serial.print(day());
  Serial.print("/");
  Serial.print(month());
  Serial.print("/");
  Serial.print(year());
  Serial.println();

  delay(1000); // Esperar un segundo
}

```

Modulo de referencia:

![image](https://github.com/user-attachments/assets/04fb34d7-efa8-4226-8b61-01d168e0b104)

#Interupciones

Las interrupciones son una característica avanzada que permite que el microcontrolador responda de inmediato a ciertos eventos, como presionar un botón, sin tener que esperar a que el programa complete su ciclo de ejecución en el loop().

**Activación por Eventos de Hardware:**

Las interrupciones se activan cuando ocurre un evento específico, como un cambio en el estado de un pin (por ejemplo, un botón que se presiona o se suelta). Esto permite a tu programa reaccionar rápidamente a estos eventos.

**Pausar el Flujo Normal del Código:**

Cuando se produce una interrupción, el flujo normal del programa se "pausa" temporalmente. El microcontrolador detiene lo que estaba haciendo en el loop() y ejecuta una función llamada "rutina de servicio de interrupción" (ISR). Después de que se completa la ISR, el programa regresa al punto donde se detuvo en el loop().

**Reacción Instantánea:**

Esto es especialmente útil en situaciones donde la velocidad de respuesta es crucial, como en aplicaciones que requieren un control preciso o la respuesta rápida a entradas del usuario.

```cpp
const int buttonPin = 2;      // pin del botón
const int ledPin = 13;        // pin del LED
volatile int buttonState = 0; // variable para guardar el estado del botón

void setup() {
  pinMode(buttonPin, INPUT);   // configurar el pin del botón como entrada
  pinMode(ledPin, OUTPUT);      // configurar el pin del LED como salida
  attachInterrupt(digitalPinToInterrupt(buttonPin), toggleLED, RISING); // configurar la interrupción
}

void loop() {
  // el cuerpo del loop puede estar vacío
  // las acciones ocurren en la función de interrupción
}

void toggleLED() {
  buttonState = digitalRead(buttonPin); // leer el estado del botón
  digitalWrite(ledPin, buttonState);     // encender o apagar el LED según el estado del botón
}
```

Otro ejemplo:

```cpp

const int pinBoton = 2;
volatile unsigned int contador Pulsos = 0;
unsigned long tiempoAnterior = 0;
const unsigned long intervalo = 1000;

void setup() {

  pinMode(pinBoton, INPUT);
  attachInterrupt (digitalPinToInterrupt (pinBoton), contarPulsos, FALLING);
  Serial.begin(9600);
  Serial.println("Iniciando...");


void loop() {
  unsigned long tiempoActual millis();

  if (tiempoActual - tiempoAnterior >= intervalo) {
    tiempoAnterior = tiempoActual;

    unsigned int frecuencia = contadorPulsos;
    contadorPulsos = 0;

    Serial.print("Frecuencia de pulsos: ");
    Serial.print(frecuencia);
    Serial.println(" Hz");
  }
}

void contarpulsos() {
  contadorPulsos++;
}

```

# **Arreglos**

Los arreglos (o arrays) en Arduino son estructuras de datos que permiten almacenar múltiples valores del mismo tipo de datos en una sola variable. Los arreglos son útiles para gestionar datos relacionados y facilitar su manipulación. 

Un arreglo se declara especificando el tipo de dato, seguido del nombre del arreglo y el número de elementos entre corchetes.

```cpp
int numeros[5]; // Declara un arreglo de enteros con 5 elementos

int numeros[5] = {1, 2, 3, 4, 5}; // Inicializa el arreglo con 5 elementos definidos

int valor = numeros[0]; // Accede al primer elemento del arreglo (1)

numeros[2] = 10; // Cambia el tercer elemento del arreglo a 10
```

Un arreglo puede inicializarse al momento de la declaración, asignando valores entre llaves.

La longitud de un arreglo debe ser conocida en tiempo de compilación y no puede cambiarse dinámicamente. Puedes usar el operador sizeof para determinar el número total de bytes ocupados por el arreglo y dividirlo por el tamaño de un elemento para obtener su longitud.

```cpp
int longitud = sizeof(numeros) / sizeof(numeros[0]); // Calcula el número de elementos en el arreglo
```

Otro ejemplo más claro:

```cpp
const int longitud = 5;      // Longitud del arreglo
int numeros[longitud] = {1, 2, 3, 4, 5}; // Arreglo inicializado

void setup() {
  Serial.begin(9600);

  // Imprimir los elementos del arreglo
  for (int i = 0; i < longitud; i++) {
    Serial.println(numeros[i]); // Imprime cada elemento del arreglo
  }

  // Modificar un elemento
  numeros[2] = 10; // Cambiar el tercer elemento a 10

  // Imprimir el arreglo modificado
  Serial.println("Arreglo modificado:");
  for (int i = 0; i < longitud; i++) {
    Serial.println(numeros[i]); // Imprime cada elemento del arreglo modificado
  }
}

void loop() {
  // Código principal
}
```
# **Arreglos bidimensionales**

Los arreglos bidimensionales (o matrices) en Arduino son estructuras de datos que permiten almacenar datos en una tabla con filas y columnas. Son útiles para representar datos en dos dimensiones, como coordenadas, tablas de puntajes, imágenes en escala de grises, y más.


```cpp
int matriz[3][4] = { // Matriz 3x4 inicializada
  {1, 2, 3, 4},
  {5, 6, 7, 8},
  {9, 10, 11, 12}
};

void setup() {
  Serial.begin(9600);

  // Imprimir los elementos de la matriz
  for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 4; j++) {
      Serial.print(matriz[i][j]); // Imprime cada elemento
      Serial.print(" ");
    }
    Serial.println(); // Nueva línea después de cada fila
  }

  // Modificar un elemento
  matriz[1][2] = 70; // Cambia el valor en la segunda fila y tercera columna a 70

  // Imprimir la matriz modificada
  Serial.println("Matriz modificada:");
  for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 4; j++) {
      Serial.print(matriz[i][j]);
      Serial.print(" ");
    }
    Serial.println(); 
  }
}

void loop() {
  // Código principal
}
```

Limitaciones a considerar:Al igual que los arreglos unidimensionales, la longitud de un arreglo bidimensional debe ser conocida en tiempo de compilación y no se puede cambiar dinámicamente. El uso de matrices grandes puede consumir mucha memoria, que es un recurso limitado en algunas placas de Arduino (como la Uno).


# **Teclados**

![image](https://github.com/user-attachments/assets/b39e6477-5226-4b75-8197-c4e6b87355fa)

(Es ideal utilizar los teclado con botones tipo push, en vez de los de membrana.)

Los teclado están organizado en filas y columnas, de forma que cada tecla se encuentra en una intersección. Permite leer más cantidad de botones con menos pines. En general, para leer FxC botones, se requiere F+C pines.

Por ejemplo: un teclado matricial de 4x5 requiere sólo 9 pines en lugar de 20.

![image](https://github.com/user-attachments/assets/2a7d9051-7a9e-411b-a511-f18bae498d1b)

Antes de trabajar con los teclados el IDE de Arduino es importante instalar la biblioteca Keypad.h. pues simplifica la gestión de teclados matriciales en Arduino. Permite detectar las pulsaciones de las teclas de forma eficiente y fácil, sin tener que implementar manualmente un escáner de filas y columnas.

(Codigo con placa UNO R3)
```cpp
#include <Keypad.h>

const byte FILAS = 4; // número de filas
const byte COLUMNAS = 4; // número de columnas

char teclas[FILAS][COLUMNAS] = { 
  {'1', '2', '3', 'A'}, 
  {'4', '5', '6', 'B'}, 
  {'7', '8', '9', 'C'}, 
  {'*', '0', '#', 'D'}
};

byte filaPins[FILAS] = {9, 8, 7, 6}; // pines que conectan a las filas
byte columnaPins[COLUMNAS] = {5, 4, 3, 2}; // pines que conectan a las columnas

Keypad teclado = Keypad(makeKeymap(teclas), filaPins, columnaPins, FILAS, COLUMNAS);

void setup() {
  Serial.begin(9600);
}

void loop() {
  char tecla = teclado.getKey(); // obtener la tecla presionada
  if (tecla) { // si se presiona una tecla
    Serial.print("Tecla presionada: ");
    Serial.println(tecla); // imprimir la tecla presionada en el monitor serie
  }
}

```

# **Matrices LED**

El Arduino R4 incluye una matriz LED 12x8 incorporada
