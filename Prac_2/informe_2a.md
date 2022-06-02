# Práctica 2a

El objetivo de la práctica és comprender el funcionamiento de las interrupciones.

Hay diferentes tipos de interrupciones:

* Evento hardware
* Un evento programado
* Llamada por software

En la primera parte de la práctica veremos una interrupción de un evento hardware, que pulsaremos un interruptor.


## Código:

```
#include <Arduino.h>
#define LED 16
#define INTER 0
 
void IRAM_ATTR isr(){
  /*codi a exetuar en l'interrupció,
  apagar el led*/
  digitalWrite(LED, LOW);
  Serial.println("OFF");
  delay(1000);
};
 
void setup() {
  Serial.begin(115200);
  pinMode(LED, OUTPUT);
  pinMode(INTER, INPUT_PULLUP);
  attachInterrupt(INTER, isr, FALLING);
}
 
void loop() {
  digitalWrite(LED, HIGH);
  Serial.println("ON");
  /*codi per aturar la interrupció despres de 5segons,
  és a dir, tornar a encendre el led*/
  static uint32_t lastMillis=0;
  //s'utilitza una variable "static" per tal que només s'inicialitzi una unica vegada amb la primera crida
  //a més la variable no es destrueix si no s'esta executant la funcio loop (amb valor inicial 0).
  if(millis()-lastMillis>60000){
    lastMillis=millis();
    detachInterrupt(INTER);
    Serial.println("Interrupcio finalitzada");
  }
}

```
---
## Funcionamiento:

Hemos asignado el PIN 16 a un led que se activara cuando el pulsador (en este caso el 0) sea accionado, y se apagará cuando se vuelva a activar.

Si el pulador no es utilizado durante mas de un minuto salrá por pantalla un mensaje que dirá: "Interrupcio finalitzada". Si quisieramos volver a activar el programa tendriamos que darle al RST.