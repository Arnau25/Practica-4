# PRACTICA 4: Real-Time Operating System

### Ejercicio 1

En este programa lo que haremos es crear una tarea llamada anotherTask la cual se va a ejecutar en paralelo con el loop del main.

Luego podemos observar que en el puerto serie se nos mostraran los mensajes "this is ESP32 Task" y "this is another Task" cada 1000 ms idependientemente una de la otra. 

Un ejemplo seria cambiar nuestra funcion anotherTask a la mitad de tiempo, entonces esto provocaria que cada 500ms aparecerá "this is another Taks" sin influir el período de realizacion de la "this is ESP32 Task".


### Ejercicio 2

En este apartado utilizaremos un semaforo mutex para coordinar el encendido y apagado del led.
````
#include <Arduino.h>

void ledON (void * pvParameters);
void ledOFF(void * pvParameters);
int LED =2; 

SemaphoreHandle_t semafor;

void setup(){

Serial.begin(9600);
pinMode(LED, OUTPUT);
semafor= xSemaphoreCreateMutex();

xTaskCreate(
    ledON,
    "LED ON",
    10000,
    NULL,
    1,
    NULL);

xTaskCreate(
    ledOFF,
    "LED OFF",
    10000,
    NULL,
    1,
    NULL);

}

void loop(){}

void ledON (void * pvParameters){
    for(;;){
        xSemaphoreTake(semafor, portMAX_DELAY);
        digitalWrite(LED, HIGH);
        delay(3000);
        xSemaphoreGive(semafor);
    }
}

void ledOFF (void * pvParameters){
    for(;;){
        xSemaphoreTake(semafor, portMAX_DELAY);
        digitalWrite(LED, LOW);
        delay(1500);
        xSemaphoreGive(semafor);
    }
}
````
