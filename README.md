# Certificación de Enlace Óptico DWDM — 8 Canales

## Introducción

Este repositorio documenta el desarrollo de la Evaluación N.° 2 de la asignatura Certificación de Cableado Estructurado, correspondiente a la certificación técnica de un enlace de transporte óptico DWDM de 8 canales.

El enlace se implementó sobre la plataforma HT6000-1U de HTFiberDWDM, simulando una distancia de fibra de 50 km mediante dos carretes de 25 km. La certificación se realizó mediante operación remota de instrumental de precisión: el Analizador Óptico de Espectro OSA RXT-4510 (montado sobre chasis RxT-1200), utilizado para el barrido espectral DWDM, y el Analizador Ethernet MTX150x de VeEX, utilizado para validar el acuerdo de nivel de servicio (SLA) mediante pruebas de rendimiento a 1G.

## Diagrama de Bloques de la Maqueta

```
                        ┌─────────────────────────────────────────────────────────┐
                        │                    ODF (Patch Panel Óptico)              │
                        │  [1A:MON DWDM1][1B:MON DWDM2][3:CLI1 D1][4:CLI1 D2]      │
                        │  [6:FIBRA1]   [7:FIBRA2]   [9:LIN D1]  [10:LIN D2]       │
                        └──────┬──────────────────────────────────────┬───────────┘
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
                          └─────────────────────────────────────────┘

  ┌──────────────────────────┐        ┌─────────────────────────────┐
  │  OSA RXT-4510            │        │  Analizador Ethernet MTX150x │
  │  (chasis RxT-1200)       │        │  192.168.1.201               │
  │  192.168.1.202           │        │  Puerto P1 → DWDM1 (CLI1)    │
  │  Conectado a MON DWDM1   │        │  Puerto P2 → DWDM2 (CLI1)    │
  └──────────────────────────┘        └─────────────────────────────┘

  ┌──────────────────────────┐
  │  Switch MikroTik CSS610  │  ← Red de gestión (192.168.1.0/24)
  │  DWDM1·DWDM2·OSA·MTX·PC  │
  └──────────────────────────┘
  PC de gestión: 192.168.1.10
```

## Tabla de Direcciones IP

| Equipo | Dirección IP | Máscara |
|---|---|---|
| DWDM 1 (HT6000) | 192.168.1.101 | /24 |
| DWDM 2 (HT6000) | 192.168.1.102 | /24 |
| Analizador Ethernet MTX150x | 192.168.1.201 | /24 |
| OSA RxT-1200/4510 | 192.168.1.202 | /24 |
| PC de gestión | 192.168.1.10 | /24 |

## Fotografías de la Maqueta

**Vista general del rack de laboratorio**

<img width="1200" height="1600" alt="maqueta1" src="https://github.com/user-attachments/assets/e640984d-c534-46eb-a45d-cf5845a09899" />

**Detalle del cableado óptico y equipos DWDM**

<img width="1600" height="1200" alt="maqueta2" src="https://github.com/user-attachments/assets/6cb2cc91-6deb-4951-8cfa-b2a3f7802fd1" />

**Instrumental de medición en operación**

<img width="1200" height="1600" alt="maqueta3" src="https://github.com/user-attachments/assets/1354b0ab-a927-4ff1-bc4a-af8a9f9fb149" />

<img width="668" height="309" alt="dwdm7" src="https://github.com/user-attachments/assets/30cc4fa8-85e4-42f5-89e1-2e6be3e69ca3" />
