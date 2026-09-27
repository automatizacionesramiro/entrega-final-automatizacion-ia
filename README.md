# 🛠️ Proyecto Final: Sistema Inteligente de Clasificación y Resolución de Tickets con IA
**Estudiante:** Ramiro
**Rol:** Arquitecto de Flujos IA
**Ecosistema:** Make + Groq + Airtable + Slack + Gmail

---

## 📌 1. Mapa de Arquitectura
El ecosistema fue diseñado bajo una arquitectura integrada síncrona en un único escenario para maximizar el control de estados y reducir el consumo innecesario de operaciones.

### Diagrama del Flujo Lógico (Ecosistema Integrado)
/--- [Filtro: Ticket recién creado] ➔ [Airtable 9: Cambiar a "Procesado"] ➔ [Slack 7: Notificación][Gmail 1: Watch Emails] ➔ [Airtable 2] ➔ [Groq 4] ➔ [Airtable 6: Crear Registro] ➔ [Router 8]--- [Filtro: Ticket Aprobado] ➔ [Gmail 11: Reply to an Email (Mismo Hilo)]