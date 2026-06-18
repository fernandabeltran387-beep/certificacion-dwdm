# Instrumental — Ecosistema de la Maqueta DWDM

## 1. Chasis RxT-1200 + Módulo OSA RXT-4510

### Descripción funcional:

El VeEX RxT-1200 es un chasis de instrumentación modular portátil. En esta maqueta porta el módulo OSA RXT-4510, un Analizador Óptico de Espectro (OSA) diseñado específicamente para el análisis de sistemas DWDM en banda C y L.

El OSA RXT-4510 realiza un barrido espectral del puerto óptico de monitoreo (MON) del chasis HT6000, detectando y clasificando cada canal DWDM individualmente. Para cada canal reporta:

- Longitud de onda del pico (nm)
- Frecuencia central medida (THz)
- Desviación respecto a grilla ITU-T — Delta (nm / GHz)
- Potencia óptica (dBm)
- OSNR (dB) en resolución 0,1 nm
- BW 3dB (nm)

### Procedimiento de uso en el laboratorio

1. Encender el chasis RxT-1200 con el módulo OSA RXT-4510 instalado.
2. Conectar la fibra desde el puerto MON DWDM 1 del ODF al puerto de entrada del OSA (conector LC/APC azul).
3. Acceder a la interfaz del OSA desde el propio chasis o vía VeEX EZ Remote (IP 192.168.1.202).
4. Seleccionar el modo OSA en el menú principal → pantalla de barrido espectral.
5. Configurar el plan de canal: banda C, espaciado 100 GHz, canales C21–C28.
6. Ejecutar *Repeat Sweep* para barrido continuo o *Single Sweep* para captura puntual.
7. Verificar que los 8 canales aparecen en el gráfico espectral como picos individuales.
8. Revisar la tabla de resultados: columnas ITU#, Peak(nm), Center(nm), Delta(nm/GHz), Power(dBm), OSNR, BW 3dB.
9. Exportar/capturar pantalla para incluir como evidencia en `pruebas.md`.

**Fotografía del equipo en operación**

<img width="668" height="309" alt="dwdm7" src="https://github.com/user-attachments/assets/ef8c980f-34e2-4abb-9cfb-5d58f0a054fa" />

## 2. Analizador Ethernet MTX150x

### Descripción funcional

El VeEX MTX150x es un analizador Ethernet portátil de doble puerto (P1 y P2), con capacidad de prueba a 1G y 10G (SFP+). En esta maqueta se emplea para validar el SLA del enlace transportado sobre el canal DWDM C21, verificando que el transporte óptico no degrada el rendimiento de la capa Ethernet.

El MTX150x puede ejecutar:

- **RFC 2544:** pruebas de throughput, latencia, frame loss rate y back-to-back en modo loopback.
- **ITU-T Y.1564 (SAM):** pruebas de configuración y rendimiento de servicios Ethernet.
- **Loopback:** modo de reflexión para pruebas punto a punto.
- **iPerf integrado:** medición de ancho de banda TCP/UDP.

En la maqueta, el flujo de tráfico sigue el siguiente recorrido:

```
MTX150x P1 (Tx) → ODF MTX1 → DWDM1 OTU-CLI1 → Fibra 50 km
                                                      ↓
MTX150x P2 (Rx) ← ODF MTX2 ← DWDM2 OTU-CLI1 ←────────┘
```

### Procedimiento de uso en el laboratorio

1. Conectar el MTX150x a la red de gestión mediante cable Ethernet (IP 192.168.1.201).
2. Acceder vía web desde el PC: `http://192.168.1.100` (interfaz VeEX Web Remote Access).
3. En el panel *Remote Control*, se visualiza la interfaz completa del equipo.
4. Navegar a Setup → IP y verificar que la IP del equipo sea 192.168.1.201, máscara /24, gateway 192.168.1.1.
5. Para prueba de throughput: ir a Rendimiento (T-PUT) o RFC 2544.
6. Seleccionar Puerto P1 como Tx y Puerto P2 como Rx (flujo P1 → P2).
7. Configurar tamaño de trama (64, 128, 256, 512, 1024, 1518 bytes) y duración (10 segundos mínimo por RFC 2544).
8. Iniciar prueba y registrar los resultados: Throughput (Mbps), Latencia (ms), Frame Loss (%).
9. Alternativamente, verificar conectividad vía Ping hacia 192.168.1.10 (PC) como validación rápida.

**Fotografía del equipo en operación**

<img width="1519" height="968" alt="dwdm4" src="https://github.com/user-attachments/assets/db14fbcc-aa8f-43f3-b254-cfc9fadf3ce2" />

## 3. Gestión NMS — Plataforma HT6000 (HT6000-NMS)

### Descripción funcional

El sistema de gestión NMS del HT6000 es una interfaz web embebida en la tarjeta HT6000-NMS (Card PN: HT6000-NMS, Hardware v1.8, Software v5.91), instalada en el slot M de cada chasis. Permite el monitoreo y configuración completa de todos los módulos del sistema DWDM: transponders OTU, módulos EDFA, OLP y amplificadores.

Funciones principales del NMS:

- **Panel (Panel view):** visualización gráfica del estado de LEDs y slots del chasis en tiempo real.
- **Device → Slot:** acceso a la información detallada de cada tarjeta (versión HW/SW, S/N, fecha de fabricación).
- **Device → OTU:** configuración de longitud de onda de cada transponder, habilitación del láser (TxEnable), umbral de recepción (RxLow Power threshold).
- **Alarm → System Alarm Log:** historial completo de alarmas con timestamp, slot afectado y descripción del evento.
- **Settings:** configuración de parámetros de red del NMS (IP, gateway, DNS).

Las dos unidades HT6000 de la maqueta son accesibles en:

| Equipo | URL de acceso | Usuario/Contraseña |
|---|---|---|
| DWDM 1 | http://192.168.1.101/main.html | admin/admin |
| DWDM 2 | http://192.168.1.102/main.html | admin/admin |

### Procedimiento de uso en el laboratorio

1. Desde el PC (192.168.1.10), abrir navegador e ingresar a `http://192.168.1.101`.
2. Autenticarse con usuario `admin` / contraseña `admin`.
3. Verificar en *Panel* que los LEDs PWR y RUN están en verde.
4. Navegar a Device → Slot M para confirmar la tarjeta NMS activa (Hardware v1.8, Software v5.91).
5. Ir a Device → OTU para revisar el estado de cada transponder: Link Status, LOS state, Potencia Rx/Tx, Wavelength.
6. En Alarm → System Alarm Log, revisar las alarmas activas. Las alarmas en rojo son activas; las en azul están normalizadas.
7. Repetir el proceso en `http://192.168.1.102` para el DWDM 2.

**Fotografías del NMS en operación**

<img width="767" height="498" alt="dwdm1" src="https://github.com/user-attachments/assets/fcbcbb59-9ab8-44e8-b8e8-f73c2e42fa9e" />

<img width="1920" height="1040" alt="dwdm3" src="https://github.com/user-attachments/assets/57642312-5800-4680-96d4-bc2bffc3a664" />

<img width="1065" height="618" alt="dwdm2" src="https://github.com/user-attachments/assets/9b51cc63-b2a7-4702-9056-ec42ffd582b8" />

## 4. VeEX EZ Remote — Control Remoto desde PC

### Descripción funcional

VeEX EZ Remote es la plataforma web de VeEX Inc. para el acceso y control remoto de los instrumentos MTX150x y OSA RxT-1200/4510 a través de la red LAN del laboratorio. No requiere instalación de software adicional: opera completamente desde el navegador web mediante la interfaz Web Remote Access embebida en cada equipo.

Desde EZ Remote es posible:

- Visualizar en tiempo real la pantalla del equipo remoto.
- Ejecutar pruebas (RFC 2544, SAM, barrido OSA) desde el PC.
- Guardar capturas de pantalla (*Screen Shots*) como evidencia.
- Acceder al histórico de resultados almacenados en el equipo (*Results*).
- Subir perfiles de prueba preconfigurados (*Upload Profile*).

### Configuración del acceso remoto

| Parámetro | MTX150x | OSA RxT-1200 |
|---|---|---|
| Dirección IP | 192.168.1.100 (acceso web) | 192.168.1.202 |
| URL de acceso | http://192.168.1.100 | http://192.168.1.202 |
| Contraseña de acceso | pass1 | pass1 |
| Red de gestión | Switch MikroTik CSS610 | Switch MikroTik CSS610 |

> **Nota:** la IP 192.168.1.100 corresponde al gateway de acceso web del MTX150x (interfaz Web Remote Access). La IP del equipo como instrumento es 192.168.1.201.

### Procedimiento de conexión

1. Asegurarse de que el PC está en la red 192.168.1.0/24.
2. Abrir navegador → ingresar `http://192.168.1.100`.
3. En la pantalla de inicio de VeEX Web Remote Access, seleccionar *Remote Control*.
4. Se carga la interfaz espejo del MTX150x con todos los botones de control virtual.
5. Para el OSA: ingresar `http://192.168.1.202` y acceder con contraseña `pass1`.
6. Desde ambas interfaces es posible ejecutar pruebas, capturar pantallas y descargar resultados.

**Fotografía del software en uso**

<img width="1519" height="968" alt="dwdm4" src="https://github.com/user-attachments/assets/53b0497a-769a-4ebf-8c7c-faa6b1b46a31" />
