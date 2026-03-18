# HelpDeskBot: Prototipo de Chatbot Guiado por Estados (No-AI)

Este proyecto consiste en un Chatbot de Telegram desarrollado en **n8n**, diseñado para la gestión de solicitudes de soporte técnico y administrativo. A diferencia de los bots basados en Agentes de IA, este prototipo utiliza una arquitectura de **Máquina de Estados (State Machine)**, donde el flujo de la conversación es controlado estrictamente por la ubicación del usuario en el sistema y su interacción con las "pantallas" o menús.

## 🚀 Características Principales

* **Lógica de Estados:** El bot reconoce en qué parte del flujo se encuentra el usuario consultando un histórico de "logs" en tiempo real.
* **Sin Agentes de IA:** Navegación determinista basada en entradas numéricas y comandos específicos, garantizando respuestas precisas y bajo consumo de recursos.
* **Gestión de Sesiones:** Incluye una lógica en JavaScript para detectar la inactividad del usuario (expiración de sesión tras 60 minutos) y reiniciar el flujo al menú principal.
* **Integración con Google Sheets:** Utiliza hojas de cálculo como base de datos dinámica para la validación de usuarios registrados y persistencia de estados.

## 🛠️ Stack Tecnológico

* **n8n:** Orquestador del flujo de trabajo y la lógica de negocio.
* **Telegram Bot API:** Interfaz de comunicación con el usuario.
* **Google Sheets API:** Almacenamiento de usuarios, logs de navegación y registros de tickets.
* **JavaScript:** Procesamiento de datos complejo y manejo de tiempos de sesión.

## 📋 Funcionalidades del Prototipo

1.  **Validación de Acceso:** Solo usuarios activos en la base de datos pueden interactuar.
2.  **Menú Principal Interactivo:**
    * `0. Ayuda`: Información sobre el uso del bot.
    * `1. Crear solicitud`: Flujo guiado para registrar tickets por tipo y prioridad.
    * `2. Consultar estado`: Verificación inmediata de tickets existentes.
    * `3. Mis solicitudes`: Historial personal de interacciones.
    * `4. Reportes`: Visualización de métricas (restringido según rol).
    * `5. Configuración`: Ajustes de perfil.
3. **Reportes:** En esta opción se visualiza el reporte total de las solicitudes, en donde la salida presenta la siguiente estructura:
```bash
📊 Reporte de solicitudes

Total de solicitudes: 6

🟢 Abiertas: 3
🟡 En proceso: 1
🔵 Cerradas: 2

¿Qué deseas hacer ahora?

1. Reporte por tipo de solicitud 📩
9. Volver al menú principal 🏠
```
Al ingresar a la opción de 'Reporte por tipo de solicitud' el usuario obtiene un resumen general con el tipo de solicutd más frecuente y el detalle por cada tipo de solicitud con la siguiente estructura de salida:
```bash
Reporte de solicitudes:

Resumen general
- Tipo más frecuente: Solicitud administrativa
- Total solicitudes: 6
- Interacciones con el bot: 25

Detaille por tipo:

1. Solicitud administrativa
   - Total: 4
   - Abiertas: 2
   - En proceso: 0
   - Cerradas: 1

2. Soporte técnico
   - Total: 2
   - Abiertas: 1
   - En proceso: 1
   - Cerradas: 0

3. Consulta general
   - Total: 10
   - Abiertas: 2
   - En proceso: 1
   - Cerradas: 8
```
Vista opción 'Reporte' en Telegram:

<img width="1264" height="903" alt="Salida en telegram de opt reporte" src="https://github.com/user-attachments/assets/66dd267a-6926-4ddf-b0ea-f6b4028677f0" />

## ⚙️ Arquitectura del Flujo

El sistema opera bajo un ciclo de tres pasos:
1.  **Activación:** El bot recibe un mensaje mediante un `Telegram Trigger`.
2.  **Mapeo de Estado:** Un nodo de `Mapeador de estado` busca en la DB el último registro del usuario para determinar su "pantalla" actual.
3.  **Ejecución:** El nodo `Switch` dirige la entrada del usuario al bloque de lógica correspondiente (ej. si el usuario está en "CREAR_SOLICITUD", procesa el mensaje como el tipo de ticket).

---

## 📊 Estructura de Datos (Google Sheets)

El bot utiliza Google Sheets como base de datos relacional para gestionar la persistencia y el estado. Se compone de tres tablas principales:

### 1. Usuarios
Almacena la información de los usuarios autorizados y su perfil en el sistema.
* **telegram_user**: ID único de Telegram del usuario.
* **nombre**: Nombre completo o alias.
* **telefono**: Número de contacto registrado.
* **rol**: Nivel de permisos (ej. Usuario, Administrador).
* **activo**: Estado de habilitación en el sistema (Verdadero/Falso).

### 2. Logs
Registra cada interacción para el manejo de la Máquina de Estados y auditoría.
* **timestamp**: Fecha y hora exacta de la interacción.
* **telegram_user**: Identificador del usuario que generó el evento.
* **pantalla**: Estado o menú actual donde se encuentra el usuario.
* **opcion**: Entrada o comando enviado por el usuario.
* **resultado**: Respuesta entregada por el bot o acción ejecutada.

### 3. Solicitudes
Contiene el registro histórico y actual de los tickets de soporte.
* **id_ticket**: Identificador único numérico del requerimiento.
* **tipo**: Categoría de la solicitud (Soporte técnico, Administrativo, etc.).
* **prioridad**: Nivel de urgencia asignado.
* **descripcion**: Detalle del problema o petición.
* **estado**: Situación actual del ticket (Abierto, En proceso, Cerrado).
* **creado_por**: Referencia al usuario que generó el ticket.
* **fecha_creacion**: Fecha de registro inicial.

---

*Prototipo desarrollado por **Jorge Gomez** como parte de un enfoque en desarrollo de software avanzado y automatización de procesos.*
