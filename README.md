// Pines seguros para NodeMCU (ESP8266)
const int pinAbanico = D1;    // Pin D1: LED de prueba (luego irá al transistor del abanico)
const int ledInhalar = D2;    // Pin D2: LED indicador de Inhalación
const int ledExhalar = D5;    // Pin D5: LED indicador de Exhalación

// Tiempos de respiración (en milisegundos)
const int tiempoInhalar = 4000;  // 4 segundos inhalando
const int tiempoSostener = 1000; // 1 segundo sosteniendo aire
const int tiempoExhalar = 4000;  // 4 segundos exhalando

void setup() {
  pinMode(pinAbanico, OUTPUT);
  pinMode(ledInhalar, OUTPUT);
  pinMode(ledExhalar, OUTPUT);
  
  // Todo apagado al arrancar
  digitalWrite(pinAbanico, LOW);
  digitalWrite(ledInhalar, LOW);
  digitalWrite(ledExhalar, LOW);
}

void loop() {
  // --- 1. INHALACIÓN ---
  digitalWrite(ledInhalar, HIGH);
  digitalWrite(ledExhalar, LOW);
  
  // En ESP8266 el PWM va de 0 a 1023
  for (int potencia = 400; potencia <= 1023; potencia += 4) {
    analogWrite(pinAbanico, potencia);
    delay(tiempoInhalar / 155); 
  }

  // --- 2. SOSTENER ---
  delay(tiempoSostener);

  // --- 3. EXHALACIÓN ---
  digitalWrite(pinAbanico, LOW);
  digitalWrite(ledInhalar, LOW);
  digitalWrite(ledExhalar, HIGH);
  
  delay(tiempoExhalar);
}
