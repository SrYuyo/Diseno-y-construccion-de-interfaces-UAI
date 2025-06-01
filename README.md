# Diseño y construcción de interfaces UAI

Recomendaciones de blibliografía:

• The design of everyday things, Don
Norman

• Exploring Arduino, Jeremy Blum

• Mastering the Arduino Uno R4, Dongan Ibrahim

• Don’t Make me think 3rd edition, Steve Krug 

• Beginner's Guide to Reading

• Schematics, Stan Gibilisco

Ramo de la UAI, en el repositorio se encontrata una profundización de arduino. Además, con este repositorio es posible preparse para la cerfificación de arduino. Cabe destacar que el repositorio utiliza placas UNO R3 y UNO R4 de Arduino.

Link electronica básica: <https://github.com/SrYuyo/Electronica-aplicada-UAI>

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

Una matriz de LED es un dispositivo compuesto por múltiples LEDs organizados en filas y columnas. Este arreglo permite crear pantallas, indicadores o displays alfanuméricos de tamaño compacto. La matriz puede ser de diferentes dimensiones, por ejemplo, 8x8, 16x8, entre otras.

![image](https://github.com/user-attachments/assets/5e2ae236-42f6-4f5d-a4d4-c6a046e31017)

Hasta ahora, las placas Arduino no traen una matriz de LED incorporada como parte estándar del hardware en los pines. Sin embargo, la Arduino Uno R4 trae algunas novedades en hardware y facilita conectividades que permiten el uso de matrices de LED:

**Integración de interfaz I2C:**

La placa cuenta con más pines y capacidad para integrar fácilmente pantallas de matriz de LED de 16x2, 8x8, o incluso pantallas OLED, a través del bus I2C, que requiere solo dos líneas (SDA y SCL). Esto simplifica la conexión y control de matrices grandes sin usar muchos pines.

**Módulo de control digital mejorado:**

La Uno R4 puede conectarse con controladores dedicados para matrices LED, como los driver IC tipo MAX7219 o HT16K33, que permiten manejar matrices de 8x8 o mayores mediante comunicación I2C o SPI, liberando pines y facilitando programación.

**Facilidades para aplicaciones visuales en proyectos:**

Gracias a sus nuevos pines y compatibilidades, el control de matrices de LED y pantallas gráficas es más sencillo, permitiendo concentrarse en el diseño y programación en lugar de en la interacción física con los componentes.

Link arduino: <https://docs.arduino.cc/hardware/uno-r4-wifi/>


```cpp
#include "Arduino_LED_Matrix.h"
ArduinoLEDMatrix matriz;
byte dibujo[8][12] = {
 { 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 },
 { 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1 },
 { 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1 },
 { 1, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1 },
 { 1, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1 },
 { 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1 },
 { 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1 },
 { 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 }
};

void setup() {
 matriz.begin();
}
void loop() {
 matriz.renderBitmap(dibujo, 8, 12);
}

```

Resultado:

![image](https://github.com/user-attachments/assets/a9e3d522-ec66-4e80-b0fd-7f53cdcef174)

Video complementario: <https://www.youtube.com/watch?v=6A_xKy1qANw&t=83s&ab_channel=MCIElectronics>


# **Pantallas tipo display**

![image](https://github.com/user-attachments/assets/b0c3aa65-4e78-41e7-8fc0-bd5783624055)

Tipos comunes de pantallas display

LCDs (Liquid Crystal Display):

LCD 16x2 o 20x4: Son las más populares en proyectos Arduino. Muestran texto en filas y columnas.

  - Pantalla OLED: Máxima resolución en tamaño compacto, muestran gráficos, textos y animaciones en color o blanco y negro.
  - Pantallas TFT: Permiten gráficos, colores y pantallas táctiles, ofreciendo interfaces similares a los smartphones.

LED Matrices: Como las controladas por MAX7219, que permiten crear patrones, textos desplazantes, imágenes simples y efectos visuales en una cuadrícula de LEDs.

Pantallas táctiles: Como las TFT táctiles o las pantallas ILI9341, que además de mostrar información, permiten la interacción mediante toque.

LED de 7 segmentos: Compuestas por 7 LEDs en forma de dígito, útiles para mostrar números simples, como en relojes o contadores.

![image](https://github.com/user-attachments/assets/d1e7cd12-f084-4b3b-8bc7-d42a7f95ffbb)

![image](https://github.com/user-attachments/assets/7393d6dc-4cba-4fe3-8f64-1b531ab2ce90)

Cabe destacar que existen diferenciacion entre este componente. Se dividen en anodo y catodo común.

![image](https://github.com/user-attachments/assets/ae257d17-3445-449e-b6a3-32e55f80b967)

Dado que todas las conexiones de ánodo de los LEDs están unidas juntas al VCC, esto significa que su estado regular es uno HIGH. Y los LEDs individuales se iluminan conectando el lado el cátodo individual a una señal LOW (0V). Así que enviamos baja para encender cada LED.

![image](https://github.com/user-attachments/assets/3fe964af-8d95-4e12-a997-e8e7b0fa944d)

Para el cátodo común, todos los LEDs tienen tierra (GND) común. Conocer las diferencias entre los dos tipos ayuda tanto en el cableado como en la programación. Para el cátodo común, el in común se conecta a tierra. Y nuevamente, dado que todas las conexiones de los cátodo de los LEDs están unidas a tierra, significa que su lógica es cero LOW. Así que para iluminar los LEDs hay que conectar el terminal de ánodo a una señal alta (5V).

Recuerda limitar la corriente para no quemar el segmento del display por lo que debes utilizar resistencias de 220 ohm.

Codigo para display de 7 segmentos:

```cpp
int a=7; 
int b=6; 
int c=5; 
int d=11; 
int e=10; 
int f=8; 
int g=9; 
int dp=4; 

void display1(void) 
{  
    digitalWrite(b,HIGH);
    digitalWrite(c,HIGH);
} 

void  display2(void) 
{
    digitalWrite(a,HIGH);
    digitalWrite(b,HIGH);
    digitalWrite(g,HIGH);
  digitalWrite(e,HIGH);
    digitalWrite(d,HIGH);
}  

void display3(void) 
{ 
    digitalWrite(a,HIGH);
    digitalWrite(b,HIGH);
    
  digitalWrite(c,HIGH);
    digitalWrite(d,HIGH);
    digitalWrite(g,HIGH);
} 

void display4(void) 
{  
    digitalWrite(f,HIGH);
    digitalWrite(b,HIGH);
    digitalWrite(g,HIGH);
  digitalWrite(c,HIGH);
  
} 

void display5(void)  
{ 
    digitalWrite(a,HIGH);
    digitalWrite(f,HIGH);
    digitalWrite(g,HIGH);
  digitalWrite(c,HIGH);
    digitalWrite(d,HIGH);
} 

void  display6(void) 
{ 
    digitalWrite(a,HIGH);
    digitalWrite(f,HIGH);
    digitalWrite(g,HIGH);
  digitalWrite(c,HIGH);
    digitalWrite(d,HIGH);  
    digitalWrite(e,HIGH);  
} 

void display7(void)  
{   
   digitalWrite(a,HIGH);
    digitalWrite(b,HIGH);
    digitalWrite(c,HIGH);
}  

void display8(void) 
{ 
    digitalWrite(a,HIGH);
    digitalWrite(b,HIGH);
    digitalWrite(g,HIGH);
  digitalWrite(c,HIGH);
    digitalWrite(d,HIGH);  
    digitalWrite(e,HIGH);  
  digitalWrite(f,HIGH);  
} 
void clearDisplay(void) 
{ 
    digitalWrite(a,LOW);
    digitalWrite(b,LOW);
    digitalWrite(g,LOW);
  digitalWrite(c,LOW);
    digitalWrite(d,LOW);  
    digitalWrite(e,LOW);  
  digitalWrite(f,LOW);  
} 
void display9(void)  
{ 
    digitalWrite(a,HIGH);
    digitalWrite(b,HIGH);
    digitalWrite(g,HIGH);
  digitalWrite(c,HIGH);
    digitalWrite(d,HIGH);  
  digitalWrite(f,HIGH);  
} 
void display0(void) 
{ 
    digitalWrite(a,HIGH);
    digitalWrite(b,HIGH);
  digitalWrite(c,HIGH);
    digitalWrite(d,HIGH);  
    digitalWrite(e,HIGH);  
  digitalWrite(f,HIGH);  
} 
void setup() 
{ 
    int i;
    for(i=4;i<=11;i++)  
        pinMode(i,OUTPUT);
} 
void loop() 
{     
    while(1) 
    {   clearDisplay();
  display0(); 
        delay(2000); 
        clearDisplay();
        display1(); 
        delay(2000); 
        clearDisplay();
        display2();  
        delay(2000); 
        clearDisplay();
        display3(); 
        delay(2000);
        clearDisplay();
        display4(); 
        delay(2000);
        clearDisplay(); 
        display5(); 
        delay(2000);
        clearDisplay();  
        display6(); 
        delay(2000);
        clearDisplay(); 
        display7(); 
        delay(2000); 
        clearDisplay();
        display8();  
        delay(2000); 
        clearDisplay();  
        display9(); 
        delay(2000);      
    }
}
```

![image](https://github.com/user-attachments/assets/97b361e3-67ae-4d62-98b5-fa6c76b4d8ca)


**Pantalla LCD 16x2**


Las pantallas LCD de 16x2 en Arduino son dispositivos de salida que permiten mostrar texto y datos de manera sencilla y eficiente. Estas pantallas tienen 16 caracteres por fila y 2 filas, lo que facilita presentar información clara y concisa en proyectos de electrónica y automatización. Las pantallas de cristal líquido se organizan como una matriz de caracteres, usualmente de 5 x 8 puntos (o píxeles).

![image](https://github.com/user-attachments/assets/11f7ea34-019f-4630-8fc3-b262dccfd4fa)

Los pines de alimentación se conectan a 5V y GND

• El pin VEE (o VO en TinkerCAD) se usa para regular el contraste: se recomienda conectar un potenciómetro.

• Los pines de datos se conectan a pines digitales.

• Los pines del backlight se conectan a 5V y GND. También se puede usar una fuente externa.

• En el código se deben identificar muy claramente los pines digitales utilizados.

![image](https://github.com/user-attachments/assets/f81216e4-11b1-4b92-913a-bdbdf4a1f8fc)

El segundo tipo común es el LCD gráfico. El dispositivo LCD gráfico utiliza una única cuadrícula grande de luces individuales para mostrar información, en lugar de una cuadrícula separada para cada carácter. La disposición gráfica más común es el LCD de 128 por 64. La ventaja de usar esa disposición es que puedes mostrar cualquier carácter en la resolución que prefieras, y no estás limitado solo a la resolución de cinco por ocho en el dispositivo alfanumérico.


La mayoría de los LCDs que son compatibles con Arduino usan el controlador HD para gestionar el LCD. El HD44780, es conocido como controlador y driver de pantalla de cristal líquido de matriz de puntos. Cada matriz de puntos dentro del LCD tiene cinco columnas y ocho filas, como puedes ver en la imagen. Cada una de las matrices de puntos dentro de los 16 caracteres tiene una matriz de cinco por ocho. Esto se utiliza para mostrar letras y números cuando se usa el LCD.

<https://cdn.sparkfun.com/assets/9/5/f/7/b/HD44780.pdf>

Ejmplo de código:

```cpp
#include <LiquidCrystal.h>

// Configuración de pines: rs, enable, d4, d5, d6, d7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  lcd.begin(16, 2);
  lcd.print("Hola Mundo!");
}

void loop() {

}
```

**Librería LiquidCrystal**

Hay algunas funciones útiles que puede usar con objetos LiquidCrystal. Algunos de ellos se enumeran a continuación:

lcd.home() se utiliza para colocar el cursor en la parte superior izquierda de la pantalla LCD sin borrar la pantalla.

lcd.blink(), muestra un bloque parpadeante de 5×8 píxeles en la posición en la que se escribirá el siguiente carácter. 

lcd.cursor() muestra un guión bajo (línea) en la posición en la que se escribirá el siguiente carácter.

lcd.noBlink() apaga el cursor LCD parpadeante.

lcd.noCursor() oculta el cursor LCD.

lcd.scrollDisplayRight() desplaza el contenido de la pantalla un espacio hacia la derecha. Si deseas que el texto se desplace continuamente, debes usar esta función dentro de un bucle for.

lcd.scrollDisplayLeft() desplaza el contenido de la pantalla un espacio hacia la izquierda. Similar a la función anterior, usa esto dentro de un ciclo for para desplazamiento continuo.

clear(): Este método se utiliza para borrar el contenido del display LCD. Por ejemplo: lcd.clear(); En este ejemplo, se llama al método clear() para borrar el contenido del display LCD.

print(): Este método se utiliza para imprimir texto en el display LCD. Por ejemplo: lcd.print("Hello, world!"); En este ejemplo, se llama al método print() para imprimir el texto "Hello, world!" en el display LCD.

setCursor(col, row): Este método se utiliza para establecer la posición del cursor en el display LCD. Por ejemplo: lcd.setCursor(0, 1); En este ejemplo, se llama al método setCursor() para establecer la posición del cursor en la primera columna de la segunda fila del display LCD.

<https://docs.arduino.cc/libraries/liquidcrystal/>

![image](https://github.com/user-attachments/assets/06f809b1-c3f0-423b-a517-59d6ce2be712)


```cpp

#include <LiquidCrystal.h>

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

byte heart[8] = {
  0b00000,
  0b01010,
  0b11111,
  0b11111,
  0b11111,
  0b01110,
  0b00100,
  0b00000
};

byte smiley[8] = {
  0b00000,
  0b00000,
  0b01010,
  0b00000,
  0b00000,
  0b10001,
  0b01110,
  0b00000
};

byte frownie[8] = {
  0b00000,
  0b00000,
  0b01010,
  0b00000,
  0b00000,
  0b00000,
  0b01110,
  0b10001
};

byte armsDown[8] = {
  0b00100,
  0b01010,
  0b00100,
  0b00100,
  0b01110,
  0b10101,
  0b00100,
  0b01010
};

byte armsUp[8] = {
  0b00100,
  0b01010,
  0b00100,
  0b10101,
  0b01110,
  0b00100,
  0b00100,
  0b01010
};

void setup() {
  lcd.begin(16, 2);

  lcd.createChar(0, heart);
  lcd.createChar(1, smiley);
  lcd.createChar(2, frownie);
  lcd.createChar(3, armsDown);
  lcd.createChar(4, armsUp);

  lcd.setCursor(0, 0);

  lcd.print("I ");
  lcd.write(byte(0)); 
  lcd.print(" Arduino");
  lcd.write((byte)1);

}

void loop() {

  int sensorReading = analogRead(A0);

  int delayTime = map(sensorReading, 0, 1023, 200, 1000);
  lcd.setCursor(4, 1);
  lcd.write(3);
  delay(delayTime);
  lcd.setCursor(4, 1);
  lcd.write(4);
  delay(delayTime);
}
```

En los siguientes links podrás personalizar caracteres:

<https://chareditor.com/>

<https://maxpromer.github.io/LCD-Character-Creator/>

# **Comunicación I2C**

El protocolo I2C (Inter-Integrated Circuit) es un estándar de comunicación serial que permite la conexión de múltiples dispositivos en una misma línea de datos, facilitando la comunicación entre sensores, pantallas, microcontroladores y otros componentes electrónicos en proyectos electrónicos y de automatización.

Características principales:

  - Conexión multicapa: Utiliza solo dos líneas principales: SDA (data) y SCL ( reloj).

  - Multimaster y esclavos: Permite que varios dispositivos puedan actuar como maestros o esclavos en la misma línea, aunque en la mayoría de los casos, uno actúa como maestro.

  - Direcciones: Cada dispositivo en la red I2C tiene una dirección única, lo que permite comunicar con uno específico en medio de varios dispositivos compartiendo línea.
 
  - Velocidad: Trabaja típicamente a velocidades de 100 kbps (modo estándar), 400 kbps (modo rápido) o más en modos avanzados.

  - Facilidad de conexión: Solo requiere dos cables principales, además de tierra y alimentación, simplificando el cableado.


Funcionamiento básico:

  - El maestro inicia la comunicación enviando la dirección del dispositivo destino, y después realiza la transferencia de datos en bits, sincronizados por el reloj SCL.

  - La comunicación se realiza en frames (marcos) que incluyen la dirección y los datos, garantizando una transferencia sincronizada y controlada.

Aplicaciones:


  - Conexión de sensores y módulos de medición (temperatura, presión, humedad).

  - Pantallas LCD, RTCs (relojes en tiempo real) y memorias EEPROM.

  - Comunicación entre microcontroladores en sistemas complejos.

Ventajas:

  - Reduce el número de cables necesarios para múltiples dispositivos.
    
  - Facilita la expansión y montaje en diferentes proyectos.

  - Soporta múltiples dispositivos en la misma línea, simplificando el diseño.

Desventajas:

  - Más lento en comparación con SPI.
  - Más susceptible a interferencias en largas distancias.
  - Requiere que cada dispositivo tenga una dirección única.

![image](https://github.com/user-attachments/assets/a6ad1cfa-04b1-4a4c-aafe-073c0a4b699c)

**Biblioteca Wire**

La biblioteca Wire en Arduino es una biblioteca estándar que facilita la comunicación mediante el protocolo I2C. Permite que uno o varios dispositivos intercambien datos de manera sencilla a través de solo dos líneas de comunicación: SDA (datos) y SCL (reloj).

<https://docs.arduino.cc/language-reference/en/functions/communication/wire/>

Codigo de ejemplo para maestro:

```cpp
#include <Wire.h>

void setup() {
  Wire.begin(); // inicia como maestro
  Serial.begin(9600);
}

void loop() {
  Wire.requestFrom(0x68, 6); // solicita 6 bytes del módulo en dirección 0x68
  while (Wire.available()) {
    int data = Wire.read(); // lee los datos
    Serial.println(data);
  }
  delay(1000);
}
```

Codigo para esclavo:

```cpp
#include <Wire.h>

const int slaveAddress = 0x08;  // Dirección del dispositivo esclavo

void setup() {
  Wire.begin(slaveAddress);    // Iniciar como esclavo en la dirección 0x08
  Wire.onRequest(requestEvent); // Registrar función para solicitar datos
  Wire.onReceive(receiveEvent); // Registrar función para recibir datos
  Serial.begin(9600);
}

void loop() {
  // El loop puede estar vacío o realizar otras tareas
}

// Esta función se llama cuando el maestro solicita datos
void requestEvent() {
  int dato = 1; // Ejemplo de dato a enviar
  Wire.write(dato); // Envia el dato al maestro
  Serial.println("Enviando dato al maestro");
}

// Esta función se llama cuando el esclavo recibe datos del maestro
void receiveEvent(int howMany) {
  while (Wire.available()) {
    int recibido = Wire.read();
    Serial.print("Dato recibido del maestro: ");
    Serial.println(recibido);
  }
}
```

**Switch case**


El condicional switch case es una estructura de control que permite ejecutar diferentes bloques de código según el valor de una variable. Es muy útil cuando se desea realizar diferentes acciones en función de múltiples valores específicos, en lugar de múltiples instrucciones if-else.

Características principales:

  - Es más limpio y legible que múltiples if-else cuando hay varias condiciones distintas.
  - La variable evaluada puede ser de tipos enteros, caracteres o enumeraciones.
  - Utiliza case para cada condición específica y default para un caso que se ejecuta si ninguno de los casos coincide.

Codigo de ejemplo:

```cpp
const int botonPin = 2;

void setup() {
  pinMode(botonPin, INPUT_PULLUP);
  Serial.begin(9600);
}

void loop() {
  int estadoBoton = digitalRead(botonPin);

  switch (estadoBoton) {
    case LOW:
      Serial.println("Botón presionado");
      break;
    case HIGH:
      Serial.println("Botón no presionado");
      break;
    default:
      Serial.println("Estado desconocido");
  }
}
```

# **Comunicación SPI**

La comunicación SPI (Serial Peripheral Interface) es un protocolo de comunicación serie muy utilizado en electrónica para conectar microcontroladores con dispositivos periféricos como sensores, memorias, pantallas, y otros módulos. Se caracteriza por su rapidez y simplicidad en comparación con otros protocolos.

Características principales:

  - Modo maestro/esclavo: La mayoría de los sistemas SPI operan con un dispositivo maestro (como un Arduino) que controla uno o varios dispositivos esclavos.
  - Lineas de comunicación: Utiliza al menos cuatro líneas:
  - MOSI (Master Out Slave In): Datos enviados desde el maestro a los esclavos.
  - MISO (Master In Slave Out): Datos enviados desde los esclavos al maestro.
  - SCLK (Serial Clock): El reloj generado por el maestro para sincronizar la transferencia.
  - SS/CS (Slave Select o Chip Select): Línea activa baja que selecciona el dispositivo esclavo con el que se comunica.
  - Velocidad: Puede trabajar a altas velocidades, típicamente desde algunos kbps hasta varios Mbps.
  - Framework en Arduino: La biblioteca SPI facilita la integración con dispositivos SPI

El maestro genera un reloj SCLK. Cuando el maestro activa el pin CS del dispositivo escogido, comienza la transferencia. Los datos se intercambian bit por bit en sincronía con el reloj, en serie. La comunicación continúa hasta que se completa la transferencia, tras lo cual el maestro desactiva el CS.

![image](https://github.com/user-attachments/assets/2607dcab-d972-440e-8da4-3ba3e49adb11)

Ahora entrando más en detalle en su funcionamiento se compone de 3 ejes principales:

  - **Inicio de comunicación:** El maestro selecciona el dispositivo esclavo deseado poniendo en bajo la línea SS del dispositivo.
  - **Transferencia de datos:** El maestro genera el reloj SCK. Con cada ciclo, se envía y recibe un bit mediante las líneas MOSI y MISO, simultáneamente.
  - **Finalización:** Cuando se termina la transferencia, el maestro libera la línea SS, finalizando la comunicación con ese dispositivo.


**Ventajas**

Alta velocidad de transmisión. Comunicación full-duplex (simultáneo envío y recepción de datos). Configurable en modo (polaridad y fase). Soporta múltiples dispositivos (mediante líneas SS dedicadas).

**Desventajas**

Requiere más líneas que otros protocolos como I2C. La gestión de líneas SS puede complicarse en sistemas con muchos dispositivos.


Codigo para el maestro:

```cpp
#include <SPI.h>

void setup() {
  // Inicializa la comunicación SPI
  SPI.begin();
  // Configura la línea SS como salida
  pinMode(10, OUTPUT);
  digitalWrite(10, HIGH); // Inicialmente desactivado
}

void loop() {
  // Inicia la comunicación con el esclavo
  digitalWrite(10, LOW); // Selecciona el esclavo
  byte dataToSend = 0x55; // Datos a enviar
  byte receivedData = SPI.transfer(dataToSend); // Envía y recibe datos
  digitalWrite(10, HIGH); // Deselecciona el esclavo
  
}
```

Codigo para esclavo:

```cpp
#include <SPI.h>

// Variable para almacenar datos recibidos
volatile byte receivedByte = 0;

void setup() {
  // Configurar el pin de esclavo SPI (por defecto en Arduino UNO es pin 10)
  pinMode(MISO, OUTPUT); // Necesario como esclavo
  // Inicializa SPI en modo esclavo
  SPCR |= _BV(SPE);
  
  // Habilitar interrupciones para SPI
  SPI.attachInterrupt();
}

ISR(SPI_STC_vect) { // Interrupción SPI: se ejecuta cuando se recibe un byte
  receivedByte = SPDR; // Leer el dato recibido
  // Aquí puedes procesar receivedByte si lo deseas
  // Por ejemplo, cargar un dato en SPDR para enviar en la próxima transferencia:
  SPDR = 0xAB; // Enviamos un byte de ejemplo en respuesta
}

void loop() {
  // En modo esclavo no es necesario poner nada en loop
  // Todo el manejo se realiza en la interrupción
}
```

# **Memoria en Arduino**

La memoria volátil es aquella que se elimina cuando se apaga el dispositivo. La memoria no volátil mantiene su contenido. La memoria RAM se utiliza para almacenar valores de variables durante la ejecución de un programa. La memoria Flash se utiliza para almacenar el código de un programa en la placa Arduino (u otro). La memoria EEPROM se utiliza para almacenar valores permanentes a largo plazo.


![image](https://github.com/user-attachments/assets/3873b2ea-4ff6-4b3d-b1b3-4d1617b766d6)

**Memoria EEPROM**

EEPROM (Electrically Erasable Programmable Read-Only Memory) es una memoria no volátil que permite almacenar datos de manera permanente, incluso cuando el dispositivo está apagado. Es útil para guardar configuraciones, estados, o cualquier dato que necesite mantenerse entre reinicios.

Dentro de las caracteristicas clave esta el conserva los datos sin alimentación, por lo que es no volátil. Se puede modificar los datos mediante comandos. En comparación a la memoria RAM es más lento, pero suficiente para almacenamiento de configuraciones. Además tiene un número limitado de ciclos de escritura, alrededor de 100,000 ciclos por celda, por lo que no se recomienda escribir repetidamente en bucles muy rápidos.

Funciones principales:
  - EEPROM.read(address): lee un byte en la posición de memoria especificada.
  - EEPROM.write(address, value): escribe un byte en la posición especificada.
  - EEPROM.put(address, data): escribe diferentes tipos de datos (structs, variables) en una sola operación.
  - EEPROM.get(address, data): lee datos complejos.

![image](https://github.com/user-attachments/assets/19db9c8d-08b9-48b2-860a-71c124c263e5)

```cpp
#include <EEPROM.h>

void setup() {
  Serial.begin(9600);
  int address = 0;

  // Escribir un valor en EEPROM
  EEPROM.write(address, 42);
  Serial.println("Valor escrito en EEPROM");
}

void loop() {
  int address = 0;
  byte valor = EEPROM.read(address);
  Serial.print("Valor leído de EEPROM: ");
  Serial.println(valor);
  delay(2000);
}
```

<https://docs.arduino.cc/learn/built-in-libraries/eeprom/>
<https://docs.arduino.cc/learn/programming/eeprom-guide/>


**Memoria externa**

Memoria no volátil, de alta velocidad, que permite almancenar grandes cantidades de datos. Requiere módulos externos. Uno muy común funciona con protocolo SPI. La tarjeta debe tener formato FAT32

Un módulo microSD es un dispositivo que permite leer y escribir datos en una tarjeta microSD, que funciona como medio de almacenamiento externo y portátil. Es ampliamente utilizado en proyectos Arduino para guardar datos, registrar sensores, firmware, imágenes, etc.

![image](https://github.com/user-attachments/assets/3a4682af-ee13-4335-852b-cacbdd02034b)

![image](https://github.com/user-attachments/assets/45abd61e-9176-4675-b01a-54daf05d4c04)

| Modulo SD  | Arduino UNO |
| ------------- | ------------- |
| VCC  | 3.3V o 5V |
| GND  | GND |
|CS (Chip Select)|Pin 10 (o cualquier otro digital)| 
|SCK|Pin 13|
|MOSI|Pin 11|
|MISO|Pin12|

Se requiere la librería SPI.h para la comunicación y la librería SD.h para las acciones de lectura y escritura. Se debe definir el pin CS como variable. Los otros pines SPI son fijos.

![image](https://github.com/user-attachments/assets/8c55ee0e-f4a8-49e2-8777-ffccc9c5936f)

```cpp
#include <SPI.h>
#include <SD.h>

const int chipSelect = 10;

void setup() {
  Serial.begin(9600);
  if (!SD.begin(chipSelect)) {
    Serial.println("Error al inicializar la tarjeta SD");
    return;
  }
  Serial.println("Tarjeta SD inicializada.");

  // Crear y escribir en un archivo
  File dataFile = SD.open("datalog.txt", FILE_WRITE);
  if (dataFile) {
    dataFile.println("Hola, microSD!");
    dataFile.close();
    Serial.println("Archivo escrito");
  } else {
    Serial.println("No se pudo abrir el archivo");
  }
  
  // Leer datos del archivo
  dataFile = SD.open("datalog.txt");
  if (dataFile) {
    Serial.println("Contenido del archivo:");
    while (dataFile.available()) {
      Serial.write(dataFile.read());
    }
    dataFile.close();
  } else {
    Serial.println("No se pudo abrir el archivo");
  }
}

void loop() {
}
```

**RECORDAR FORMATEAR LA TARJETA ANTES DE UTILIZAR**

<https://cursos.mcielectronics.cl/2023/07/03/modulo-de-tarjeta-micro-sd-de-interfaz-con-arduino/>
<https://docs.arduino.cc/libraries/sd/>


# **Protocolo UART**

UART es un protocolo de comunicación serial asincróna, ampliamente utilizado para la transmisión de datos entre dispositivos, como Arduino, computadoras, módulos Bluetooth, GPS, entre otros. Es un método simple y efectivo para enviar datos en forma de bytes en serie, sin necesidad de una señal de reloj compartida. Respecto a arduino se deben alternan roles de transmisor (TX) y receptor (RX). Se deben acorda la velocidad de transmission, llamada Baud Rate. Se mide be bits por Segundo. Por último se puedes conectar dos placas Arduino diferente. 

![image](https://github.com/user-attachments/assets/c9566af8-adc6-4915-91e8-6fd363aff823)

**Librería Serial**

![image](https://github.com/user-attachments/assets/333a58f1-5e74-4c99-88dc-dc1dcde50759)

Ej practico:

![image](https://github.com/user-attachments/assets/6ff8361e-b4f1-4042-8973-fcb7099cd692)

RECORDAR PRIMERO CARGAR LOS CODIGOS POR SEPARADO Y LUEGO ENERGIZAR Y CONECTAR PLACAS

Codigo primera placa
```cpp
int recibido;
void setup() {
Serial1.begin(9600);
pinMode(9, OUTPUT);
}
void loop() {
if (Serial1.available()) {
recibido = Serial1.parseInt();
analogWrite(9, recibido);
}
```

Codigo Segunda placa
```cpp
int valor;
int brillo;
void setup() {
Serial1.begin(9600);
void loop() {
valor = analogRead(A0);
brillo = map (valor, 0, 1023, 0, 255);
Serial1.println(brillo);
delay(100);
}
```

**Comunicación con otros progamas**

La conexión con software usando UART como vimos en Arduino, específicamente en el contexto de integrar con diferentes programas como Processing, Excel, Python y Grasshopper. Cada uno tiene diferentes métodos y herramientas para comunicarse con Arduino a través de UART (Universal Asynchronous Receiver-Transmitter), que es el método más común de comunicación serial.

**La comunicación puede ser a diferentes velocidades de baudios (baud rate), como 9600, 115200, etc. Esto puede ser uno de los problemas de porque no funciona el código**

Algunos ejemplos según programa:

**Processing**

processing es una plataforma de programación para visualización y multimedia que puede conectarse en tiempo real con Arduino vía UART. Usas la librería Serial para abrir el puerto serie y leer/escribir datos.

```java
import processing.serial.*;

Serial port;

void setup() {
  String portName = Serial.list()[0]; // Selecciona el puerto
  port = new Serial(this, portName, 9600);
}

void draw() {
  if (port.available() > 0) {
    String datos = port.readStringUntil('\n');
    println(datos);
  }
}
```

El codigo envia datos para visualizar en pantalla, controlar elementos gráficos, etc.

Rerefencias:
  - <https://processing.org/tutorials/electronics>
  - <https://www.arduino.cc/education/visualization-with-arduino-and-processing/>

**Excel**

Excel puede comunicarse con Arduino usando Puertos COM mediante el complemento Power Query u otras herramientas como Plink en Windows o Serial Port Tool que lee serial y conecta con Excel. La forma más sencilla es usar un programa intermedio que pase los datos a Excel (por ejemplo, un script en Python que lea desde el puerto serie y guarde en un CSV o en una hoja de Excel en tiempo real).

De todas formas ten en cuenta que si la base de datos es grande Excel no sea la herramienta más idonea.

Referencias:

  - <https://www.youtube.com/watch?v=cd_pDVVApak&pp=ygUSYXJkdWlubyB3aXRoIGV4Y2Vs>
  - <https://www.youtube.com/watch?v=umBarmiCWEo&list=PLx1tajvUlOMeZNTfaRc1rt4nxgC46aMJJ>
  - <https://projecthub.arduino.cc/m_karim02/arduino-to-excel-communication-adb318>
  - <https://support.microsoft.com/es-es/office/ejecuci%C3%B3n-de-c%C3%B3digo-y-conexi%C3%B3n-a-un-microcontrolador-al-equipo-9428e1de-160f-4255-b887-741062b39317>


**Python**

Python es una de las opciones más flexibles y potentes para interactuar con Arduino vía UART usando la librería PySerial. Registrar datos, análisis en tiempo real, control remoto, integración con bases de datos, etc.

```py
import serial

ser = serial.Serial('COM3', 9600)  # Cambia 'COM3' por tu puerto

while True:
    if ser.inWaiting() > 0:
        data = ser.readline().decode('utf-8').strip()
        print(data)
```

Referencias:

  - <https://forum.arduino.cc/t/comunicacion-serial-python-arduino/1277800>
  - <https://projecthub.arduino.cc/ansh2919/serial-communication-between-python-and-arduino-663756>
  - <https://www.youtube.com/playlist?list=PLCYCyKTQZlD1t569SgejJL9G42KBKq0Y->


**Conceptualización y diseño de algoritmos**

La conceptualización y el diseño de algoritmos son pasos fundamentales en la programación y la resolución de problemas computacionales. Se realiza utilizando herramientas como seudocódigo y diagramas de flujo, los cuales ayudan a planificar y comunicar claramente cómo se debe realizar una tarea antes de implementarla en un lenguaje de programación.

El seudocódigo es una representación textual sencilla y clara del algoritmo, que combina elementos del lenguaje natural con estructuras básicas de programación. Se usa para planificar la lógica sin preocuparse por la sintaxis específica de un lenguaje de programación.

Por ejemplo:

```txt
Inicio
    Leer número n
    Si n es mayor que 0 Entonces
        Escribir "Número positivo"
    Sino
        Escribir "Número no positivo"
    FinSi
Fin
```

El diagrama de flujo es una representación gráfica del algoritmo, usando símbolos estandarizados (como flechas, rombos, rectángulos) que muestran la secuencia y decisiones del proceso. Es útil para visualizar el orden de las acciones y las condiciones que afectan el flujo del programa.

  - Un rectángulo: proceso o acción.
  - Un rombo: decisión (sí/no).
  - Flechas: indican la dirección del flujo.

Una herramienta muy recomendable es Lucid. También existen otras como Miro y Canva.

<https://www.lucidchart.com/pages/es/ejemplos/diagrama-de-flujo-online>

![image](https://github.com/user-attachments/assets/6c5b7a4b-ba96-45ca-8087-02d89f943d21)

# **Wi-Fi y Arduino**

Arduino, por sí mismo, no incluye un módulo WiFi en sus modelos básicos (como Arduino Uno R3 o Nano). Sin embargo, existen varias formas de agregarle conectividad WiFi a un proyecto de Arduino, lo que permite comunicarse con internet, interactuar con servidores, controlar dispositivos remotamente, etc. Además de las plascas más recientes que tienen incorporado el chip que se encarga de la conexión.

```cpp
#include <SPI.h>
#include <WiFiNINA.h>

char ssid[] = "TU_SSID";       // SSID de tu red WiFi
char pass[] = "TU_PASSWORD";   // Contraseña de tu red WiFi

void setup() {
  Serial.begin(9600);
  while (!Serial);

  if (WiFi.status() == WL_NO_MODULE) {
    Serial.println("El módulo WiFi no está conectado");
    while (true);
  }

  Serial.println("Conectando a WiFi...");
  WiFi.begin(ssid, pass);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nConectado!");
  Serial.print("Dirección IP: ");
  Serial.println(WiFi.localIP());
}

void loop() {
  // Aquí puedes agregar código para comunicarte con servidores, IoT, etc.
}
```

Referencias:
  - <https://docs.arduino.cc/libraries/wifinina/>
  -  <https://cloud.arduino.cc/>

**Enviar información con SMTP**

ara enviar información por medio de SMTP (Simple Mail Transfer Protocol) en un microcontrolador como Arduino, necesitarás usar la librería adecuada y un servidor SMTP que permita enviar correos electrónicos. La Arduino Uno R4, con su módulo WiFi integrado y librerías compatibles, puede enviar correos electrónicos usando SMTP. La librería ESP8266SMTPClient o SMTPClient para Arduino (en algunos casos, puede usar la librería WiFiSSLClient junto con librerías SMTP).

```cpp
#include <WiFiNINA.h>
#include <WiFiClientSecure.h>

const char* ssid = "TU_SSID";
const char* password = "TU_PASSWORD";

// Credenciales de la cuenta de Gmail
const char* serverSMTP = "smtp.gmail.com";
const int portSMTP = 587; // Para TLS

const char* emailFrom = "tucorreo@gmail.com";
const char* emailPassword = "tucontraseña";

const char* emailTo = "destinatario@ejemplo.com";

void setup() {
  Serial.begin(9600);
  while (!Serial);
  
  // Conectar a WiFi
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nConectado a WiFi");
  
  sendEmail();
}

void loop() {
  // No se necesita nada en loop para este ejemplo
}

void sendEmail() {
  WiFiClientSecure client;
  if (!client.connect(serverSMTP, portSMTP)) {
    Serial.println("Error de conexión");
    return;
  }

  // Aquí debes implementar la secuencia SMTP para enviar un correo
  // Nota: La implementación puede variar según la librería SMTP que uses
  // Este ejemplo es completo, pero en la práctica, necesitas gestionar comandos SMTP
  // usando la librería SMTPClient o SMTPUsar la librería SMTPClient para Arduino.
   
  Serial.println("Conexión establecida, enviando mensaje...");
  // (Aquí deberías seguir la secuencia SMTP: EHLO, AUTH LOGIN, MAIL FROM,
  // RCPT TO, DATA, enviar cuerpo, terminación, etc.)
  
  // Nota: La implementación completa requiere gestionar múltiples comandos SMTP.
  // Para simplificar, considera usar librerías específicas como "ESP8266SMTPClient".

}

// NOTA: Este ejemplo es solo un esquema; en la práctica, debes usar librerías específicas 
// para SMTP, como https://github.com/knolleary/pubsubclient o librerías SMTP específicas.
```


# **Arduino Cloud**

Arduino Cloud es la plataforma oficial de Arduino para crear, gestionar y monitorear proyectos IoT (Internet of Things o Internet de las Cosas) en la nube. Ofrece un entorno intuitivo y fácil de usar para conectar dispositivos, recopilar datos y automatizar sistemas sin necesidad de ser un experto en redes o programación avanzada.

Nota: Arduino ofrece un srevicio gratuito y por ende la cantidad placas que se pueden tener conectadas, nombrasdas y entre otras cosas en temas de cantidad es limitado.

![image](https://github.com/user-attachments/assets/22007659-252c-4bbc-a3b2-f73268e01c3b)

Referencias:
  - <https://www.youtube.com/watch?v=pC5W88NTJ1U&pp=ygUNYXJkdWlubyBjbG91ZA%3D%3D>
  - <https://www.youtube.com/watch?v=uaLrmLCqGnc&pp=ygUNYXJkdWlubyBjbG91ZA%3D%3D>

Supongamos que tienes una placa Arduino Uno WiFi o una compatible, y un sensor de temperatura (por ejemplo, un DHT11 o DHT22).

**1. Configuración en Arduino Cloud**

Primero, debes crear tu proyecto en Arduino Cloud:
  - Accede a Arduino Cloud.
  - Crea un nuevo dispositivo y un escenario.
  - Añade variables (ej., temperatura) que serán controladas y visualizadas.

**2. Código ejemplo en Arduino IDE**

Aquí tienes el código básico que debes cargar en tu placa, usando las librerías oficiales de Arduino Cloud y la librería para sensor DHT.

Nota: El codigo debe realizarse desde la página de arduino cloud, además de tener el programa que conecta el IDE con los servidores de Arduino Clo

```cpp
#include "thingProperties.h"
#include <DHT.h>

// Sensor configuration
#define DHTPIN 2      // Pin connected to sensor
#define DHTTYPE DHT22 // Or DHT11
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  delay(1500); // Give serial monitor time to open
  
  // Initialize cloud properties
  initProperties();
  // Connect to Arduino IoT Cloud
  ArduinoCloud.begin(ArduinoIoTPreferredConnection);
  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();
  
  // Initialize sensor
  dht.begin();
}

void loop() {
  ArduinoCloud.update();
  
  // Read temperature every 2 seconds
  static unsigned long previousMillis = 0;
  if (millis() - previousMillis >= 2000) {
    previousMillis = millis();
    
    float t = dht.readTemperature();
    if (isnan(t)) {
      Serial.println("Error reading sensor");
    } else {
      temperatura = t;
      Serial.print("Temperature: ");
      Serial.println(temperatura);
    }
  }
}
```

Nota: También necesitas crear un IoT Thing en <https://app.arduino.cc/things> con una variable de nube "temperatura" (tipo Float). Esto es en el apartado de cosas o variables de la página web.


