# Sistema de Telemetría Industrial bajo Perturbaciones Impulsivas

**Curso:** Comunicaciones II (27145) – Proyecto Integrador 2026  
**Escuela:** Escuela de Ingenierías Eléctrica, Electrónica y de Telecomunicaciones (EIEET) – Universidad Industrial de Santander (UIS)  

---

## 🔗 Enlaces de Interés
* **Blog Público de Divulgación Técnica (Estilo ABET):** [Enlace al Blog del Proyecto](https://tu-blog-publico-aqui.com) *(Reemplazar con el enlace público)*
* **Video de Socialización:** [Enlace al Video de YouTube / Presentación](https://tu-enlace-de-video-aqui.com) *(Reemplazar cuando corresponda)*

---

## 👥 Integrantes
* **Wilmer Camilo Patiño Martínez** *(2192561)*
* **Miguel Angel Ayala Duarte** *(2231055)*
* **Jordy Alejandro Pabon Carrillo** *(2210397)*
* **Cristian Felipe Albarracin Rincon** *(2194244)*
---

## 📝 Resumen del Proyecto
El proyecto aborda el diseño y evaluación de un sistema de telemetría industrial encargado de transmitir variables de baja velocidad (como temperatura, vibración o estado de maquinaria) desde nodos sensores remotos hacia una estación central de supervisión. El entorno industrial real está expuesto a interferencias electromagnéticas generadas por motores, contactores y variadores de frecuencia, los cuales producen transitorios de gran amplitud conocidos como perturbaciones impulsivas. Para estudiar este fenómeno, se implementó un sistema de modulación digital coherente BPSK en GNU Radio Companion (GRC), evaluando su desempeño bajo cuatro condiciones de interferencia: referencia ($A_{int} = 0$), suave ($A_{int} = 0.3$), media ($A_{int} = 0.6$) y fuerte ($A_{int} = 1.0$). Mediante el análisis de la Densidad Espectral de Potencia (DEP) y los diagramas de constelación, se evidenció el ensanchamiento espectral y la dispersión de los símbolos en recepción, demostrando la necesidad de incorporar técnicas de filtrado y mitigación de ruido.

---

## ⚙️ Parámetros Técnicos del Sistema
* **Modulación:** BPSK ($M = 2, k = 1$)
* **Tasa de símbolos ($R_s$):** $32\text{ ksyms/s}$
* **Tasa de bits ($R_b$):** $32\text{ kb/s}$
* **Muestras por símbolo ($SPS$):** $8$
* **Frecuencia de muestreo ($f_s$):** $256\text{ ksamples/s}$
* **Perturbación:** Ruido AWGN de fondo ($\text{amplitud} = 0.1$) superpuesto con pulsos impulsivos periódicos (amplitud variable según la prueba).

---

## 🚀 Archivo GRC Principal y Ejecución
El archivo principal de flujo ejecutable se encuentra en la ruta `grc/archivos_modificados/enlace_telemetria_s1.grc`.

### Pasos para ejecutar:
1. Asegurarse de tener instalado **GNU Radio** (versión compatible con flujogramas de bloques Qt GUI).
2. Clonar el repositorio y abrir una terminal en Linux Mint. `git clone https://github.com/Camilo12p/Laboratorio-comunicaciones-II.git`.
3. Entrar a la carpeta grc/archivos modificados.
4. Ejecutar el entorno gráfico y abrir el archivo GRC:
   `Enlace_con_pulsos_controlados.grc`.
5. Presionar el botón **Play (Execute)** en la interfaz de GNU Radio para correr la simulación y visualizar en tiempo real las gráficas de tiempo, PSD y constelación.

---

## 📊 Tabla Breve de Pruebas

| Prueba | Nivel de Severidad | Amplitud del Impulso ($A_{int}$) | Descripción del Comportamiento |
| :---: | :---: | :---: | :--- |
| **A** | Referencia | $0$ | Comportamiento base ideal con ruido AWGN nominal ($0.1$). Constelación compacta y espectro estable. |
| **B** | Suave | $0.3$ | Aparición de picos transitorios breves en el tiempo e inicio de una ligera dispersión en los símbolos. |
| **C** | Media | $0.6$ | Condición principal de análisis; elevación apreciable del piso espectral y desplazamiento de las nubes de constelación. |
| **D** | Fuerte | $1.0$ | Estrés máximo del sistema; desplazamiento de la constelación y enmascaramiento parcial de los lóbulos secundarios en la PSD. |

---

## 🖼️ Imagen Principal del Escenario

*(A continuación se muestra el diagrama de bloques técnico implementado en GNU Radio para el escenario S1)*

```text
[ Fuente ] ---> [ Modulación BPSK ] ---> [ Canal (AWGN + Perturbación Impulsiva) ] ---> [ Receptor / Qt GUI (PSD, Constelación, Tiempo) ]
```

*Nota: La imagen detallada del flujograma se encuentra almacenada en el repositorio bajo la ruta `figures/diagramas_tecnicos/Diagrama de boque telemetria S1.png`.*

## 💡 Conclusión Principal del Proyecto

La simulación demostró que la presencia de ruido impulsivo de alta amplitud en entornos industriales altera la integridad de los enlaces de telemetría digital basados en BPSK. Conforme aumenta la severidad del pulso transitorio ($A_{int}$ de $0$ a $1.0$), se produce un ensanchamiento de la Densidad Espectral de Potencia y una dispersión vectorial de los símbolos hacia el origen de la constelación, lo que incrementa severamente la probabilidad de error en el receptor y justifica la implementación indispensable de esquemas de filtrado y codificación de canal.

**Fecha de última actualización:** 12 de noviembre de 2026
