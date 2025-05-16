#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// Configuración de pantalla OLED
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Pines del sensor ultrasónico
const int trigPin = 9;
const int echoPin = 10;

// Pin del relé
const int relayPin = 8;

// Distancia máxima del contenedor (ajústala a la altura en cm)
const int distanciaMaxima = 40;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(relayPin, OUTPUT);
  digitalWrite(relayPin, LOW);

  Serial.begin(9600);

  // Inicializar pantalla OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("No se pudo iniciar la pantalla OLED"));
    while (true); // Detener si falla
  }
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0,0);
  display.println("Sistema iniciado...");
  display.display();
  delay(2000);
}

void loop() {
  long duracion;
  int distancia;
  int porcentajeLlenado;

  // Pulso ultrasónico
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Medir eco
  duracion = pulseIn(echoPin, HIGH);
  distancia = duracion * 0.034 / 2;

  // Calcular porcentaje de llenado
  if (distancia >= distanciaMaxima) {
    porcentajeLlenado = 0;
  } else if (distancia <= 0) {
    porcentajeLlenado = 100;
  } else {
    porcentajeLlenado = map(distancia, distanciaMaxima, 0, 0, 100);
  }

  // Mostrar en monitor serial
  Serial.print("Distancia: ");
  Serial.print(distancia);
  Serial.print(" cm\tLlenado: ");
  Serial.print(porcentajeLlenado);
  Serial.println("%");

  // Mostrar en OLED
  display.clearDisplay();
  display.setCursor(0, 0);
  display.println("Nivel del Contenedor:");
  display.setTextSize(2);
  display.setCursor(10, 20);
  display.print(porcentajeLlenado);
  display.print(" %");
  display.setTextSize(1);
  display.setCursor(0, 50);
  if (porcentajeLlenado == 0) {
    display.println("Estado: Vacio - Motor ON");
    digitalWrite(relayPin, HIGH); // Encender motor
  } else {
    display.println("Estado: Lleno - Motor OFF");
    digitalWrite(relayPin, LOW);  // Apagar motor
  }
  display.display();

  delay(1000);
}
