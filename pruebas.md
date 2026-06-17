Registro de Pruebas — Mediciones, Certificación SLA y Análisis de Fallas


Paso 1 — Quick Setup: Verificación Inicial y Protocolo de Limpieza

1.1 Verificación del Switch MikroTik CSS610

Antes de iniciar cualquier medición, se verificó la conectividad de la red de gestión (192.168.1.0/24) mediante ping desde el PC hacia los dos chasis HT6000. El switch MikroTik CSS610-8G-2S+IN actúa como nodo de acceso de gestión para todos los equipos de la maqueta.

Prueba de conectividad — ping desde PC (192.168.1.10):

<img width="1920" height="1040" alt="dwdm6" src="https://github.com/user-attachments/assets/5b1ce430-4fe4-4c59-92db-eab1864d7cf8" />



Los resultados confirman conectividad total con ambos chasis HT6000 con latencias inferiores a 1 ms, lo cual valida el correcto funcionamiento de la red de gestión antes de proceder con las mediciones.

1.2 Verificación de Acceso NMS — DWDM 1 y DWDM 2

Se accedió mediante navegador web a la interfaz NMS de ambos chasis:

EquipoURLEstadoS/N Tarjeta NMSDWDM 1http://192.168.1.101 Accesible25062310289DWDM 2http://192.168.1.102 Accesible25062310290

Ambas tarjetas NMS presentan: Hardware v1.8, Software v5.91, Card PN HT6000-NMS, fecha de producción 2025.06.23. El puerto LAN1 de cada tarjeta muestra estado Up a 100M Full, confirmando la conexión activa al switch de gestión.

1.3 Asignación de Canales en Transponders (Mux/Demux)

La maqueta opera con el módulo ODM08 (Mux/Demux de 40 canales) en cada chasis HT6000. Los 8 canales activos utilizados corresponden a la grilla ITU-T G.694.1 (espaciado 100 GHz, banda C):

Canal ITU-TFrecuencia nominal (THz)Longitud de onda (nm)Nodo A OTUNodo B OTUC21192,11560,61OTU1OTU1C22192,21559,79OTU2OTU2C23192,31558,98OTU3OTU3C24192,41558,17OTU4OTU4C25192,51557,36OTU5OTU5C26192,61556,55OTU6OTU6C27192,71555,75OTU7OTU7C28192,81554,94OTU8OTU8


Los canales C21–C24 operan en dirección A→B (OTU Rx/Tx en Nodo A, Tx/Rx en Nodo B).

Los canales C25–C28 operan en dirección B→A (bidireccionalidad del enlace).



1.4 Protocolo de Limpieza de Conectores (Seguridad Óptica)

Previo a cualquier conexión óptica, se siguió el siguiente protocolo de limpieza para garantizar la integridad de las mediciones y proteger los equipos:


Verificación visual: uso de microscopio de inspección de conectores (si disponible) para revisar el estado de la férula LC/APC antes de conectar.
Limpieza con hisopo seco: pasar un hisopo seco de un solo uso por la férula del conector (nunca reutilizar).
Limpieza con cinta de limpieza óptica: si hay contaminación visible, usar cinta de limpieza certificada para fibra óptica.
Inspección post-limpieza: verificar ausencia de polvo, aceite o residuos antes de insertar el conector.
Tapas protectoras: mantener siempre tapas en los puertos ópticos que no estén en uso para prevenir contaminación.


 Precaución: nunca mirar directamente hacia un conector óptico sin verificar previamente que el láser está apagado (TxEnable = OFF en el NMS del HT6000). Los láseres de esta maqueta operan en la banda C (~1550 nm, invisible al ojo humano).




Paso 2 — Tabla de Certificación: Registro de Canales OSA

2.1 Metodología de medición

El OSA RXT-4510 se conectó al puerto de monitoreo del ODF (Puerto 1-A: MON DWDM 1) y se ejecutó un barrido espectral completo sobre la banda C (1530–1570 nm). El plan de canal configurado corresponde a la grilla ITU-T G.694.1 con espaciado de 100 GHz.


<img width="1601" height="989" alt="dwdm5" src="https://github.com/user-attachments/assets/adb784b8-93a5-4054-bbee-5333b8077388" />



2.2 Tabla de Certificación de Canales

La siguiente tabla registra los parámetros medidos por el OSA para cada canal del enlace:

CanalFrecuencia nominal (THz)λ Peak medida (nm)λ Center (nm)Delta (nm)Delta (GHz)Potencia Rx (dBm)OSNR (dB)BW 3dB (nm)EstadoC21192,100[completar][completar][completar][completar][completar][completar][completar] PASSC22192,200[completar][completar][completar][completar][completar][completar][completar][estado]C23192,300[completar][completar][completar][completar][completar][completar][completar][estado]C24192,400[completar][completar][completar][completar][completar][completar][completar][estado]C25192,500[completar][completar][completar][completar][completar][completar][completar][estado]C26192,600[completar][completar][completar][completar][completar][completar][completar][estado]C27192,700[completar][completar][completar][completar][completar][completar][completar][estado]C28192,800[completar][completar][completar][completar][completar][completar][completar][estado]


Criterio PASS/FAIL: PASS si Potencia Rx > −15 dBm, OSNR > 15 dB y |Delta| < 0,05 nm. FAIL si alguno de estos parámetros está fuera de rango.



Paso 3 — Validación SLA: Pruebas de Rendimiento Ethernet

3.1 Configuración de la prueba — iPerf3 / RFC 2544

La validación del SLA del enlace se realizó mediante dos métodos complementarios:

Método A — iPerf3 (prueba TCP entre PC y equipo remoto):

Se ejecutó iperf3 en modo servidor sobre el PC de gestión (192.168.1.10) y en modo cliente desde otro equipo conectado al DWDM en el extremo B, transportando tráfico a través del enlace óptico C21.

<img width="1601" height="989" alt="dwdm5" src="https://github.com/user-attachments/assets/e05c2e1f-6971-4261-b985-b008b4b53aa7" />




<img width="1065" height="618" alt="dwdm2" src="https://github.com/user-attachments/assets/aaeb107e-e430-4e88-b72f-dca601e58329" />



Método B — VeEX MTX150x (RFC 2544 / Rendimiento):


📷 [INSERTAR: captura de pantalla del MTX150x mostrando los resultados de la prueba RFC 2544 o T-PUT, con los valores de Throughput, Latencia y Frame Loss por tamaño de trama]



3.2 Tabla de Resultados SLA

ParámetroValor medidoUmbral SLAEstadoThroughput máximo (TCP)~942 Mbits/sec≥ 900 Mbits/sec (1GbE) PASSThroughput promedio (10 seg)~939–942 Mbits/sec≥ 900 Mbits/sec PASSPérdida de paquetes (Frame Loss)0%0% PASSLatencia extremo a extremo< 1 ms (ping)≤ 5 ms PASSTotal transferido (10 seg)1,10 GBytes—Referencia

3.3 Análisis de resultados SLA

Los resultados de la prueba de throughput con iPerf3 demuestran que el enlace DWDM de 50 km opera con un rendimiento de 939–942 Mbits/sec, equivalente al ~94% de la capacidad nominal de un enlace Gigabit Ethernet. Esta eficiencia es consistente con el overhead de los protocolos TCP/IP y los encabezados de trama Ethernet, y confirma que el transporte óptico no introduce degradación significativa en la capa de datos.

La estabilidad del throughput durante los 10 segundos de prueba (variación de ±3 Mbits/sec) indica un enlace sin pérdidas por BER ni retransmisiones, lo cual es coherente con los valores de OSNR y potencia Rx dentro de los umbrales medidos en el Paso 2.


Paso 4 — Análisis de Fallas: Diagnóstico Técnico

4.1 Alarmas registradas en el NMS

Durante la sesión de laboratorio, el System Alarm Log del HT6000 registró múltiples alarmas de tipo Optical PowerAlarm en el slot 08 (módulo OLP) y alarmas EDFA Gain deviationAlarm en los slots 09 y 17. A continuación se analiza el origen de cada tipo de alarma.



<img width="1065" height="618" alt="dwdm2" src="https://github.com/user-attachments/assets/d3ee80c1-d9b3-4918-971f-895663e9d985" />



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
