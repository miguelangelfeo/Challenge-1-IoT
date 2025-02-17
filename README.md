# Detección Temprana de Crecidas Mediante Soluciones IoT

## Resumen general

Colombia es un país caracterizado por su variada topografía y patrones climáticos variados. Esta variedad combinada con fenómenos climáticos como La Niña, incrementa significativamente la vulnerabilidad del país a crecidas de ríos e inundaciones, afectando gravemente a comunidades y generando daños a la infraestructura. Frente a este panorama, la detección temprana de crecidas se convierte en un pilar fundamental para la gestión del riesgo de desastres, enfoque que se decidió adoptar en el desarrollo de un proyecto para la clase de Internet de las Cosas. 

Se considera favorable una solución tecnológica basada en Internet de las Cosas (IOT), debido a la capacidad que tiene de monitorear variables ambientales en tiempo real, permitiendo una detección oportuna de puedan desencadenar inundaciones. Además, el traspaso de información de sensores conectados y sistemas de alerta automáticos puede marcar la diferencia entre una evacuación exitosa y una tragedia. 

## Estructura del Documento

1. **Solución Propuesta**
   - Arquitectura propuesta
   - Restricciones de diseño identificadas
   - Criterios de diseño
   - Estándares de ingeniería aplicados
2. **Desarrollo y evaluación del prototipo**
   - Configuración experimental
   - Resultados y Análisis
   - Autoevaluación del protocolo  
3. **Conclusiones**
4. **Referencias**

---

## 1. Solución propuesta

### 1.1 Arquitectura propuesta

**Diagrama de bloques**

La solución IoT propuesta consta de tres capas:
1. **Capa de recolección de datos**: Compuesta por un sensor **HC-SR04** (ultrasónico para medir distancias) y un **módulo de sensor de lluvia**.
2. **Procesamiento**: Implementado con un **Arduino Uno**, que recibe y procesa los datos de los sensores.
3. **Actuación**: Incluye un **LED de alerta**, una **pantalla LCD 16x2** y un **buzzer** para generar advertencias visuales y sonoras.

### 1.2 Restricciones de diseño identificadas

- Sensibilidad del cableado ante el contacto con líquidos.
- Necesidad de protección contra condiciones climáticas extremas.
- Costos asociados a la implementación en gran escala.

### 1.3 Criterios de diseño

#### Sensores
- **HC-SR04**: Permite medir la distancia entre el nivel del agua y el límite permitido.
- **Módulo de sensor de lluvia**: Detecta la presencia y la intensidad de la lluvia.

#### Microcontrolador
- **Arduino Uno**: Facilita la conexión y el procesamiento de datos de los sensores.

#### Actuadores
- **LED de alerta**: Se enciende para advertir lluvias moderadas o fuertes.
- **LCD 16x2**: Muestra la distancia del nivel del agua y mensajes de advertencia.
- **Buzzer**: Suena cuando el nivel del agua alcanza un umbral crítico.

### 1.4 Estándares de ingeniería aplicados

- **ISO 9001**: Gestión de calidad en diseño electrónico.
- **ISO 4373**: Medición del nivel de la superficie del agua.

---

## 2. Desarrollo y evaluación del prototipo

### 2.1 Configuración experimental

Se realizó una verificación de los componentes disponibles, seguida de una fase de ideación para diseñar un sistema funcional. Se utilizó **Fritzing** para modelar el circuito antes de la implementación física. Finalmente, se montó un prototipo en una maqueta que simula el crecimiento de un río.

### 2.2 Resultados

Los sensores y actuadores funcionaron correctamente:
- **El sensor ultrasónico HC-SR04** detectó con precisión el nivel del agua.
- **El sensor de lluvia** activó correctamente la alerta visual.
- **El Arduino Uno** gestionó la interacción entre los sensores y actuadores.
- **La pantalla LCD** mostró información relevante.
- **El buzzer** emitió una alarma efectiva en situaciones críticas.

### 2.3 Autoevaluación del protocolo de pruebas

El sistema desarrollado cumplió con los requisitos mínimos, generando notificaciones locales efectivas. Se identificaron oportunidades de mejora, como la protección de los componentes electrónicos y la integración de conectividad inalámbrica para monitoreo remoto.

---

## 3. Conclusiones

El proceso de prototipado mostró que las soluciones IoT para la detección de crecidas pueden ser económicamente asequibles sin afectar su rendimiento. Se concluye que:
- Se lograron integrar sensores y actuadores de manera efectiva.
- La solución cumple con los requerimientos técnicos establecidos.
- Se pueden explorar mejoras como la comunicación inalámbrica y la optimización del diseño físico.

---

## 4. Referencias

1. **Androbot TECH** - Uso del sensor para medir el nivel de agua. [Ver video](https://www.youtube.com/watch?v=VobYZbzTXzc)
2. **SriTu Hobby** - Tutorial del sensor de lluvia con Arduino Uno. [Leer más](https://srituhobby.com/rain-sensor-tutorial-rain-sensor-with-arduino-uno/)
3. **ISO 9001** - Normas de calidad en diseño electrónico. [Consultar ISO](https://www.normas-iso.com/iso-9001/)
4. **ISO 4373** - Hidrometría y medición del nivel del agua. [Ver normativa](https://www.une.org/encuentra-tu-norma/busca-tu-norma/norma/?c=N0070707)

---

## 5. Código fuente documentado

```cpp
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

const int echo = 4;
const int trigger = 5;
const int buzzer = 12;
const int led = 6;
const int sensor_lluvia = A3;

long tiempo;
float distancia;
int isRaining = 100;
int value = 1000;
const float water_level = 10.0;

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  pinMode(trigger, OUTPUT);
  pinMode(echo, INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(led, OUTPUT);
  Serial.begin(9600);
  lcd.init();
  lcd.backlight();
}

void loop() {
  medir();
  checkRain();
  updateDisplay();

  if (distancia <= water_level) {
    tone(buzzer, 523, 500);
    delay(500);
  }
}

void medir() {
  digitalWrite(trigger, LOW);
  delayMicroseconds(2);
  digitalWrite(trigger, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigger, LOW);

  tiempo = pulseIn(echo, HIGH);
  distancia = float(tiempo * 0.0343) / 2;
  delay(10);
}

void checkRain() {
  value = analogRead(sensor_lluvia);
  if (value < 300) {
    isRaining = true;
    digitalWrite(led, HIGH);
  } else {
    digitalWrite(led, LOW);
  }
  isRaining -= 1;
  delay(100);
}

void updateDisplay() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Al limite: ");
  lcd.print(distancia);
  lcd.setCursor(0, 1);
  lcd.print(distancia > water_level ? "Seguro" : "Corran");
  delay(250);
}
```
