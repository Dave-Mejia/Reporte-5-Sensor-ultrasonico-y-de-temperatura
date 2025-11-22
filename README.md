# Reporte 5 Tarjeta ESP32 con sensor ultrasonico y de temperatura

Programación de un ESP32 con un sensor ultrasónico HC-SR04 un sensor de temperaura DHT22 y una pantalla LCD 16x2(I2C)
## Introducción
## Descripción
Utilizaremos l plataforma WOKWI para simular la adquisición de datos de distancia mediante un sensor ultrasónico HC-SR04, un sensor de temperaura DHT22 y la programación del mismo en un microcontrolador ESP32, los datos se mostrarán en una pantalla LCD

## Material Necesario
Para realizar esta practica necesitas lo siguiente

Plataforma WOKWI
Tarjeta ESP 32
Sensor ultrasónico HC-SR04
Sensor de temperaura DHT22
Pantalla LCD 16x2(I2C)

## Instrucciones
### PREVIO
1. Abrir la plataforma WOKWI.

### Preparación
3. Ir a la pestaña de sketch.ino y borrar el codigo e programación predeterminado
4. Abrir la terminal de programación y colocar la siguente programación:

```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

const int DHT_PIN = 16;
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 15;   //Pin digital 3 para el Echo del sensor

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();

}

void loop()
 {


//SENSOR ULTRSONICO
  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm

//SENSOR DETEMPERATURA
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");


  lcd.clear();
  lcd.setCursor(1, 0);
  lcd.print("Diplomado AIyM");
  lcd.setCursor(3, 1);
  lcd.print("22 Nov 25");
  delay(1500);
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Ing.David Mejia");
  delay(1500);
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia:");
  lcd.print(d);
  lcd.setCursor(14, 0);
  lcd.print("cm");
  delay(1500);
  lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  delay(1500); 
}
```

4. Ir a la pestaña "Library manager" haer clic sobre el icon "+", buscar la libreria "HCSR04 ultrasonic sensor" y agregarla
![](https://github.com/Dave-Mejia/Reporte-4-ESP32-con-sensor-ultrasonico/blob/main/libreria%20sensor%20ultrasonico.png?raw=true)
5. 4. Ir a la pestaña "Library manager" haer clic sobre el icon "+", buscar la libreria "DHT sensor library for ESPx" y agregarla
![](https://github.com/Dave-Mejia/Reporte-5-Sensor-ultrasonico-y-de-temperatura/blob/main/Libreria%20DHT.png?raw=true)

6. Ir al esquema de simulacón, dar clic al icono "+ (add new part)", buscar el sensor HCSR04 y agregar
7. Ir al esquema de simulacón, dar clic al icono "+ (add new part)", buscar el sensor DHT22 y agregar 
8. Ir al esquema de simulacón, dar clic al icono "+ (add new part)", buscar la pantalla LCD 16x2(I2C)
   
9. Colocar los sensores y la pantalla lcd sobre el esquema de simulación y conectar como indica la figura de abajo
![](<img width="817" height="527" alt="image" src="https://github.com/user-attachments/assets/0f56525b-abe7-4530-86f1-aeb58065c6e9" />)

### Operación
10. Iniciar simulador dando clic en el icono "play"
11. Visualizar los datos en el monitor serial.

## Resultados
Cuando haya funcionado, verás los valores dentro del monitor serial como se muestra en la siguente imagen.
![](https://github.com/Dave-Mejia/Reporte-5-Sensor-ultrasonico-y-de-temperatura/blob/main/resultado%20sensor%20ultrasonico%20y%20temperatura%201.png?raw=true)
![](https://github.com/Dave-Mejia/Reporte-5-Sensor-ultrasonico-y-de-temperatura/blob/main/resultado%20sensor%20ultrasonico%20y%20temperatura%202.png?raw=true)
![](<img width="950" height="736" alt="image" src="https://github.com/user-attachments/assets/48dd1122-448b-40a9-a5db-53cb1d3cf683" />)
![](<img width="952" height="738" alt="image" src="https://github.com/user-attachments/assets/46a264a2-3641-413d-aa16-3a3009acb941" />=



## Créditos
Ralizado por el Ingeniero David Mejía

