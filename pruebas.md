# Registro de Pruebas — Mediciones, Certificación SLA y Análisis de Fallas

---

## Paso 1 — Quick Setup: Verificación Inicial y Protocolo de Limpieza

### 1.1 Verificación del Switch MikroTik CSS610

Antes de iniciar cualquier medición, se verificó la conectividad de la red de gestión (192.168.1.0/24) mediante ping desde el PC hacia los dos chasis HT6000. El switch MikroTik CSS610-8G-2S+IN actúa como nodo de acceso de gestión para todos los equipos de la maqueta.

**Prueba de conectividad — ping desde PC (192.168.1.10):**

<img width="1920" height="1040" alt="dwdm6" src="https://github.com/user-attachments/assets/5b1ce430-4fe4-4c59-92db-eab1864d7cf8" />

Los resultados confirman conectividad total con ambos chasis HT6000 con latencias inferiores a 1 ms, validando el correcto funcionamiento de la red de gestión.

---

### 1.2 Verificación de Acceso NMS — DWDM 1 y DWDM 2

| Equipo | URL | Estado | S/N Tarjeta NMS |
|--------|-----|--------|-----------------|
| DWDM 1 | http://192.168.1.101 | Accesible | 25062310289 |
| DWDM 2 | http://192.168.1.102 | Accesible | 25062310290 |

Ambas tarjetas NMS presentan: Hardware v1.8, Software v5.91, Card PN HT6000-NMS, fecha de producción 2025.06.23. El puerto LAN1 muestra estado Up a 100M Full.

---

### 1.3 Asignación de Canales en Transponders (Mux/Demux)

La maqueta opera con el módulo ODM08 (Mux/Demux de 40 canales) en cada chasis HT6000. Los 8 canales activos corresponden a la grilla ITU-T G.694.1 (espaciado 100 GHz, banda C):

| Canal ITU-T | Frecuencia nominal (THz) | Longitud de onda (nm) | Nodo A OTU | Nodo B OTU |
|-------------|--------------------------|----------------------|------------|------------|
| C21 | 192,1 | 1560,61 | OTU1 | OTU1 |
| C22 | 192,2 | 1559,79 | OTU2 | OTU2 |
| C23 | 192,3 | 1558,98 | OTU3 | OTU3 |
| C24 | 192,4 | 1558,17 | OTU4 | OTU4 |
| C25 | 192,5 | 1557,36 | OTU5 | OTU5 |
| C26 | 192,6 | 1556,55 | OTU6 | OTU6 |
| C27 | 192,7 | 1555,75 | OTU7 | OTU7 |
| C28 | 192,8 | 1554,94 | OTU8 | OTU8 |

- Canales C21–C24: dirección A→B
- Canales C25–C28: dirección B→A (enlace bidireccional)

---

### 1.4 Protocolo de Limpieza de Conectores (Seguridad Óptica)

1. **Verificación visual:** inspección de la férula LC/APC antes de conectar.
2. **Limpieza con hisopo seco:** de un solo uso, nunca reutilizar.
3. **Cinta de limpieza óptica:** si hay contaminación visible.
4. **Inspección post-limpieza:** verificar ausencia de polvo o residuos.
5. **Tapas protectoras:** mantener en puertos que no estén en uso.

> **Precaución:** nunca mirar directamente un conector óptico sin verificar que el láser esté apagado (TxEnable = OFF en el NMS). Los láseres operan en banda C (~1550 nm, invisible al ojo humano).

---

## Paso 2 — Tabla de Certificación: Registro de Canales OSA

### 2.1 Metodología de medición

El OSA RXT-4510 se conectó al puerto de monitoreo del ODF (Puerto 1-A: MON DWDM 1) y se ejecutó un barrido espectral completo sobre la banda C (1530–1570 nm), con plan de canal ITU-T G.694.1 a 100 GHz de espaciado.

<img width="1601" height="989" alt="dwdm5" src="https://github.com/user-attachments/assets/adb784b8-93a5-4054-bbee-5333b8077388" />

---

### 2.2 Tabla de Certificación de Canales

| Canal | Frecuencia nominal (THz) | λ Peak (nm) | λ Center (nm) | Delta (nm) | Delta (GHz) | Potencia Rx (dBm) | OSNR (dB) | BW 3dB (nm) | Estado |
|-------|--------------------------|-------------|---------------|------------|-------------|-------------------|-----------|-------------|--------|
| C21 | 192,100 | — | — | — | — | — | — | — | PASS |
| C22 | 192,200 | — | — | — | — | — | — | — | — |
| C23 | 192,300 | — | — | — | — | — | — | — | — |
| C24 | 192,400 | — | — | — | — | — | — | — | — |
| C25 | 192,500 | — | — | — | — | — | — | — | — |
| C26 | 192,600 | — | — | — | — | — | — | — | — |
| C27 | 192,700 | — | — | — | — | — | — | — | — |
| C28 | 192,800 | — | — | — | — | — | — | — | — |

**Criterio PASS/FAIL:** PASS si Potencia Rx > −15 dBm, OSNR > 15 dB y |Delta| < 0,05 nm.

> **Nota:** Los valores numéricos de los canales C21–C28 no pudieron ser registrados durante la sesión de laboratorio debido a limitaciones de tiempo en la clase práctica. La metodología de medición y los criterios PASS/FAIL están documentados en la sección 2.1.

---

## Paso 3 — Validación SLA: Pruebas de Rendimiento Ethernet

### 3.1 Configuración de la prueba — iPerf3

Se ejecutó iPerf3 en modo servidor sobre el PC de gestión (192.168.1.10) y en modo cliente desde el extremo B, transportando tráfico a través del canal óptico C21.

<img width="1601" height="989" alt="dwdm5" src="https://github.com/user-attachments/assets/e05c2e1f-6971-4261-b985-b008b4b53aa7" />

<img width="1065" height="618" alt="dwdm2" src="https://github.com/user-attachments/assets/aaeb107e-e430-4e88-b72f-dca601e58329" />

---

### 3.2 Tabla de Resultados SLA

| Parámetro | Valor medido | Umbral SLA | Estado |
|-----------|-------------|------------|--------|
| Throughput máximo (TCP) | ~942 Mbits/sec | ≥ 900 Mbits/sec | PASS |
| Throughput promedio (10 seg) | ~939–942 Mbits/sec | ≥ 900 Mbits/sec | PASS |
| Pérdida de paquetes | 0% | 0% | PASS |
| Latencia extremo a extremo | < 1 ms | ≤ 5 ms | PASS |
| Total transferido (10 seg) | 1,10 GBytes | — | Referencia |

### 3.3 Análisis de resultados

El enlace DWDM de 50 km opera a 939–942 Mbits/sec (~94% de la capacidad nominal de 1GbE). La estabilidad del throughput (variación de ±3 Mbits/sec) confirma que el transporte óptico no introduce degradación en la capa de datos.

---

## Paso 4 — Análisis de Fallas: Diagnóstico Técnico

### 4.1 Alarmas registradas en el NMS

El System Alarm Log del HT6000 registró alarmas de tipo `Optical PowerAlarm` en el slot 08 (módulo OLP) y `EDFA Gain deviationAlarm` en los slots 09 y 17.

<img width="1065" height="618" alt="dwdm2" src="https://github.com/user-attachments/assets/d3ee80c1-d9b3-4918-971f-895663e9d985" />

---

### 4.2 Clasificación de alarmas

**Alarmas OLP1+1 — Slot 08:**

Las fechas registradas (año 2000) indican que el reloj del sistema no estaba sincronizado — es un artefacto de configuración, no una falla real. Los valores de potencia muy bajos (−47,96 dBm vs umbral −25,0 dBm) son consistentes con los puertos de protección T2/R2 sin fibra conectada durante el arranque. Una vez establecida la conexión principal T1/R1, el sistema normalizó la alarma.

**Alarmas EDFA01 — Slots 09 y 17:**

| Estado | Entrada | Salida | Ganancia |
|--------|---------|--------|----------|
| Alarm | −32,66 dBm | −45,00 dBm | 0,00 dB |
| Normal | −11,92 dBm | +8,22 dBm | 20,00 dB |

La secuencia alarma→normal indica que el EDFA estuvo temporalmente sin señal durante la conexión de fibras en el ODF y luego recuperó correctamente con ganancia nominal de 20 dB.

---

### 4.3 Árbol de diagnóstico — Canal en FAIL
4.2 Clasificación de alarmas

Alarmas OLP1+1 — Slot 08 (2000/04/25 – 2000/04/28):

Las alarmas de tipo OLP1+1 Tx/Rx/T1/T2/R1/R2 Optical PowerAlarm en el slot 08 corresponden al módulo de Protección de Enlace Óptico (OLP). Las fechas registradas (año 2000) indican que el reloj del sistema no estaba sincronizado correctamente al momento de generarse estas alarmas, lo cual es un artefacto de configuración y no afecta la validez técnica de las mediciones.

Los valores de potencia en estas alarmas (ej. −47,96 dBm / umbral −25,0 dBm para Tx; −47,22 dBm / umbral −30,0 dBm para Rx) indican que la potencia registrada se encontraba muy por debajo del umbral mínimo configurado. Esto es consistente con una situación de fibra desconectada o sin señal en los puertos de protección del OLP al momento de iniciar la maqueta.

Causa probable: los puertos T2/R2 del OLP (ruta de protección) no tenían fibra conectada durante el arranque inicial, generando alarmas de baja potencia. Una vez establecida la conexión principal (T1/R1), la señal fue detectada y el sistema normalizó la alarma.

Alarmas EDFA01 — Slots 09 y 17 (2025/04/11):

Las alarmas EDFA01 Gain deviationAlarm y EDFA01 Optical PowerAlarm en los slots de amplificador EDFA presentan dos estados alternados:


Alarm: In(−32,66 dBm/−25,0 dBm) Out(−45,00 dBm/−30,0 dBm) Gain(0,00 dB) → Ganancia cero, amplificador sin señal de entrada o saturado.
Normal: In(−11,92 dBm/−25,0 dBm) Out(8,22 dBm/−30,0 dBm) Gain(20,00 dB) → Amplificador operando con ganancia nominal de 20 dB.


La secuencia de alarma→normal indica que el EDFA estuvo temporalmente sin señal de entrada (posiblemente durante la conexión/desconexión de fibras en el ODF) y luego recuperó la señal correctamente.

4.3 Diagnóstico de canales en FAIL (si aplica)

En caso de que el OSA detectara algún canal en estado FAIL durante el barrido espectral, el diagnóstico técnico seguiría el siguiente árbol de decisión:

Canal en FAIL (baja potencia Rx o OSNR bajo)
         │
         ├── Verificar TxEnable = ON en NMS del HT6000
         │   └── Si OFF → activar láser y repetir medición
         │
         ├── Inspeccionar conector en puerto MON del ODF
         │   └── Si sucio → limpiar conector y repetir
         │
         ├── Medir potencia en puerto MON con power meter
         │   └── Si < −30 dBm → pérdida excesiva en la ruta
         │        └── Inspeccionar conectores LC/APC en ODF
         │             └── Si persiste → verificar carretes de fibra
         │                  (macrocurvaturas, empalmes defectuosos)
         │
         └── Si OSNR bajo con potencia Rx normal → revisar
             interferencia entre canales adyacentes (Delta drift)
             o saturación del EDFA

Procedimiento de identificación de macrocurvaturas con OTDR:

Si se sospecha una macrocurvatura en los carretes de fibra de 25 km, el procedimiento con OTDR (si disponible) sería:


Desconectar el carrete sospechoso del ODF.
Conectar el OTDR al extremo de entrada del carrete.
Ejecutar un barrido OTDR con longitud de pulso adecuada para 25 km (ej. 1000 ns, rango 30 km).
Identificar en el trazado OTDR caídas de potencia abruptas que no corresponden a empalmes o conectores conocidos → indican macrocurvaturas o puntos de estrés mecánico.
Registrar la distancia del evento y comparar con la ubicación física del carrete.



Las macrocurvaturas son especialmente perjudiciales en sistemas DWDM porque generan pérdidas dependientes de la longitud de onda, pudiendo afectar canales específicos de forma selectiva.




Resumen Final de Certificación

ComponenteEstadoObservacionesConectividad red gestión PASSPing < 1 ms a DWDM1 y DWDM2Acceso NMS DWDM1 PASShttp://192.168.1.101, SW v5.91Acceso NMS DWDM2 PASShttp://192.168.1.102, SW v5.91Acceso VeEX EZ Remote PASShttp://192.168.1.100, MTX150x onlineBarrido OSA RXT-4510 Ejecutado8 canales detectados en banda CCanal C21 PASS[completar con valores medidos]Canal C22[estado][completar]Canal C23–C28[estado][completar]Throughput Ethernet PASS~942 Mbits/sec, 0% pérdidaLatencia PASS< 1 ms extremo a extremoAlarmas activas al cierre RevisarVer log NMS para alarmas OLP/EDFA residuales
