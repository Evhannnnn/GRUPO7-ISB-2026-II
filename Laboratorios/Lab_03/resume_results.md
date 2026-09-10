# Laboratorio 03: Adquisición de señales EMG
## Teoría

### 1. Qué es la electromiografía

La electromiografía es el procedimiento que mide la actividad muscular a través de la manifestación eléctrica de los potenciales de acción. El término combina tres raíces: electro, relativo a la electricidad; myo, músculo; y graphy, resultado descriptivo.

### 2. Origen de la señal

La contracción muscular se origina en una cadena de eventos que ocurre en pocos milisegundos. Las motoneuronas superiores del cerebro emiten la orden motora, que se propaga por la médula espinal hasta las motoneuronas inferiores. Estas disparan un potencial de acción que alcanza las fibras musculares, donde genera desequilibrios de voltaje que provocan la contracción. Una sola motoneurona superior puede controlar desde unas pocas fibras hasta miles, según el tamaño del músculo.

El músculo esquelético es el de interés en esta práctica, por encontrarse inmediatamente bajo la superficie de la piel y por estar bajo control voluntario.

### 3. Adquisición de superficie

Existen dos métodos para registrar EMG: desde la superficie de la piel, o mediante agujas concéntricas que miden a nivel de fibra. Este laboratorio emplea el primero, denominado EMG de superficie.

Las señales registradas desde la piel se sitúan en el rango de milivoltios a microvoltios, según el tamaño del músculo y por tanto la cantidad de fibras involucradas. Esta amplitud reducida las hace especialmente sensibles al ruido y al artefacto de movimiento, lo que exige acondicionamiento antes de su análisis.

### 4. Montaje bipolar

El registro se realiza con configuración bipolar: dos electrodos de medición, con conducción positiva y negativa, se ubican sobre el músculo de interés, y un tercero se coloca en una zona neutra como referencia.

Los electrodos de medición deben situarse sobre el vientre del músculo, alineados con la dirección de las fibras y separados unos 2 cm. El electrodo de referencia debe ubicarse sobre una prominencia ósea, como el codo, la frente, la cresta ilíaca o la rodilla.

La lógica del montaje es la siguiente: el ruido que alcanza únicamente a uno de los electrodos de medición se cancela mediante el segundo, mientras que la referencia establece la línea base del sistema.

Los colores del cable de tres hilos indican la función de cada electrodo: rojo para el positivo, negro para el negativo y blanco para la referencia.

### 5. Acondicionamiento y conversión

La señal atraviesa varias etapas antes de convertirse en un dato utilizable:


Electrodo → Amplificación → Filtrado → Conversión analógica a digital


El electrodo actúa como interfaz entre la piel y el sensor, maximizando la conductividad eléctrica y minimizando los artefactos mecánicos. Dada la baja amplitud de la señal, se aplica una amplificación del orden de mil veces para llevarla a una escala menos sensible al ruido. El filtrado limita la señal a la banda de interés, que en EMG de superficie se sitúa típicamente entre 20 y 450 Hz. Finalmente, el conversor analógico a digital de 10 bits produce códigos digitales entre 0 y 1023.

Considerando la tensión de operación de 3.3 V, el paso de cuantización resulta de 3.3 V dividido entre 1024, aproximadamente 3.223 mV medidos en la entrada del conversor.

### 6. Especificaciones del sensor

| Parámetro | Valor |
|---|---|
| Ganancia | 1009 |
| Rango | ±1.64 mV con VCC = 3.3 V |
| Ancho de banda | 25 a 482 Hz |
| CMRR | 86 dB |
| Impedancia de entrada | 10 GOhm / 7.5 pF |
| Consumo | aproximadamente 0.17 mA |
| Medición | diferencial bipolar |

Conviene notar que el ancho de banda del sensor, de 25 a 482 Hz, no coincide exactamente con la banda fisiológica de interés de 20 a 450 Hz. El primero corresponde a la respuesta real del hardware y el segundo al rango que se busca conservar.

### 7. Equipo y procedimiento

La adquisición se realizó con el kit *BITalino (r)evolution*, empleando su sensor de electromiografía conectado a un canal analógico de la placa. El sensor entrega una salida analógica preacondicionada, es decir, ya amplificada y filtrada, que la placa digitaliza con su conversor de 10 bits y transmite por Bluetooth a la computadora.

La visualización y el registro se realizaron en el software *OpenSignals (r)evolution*, desde donde se exportaron los datos para su posterior graficado en el dominio temporal y frecuencial.

Se registraron señales EMG de dos músculos, en tres condiciones cada uno:

| Músculo | Ubicación de la referencia | Condiciones registradas |
|---|---|---|
| Bíceps braquial | Codo | Reposo, contracción leve, contracción fuerte |
| Gastrocnemio | Rodilla | Reposo, contracción leve, contracción fuerte |

En ambos casos los electrodos de medición se ubicaron sobre el vientre del músculo, alineados con la dirección de las fibras, y el electrodo de referencia sobre la prominencia ósea correspondiente, siguiendo el criterio de zona de baja actividad muscular.

### 8. Procesamiento de los registros

Los archivos exportados desde OpenSignals se procesaron en Python con el siguiente tratamiento:

| Etapa | Parámetro |
|---|---|
| Frecuencia de muestreo | 1000 Hz |
| Canal utilizado | columna 5 del archivo de datos |
| Eliminación de componente continua | resta del valor medio de la señal |
| Filtrado | notch de 60 Hz, factor de calidad Q = 30, aplicado con filtfilt |
| Análisis frecuencial | FFT con normalización por el número de muestras |

La eliminación de la componente continua es necesaria porque el valor medio de la señal genera un pico en 0 Hz cuya magnitud comprime la escala del espectro y dificulta observar la banda de interés.

El filtrado se aplicó con filtfilt, que recorre la señal en ambos sentidos y cancela la distorsión de fase. Esto resulta relevante en EMG, donde interesa preservar la posición temporal de las ráfagas de activación.

## Resultados
**VIDEO DE LA SEÑAL Y PLOTEO DE LAS MISMAS**

**A) MUSCULO BÍCEPS**

Durante el desarrollo del laboratorio se procedió a la adquisición de señales en condiciones de reposo y contracción del músculo bíceps braquial. La referencia de conexión a tierra se estableció en la región del codo, garantizando la estabilidad eléctrica del sistema de medición.

Se documentó el proceso mediante videos de adquisición, que muestran la ejecución de las maniobras musculares. Asimismo, se presentan las señales registradas y graficadas.

> **1) Músculo en reposo**

| Video | Señal Ploteada | Espectro |
|-------|----------|----------|
| ![Bicep Reposo](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/bicep_reposo.jpg?raw=true)<br>[Ver video](https://drive.google.com/file/d/1Fn9X9EgTd7UXgnSBJ60JQZXrCA5AoUNA/view?t=3.966) |![Bicep RPloteo](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/breposo.png?raw=true)|![Bicep RFrecuencia](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/breposo_f.png?raw=true)  |

La señal se muestra uniforme y completamente plana, sin variaciones significativas en su trazado. Esto refleja el ruido de fondo del sistema y la inactividad del músculo, confirmando que no hay un reclutamiento activo de fibras musculares durante el estado de relajación.

> **2) Músculo con contracción leve**

| Video | Señal Ploteada | Espectro |
|-------|----------|----------|
| ![Bicep Contraccion Leve](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/bicep_l_contraccion.jpg?raw=true)<br>[Ver video](https://drive.google.com/file/d/14DHb8bwDTcGz8PeXKCmvlgOmSMmzHyko/view?t=0.315) | ![Bicep Contraccion Leve Plot](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/bleve.png?raw=true) | ![Bicep Contraccion Leve Sign](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/bleve_f.png?raw=true) |

Al levantar el brazo sin peso adicional, la señal presenta elevaciones moderadas y esporádicas sobre la línea base. Este comportamiento describe un reclutamiento inicial y controlado de unidades motoras pequeñas, destinadas únicamente a vencer la fuerza de gravedad y mover el peso del propio miembro.

> **3) Músculo con contracción fuerte**

| Video | Señal Ploteada | Espectro |
|-------|----------|----------|
| ![Bicep Contraccion Fuerte](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/bicep_f_contraccion.jpg?raw=true)<br>[Ver video](https://drive.google.com/file/d/1SCEY_dGpYrWTbFaCioJcosE57UzouMJM/view) |  ![Bicep Contraccion Fuerte Plot](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/bcontraccion_fuerte.png?raw=true) | ![Bicep Contraccion Fuerte Sign](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/bcontraccion_fuerte_f.png?raw=true) |

Al aplicar la fuerza opuesta, el trazado experimenta una expansión drástica con deflexiones muy densas y de gran envergadura. Esta saturación de la señal evidencia la activación masiva de fibras musculares de mayor umbral para lograr vencer la resistencia externa ejercida por la otra persona.

**B) MUSCULO GASTROCNEMIO**

También se procedió a la adquisición de señales en condiciones de reposo y contracción del músculo gastrocnemio. La referencia de conexión a tierra se estableció en la región de la rodilla, garantizando la estabilidad eléctrica del sistema de medición. A continuación, se presentan las señales registradas en video y graficadas.

> **1) Músculo en reposo**

| Video | Señal Ploteada | Espectro |
|-------|----------|----------|
| ![Gast Reposo](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/gastr_reposo.jpg?raw=true)<br>[Ver video](https://drive.google.com/file/d/1bDvEtDRJDZCbmxEY6V7gr5d4GQ7noBHe/view?t=0.244) |![Gast Reposo Plot](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/greposo.png?raw=true)| ![Gast Reposo Sign](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/greposo_f.png?raw=true) |

El registro se mantiene constante en una franja muy delgada, libre de fluctuaciones o picos de actividad. La estabilidad del trazado demuestra que el músculo se encuentra en reposo absoluto, sin generar tensión ni recibir impulsos eléctricos relevantes.

> **2) Músculo con contracción leve**

| Video | Señal Ploteada | Espectro |
|-------|----------|----------|
| ![Gast Contraccion Leve](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/gastr_leve.jpg?raw=true)<br>[Ver video](https://drive.google.com/file/d/19h4c9DfBZVGzr0PW6xwsZ0_yiwLpGA3Y/view?t=18.737) |![Gast Contraccion Leve Plot](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/g_contraccion_leve.png?raw=true)| ![Gast Contraccion Leve Sign](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/gcontraccion_leve_f.png?raw=true) |

Durante el levantamiento de la punta del pie, se observan ligeras ondulaciones en la señal que sobresalen apenas del nivel de reposo. Dado que el gastrocnemio no es el motor principal de este movimiento, la respuesta refleja una activación mínima, cumpliendo un rol netamente co-contractil o estabilizador de la articulación.

> **3) Músculo con contracción fuerte**

| Video | Señal Ploteada | Espectro |
|-------|----------|----------|
| ![Gast Contraccion Fuerte](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/gastr_fuerte.jpg?raw=true)<br>[Ver video](https://drive.google.com/file/d/1HKxYFPiL_liBiuSLwm8nFzHgD_mM1QsX/view?t=2.368) |![Gast Contraccion Fuerte Plot](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/gcontraccion_fuerte.png?raw=true)  | ![Gast Contraccion Fuerte Sign](https://github.com/Evhannnnn/GRUPO7-ISB-2026-II/blob/main/Resources/gcontraccion_fuerte_f.png?raw=true) |

Al hacer fuerza directa contra la resistencia del compañero, el gráfico muestra un empaquetamiento denso de la señal con aumentos notables en su grosor y dispersión vertical. Esto describe un aumento directo en la suma de potenciales de acción, indicando que el músculo debe reclutar simultáneamente una gran cantidad de unidades motoras para sostener la fuerza requerida.

