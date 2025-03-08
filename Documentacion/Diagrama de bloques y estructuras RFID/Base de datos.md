# Estructura de Datos RFID - MediTrack (MIFARE Classic EV1 1K)

## **Sector 0 (Bloques 0-3)**
| Bloque | Contenido                     | Ejemplo (Hex/Texto)                |
|--------|-------------------------------|------------------------------------|
| **0**  | NUID + Datos Fabricante (RO)  | `01 23 45 67 00 00 A0 08 04 00 62 63 64 65 66 67` |

---

## **Sector 1 (Bloques 4-7) - Autenticación Médico**
| Bloque | Contenido                | Formato                           |
|--------|--------------------------|-----------------------------------|
| **4**  | ID Médico + Permisos + CRC | `A1 B2 C3 D4 00 FF 1A 2B`        |
| **5**  | Nombre Médico (16 chars)  | `"Dr. Ana López␣␣␣␣"`           |
| **6**  | Especialidad (16 chars)   | `"Cardiología␣␣␣␣␣"`           |
| **7**  | **Key A** \| Access Bits \| **Key B** | `FF FF FF FF FF FF 78 77 88 00 FF FF FF FF FF FF` |

---

## **Sector 2 (Bloques 8-11) - Datos Paciente**
| Bloque | Contenido                   | Ejemplo                          |
|--------|-----------------------------|----------------------------------|
| **8**  | ID Paciente + Timestamp     | `E5 F6 78 9A 5F D4 B7 00`       |
| **9**  | Diagnóstico (32 chars)      | `"Taquicardia␣ventricular..."`  |
| **10** | Medicación (32 chars)       | `"Amiodarona␣200mg␣c/12h..."`   |
| **11** | **Key A** \| Access Bits \| **Key B** | `A0 B1 C2 D3 E4 F5 00 F0 0F 00 A0 B1 C2 D3 E4 F5` |

---

## **Sector 3 (Bloques 12-15) - Signos Vitales**
| Bloque | Contenido                 | Formato                     |
|--------|---------------------------|----------------------------|
| **12** | SPO2 + Pulso              | `00 98 00 78` (98% - 120BPM) |
| **13** | ECG Raw Data (16 bytes)   | `7F 80 82 7E ... A3 45`    |
| **14** | Fecha + CRC               | `20 23 10 15 2A 4B C8 DD`  |
| **15** | **Key A** \| Access Bits \| **Key B** | `12 34 56 78 9A BC DE F0 01 02 12 34 56 78 9A BC` |

---

## **Leyenda**
- ␣: Espacio en blanco de relleno  
- **RO**: Bloque de solo lectura  
- **Key A/B**: Claves de autenticación (6 bytes cada una)  
- **Access Bits**: 4 bytes que definen permisos de lectura/escritura  
- **CRC**: Checksum para verificación de integridad (2-4 bytes)  
- **Timestamp**: Formato Unix (4 bytes)  
