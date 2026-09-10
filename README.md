# Proyecto-de-Automatizaci-n-de-Procesos
Este repositorio contiene la documentación, arquitectura y código fuente de los flujos de automatización desarrollados como **avances a la fecha**, utilizando herramientas del ecosistema **Microsoft Power Platform**: **Power Automate Desktop (PAD)** y **Power Automate Cloud Flow**.
## Herramienta 1: Flujo de Escritorio (Power Automate Desktop)




## Herramienta 2: Flujo en la Nube (Power Automate Cloud)

* **Nombre del flujo:** `Enviar correos SS y PP`
* **Tipo:** Flujo desencadenado por eventos en la nube (Cloud Flow)
* **Conectores utilizados:** OneDrive for Business, Excel Online (Business), Office 365 Outlook.

### Función y Lógica del proceso:
1. **Desencadenador (Trigger):** Se activa automáticamente cada vez que se sube un nuevo archivo Excel con registros de alumnos a la carpeta `/PowerAutomate/Student` en OneDrive.
2. **Creación de Tabla Dinámica:** Convierte el rango de datos del archivo Excel recibido en una tabla estructurada para poder manipular sus filas.
3. **Lectura y Selección de Datos:** Extrae los campos clave (`EXPEDIENTE`, `NOMBRE`, `APELLIDOS`, `CREDITOS`, `Estatus SS`, `Estatus PP`) y genera dinámicamente el correo institucional (`a<EXPEDIENTE>@unison.mx`).
4. **Evaluación de Reglas de Negocio (Bucle `For_each`):**
   * **Servicio Social (SS):** Si el alumno cuenta con **261 o más créditos** (70% del plan) y su estatus no es "Enviado", le envía un correo electrónico notificándole los requisitos, pasos y enlaces para iniciar el Servicio Social.
   * **Prácticas Profesionales (PP):** Si el alumno cuenta con **273 o más créditos** (73% del plan) y su estatus no es "Enviado", le envía una notificación por correo electrónico para el inicio de Prácticas Profesionales.
5. **Actualización de Estatus:** Actualiza la fila del alumno en el archivo de Excel registrando "Enviado SS" o "Enviado PP" para evitar envíos duplicados en futuras ejecuciones.
6. **Generación de Respaldo CSV:** Exporta un reporte consolidado con la fecha de procesamiento y lo guarda en la carpeta `/PowerAutomate/Historial/` con la nomenclatura `Respaldo_YYYY-MM-DD_HH-mm.csv`.

---

## Estructura del Repositorio

```text
├── README.md                           <-- Documentación general
├── 01-Flujo-Escritorio-PAD/            <-- Herramienta 1: RPA Desktop
│   └── script_pad.txt                  <-- Código de acciones del flujo
    
└── 02-Flujo-Nube-CloudFlow/            <-- Herramienta 2: Cloud Flow
    ├── definition.json                 <-- Definición de la lógica en JSON
    └── CORREOSSSYPP.zip                <-- Paquete de solución importable
