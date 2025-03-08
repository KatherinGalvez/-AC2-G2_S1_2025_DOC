### *Solución para el proyecto MediTrack*

El proyecto asignado requiró un proceso minusioso para la elaboración de un prototipo apto para la detección de estados en la salud de los pacientes así como el control de los doctores. El problema planteado fue crear un sistema capaz de organizar la interacción entre pacientes y doctores para el diagnostico adecuado dependiento de los relustados de la salud del paciente. La solución está diseñada para monitorear en tiempo real las camillas y cubículos de pacientes en el Hospital Roosevelt, integrando sensores de monitoreo (SPO2, MDMAX30102, AD8032) con una arquitectura de comunicación eficiente para el uso del personal medico.

---

## *Tecnologías Utilizadas*
1. *Python (Comunicación Serial y API REST)*
2. *Arduino (Sensores y adquisición de datos)*
3. *WebSockets (Comunicación en tiempo real)*
4. *React (Interfaz gráfica del usuario)*
5. *Tailwind CSS + Flowbite (Diseño de la interfaz)*
6. *API REST (Envío de formularios médicos)*
7. *MAX30105.H (Sensor MDMAX30102)*
8. *spo2_algorithm.H (Sensor SPO2)*
9. *Arduino_FreeRTOS.H (Interrupciones)*

---

## *Arquitectura de la Solución*
El sistema se compone de módulos estructurados por partes para una mejor comprensión del flujo en la creación del sistema:

### *1. Captura de Datos (Arduino)*
   - El *Arduino* recolectará datos de sensores como:
     - *ECG (Electrocardiograma)*: este sensor se encarga de la lectura de la parte de ambos antebrazos y la parte superior de la rodilla
     - *MD-MAX30102 (Oxígeno en la sangre y pulso)*
     - *RFID (Autenticación del médico)*
   - Los datos son enviados al computador a través del *puerto serial (UART/USB)*: los datos son enviados con la funcion write para recolectarlos en pyton con WebSoket a travez del puerto de Arduino

### *2. API de Comunicación*
   - MQTT (HiveMQ Broker):
     - sensores/datos: Publica datos en tiempo real de ECG y oximetría.
     - diagnostico/realizado: Notifica la finalización de diagnósticos.
     - sensores/oxigeno y sensores/ecg: Envían métricas específicas en formato JSON.

      - Spring Boot actúa como intermediario: recibe datos seriales, los procesa y publica en MQTT.

   - API REST (Spring Boot):

      - Endpoints para:

         - Autenticación RFID (/auth/rfid).
         - Gestión de pacientes (/pacientes, /diagnosticos).
         - Consultas de estadísticas (/metricas).

     

### *3. Comunicación en Tiempo Real (WebSockets)*
   - WebSockets permitirá la comunicación en tiempo real entre:
     - *El servidor Python y el frontend en Angular*.
     - Esto asegurará que los médicos visualicen datos de los pacientes en tiempo real sin necesidad de refrescar la página.

### *4. Interfaz de Usuario (React + Tailwind + Flowbite)*
   - *React* será el framework principal para construir la interfaz gráfica.
   - *Tailwind CSS + Flowbite* se usarán para diseñar una UI moderna y accesible.
   - La interfaz permitirá:
     - *Monitoreo en tiempo real* de signos vitales.
     - *Ingreso de datos médicos* mediante formularios protegidos con RFID.
     - *Generación de reportes en PDF* con información médica relevante.

   - *Spring Boot:*

      - Valida permisos RFID y gestiona lógica de negocio.
      - Calcula estadísticas en tiempo real (ej: promedio de SpO2 durante un diagnóstico).

   - *Grafana:*

      - Paneles configurados:

         - Vista por camilla: Gráficas de línea para ECG/SpO2 y tablas de estadísticas.
         - Ocupación hospitalaria: Gauge con porcentaje de camillas ocupadas y tablas de estado.
         - Alertas críticas: Notifica valores anómalos mediante umbrales predefinidos.
### *5. Envío y Gestión de Formularios (API REST)*
   - Los datos ingresados en Angular se enviarán a la API mediante *peticiones HTTP (POST, PUT, GET, DELETE)*.
   - Los formularios de diagnóstico estarán protegidos y solo accesibles mediante autenticación *RFID*.

---