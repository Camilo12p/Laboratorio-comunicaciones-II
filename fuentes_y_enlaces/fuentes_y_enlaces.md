# Fuentes y Enlaces - Proyecto Integrador: Telemetría Industrial bajo Perturbaciones Impulsivas (Escenario S1)

Este documento recopila las fuentes bibliográficas, artículos científicos, notas de aplicación técnica y documentación de software utilizadas para el sustento teórico, la modelación del canal industrial y la simulación en GNU Radio Companion (GRC) del proyecto de Comunicaciones II.

---

## 📚 Referencias Bibliográficas Principales (Estilo IEEE)

1. **Modelado de ruido impulsivo en redes industriales:**
   * **Autores:** M. Cheffena
   * **Título:** *Industrial wireless sensor networks: Channel modeling and performance evaluation*
   * **Publicación:** *EURASIP Journal on Wireless Communications and Networking*, vol. 2012, no. 1, p. 189, 2012
   * **Enlace:** [ResearchGate - Cheffena (2012)](https://link-springer-com.bibliotecavirtual.uis.edu.co/article/10.1186/1687-1499-2012-297)
   * **Relevancia en el proyecto:** Sustenta el análisis del canal industrial, demostrando que el ruido en plantas de proceso no puede modelarse únicamente como AWGN, sino mediante procesos estocásticos (como Bernoulli-Gauss o cadenas de Markov) que generan ruido impulsivo en ráfagas (*bursty impulsive noise*) debido a motores y contactores.

2. **Interferencia electromagnética (EMI) de variadores de frecuencia (VFD):**
   * **Autores / Organización:** LongVista Industrial Automation Insights
   * **Título:** *VFD Interference on Sensors: Causes, Cable Shielding & Solutions*
   * **Publicación:** *Industrial Automation Insights*, 2024.
   * **Enlace:** [LongVista Automation Insights](https://www.longvista.ch/industrial-automation-insights/dealing-with-vfd-disturbances-identifying-issues-and-finding-solutions/)[cite: 1]
   * **Relevancia en el proyecto:** Detalla el mecanismo físico de los transistores IGBT en variadores PWM, los cuales generan pendientes de voltaje elevadas ($\text{d}v/\text{d}t$) y picos transitorios (*spikes*) que se acoplan a las líneas de instrumentación de baja velocidad.

3. **Fundamentos de Comunicaciones Digitales:**
   * **Autores:** J. G. Proakis y M. Salehi
   * **Título:** *Digital Communications*
   * **Editorial:** 5th ed., New York, NY, USA: McGraw-Hill, 2008
   * **Relevancia en el proyecto:** Base matemática para las expresiones de modulación digital BPSK, densidad espectral de potencia y análisis de probabilidades de error.

4. **Sistemas de Comunicación:**
   * **Autor:** S. Haykin
   * **Título:** *Communication Systems*
   * **Editorial:** 5th ed., Hoboken, NJ, USA: Wiley, 2009.
   * **Relevancia en el proyecto:** Referencia teórica para la conformación de pulso, el filtrado en banda base y el comportamiento de las señales en presencia de ruido térmico.

---

## 🛠️ Software y Herramientas

* **GNU Radio & GNU Radio Companion (GRC):**
  * **Descripción:** Plataforma de desarrollo de radio definida por software (SDR) utilizada para implementar los flujogramas de simulación del transmisor, canal industrial y receptor con Qt GUI.
  * **Sitio Web Oficial:** [https://www.gnuradio.org/](https://www.gnuradio.org/)

* **Entorno de Desarrollo y Control de Versiones:**
  * **Sistema Operativo:** Linux Mint
  * **Control de Versiones:** Git y repositorio remoto en GitHub para la gestión de entregables y documentación.
