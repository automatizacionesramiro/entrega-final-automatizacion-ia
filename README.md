Entrega Final: 
**Ecosistema:** Make + Groq + Airtable + Slack + Gmail

---

1. Mapa de Arquitectura
El ecosistema fue diseñado bajo una arquitectura integrada síncrona en un único escenario para maximizar el control de estados y reducir el consumo innecesario de operaciones.

Diagrama del Flujo Lógico (Ecosistema Integrado)
/--- [Filtro: Ticket recién creado] ➔ [Airtable 9: Cambiar a "Procesado"] ➔ [Slack 7: Notificación][Gmail 1: Watch Emails] ➔ [Airtable 2] ➔ [Groq 4] ➔ [Airtable 6: Crear Registro] ➔ [Router 8]--- [Filtro: Ticket Aprobado] ➔ [Gmail 11: Reply to an Email (Mismo Hilo)]

Componentes Técnicos:
Trigger Inteligente:** Nodo `Gmail 1 (Watch Emails)` optimizado en modo *From now on* para ignorar correos históricos.
Cerebro Central:** Relación relacional de datos ejecutada en `Airtable`.
Motor de Inferencia: `Groq API (Llama 3)` encargado del procesamiento de lenguaje natural.
Orquestador Central: `Router 8` encargado de la bifurcación lógica según el estado del ciclo de vida del ticket.
Human-In-The-Loop:** Interfaz interactiva de notificaciones mediante `Slack 7`.
Canal de Salida Multicanal: `Gmail 11 (Reply to an email)` enlazado al hilo de conversación nativo mediante el mapeo del `Thread ID`.


2. Estructuras de Datos Documentadas

Modelo de Datos Relacional (Airtable)
Para garantizar una arquitectura limpia y escalable, se normalizó la base de datos en dos tablas vinculadas:

1. Tabla `Clientes`:
   * `ID Cliente` (Texto - Email Único - Llave Primaria).
   * `Nombre de la empresa` (Texto).
   * `Plan` (Single Select: Básico, Premium, VIP).
   * `Tickets relacionados` (Link to another record -> Tabla Tickets).

2. Tabla `Tickets`:
   * `ID Ticket` (Autonumérico - Llave Primaria).
   * `Cliente` (Link to another record -> Tabla Clientes - Llave Foránea).
   * `Asunto` (Texto corto).
   * `Descripción` (Texto largo).
   * `Categoría IA` (Single Select: Facturación, Error de Software, Consulta General, Alta Prioridad).
   * `Estado` (Single Select: Pendiente, Procesado por IA, Aprobado, Cerrado).
   * `Respuesta Sugerida IA` (Texto largo).

B. Esquema JSON de Transferencia (Payload de la IA)
El motor de Groq devuelve estrictamente un objeto estructurado para que el orquestador lo procese sin romper las columnas de la base de datos:

```json
{
  "categoria": "string (Facturación | Error de Software | Consulta General | Alta Prioridad)",
  "respuesta_sugerida": "string (Texto formal redactado en español)"
}
```
*Procesamiento en Make:* El mapeo se ejecuta de forma nativa mediante la función:
`{{get(parseJSON(4.choices[].message.content); "categoria")}}`



