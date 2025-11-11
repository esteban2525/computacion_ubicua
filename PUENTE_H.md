// --- Motor Shield L298P ---
// Motor A conectado a M1 (izquierdo)
// Motor B conectado a M2 (derecho)

// Pines Motor A
#define IN1 12
#define IN2 13
#define ENA 10

// Pines Motor B
#define IN3 8
#define IN4 9
#define ENB 11

// Pines receptor KRF100
#define D0 2  // Botón adelante
#define D1 3  // Botón atrás
#define D2 4  // Botón izquierda
#define D3 5  // Botón derecha

// --- Variables de potencia ---
int velAdelante = 200;   // Potencia base al ir hacia adelante
int velAtras = 200;      // Potencia base al ir hacia atrás
int velGiro = 100;       // Potencia para giros (izq/der)

// ---- Calibración para corregir desviación a la derecha ----
int compIzq = -20;         // Baja potencia motor izquierdo
int compDer = 20;          // Sube potencia motor derecho

void setup() {
  Serial.begin(9600);

  // Pines del motor
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENB, OUTPUT);

  // Pines del receptor
  pinMode(D0, INPUT);
  pinMode(D1, INPUT);
  pinMode(D2, INPUT);
  pinMode(D3, INPUT);

  Serial.println("Sistema listo para controlar potencias...");
}

void loop() {
  // Lectura de botones
  bool adelante = digitalRead(D0);
  bool atras = digitalRead(D1);
  bool izquierda = digitalRead(D2);
  bool derecha = digitalRead(D3);

  // --- Control lógico ---
  if (adelante) {
    Adelante(velAdelante);
  }
  else if (atras) {
    Atras(velAtras);
  }
  else if (izquierda) {
    Izquierda(velGiro);
  }
  else if (derecha) {
    Derecha(velGiro);
  }
  else {
    Parar();
  }
}

// --- FUNCIONES DE MOVIMIENTO ---

void Adelante(int vel) {
  int pwmA = limitar(vel + compIzq);
  int pwmB = limitar(vel + compDer);
  Serial.print("→ Adelante | PWM Izq: "); Serial.print(pwmA);
  Serial.print(" | PWM Der: "); Serial.println(pwmB);

  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  analogWrite(ENA, pwmA);

  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENB, pwmB);
}

void Atras(int vel) {
  int pwmA = limitar(vel + compIzq);
  int pwmB = limitar(vel + compDer);
  Serial.print("← Atrás | PWM Izq: "); Serial.print(pwmA);
  Serial.print(" | PWM Der: "); Serial.println(pwmB);

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  analogWrite(ENA, pwmA);

  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  analogWrite(ENB, pwmB);
}

void Izquierda(int vel) {
  Serial.print("↺ Izquierda (potencia: "); Serial.print(vel); Serial.println(")");
  // Motor izquierdo parado
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  analogWrite(ENA, 0);

  // Motor derecho avanza
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENB, limitar(vel + compDer));
}

void Derecha(int vel) {
  Serial.print("↻ Derecha (potencia: "); Serial.print(vel); Serial.println(")");
  // Motor derecho parado
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
  analogWrite(ENB, 0);

  // Motor izquierdo avanza
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  analogWrite(ENA, limitar(vel + compIzq));
}

void Parar() {
  Serial.println("■ Parado");
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 0);
  analogWrite(ENB, 0);
}

// --- Función de seguridad ---
int limitar(int valor) {
  if (valor > 255) return 255;  // No pasar del máximo PWM
  if (valor < 0) return 0;
  return valor;
}

// --- CONTROLAR POTENCIA DE MOTORES INDIVIDUALES ---

void setPotenciaMotorA(int potencia) {
  analogWrite(ENA, limitar(potencia));
  Serial.print("Potencia Motor A: ");
  Serial.println(limitar(potencia));
}

void setPotenciaMotorB(int potencia) {
  analogWrite(ENB, limitar(potencia));
  Serial.print("Potencia Motor B: ");
  Serial.println(limitar(potencia));
}


<img width="1096" height="615" alt="image" src="https://github.com/user-attachments/assets/bd97ebe5-1cac-4656-ad87-2ff1f3196426" />
