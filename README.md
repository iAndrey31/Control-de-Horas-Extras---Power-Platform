# 🕒 Sistema de Gestión y Control de Horas Extras (Power Platform)

[![Power Apps](https://img.shields.io/badge/Power%20Apps-001F5F?style=for-the-badge&logo=powerapps&logoColor=white)](https://powerapps.microsoft.com/)
[![Power Automate](https://img.shields.io/badge/Power%20Automate-0066FF?style=for-the-badge&logo=powerautomate&logoColor=white)](https://flow.microsoft.com/)
[![SharePoint](https://img.shields.io/badge/SharePoint-0078D4?style=for-the-badge&logo=microsoftsharepoint&logoColor=white)](https://www.microsoft.com/sharepoint)

## 📝 Descripción del Proyecto
Solución empresarial desarrollada para automatizar el ciclo de vida de las horas extras: desde la solicitud del empleado hasta la aprobación jerárquica y el ajuste contable automático de saldos. 

Este proyecto sustituye formularios físicos y cadenas de correos manuales por un ecosistema digital integrado que garantiza la integridad de los datos.

---

## ⚙️ Automatización con Power Automate (Backend Logic)

El flujo de trabajo es el motor de la solución y ejecuta las siguientes fases críticas:

### 1. Flujo de Aprobación Interactiva
Al registrarse una nueva solicitud, el sistema identifica al responsable y envía un correo electrónico dinámico utilizando el conector **Approvals**:
* **Interacción Directa:** El aprobador puede **Aprobar** o **Rechazar** directamente desde su bandeja de entrada (Outlook/Teams) sin necesidad de abrir la aplicación.
* **Comentarios Obligatorios:** Permite registrar el motivo de la decisión, que se guarda automáticamente en el historial de la solicitud.

### 2. Lógica Condicional y Cálculo Contable
Una vez aprobada la solicitud, el flujo evalúa el `TIPO` de transacción para decidir qué operación matemática realizar sobre el saldo del empleado:

```powerapps
// Condición: Si el Tipo es igual a "Autorización de horas extras"
// Operación: SUMA (add)
add(
    outputs('Obtener_saldo_actual')?['body/HORAS_ACUMULADAS'], 
    triggerOutputs()?['body/HORAS_SOLICITADAS']
)

// Condición: Si el Tipo es igual a "Canje de horas" (Else)
// Operación: RESTA (sub)
sub(
    outputs('Obtener_saldo_actual')?['body/HORAS_ACUMULADAS'], 
    triggerOutputs()?['body/HORAS_SOLICITADAS']
)
```
## 📸 Galería del Proyecto

### 📱 Power Apps: Interfaz de Usuario
Esta sección muestra la experiencia del empleado, desde la consulta de saldos hasta el envío de formularios.

<table align="center">
  <tr>
    <th align="center">Pantalla Principal (Saldos)</th>
    <th align="center">Formulario de Solicitud</th>
  </tr>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/9c036d41-58ec-4cb1-a1a2-530ca997be17" width="300" alt="Dashboard de Power Apps" />
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/cd591a7b-28ff-4877-bb83-610c053e8a1d" width="300" alt="Formulario de Solicitud" />
    </td>
  </tr>
  <tr>
    <td align="center"><i>Visualización dinámica de horas disponibles según el usuario logueado.</i></td>
    <td align="center"><i>Validación de datos y selección de tipo de movimiento (Suma/Resta).</i></td>
  </tr>
</table>

---

### ⚙️ Power Automate: Lógica y Aprobaciones
Aquí se observa la "inteligencia" del sistema que procesa las solicitudes y gestiona los correos.

#### 📩 Flujo de Aprobación por Correo
El responsable recibe una notificación interactiva para decidir sobre la solicitud desde Outlook o Teams.

<p align="center">
  <img src="https://github.com/user-attachments/assets/165c23c7-0a6d-4e75-9637-5f489f81c7c2" width="700" alt="Correo de Aprobación" />
</p>

#### 🧠 Diagrama de Lógica Condicional
El flujo evalúa el tipo de solicitud para actualizar la base de datos de SharePoint de forma automática.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7cb69237-5f52-4028-9480-808bc42fb901" width="500" alt="Diagrama del Flujo" />
  <br>
  <i>Estructura que incluye: Disparador, Acción de Aprobación, Condición Logica y Actualización de Saldos.</i>
</p>
