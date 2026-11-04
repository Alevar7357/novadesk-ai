# 🤖 NovaDesk AI — Asistente Inteligente de Soporte Técnico

## 📌 Descripción del proyecto

NovaDesk AI es un asistente de inteligencia artificial desarrollado con **n8n**, diseñado para automatizar la primera atención de consultas e incidentes relacionados con **NovaERP**, un sistema ERP ficticio utilizado como caso de estudio.

El asistente interpreta solicitudes en lenguaje natural, brinda orientación inicial, clasifica incidentes y utiliza herramientas conectadas a Google Sheets para gestionar tickets de soporte.

El objetivo es demostrar cómo un agente de IA puede integrarse con herramientas externas para asistir a un equipo de soporte técnico L2, manteniendo mecanismos de escalamiento humano para situaciones que requieren investigación o intervención especializada.

## 🎯 Objetivos

- Automatizar la recepción y clasificación inicial de solicitudes.
- Brindar respuestas orientativas a consultas funcionales.
- Registrar incidentes que requieren seguimiento.
- Consultar tickets existentes para reducir duplicaciones.
- Actualizar observaciones de tickets previamente identificados.
- Notificar por correo electrónico las solicitudes procesadas.
- Centralizar diferentes canales de entrada en un mismo agente.

## 🛠️ Tecnologías utilizadas

| Tecnología | Función |
|---|---|
| n8n | Orquestación de workflows y automatizaciones |
| DeepSeek | Modelo de lenguaje utilizado por el agente |
| AI Agent de n8n | Interpretación de solicitudes y selección de herramientas |
| Google Sheets | Almacenamiento y gestión de tickets |
| Telegram | Canal de interacción con usuarios |
| Webhook | Recepción de solicitudes mediante HTTP |
| Chat de n8n | Canal de conversación y pruebas |
| Gmail | Notificación al equipo técnico |
| Simple Memory | Conservación del contexto conversacional |

## 🤖 Modelo de inteligencia artificial

NovaDesk AI utiliza **DeepSeek**, mediante el nodo nativo **DeepSeek Chat Model** de n8n, con el modelo `deepseek-flash`.

El modelo funciona como motor de razonamiento y generación de respuestas del agente, permitiéndole:

- Interpretar consultas e incidentes expresados en lenguaje natural.
- Clasificar solicitudes según su categoría y prioridad.
- Decidir cuándo utilizar las herramientas disponibles.
- Procesar los resultados devueltos por Google Sheets.
- Generar respuestas contextualizadas para los usuarios.

La arquitectura utiliza un **AI Agent de n8n**, que coordina el modelo de lenguaje, la memoria conversacional y las herramientas externas.

DeepSeek no almacena los tickets ni ejecuta directamente operaciones sobre Google Sheets: estas acciones se realizan mediante los nodos y herramientas conectados en n8n.

## 🧩 Arquitectura del workflow

El sistema dispone de tres canales de entrada:

1. **Telegram Trigger:** recibe mensajes desde un bot de Telegram.
2. **Webhook:** recibe solicitudes HTTP mediante el método POST.
3. **Chat Trigger:** permite interactuar con el agente desde el chat de n8n.

Cada entrada se normaliza para generar una estructura común con información como el canal, identificador de sesión, mensaje e identificador de solicitud.

Las solicitudes son procesadas por el agente **NovaDesk AI Manager**, conectado al modelo DeepSeek, una memoria conversacional y herramientas de Google Sheets.

Una vez generada la respuesta, el workflow identifica el canal de origen para dirigirla a la salida correspondiente. Paralelamente, utiliza Gmail para notificar al técnico.

## 🔧 Herramientas del agente

### Crear ticket

Registra una solicitud en Google Sheets, incluyendo descripción, categoría, prioridad, observaciones, fecha, estado y datos de identificación.

### Buscar Tickets

Consulta los tickets asociados a la sesión del usuario para recuperar información y comprobar si existen incidentes previamente registrados.

### Actualizar Ticket

Permite incorporar nuevas observaciones a un ticket existente, identificándolo mediante su ID y conservando el historial anterior.

## 🧠 Comportamiento del agente

El agente utiliza un System Prompt estructurado que define:

- Rol y alcance del asistente.
- Objetivos y contexto funcional.
- Clasificación de solicitudes.
- Criterios de prioridad.
- Reglas para crear, consultar y actualizar tickets.
- Prevención de registros duplicados.
- Condiciones de escalamiento humano.
- Restricciones de seguridad y uso de herramientas.
- Formato de respuesta al usuario.

El agente no tiene acceso directo al ERP ni a sus bases de datos. Tampoco ejecuta modificaciones operativas sobre el sistema.

## 📂 Archivo del proyecto

El repositorio incluye el workflow exportado desde n8n en formato JSON, preparado para su publicación sin credenciales personales.

El archivo puede importarse en una instancia compatible de n8n. Para utilizarlo es necesario configurar las credenciales de los servicios externos y reemplazar los valores de ejemplo correspondientes a Google Sheets y Gmail.

## 🧪 Pruebas realizadas

Durante el desarrollo se realizaron pruebas funcionales de:

- Recepción de mensajes mediante Telegram y Chat.
- Procesamiento de solicitudes desde Webhook.
- Creación de tickets en Google Sheets.
- Consulta de tickets existentes.
- Actualización de observaciones mediante el ID del ticket.
- Enrutamiento de respuestas según el canal.
- Envío de notificaciones por Gmail.

Estas pruebas corresponden al entorno de desarrollo y no constituyen una validación de producción.

## ⚠️ Limitaciones actuales

- Google Sheets se utiliza como almacenamiento de demostración, no como una plataforma de ticketing empresarial.
- El agente no accede directamente a NovaERP.
- El Webhook está configurado para responder inicialmente a la solicitud HTTP, por lo que la respuesta posterior del agente no se entrega en esa misma petición.
- La calidad de las respuestas depende del modelo de IA, las instrucciones y los datos disponibles.
- El escalamiento humano se realiza mediante registro de tickets y notificaciones, sin asignación automática de técnicos.

## 🚀 Próximas mejoras

- Implementar manejo centralizado de errores.
- Incorporar validaciones humanas para operaciones sensibles.
- Mejorar la trazabilidad y el seguimiento de solicitudes.
- Ampliar las pruebas de extremo a extremo.
- Evaluar mecanismos de respuesta asíncrona para integraciones externas.

## 📚 Contexto académico

Proyecto desarrollado como parte de un curso avanzado de inteligencia artificial y automatización con agentes de IA.

**Estado:** Checkpoint 1 — Primera entrega.
