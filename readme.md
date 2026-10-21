El código es: 

const int pinEcho = 9;
const int pinTrig = 10;
const int pinLDR = A0;
const int pinLED = 3;

// Parámetros ajustables
const int distanciaMaxima = 30; // Distancia en cm
const int umbralOscuridad = 400; // Ajustar según la luz

void setup() {
pinMode(pinTrig, OUTPUT);
pinMode(pinEcho, INPUT);
pinMode(pinLED, OUTPUT);

Serial.begin(9600);
}

void loop() {
// Medir distancia con el HC-SR04
digitalWrite(pinTrig, LOW);
delayMicroseconds(2);

digitalWrite(pinTrig, HIGH);

delayMicroseconds(10);
digitalWrite(pinTrig, LOW);

long duracion = pulseIn(pinEcho, HIGH, 30000);

float distancia = duracion * 0.0343 / 2;

// Leer la luminosidad de la LDR
int luz = analogRead(pinLDR);

// Mostrar valores en el monitor serie
Serial.print(&quot;Distancia: &quot;);
Serial.print(distancia);
Serial.print(&quot; cm | Luz: &quot;);
Serial.println(luz);

// Encender el LED si hay un objeto cerca
// y el ambiente está oscuro
if (duracion &gt; 0 &amp;&amp;
distancia &lt;= distanciaMaxima &amp;&amp;
luz &lt; umbralOscuridad) {
digitalWrite(pinLED, HIGH);
} else {
digitalWrite(pinLED, LOW);
}

delay(200);
}
