Certificación de Enlace Óptico DWDM — 8 Canales
Asignatura: Certificación de Cableado Estructurado
Carrera: Ingeniería en Telecomunicaciones y Servicios Digitales
Sede: INACAP La Serena
Docente: Daniel Ruz Moreno
Plataforma evaluada: HT6000-1U · VeEX RxT-1200/OSA RXT-4510 · VeEX MTX150x
Integrantes
Nombres: Fernanda Beltrán - Nelly Antinao - Angélica Rojas.


Introducción.
El presente repositorio constituye la bitácora de ingeniería de la evaluación N.° 2 de la asignatura Certificación de Cableado Estructurado. La actividad consistió en la certificación técnica de un enlace de transporte óptico DWDM de 8 canales, implementado sobre la plataforma HT6000-1U de HTFiberDWDM, con una distancia de fibra simulada de 50 km (dos carretes de 25 km cada uno).

El proceso incluyó la operación analógica y remota de instrumental de precisión: el Analizador Óptico de Espectro OSA RXT-4510 (montado sobre chasis RxT-1200) para el barrido espectral DWDM, y el Analizador Ethernet MTX150x de VeEX para la validación del acuerdo de nivel de servicio (SLA) mediante pruebas de rendimiento a 1G.

Diagrama de Bloques de la Maqueta

                        ┌─────────────────────────────────────────────────────────┐
                        │                    ODF (Patch Panel Óptico)              │
                        │  [1A:MON DWDM1][1B:MON DWDM2][3:CLI1 D1][4:CLI1 D2]   │
                        │  [6:FIBRA1]   [7:FIBRA2]   [9:LIN D1]  [10:LIN D2]    │
                        └──────┬──────────────────────────────────────┬────────────┘
                               │                                      │
              ┌────────────────▼────────────────┐    ┌───────────────▼─────────────────┐
              │         NODO A — DWDM 1          │    │         NODO B — DWDM 2          │
              │      HT6000-1U · 192.168.1.101   │    │      HT6000-1U · 192.168.1.102   │
              │  ┌──────────────────────────┐    │    │    ┌──────────────────────────┐  │
              │  │  ODM08 (Mux/Demux 40ch)  │    │    │    │  ODM08 (Mux/Demux 40ch)  │  │
              │  │  C21·C22·C23·C24         │◄───┼────┼───►│  C21·C22·C23·C24         │  │
              │  │  C25·C26·C27·C28         │    │    │    │  C25·C26·C27·C28         │  │
              │  └──────────────────────────┘    │    │    └──────────────────────────┘  │
              │  OTU1(C21) OTU2(C22) ... OTU8    │    │    OTU1(C21) OTU2(C22) ... OTU8  │
              └────────────────┬────────────────┘    └───────────────┬─────────────────┘
                               │  LIN DWDM1 TX/RX                    │  LIN DWDM2 TX/RX
                               │                                      │
                               └──────────────┬───────────────────────┘
                                              │
                          ┌───────────────────▼───────────────────┐
                          │         FIBRA ÓPTICA SMF — 50 km       │
                          │   Carrete A (25 km) + Carrete B (25 km)│
                          │   Atenuación estimada: ~10 dB total    │
                          └───────────────────────────────────────┘

  ┌──────────────────────────┐        ┌─────────────────────────────┐
  │  OSA RXT-4510            │        │  Analizador Ethernet MTX150x │
  │  (chasis RxT-1200)       │        │  192.168.1.201               │
  │  192.168.1.202           │        │  Puerto P1 → DWDM1 (CLI1)    │
  │  Conectado a MON DWDM1   │        │  Puerto P2 → DWDM2 (CLI1)    │
  └──────────────────────────┘        └─────────────────────────────┘

  ┌──────────────────────────┐
  │  Switch MikroTik CSS610  │  ← Red de gestión (192.168.1.0/24)
  │  DWDM1·DWDM2·OSA·MTX·PC │
  └──────────────────────────┘
  PC de gestión: 192.168.1.10

Tabla de Direcciones IP

Equipo: DWDM 1 (HT6000) - DWDM 2 (HT6000) - Analizador Ethernet MTX150x - OSA RxT-1200/4510 - PC de gestión 
DirecciónIP/Máscara: DWDM 1 (HT6000)192.168.1.101/24 - DWDM 2 (HT6000)192.168.1.102/24 - Analizador Ethernet MTX150x 192.168.1.201/24 - OSA RxT-1200/4510 - PC de gestión 192.168.1.10/24

Fotografías de la Maqueta
Vista general del rack de laboratorio


📷 [INSERTAR: maqueta1.jpeg — Vista frontal del rack completo con todos los equipos montados]




Detalle del cableado óptico y equipos DWDM.


📷 [INSERTAR: maqueta2.jpeg — Detalle interior del rack: cableado amarillo, carretes de fibra, HT6000 x2 y ODF]




Instrumental de medición en operación.


📷 [INSERTAR: maqueta3.jpeg — Vista exterior del rack con OSA RxT-1200 y MTX150x sobre la mesa]

📷 [INSERTAR: dwdm7.jpeg — OSA RxT-1200 y MTX150x operando en simultáneo, con fibras conectadas]




