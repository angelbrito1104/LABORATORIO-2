# LABORATORIO-2

## Punto 1:
Sistema de Iluminación y Monitoreo de Temperatura mediante Chatbot con Reconocimiento de Voz

## Introducción
Este proyecto tiene como finalidad integrar hardware y software mediante el uso de Arduino UNO y Python, permitiendo el control de iluminación y el monitoreo de temperatura a través de un chatbot con reconocimiento de voz.


## Objetivo General
Desarrollar un sistema interactivo que permita encender y apagar luces, así como consultar la temperatura, utilizando comandos de voz procesados por un chatbot.


## Materiales
- Arduino UNO
- Sensor DHT11
- 2 LEDs (rojo y verde)
- Resistencias 220Ω
- Protoboard
- Cables
- Computador
- Python 3.12
- Librerías: pyserial, SpeechRecognition, PyAudio

## Metodología
El desarrollo se realizó en varias fases: configuración del Arduino, prueba de LEDs, integración del sensor, comunicación serial y desarrollo del chatbot con reconocimiento de voz.




# Desarrollo del Sistema
## Código Arduino
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

int ledRojo = 8;
int ledVerde = 9;

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(ledRojo, OUTPUT);
  pinMode(ledVerde, OUTPUT);
}

void loop() {
  if (Serial.available()) {
    String comando = Serial.readStringUntil('\n');
    comando.trim();

    if (comando == "ROJO") {
      digitalWrite(ledRojo, HIGH);
      digitalWrite(ledVerde, LOW);
    } else if (comando == "VERDE") {
      digitalWrite(ledRojo, LOW);
      digitalWrite(ledVerde, HIGH);
    } else if (comando == "APAGAR") {
      digitalWrite(ledRojo, LOW);
      digitalWrite(ledVerde, LOW);
    } else if (comando == "TEMP") {
      float t = dht.readTemperature();
      Serial.println(t);
    }
  }
}
## Código Python con voz (este codigo se guarda en un bloc de notas y se ejecuta desde el cmd)
import serial
import time
import speech_recognition as sr

arduino = serial.Serial('COM6', 9600, timeout=2)
time.sleep(2)

recognizer = sr.Recognizer()

def escuchar():
    with sr.Microphone() as source:
        print("Escuchando...")
        audio = recognizer.listen(source)
        try:
            texto = recognizer.recognize_google(audio, language="es-ES")
            return texto.lower()
        except:
            return ""

def enviar_comando(cmd):
    arduino.write((cmd + '\n').encode())
    time.sleep(1)
    return arduino.readline().decode().strip()

while True:
    mensaje = escuchar()
    if "rojo" in mensaje:
        print(enviar_comando("ROJO"))
    elif "verde" in mensaje:
        print(enviar_comando("VERDE"))
    elif "apaga" in mensaje:
        print(enviar_comando("APAGAR"))
    elif "temperatura" in mensaje:
        print(enviar_comando("TEMP"))
    elif "salir" in mensaje:
        break

## Resultados
El sistema permitió controlar los LEDs mediante voz y consultar la temperatura en tiempo real. Se verificó la correcta comunicación entre Python y Arduino.
## Evidencias


<img width="509" height="257" alt="image" src="https://github.com/user-attachments/assets/9cc8783c-4778-47ec-a6da-76fbc197fdbe" />





https://youtu.be/e6LQsMiRfN8?si=6TPcpu_ttTJgGiQO




- Capturas del chatbot funcionando



<img width="600" height="396" alt="image" src="https://github.com/user-attachments/assets/9ba2d6ab-bf61-439f-b674-a94ae32dbb40" />






## Conclusiones
El proyecto demuestra la integración efectiva entre hardware y software, permitiendo el control de dispositivos físicos mediante comandos de voz.





## Punto 2:
Desarrollo de Juego en Pantalla OLED con Arduino UNO


# Introducción
En este punto se desarrolló un juego interactivo utilizando una pantalla OLED 128x64 con Arduino UNO, permitiendo aplicar conceptos de programación, manejo de entradas digitales y representación gráfica.


# Objetivo
Diseñar e implementar un juego en el que el usuario controle un elemento en pantalla para esquivar obstáculos, incrementando la dificultad progresivamente.



# Materiales
- Arduino UNO
- Pantalla OLED I2C 128x64 (SSD1306)
- 2 botones
- Protoboard
- Cables de conexión


# Conexiones
OLED:
GND → GND
VCC → 5V
SCL → A5
SDA → A4

Botones:
Botón izquierda → Pin 2 y GND
Botón derecha → Pin 3 y GND



# Metodología
Se inició con la verificación del funcionamiento de la pantalla OLED mediante un mensaje simple. Posteriormente, se conectaron los botones y se verificó su lectura. Finalmente, se desarrolló el juego implementando lógica de movimiento, colisiones y puntaje.


# Desarrollo del Juego
El juego consiste en controlar una barra en la parte inferior de la pantalla para esquivar obstáculos que caen desde la parte superior. Los obstáculos tienen tamaños variables y la velocidad aumenta progresivamente. Además, se implementó un sistema de mejor puntaje para aumentar la dificultad.



# Código del Juego
Insertar aquí el código completo desarrollado en Arduino IDE.



# Resultados
El sistema permitió visualizar el juego correctamente en la pantalla OLED, respondiendo a los botones y mostrando el puntaje y el mejor puntaje alcanzado.



# Evidencias


- Imagen de la pantalla OLED mostrando el juego en ejecución

<img width="493" height="345" alt="image" src="https://github.com/user-attachments/assets/1a49cea1-e60d-41ff-8caf-5b684838c65b" /> 



video de juego ejecutandose 

https://youtube.com/shorts/P3BpWpIdOw8?si=x9BcfDQMmfeNKI-p
















<img width="492" height="397" alt="image" src="https://github.com/user-attachments/assets/c08c01ac-72b9-441d-ad57-d1a3fe482e9a" />



- Imagen de la pantalla de GAME OVER

<img width="336" height="352" alt="image" src="https://github.com/user-attachments/assets/aa572517-ffd8-469b-b82c-e6e9422a3cf9" />



## Conclusiones
El desarrollo del juego permitió aplicar conceptos de programación en Arduino, manejo de interfaces gráficas y lógica de videojuegos, evidenciando la integración de hardware y software.






## Punto 3:
Detector de Colores con Sensor CNY70

## Introducción

En este laboratorio se implementa un sistema de detección de colores usando el sensor CNY70 y Arduino UNO. El sensor mide la reflectancia de la luz infrarroja en distintas superficies, permitiendo identificar colores mediante valores analógicos y rangos previamente definidos.

## Objetivo general

Desarrollar un sistema con Arduino y el sensor CNY70 para identificar colores según su nivel de reflectancia.


# Materiales

- Arduino UNO
- Sensor CNY70
- Resistencia 220 Ω
- Resistencia 10 kΩ
- Protoboard
- Cables de conexión
- Superficies de colores (negro, amarillo, azul, rojo, verde, blanco)

# Tabla de Rangos de Colores
Color	Rango
Negro	0 – 30
Amarillo	31 – 55
Azul	56 – 70
Rojo	71 – 90
Verde	91 – 110
Blanco	150 en adelante



# Código Arduino

int sensorPin = A0;
int valor = 0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  valor = analogRead(sensorPin);

  Serial.print("Lectura: ");
  Serial.print(valor);
  Serial.print(" -> ");

  if (valor >= 0 && valor <= 30) {
    Serial.println("NEGRO");
  }
  else if (valor >= 31 && valor <= 55) {
    Serial.println("AMARILLO");
  }
  else if (valor >= 56 && valor <= 70) {
    Serial.println("AZUL");
  }
  else if (valor >= 71 && valor <= 90) {
    Serial.println("ROJO");
  }
  else if (valor >= 91 && valor <= 110) {
    Serial.println("VERDE");
  }
  else if (valor >= 150) {
    Serial.println("BLANCO");
  }
  else {
    Serial.println("COLOR NO DEFINIDO");
  }

  delay(300);
}

# Explicación del Código

El programa lee el valor analógico del sensor CNY70 conectado al pin A0 del Arduino. 
Dependiendo del valor obtenido, se compara con rangos previamente calibrados para identificar el color de la superficie. 
Cada rango corresponde a un nivel de reflectancia de la luz infrarroja emitida por el sensor.

# Tabla de Resultados Experimentales
Color	Lecturas	Clasificación
Negro	0, 6, 14, 25, 28	NEGRO
Amarillo	32, 33, 40, 42, 54	AMARILLO
Azul	56, 57, 58	AZUL
Rojo	78, 80, 82, 87	ROJO
Verde	103, 109, 118, 126	VERDE
Blanco	741, 912, 923, 926	BLANCO


# Evidencias

https://youtube.com/shorts/6MuyHEWUEtI?si=CaT0Sx2kKo-njw-2


# Conclusión

Se logró implementar un sistema funcional de detección de colores mediante el uso del sensor CNY70 y Arduino. Los resultados demostraron que es posible identificar colores de forma sencilla a partir de la reflectancia, lo que evidencia su utilidad en aplicaciones de automatización y robótica.
