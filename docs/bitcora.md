# Bitácora de Desarrollo del Proyecto Integrador

**Asignatura:** Comunicaciones II (27145)  
**Proyecto:** Telemetría Industrial bajo Perturbaciones Impulsivas (Escenario S1)  
**Institución:** Universidad Industrial de Santander — UIS  
**Integrantes:**
- Wilmer Camilo Patiño (2192561)
- Cristian Felipe Albarracin (2194244)
- Miguel Angel Ayala (2231055)
- Jordy Alejandro Pabon (2210397)

---

## Registro de Sesiones y Evolución del Proyecto

### Sesión 1: Definición del Escenario y Parámetros Nominales
* **Objetivo:** Definir el problema técnico, justificar el esquema de modulación y concertar las constantes del sistema.
* **Actividades Realizadas:**
  - Análisis del escenario **S1 (Telemetría Industrial)**: Monitoreo de variables de baja velocidad (temperatura, vibración) en ambientes con interferencia electromagnética generada por conmutación PWM en variadores de frecuencia (VFD) y motores inductivos.
  - Selección de la modulación **BPSK** ($M=2$, $k=1\text{ bit/símbolo}$) por su envolvente constante y alta robustez frente a variaciones de amplitud.
  - Fijación del conjunto de parámetros nominales:
    - Tasa de símbolos: $R_s = 32\text{ ksyms/s}$
    - Tasa de bits: $R_b = R_s \cdot \log_2(M) = 32\text{ kb/s}$
    - Muestras por símbolo: $SPS = 8$
    - Frecuencia de muestreo: $f_s = R_s \cdot SPS = 256\text{ ksamples/s}$
    - Ancho de banda nulo a nulo (conformación rectangular): $BW = 2 R_s = 64\text{ kHz}$
* **Dificultades:** Inicialmente se consideró utilizar una modulación de mayor orden ($16\text{-QAM}$), pero se descartó al evaluar su alta sensibilidad a las variaciones de amplitud provocadas por los impulsos.

---

### Sesión 2: Montaje del Flujograma Base y Simulación de perturbaciones en GNU Radio
* **Objetivo:** Implementar la arquitectura del sistema en GNU Radio Companion (GRC) e inyectar el canal compuesto (AWGN + Impulsivo).
* **Actividades Realizadas:**
  - Estructuración del flujograma en GRC:
    - **Transmisor:** `Random Source` $\rightarrow$ `Pack K Bits` $\rightarrow$ `Chunks to Symbols` $\rightarrow$ `Interpolating FIR Filter` ( conformación rectangular con sobremuestreo $SPS=8$).
    - **Canal:** `Throttle` ($256\text{ ksamples/s}$), `Noise Source` (modo Gaussiano complejo con $\sigma_w = 0.1$), `Vector Source` (pulsos periódicos cada $N_p = 256$ muestras) y bloques `Add (Complex)`.
    - **Receptor / Monitorización:** `QT GUI Constellation Sink`, `QT GUI Frequency Sink` (PSD) y `QT GUI Time Sink`.
  - Definición de la matriz de pruebas comparativas:
    - **Prueba A (Referencia):** $A_{int} = 0.0$
    - **Prueba B (Suave):** $A_{int} = 0.3$
    - **Prueba C (Media):** $A_{int} = 0.6$
    - **Prueba D (Fuerte):** $A_{int} = 1.0$
* **Decisiones Tomadas:** Mantener el ruido AWGN base constante ($\sigma_w = 0.1$) para aislar la variable independiente (amplitud del impulso $A_{int}$).

---

### Sesión 3: Revisión de Resultados y Retroalimentación Docente
* **Objetivo:** Evaluar las evidencias capturadas (PSD y constelaciones) y someter el informe preliminar a revisión técnica.
* **Observaciones del Docente (Retroalimentación):**
  1. *Efecto del `Vector Source`:* Explicar que el impulso periódico genera un peine discreto de frecuencias cada $\Delta f = f_s / N_p = 1\text{ kHz}$ en la FFT de GNU Radio, en lugar de modelar un piso de ruido homogéneo plano.
  2. *Falta de Rigor Analítico:* El informe original limitaba el análisis a descripciones cualitativas de las capturas de pantalla, omitiendo el cálculo de la energía por bit $E_b$ y la degradación de $(E_b/N_0)_{\text{ef}}$.
  3. *Inconsistencias de Notación:* Basura tipográfica en la fórmula de ancho de banda y desalineación en el balance de potencia de ruido para señales complejas en banda base ($N_0 = P_w / f_s$).
  4. *Entrega Incompleta:* Omisión de la imagen contextual del escenario físico requerida por la guía (Sección 4.1).

---

### Sesión 4: Corrección Rigurosa, Balance Matemático y Cierre
* **Objetivo:** Corregir las debilidades del informe IEEE, depurar las ecuaciones en LaTeX y consolidar el repositorio y blog ABET.
* **Ajustes Realizados:**
  - **Cálculo exacto de $E_b$:** $E_b = P_s / R_b = 1 / (32 \times 10^3) = 31.25\text{ }\mu\text{J/bit}$.
  - **Densidad de ruido complejo en banda base:** $N_0 = P_w / f_s = 0.01 / 256000 = 3.906 \times 10^{-8}\text{ W/Hz}$, obteniendo un $(E_b/N_0)_{\text{ref}} \approx 29.03\text{ dB}$.
  - **Evaluación cuantitativa del impacto:**
    - Prueba B ($A_{int} = 0.3$): $(E_b/N_0)_{\text{ef}} = 28.51\text{ dB}$ ($\Delta = -0.52\text{ dB}$).
    - Prueba C ($A_{int} = 0.6$): $(E_b/N_0)_{\text{ef}} = 26.68\text{ dB}$ ($\Delta = -2.35\text{ dB}$).
    - Prueba D ($A_{int} = 1.0$): $(E_b/N_0)_{\text{ef}} = 24.12\text{ dB}$ ($\Delta = -4.91\text{ dB}$).
  - **Clarificación de la distancia euclidiana:** Se estableció $d_{\min} = 2A_c = 2$ en el espacio de constelación normalizado, demostrando que cuando $A_{int} = 1.0$, la nube del símbolo $-1$ se desplaza hasta el origen ($0+0j$), causando una tasa de error instantánea del $50\%$ en la muestra afectada.
  - **Diseño e inclusión del escenario físico contextual:** Creación e integración de la Figura 1 en el informe con etiquetas técnicas ($T_x$, $R_x$, VFD, líneas de transmisión e interferencia impulsiva).

---

## Tabla de Trazabilidad de Decisiones

| Problema / Desafío | Causa Raíz | Solución Aplicada | Resultado Obtención |
| :--- | :--- | :--- | :--- |
| **Elevación aparente del piso de ruido en PSD** | Periodicidad del bloque `Vector Source` ($N_p=256$) procesado por la FFT. | Explicar el fenómeno como un peine espectral discreto ($\Delta f = 1\text{ kHz}$) promediado por la ventana. | Análisis espectral riguroso validado por el docente. |
| **Desalineación en cálculo de $N_0$** | Confusión entre ancho de banda real pasa-banda ($f_s/2$) y banda base compleja ($f_s$). | Aplicar $N_0 = P_w / f_s$ para señales en envolvente compleja en GNU Radio. | Referencia nominal exacta de $(E_b/N_0)_{\text{ref}} = 29.03\text{ dB}$. |
| **Contradicción en $d_{\min}$** | Mezcla entre energía física en Joules ($E_b$) y distancia euclidiana en el plano de constelación. | Definir $d_{\min} = 2A_c = 2$ en el espacio discreto normalizado. | Demostración limpia del colapso del umbral de decisión en el origen ($0+0j$). |
| **Falta de figura contextual** | Interpretación errónea de la guía (asumir que el flujograma de GRC era suficiente). | Elaboración e incorporación de la figura del escenario físico industrial S1. | Cumplimiento total de la Sección 4.1 y rúbrica ABET. |

---

## Reflexión Metacognitiva del Equipo

1. **¿Qué aprendió el equipo?**  
   Comprendimos que la simulación por software mediante SDR (GNU Radio) no debe ser tratada como una "caja negra". Cada parámetro visible en la interfaz gráfica posee una correspondencia analítica estricta con la teoría de señales y procesos estocásticos. El análisis de potencia media por sí solo no describe adecuadamente los fenómenos impulsivos; es necesario evaluar el comportamiento en el dominio del tiempo y la distorsión instantánea de la constelación.

2. **¿Qué cambiaríamos en una versión futura?**  
   - Sustituir el modelo del canal periódico por un proceso estocástico no estacionario de Bernoulli-Gauss para reflejar de forma más realista la naturaleza aleatoria de las ráfagas en entornos industriales.
   - Implementar un bloque limitador de amplitud (*clipper*) o un filtro adaptativo previo al detector para mitigar la degradación antes de la toma de decisión.

3. **Herramientas de IA y Validación Humana:**  
   Se utilizó asistencia de LLM para la revisión de código LaTeX y optimización de expresiones matemáticas. Toda formulación y resultado de balance de potencia fue verificado manualmente mediante cálculos de ingeniería aplicados a las constantes de diseño del sistema.