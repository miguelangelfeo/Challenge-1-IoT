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
5. **Anexos**

---

## 1. Solución propuesta

### 1.1 Arquitectura propuesta

![Texto alternativo](https://github.com/miguelangelfeo/Challenge-1-IoT/blob/daa022315f03e84a254685aa19d3523a8580e7c4/Diagrama%20de%20Bloques.jpg)

**Figura 1. Diagrama de bloques sobre la arquitectura de la solución IoT**

El presente diagrama permite visualizar la división de módulos en que se compone la solución IoT propuesta. Consiste, principalmente, en 3 capas dedicadas a la recolección de datos, procesamiento y actuación a partir de dicha información. La primera de ellas, la capa de recolección de información a partir del entorno se compone de un sensor HC-SR04, un ultrasónico que permite cuantificar distancias [1], empleado para medir la lejanía entre el nivel del agua y los límites definidos como permitidos, junto con un ‘raindrop sensor module’ [2], los cuales en este caso permiten determinar si la fuerza de la lluvia es algún signo de peligro de crecimiento en los niveles del agua o si la lluvia está siendo moderada. Los datos son recibidos y procesados mediante un Arduino uno, y este envía la información a los actuadores que permiten saber lo que está sucediendo. El led permite saber en qué momento está lloviendo, activándose como señal de alerta, junto con una pantalla LCD 16X2 permitiendo mostrar la distancia entre el agua y el límite propuesto, además de mostrar un mensaje de precaución. Por último, se tiene el Buzzer, el cual es la alarma que suena una vez el agua alcanza o sobrepasa los limites dispuestos con la medición. 

![Texto alternativo](https://github.com/miguelangelfeo/Challenge-1-IoT/blob/daa022315f03e84a254685aa19d3523a8580e7c4/Diagrama%20UML.jpg)

**Figura 2. Diagrama UML de la solución**

### 1.2 Restricciones de diseño identificadas

Una restricción técnica identificada en la fase de diseño consiste en la sensibilidad del cableado ante el flujo de líquidos en las condiciones actuales. Durante las pruebas físicas del montaje, hubo que manejar con especial cuidado el sensor de lluvia por su distintivo contacto con el agua, elemento eléctricamente riesgoso. Esto no sólo se limita a las pruebas de funcionamiento, sino que constituye toda una restricción de espacio si se considera la implementación a nivel masivo, teniendo en cuenta que proteger dispositivos eléctricos de condiciones climáticas extremas podría resultar costoso a nivel monetario y logístico. 

### 1.3 Criterios de diseño

#### Sensores
- **HC-SR04**: Este sensor fue seleccionado debido a la facilidad de uso y la forma en la que cumplía con lo planeado a la hora de buscar solución ante el problema, es decir permitía de manera sencilla medir la distancia entre el límite y el nivel del agua.
- **Módulo de sensor de lluvia**: Este sensor era el más adecuado a la hora de implementar algún modulo que permitiera saber si estaba lloviendo, ayudando a la resolución del problema y a la recepción de más datos. 

#### Microcontrolador
- **Arduino Uno**:  Se utilizo un Arduino Uno debido a su facilidad de uso y a su compatibilidad con los sensores que se nombraron anteriormente, siendo el más adecuado para la práctica y ayudando a la fácil conexión entre dispositivos. 

#### Actuadores
- **LED de alerta**: Este LED fue utilizado con el propósito de mostrar una advertencia cuando se sienta que está lloviendo en cantidades medianas/fuertes. Permite ayudar al cliente a estar alerta una vez se encienda el LED.
- **LCD 16x2**: Este LCD permite mostrar la distancia entre el agua y el límite propuesto, siendo un aporte a la solucion, y permite mostrar un mensaje que define el estado del agua, es decir si se necesita tomar precaución o si es un peligro el nivel del agua.
- **Buzzer**: El buzzer es la alarma que suena una vez el nivel del agua llega al límite, lo que permite alertar a las personas del peligro que esto puede representar. El buzzer se seleccionó debido a esta función, siendo uno de los principales componentes de la solución.

### 1.4 Estándares de ingeniería aplicados

- **ISO 9001**: Asegura que el diseño del hardware cumple con procesos estructurados de calidad. [3].
- **ISO 4373**: Asegura la correcta medición del nivel del agua. [4].

---

## 2. Desarrollo y evaluación del prototipo

### 2.1 Configuración experimental

En primera instancia, se revisaron los componentes disponibles dentro del kit Raspberry Pi de SunFounder, así como otros sensores, actuadores y microcontroladores que podrían ser útiles para el proyecto. Esta etapa permitió identificar qué elementos estaban a disposición del equipo y cuáles serían necesarios para implementar la solución.

![Texto alternativo](https://github.com/miguelangelfeo/Challenge-1-IoT/blob/daa022315f03e84a254685aa19d3523a8580e7c4/Materiales%20disponibles.jpg)

**Figura 3. Listado de los materiales disponibles dentro del kit Raspberry Pi**

Una vez verificados los recursos, se procedió a la fase de ideación, donde se discutieron diversas estrategias para diseñar un sistema funcional que cumpliera con los requisitos mínimos establecidos en el reto. Se analizaron diferentes componentes y se definió cómo los sensores interactuarían con el microcontrolador, qué tipo de alertas se generarían y cómo se estructuraría la visualización de los datos. Una vez se definió cómo el sistema cumpliría con los requisitos mínimos, se ideó distintas formas de mejorar el sistema, incluyendo la capacidad de monitorear cuánto crece el nivel del agua y el poder ofrecer alertas progresivas. 

Posteriormente, fue necesario realizar una simulación utilizando Fritzing. Inicialmente, se modelo el circuito para verificar la interconexión de todos los componentes escogidos, permitiendo detectar posibles problemas de conexión antes de la implementación física. Una vez establecida la simulación con todos los componentes, se realizó el código, con el cual se configuraban todas las acciones que se iban a realizar mediante el circuito, como la lectura de sensores, activación de los actuadores y actualización de la pantalla LCD. Una vez se tenía listo todos los aspectos relacionados con el software, llego el momento de realizar la parte de hardware, donde se realizaron todas las conexiones de los componentes físicamente, y su posicionamiento en una maqueta que simula el crecimiento de un río.  

![Texto alternativo](https://github.com/miguelangelfeo/Challenge-1-IoT/blob/daa022315f03e84a254685aa19d3523a8580e7c4/Diagrama%20de%20Hardware.jpg)

**Figura 4. Diagrama de hardware simulado en Fritzing**

### 2.2 Resultados

Comprobar el funcionamiento del sistema era de vital importancia. Para lo cual, se posicionó el sensor de lluvia sobre un recipiente, y se le regaba agua para funcionar como una simulación de lluvia, verificando su funcionamiento prendiendo un led de emergencia y mostrando un mensaje en la pantalla LCD. Utilizando este recipiente también se verifico el funcionamiento del sensor, una vez el nivel del agua estaba muy cerca al límite definido, el sensor detectaba que tan cerca estaba el agua del límite, permitiendo enviar el dato al Arduino, encendiendo de esta manera el buzzer y mostrando un mensaje de peligro en la pantalla LCD. 

Se obtuvo que todos los componentes funcionaban de manera correcta. El sensor ultrasonido HC-SR04 permitió detectar que tan alto estaba el nivel del agua, ayudando a cumplir con la parte más importante de la solución. El sensor de lluvia también recibió el dato de manera eficiente, demostrando su utilidad, ayudando a tener diferentes precauciones a la hora de implementar este proyecto para resolver el problema. El Arduino Uno permitió la correcta comunicación entre componentes y es el que logra que todo funcione. Por último, el buzzer, el led y la pantalla LCD funcionaron de forma correcta, advirtiendo cuando había presencias de lluvias moderadas y fuertes, y permiten avisar cuando el nivel del agua está muy alto, siendo esta la forma de mostrar el peligro, y son medidas de advertencia muy eficientes a la hora de presentar la solución.  

### 2.3 Autoevaluación del protocolo de pruebas

El desarrollo del prototipo IoT para la detección temprana de crecidas en ríos tuvo como base el cumplimiento de los requisitos mínimos establecidos en el reto. Estos consistían en la creación de un sistema funcional capaz de monitorear en tiempo real la presencia de crecidas debido a lluvias intensas y generar una notificación local cuando se detectara un evento de este tipo. 

Para cumplir con estos requerimientos, se implementó un sensor ultrasónico HC-SR04, el cual permitió medir la distancia entre el nivel del agua y un límite crítico definido. Junto a este, se utilizó un sensor de lluvia que detectó la presencia e intensidad de precipitaciones, activando un LED de alerta cuando estas alcanzaban niveles moderados o fuertes. La información de ambos sensores fue procesada mediante un microcontrolador Arduino Uno, que gestionó la activación de una pantalla LCD para mostrar datos relevantes y un buzzer para emitir una alarma sonora en caso de niveles críticos de agua. Estos elementos permitieron la notificación in situ requerida en el planteamiento del problema. 

Más allá de cumplir con los requisitos mínimos, se incorporaron mejoras adicionales que aumentaron la funcionalidad del sistema. La solución no solo detecta cuando el agua alcanza un nivel crítico, sino que también permite monitorear cómo evoluciona la crecida, proporcionando datos sobre cuánto ha aumentado el nivel del río en un período de tiempo. Esta capacidad amplía el alcance del sistema, permitiendo un análisis del comportamiento del río que puede ser utilizada para detectar posibles desbordamientos con una antelación aún mayor. Además, se estableció una estructura de alertas progresivas mediante la pantalla LCD, proporcionando información diferenciada sobre la situación del río y factores que pueden afectarlo, como lo son las precipitaciones. 

Entre las oportunidades de mejora identificadas, se encuentra la necesidad de desarrollar una protección adecuada para los componentes electrónicos expuestos a condiciones climáticas adversas, garantizando su durabilidad en aplicaciones reales. También se plantea la integración de conectividad inalámbrica para permitir el monitoreo remoto y la notificación automática a las autoridades locales en un área más amplia. 

En conclusión, el sistema desarrollado cumplió con los requisitos mínimos del reto al ofrecer un monitoreo en tiempo real de las crecidas y una notificación local efectiva en caso de riesgo. Además, las mejoras implementadas y las oportunidades de optimización identificadas posicionan esta solución como un sistema prometedor para minimizar los daños causados por inundaciones en Colombia. 

---

## 3. Conclusiones

El proceso de prototipado de la presente solución IoT ha sido un proceso lleno de altibajos y cuestionamientos desde la parte técnica a causa del por entonces desconocido funcionamiento de los componentes disponibles. Sin embargo, una vez entendido el comportamiento de dichos elementos electrónicos y cómo estos pueden ser integrados para ofrecer un producto funcional y robusto, la más contundente conclusión respecto al proceso ha sido que soluciones IoT de este calibre pueden resultar económicamente asequibles sin afectar el rendimiento o calidad del prototipo. 

Afortunadamente para el equipo de trabajo, hoy en día el estado del arte de IoT permite consultas rápidas y provechosas sobre múltiples dificultades durante el proceso de ideación y construcción de la solución. La comunidad mundial alrededor del internet de las cosas se ha desarrollado exponencialmente y, a su vez, facilita su mismo desarrollo al haber, cada vez más, números fuentes de información y fotos de debate donde se solidifique el conocimiento respecto a esta rama de la ingeniería y permita solventar dificultades de todo tipo relacionadas con IoT. 

La solución propuesta cumple con los requerimientos técnicos planteados por el docente y constituye una sólida propuesta de solución para la problemática del reto, permitiendo la medición del nivel de altura del agua, así como la detección de lluvias, con el fin de fortalecer los protocolos de prevención de desastres dentro de la comunidad. 

---

## 4. Referencias

[1] Androbot TECH. Utiliza este Sensor para medir el nivel de agua! (28 de junio de 2022). Accedido el 17 de febrero de 2025. [Video en línea]. Disponible: https://www.youtube.com/watch?v=VobYZbzTXzc 

[2] “RAIN SENSOR Tutorial | RAIN SENSOR with Arduino Uno - SriTu Hobby”. SriTu Hobby. Accedido el 17 de febrero de 2025. [En línea]. Disponible: https://srituhobby.com/rain-sensor-tutorial-rain-sensor-with-arduino-uno/ 

[3] “ISO 9001 Sistemas de Gestión de la Calidad | Normas ISO”. Normas ISO | Normas ISO Asesoría y Formación en Sistemas de Gestión. Accedido el 17 de febrero de 2025. [En línea]. Disponible: https://www.normas-iso.com/iso-9001/ 

[4] “UNE-EN ISO 4373:2022 Hidrometría. Dispositivos de medida del n...” UNE - Asociación Española de Normalización. Accedido el 17 de febrero de 2025. [En línea]. Disponible: https://www.une.org/encuentra-tu-norma/busca-tu-norma/norma/?c=N0070707 

---

## 5. Anexos
### Código fuente documentado

```cpp
// Librerias instaladas 
#include <Wire.h> //Libreria para permitir la comunicacion i2c 
#include <LiquidCrystal_I2C.h>//Libreria para poder usar la pantalla LCD 


const int echo = 4; //Pin echo del HC-SR04 
const int trigger = 5; //Pin trigger del HC-SR04 
const int buzzer = 12; //Pin al que estara conectado el buzzer 
const int led = 6; //Pin en el que se conecto el led 
const int sensor_lluvia = A3; //Pin en el que se conecto el sensor de lluvia  

 
long tiempo; //tiempo del echo del HC-SR04 
float distancia; //Distancia que esta midiendo el sensor  
int isRaining = 100; //Estado de la lluvia 
int value = 1000; //Valor del sensor de lluvia  
const float water_level = 10.0;//Nivel critico del agua  

 
// Configuración del LCD (Dirección 0x27) 
LiquidCrystal_I2C lcd(0x27, 16, 2); 


void setup() { 
  //Configuracion los pines 
  pinMode(trigger, OUTPUT); 
  pinMode(echo, INPUT); 
  pinMode(buzzer, OUTPUT); 
  pinMode(led, OUTPUT); 

  //Inicia la comunicacion serial 
  Serial.begin(9600); 
  //Inicia el LCD 
  lcd.init(); 
  lcd.backlight(); 
  delay(100); 
} 

 
void loop() { 
  medir(); //Funcion para obtener la distancia del agua 
  checkRain(); //Funcion para verificar si esta lloviendo 
  updateDisplay(); //Funcion para actualizar la pantalla lcd 

  // Si el nivel del agua es menor o igual a la distancia, se activa la alarma(el buzzer) 
  if (distancia <= water_level) { 
    tone(buzzer, 523, 500); 
    delay(500); 
  }
} 

 
// Funcion que mide la distancia entre el agua y el limite con el sensor ultrasonico  
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


//Funcion para detectar la lluvia
void checkRain() { 
  value = analogRead(sensor_lluvia); 
  if (value < 300) { 
    isRaining = true; 
    digitalWrite(led, HIGH); //Si el valor es menor a 300 hay lluvia fuerte, entonces toca encender el led 
  } else { 
    digitalWrite(led, LOW); //de lo contrario apagar el led 
  } 

//mantener el led prendido un tiempo mas  
  if (isRaining > 0) { 
    digitalWrite(led, HIGH); 
  }
  isRaining -= 1; 
  delay(100); 
} 

 
//Funcion para poner la informacion en el display  
void updateDisplay() { 
  lcd.clear(); 
  lcd.setCursor(0, 0); 
  lcd.print("Al limite: "); //Mostrar la distancia que esta midiendo el sensor  
  lcd.print(distancia); 
  lcd.setCursor(0, 1); 
  if (distancia > water_level && value > 300) { 
    lcd.print("Seguro"); 
  } else if (distancia > water_level && value < 300) { 
    lcd.print("Riesgo"); 
  } else { 
    lcd.print("Corran"); 
  } 
  delay(250); 
}
```
