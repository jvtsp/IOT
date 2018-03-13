#include "SoftwareSerial.h"

int anterior = 0;
int microphone = A0;
int viber = 5;

unsigned long inicioVibracao;

#define DURACAO_VIBRACAO 300
#define SENSIBILIDADE 100
#define INTERVALO_LEITURA 20
#define BAUD_RATE 9600
#define PORCENTAGEM_DE_GIRO 1.3
//Media
const int numReadings = 10;

int readings[numReadings];
int readIndex = 0;          
int total = 0;              
int average = 0;  

//

SoftwareSerial bluetooth(2, 3);
void setup()
{
  pinMode(viber, OUTPUT);
  pinMode(microphone, INPUT);
  Serial.begin(9600);
  bluetooth.begin(BAUD_RATE);
  //
  int valorInicial = analogRead(microphone);
for (int thisReading = 0; thisReading < numReadings; thisReading++) {
    readings[thisReading] = valorInicial;
  }
  total = valorInicial * numReadings;

  //
}
void loop()
{
  ///
// subtract the last reading:
  total = total - readings[readIndex];
  // read from the sensor:
  readings[readIndex] = analogRead(microphone);
  
  // add the reading to the total:
  total = total + readings[readIndex];
  // advance to the next position in the array:
  readIndex = readIndex + 1;
  
  if (readIndex >= numReadings) {
    readIndex = 0;
  }

  average = total / numReadings;
  ///
 int now = analogRead(microphone);
  Serial.println(now);
  Serial.println(average);
  delay(INTERVALO_LEITURA);
  delay(20);
  if ( average * PORCENTAGEM_DE_GIRO < now )
  {
    comecarVibrar();
  }
  // determinar se é pra parar
  if (inicioVibracao > 0 && inicioVibracao + DURACAO_VIBRACAO < millis())
  {
    pararVibrar();
  }

  anterior = now;
}

void enviarHistoricoVibracao()
{
      bluetooth.println("[v]=1");
}

void comecarVibrar()
{
  inicioVibracao = millis();
  digitalWrite(viber, HIGH);
  enviarHistoricoVibracao();
}
void pararVibrar()
{
  digitalWrite(viber, LOW);
  inicioVibracao = 0;
}
