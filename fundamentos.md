# Fundamentos Técnicos

## 1. Potencia Óptica (dBm)

La potencia óptica mide la cantidad de energía luminosa transmitida en una fibra óptica.
Se expresa en dBm (decibelios respecto a 1 miliwatt).

- **Tx (Transmisión):** Potencia con la que el transpondedor envía la señal.
- **Rx (Recepción):** Sensibilidad mínima que necesita el receptor para decodificar la señal.
- Un enlace es viable cuando la potencia Rx está dentro del rango aceptable del equipo.

---

## 2. OSNR (Relación Señal/Ruido Óptico)

El OSNR mide la calidad de la señal óptica comparando la potencia de la señal útil
versus el ruido acumulado en el enlace.

- Un **OSNR alto** (ej. >25 dB) garantiza que la señal llega limpia y los datos son íntegros.
- Un **OSNR bajo** indica degradación de la señal, causando errores en la transmisión.

---

## 3. Atenuación del Enlace

La atenuación es la pérdida de potencia óptica a lo largo de la fibra.

- Fibra monomodo estándar: ~0.2 dB/km
- Enlace total: 50 km (2 carretes de 25 km)
- **Pérdida estimada:** 50 km × 0.2 dB/km = **10 dB**

---

## 4. Center Frequency Drift

Desviación de la frecuencia central de un canal respecto a la grilla oficial ITU-T G.694.1.

- La grilla ITU-T define frecuencias exactas para cada canal DWDM (ej. 193.1 THz para C31).
- Un drift elevado puede causar interferencia entre canales adyacentes.
- Valor aceptable: generalmente **< ±1.5 GHz**.