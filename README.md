Proyecto: Luz de Noche Automática e Inteligente.
Se seleccionó este proyecto debido a que soluciona un problema cotidiano de seguridad dentro del hogar, requiere un presupuesto accesible para el equipo y permite una simulación fluida dentro de la plataforma Wokwi.   

Problematica: Transitar por pasillos, escaleras o habitaciones a oscuras durante la noche representa un riesgo constante de tropiezos y caídas dentro del hogar. Por otro lado, dejar luces encendidas de forma continua durante toda la noche genera un gasto innecesario de energía eléctrica. Los interruptores de luz tradicionales resultan incómodos de encontrar en la oscuridad, en especial para niños, adultos mayores o personas con movilidad reducida.

Historias de usuario: Como usuario que circula por la casa durante la noche,
Quiero que la luz se encienda de forma automática únicamente cuando me acerque y el ambiente esté oscuro,
Para transitar con seguridad sin tener que buscar interruptores a ciegas ni desperdiciar energía eléctrica toda la noche.

Requerimientos Funcionales
RF1 - Lectura de Luminosidad: El sistema debe monitorear en tiempo real la luz ambiental mediante una fotorresistencia (LDR).
RF2 - Detección de Proximidad: El sistema debe medir continuamente la distancia a través del sensor ultrasónico HC-SR04 para detectar la presencia de personas.
RF3 - Lógica de Encendido: El sistema debe activar el LED de iluminación únicamente cuando la luz ambiental esté por debajo del umbral establecido (oscuridad) Y se detecte una persona a menos de 50 cm.
RF4 - Desactivación Automática: El sistema debe apagar el LED automáticamente cuando la persona se aleje del rango o la luz ambiental vuelva a subir.

Requerimientos No Funcionales:
RNF1 - Tiempo de Respuesta: El tiempo entre la detección de presencia y el encendido del LED debe ser menor a 300 ms.
RNF2 - Simulación y Validación: Toda la lógica del código y las conexiones deben estar 100% probadas en Wokwi antes de comprar el hardware físico.   
RNF3 - Bajo Costo: El presupuesto global de componentes debe ser mínimo para facilitar la compra compartida entre los integrantes del equipo.   
RNF4 - Eficiencia Energética: La iluminación operará solo por demanda corta para reducir el consumo eléctrico al mínimo.

Precios estimados: 
Arduino Nano V3 (con cable USB) $8.000
Sensor Ultrasónico HC-SR041 $3.800
Fotorresistencia (LDR) + Resistencia 10Omega $2.000
LED Blanco/Cálido + Resistencia 220 1=$500 2=$1.000 
Protoboard de 400 puntos $3.500
Cables Jumper (Macho-Macho / Macho-Hembra) 1=$200 10=$2.000
TOTAL ESTIMADO$20.300

El codigo es:
const int TRIG_PIN = 10; 
const int ECHO_PIN = 9;  
const int LDR_PIN = A0;      
const int LED_PIN = 3;       

const int UMBRAL_OSCURIDAD = 400;   
const float UMBRAL_DISTANCIA = 200.0; 

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);

  Serial.begin(9600);
  Serial.println("--- Sistema Reiniciado ---");
}

void loop() {
  int valorLDR = analogRead(LDR_PIN);

  // Asegurar pulso limpio en TRIG
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(4);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);


  long duracion = pulseIn(ECHO_PIN, HIGH, 30000);
  float distanciaCm = duracion * 0.0343 / 2.0;

  bool hayOscuridad = (valorLDR < UMBRAL_OSCURIDAD); 
  bool hayPresencia = (distanciaCm > 2.0 && distanciaCm < UMBRAL_DISTANCIA);

  Serial.print("LDR: "); Serial.print(valorLDR);
  Serial.print(" (Oscuro: "); Serial.print(hayOscuridad ? "SI" : "NO");
  Serial.print(") | Dist: "); Serial.print(distanciaCm);
  Serial.print(" cm (Cerca: "); Serial.print(hayPresencia ? "SI" : "NO");
  Serial.println(")");

  if (hayOscuridad && hayPresencia) {
    digitalWrite(LED_PIN, HIGH);
  } else {
    digitalWrite(LED_PIN, LOW);
  }

  delay(400);
}
