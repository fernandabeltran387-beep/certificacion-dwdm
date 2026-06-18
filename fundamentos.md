Fundamentos Técnicos — Parámetros Críticos del Enlace DWDM


1. Potencia Óptica (dBm)

La potencia óptica expresa la intensidad de la señal luminosa en un punto del enlace. Se mide en dBm (decibelios referenciados a 1 miliWatt), donde:

PdBm=10⋅log⁡10(PmW1 mW)P_{dBm} = 10 \cdot \log_{10}\left(\frac{P_{mW}}{1\ mW}\right)PdBm​=10⋅log10​(1 mWPmW​​)

En un sistema DWDM, cada transponder (OTU) posee dos parámetros críticos:


Potencia de transmisión (Tx): nivel de señal que emite el láser del transponder hacia el Mux. En la plataforma HT6000, los OTU transmiten en la banda C (~1530–1565 nm), con valores típicos entre −3 dBm y +3 dBm.

Sensibilidad de recepción (Rx): nivel mínimo de señal que el fotodetector puede procesar sin errores. Para los OTU del HT6000, el umbral configurado en laboratorio es de −15,00 dBm (campo RxLow Power threshold).


Un nivel de potencia Rx por debajo del umbral activa la alarma Optical PowerAlarm que se registra en el log del NMS. Si la potencia Rx cae demasiado (por atenuación excesiva o conector sucio), el enlace entra en estado LOS (Loss of Signal).


En este laboratorio, los valores de Rx esperados post-fibra de 50 km se sitúan entre −10 dBm y −14 dBm, dentro del margen operativo.




2. OSNR — Relación Señal/Ruido Óptico

El OSNR (Optical Signal-to-Noise Ratio), mide la calidad de la señal luminosa respecto al ruido de fondo generado principalmente por la Emisión Espontánea Amplificada (ASE) de los amplificadores EDFA.

OSNR (dB)= 10⋅log⁡10 (Psen alPruido, 0.1nm) OSNR\ (dB) = 10 \cdot \log_{10}\left (\frac{P_{señal}}{P_{ruido,\ 0.1nm}}\right) OSNR (dB)=10⋅log10​(Pruido,0.1nm​Psen al​​)


El OSNR se mide en una resolución de ancho de banda de 0,1 nm (convención ITU-T). Para el OSA RXT-4510, este parámetro se obtiene automáticamente en cada barrido espectral.

¿Por qué un OSNR alto garantiza la integridad de los datos?

Un OSNR bajo incrementa la Tasa de Errores de Bit (BER), ya que el ruido óptico interfiere con la detección del estado lógico "1" o "0" en cada símbolo. En sistemas de 10G como el de este laboratorio, se requiere un OSNR mínimo de:

ModulaciónOSNR mínimo requeridoOOK (NRZ) 10G≥ 15 dBDWDM 8 canales típico≥ 20 dB (con margen)

En el OSA, los valores de OSNR medidos para cada canal aparecen en la columna OSNR del display. Canales con OSNR inferior al umbral configurado generan una alarma Gain deviationAlarm en el EDFA o directamente en el NMS del HT6000.


3. Atenuación del Enlace — Cálculo de Pérdidas en 50 km

La atenuación total del enlace óptico es la suma de todas las pérdidas que experimenta la señal entre el Tx del Nodo A y el Rx del Nodo B. El presupuesto de pérdidas (loss budget) se calcula como:

Atotal=Afibra+Aconectores+Aempalmes+Amux/demuxA_{total} = A_{fibra} + A_{conectores} + A_{empalmes} + A_{mux/demux}Atotal​=Afibra​+Aconectores​+Aempalmes​+Amux/demux​
Fibra óptica SMF-28 (50 km)

La fibra monomodo estándar (ITU-T G.652) presenta una atenuación típica de 0,2 dB/km en la ventana de 1550 nm (banda C):

Afibra=0,2 dBkm×50 km=10 dBA_{fibra} = 0{,}2\ \frac{dB}{km} \times 50\ km = 10\ dBAfibra​=0,2 kmdB​×50 km=10 dB
En la maqueta se utilizan dos carretes de 25 km cada uno, conectados en serie a través del ODF. Cada carrete introduce su propia pérdida, más la pérdida adicional del conector de unión entre ambos.

Estimación del presupuesto de pérdidas total

ComponentePérdida estimadaFibra óptica (2 × 25 km @ 0,2 dB/km)~10,0 dBConectores LC/APC en ODF (4 conexiones × 0,3 dB)~1,2 dBPérdida inserción Mux/Demux ODM08~3,0–4,0 dBMargen de sistema recomendado3,0 dBPresupuesto total estimado~17–18 dB


El atenuador óptico variable JW3303 instalado en la maqueta permite ajustar la atenuación del enlace de forma controlada para simular diferentes condiciones de planta y verificar los umbrales de alarma del sistema.




4. Center Frequency Drift — Desviación de Frecuencia Respecto a la Grilla ITU-T

La norma ITU-T G.694.1 define la grilla de frecuencias DWDM en la banda C con un espaciado de 100 GHz (≈ 0,8 nm) como referencia. El canal C21, por ejemplo, corresponde a 192,1 THz (1560,61 nm).

El Center Frequency Drift (deriva de frecuencia central) es la desviación que presenta el láser de un transponder respecto a la frecuencia nominal asignada en la grilla. Se expresa en GHz o en fracciones de la separación de canal.

Δf=fmedida−fnominal_ITU−T\Delta f = f_{medida} - f_{nominal\_ITU-T}Δf=fmedida​−fnominal_ITU−T​
El OSA RXT-4510 mide este parámetro en la columna Delta(nm/GHz) del display espectral. En condiciones normales de operación, los valores típicos para los OTU del HT6000 son:

CondiciónDelta (nm)Delta (GHz)EstadoCanal estable±0,02 nm±2,5 GHz PASSDeriva moderada±0,05 nm±6,0 GHz AdvertenciaFuera de grilla> ±0,1 nm> ±12 GHz FAIL

Una deriva excesiva puede provocar interferencia entre canales adyacentes (crosstalk), degradando el OSNR y aumentando la BER. En el OSA, los valores de Delta medidos en este laboratorio se encuentran en la columna Deltas(nm) del reporte de canales, y en el NMS del HT6000 esta desviación puede consultarse en la configuración de cada transponder.


Conversión práctica: para la banda C, 1 GHz ≈ 0,008 nm a 1550 nm.

Herramienta de conversión THz ↔ nm: unitconverters.net




Documento de fundamentación técnica — Evaluación 2, Certificación DWDM.
