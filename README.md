# 🤖 Agente_RRHH - Asistente Virtual de RR.HH. con IA

Asistente corporativo automatizado en Telegram que optimiza la atención interna. Valida la identidad del empleado con **MySQL** y resuelve dudas complejas sobre políticas de la empresa usando **Inteligencia Artificial (Cohere + LangChain)** mediante arquitectura RAG.

---

##  Demostración 

A continuación se detalla el comportamiento del sistema mediante los dos escenarios principales de control de acceso:

### 1️⃣ Validación, Saludo Personalizado y Anti-Suplantación (Usuario Registrado)
El sistema analiza automáticamente el ID único e inmutable de Telegram del usuario y comprueba si existe en la base de datos MySQL. Al confirmar el registro, extrae su nombre en segundo plano y le da una bienvenida personalizada con acceso total a sus datos privados. 

🛡️ **Seguridad Estricta:** Gracias a esta validación por ID, es imposible que un usuario intente hacerse pasar por otra persona. Aunque el usuario le pida explícitamente al bot por texto *"Soy Juan"* o *"Dame los datos de mi compañero"*, el Agente de IA ignorará el texto y se basará únicamente en el registro real de MySQL, rechazando cualquier intento de suplantación.

![1. Usuario Registrado y Seguro](img/demo-usuario-registrado.png)

### 2️⃣ Capa de Restricción Automática (Usuario No Registrado)
Si un usuario inicia el bot pero su ID de Telegram no figura en la base de datos de la empresa, el nodo **If** desvía el flujo de inmediato. El bot le entrega un saludo genérico y activa una restricción perimetral: el usuario no registrado solo podrá hacer preguntas sobre políticas generales de la empresa (RAG) y tendrá el acceso completamente bloqueado a cualquier dato privado o sensible.

![2. Usuario No Registrado](img/demo-usuario-no-registrado.png)
---

##  Características Clave

* ** Validación Automática:** Reconoce al empleado por su Telegram ID (sin contraseñas).
* ** Capa de Seguridad (IF):** Filtra de inmediato si el usuario está registrado o no antes de dar acceso.
* ** Consultas en Tiempo Real:** Extrae saldos de vacaciones y banco de horas directamente desde MySQL.
* ** Base de Conocimiento:** Responde sobre normativas de la empresa usando búsqueda semántica avanzada.
* ** Anti-Suplantación:** Confía únicamente en los registros de la base de datos, ignorando nombres ingresados por texto.

---

##  Mapa del Workflow (n8n)

Estructura visual de los nodos implementados para el enrutamiento inteligente y el agente de IA:

![Estructura del Workflow en n8n](img/workflow-full.png)

---

##  Stack Tecnológico

* **Orquestador:** n8n (Self-Hosted)
* **Modelo de IA:** Cohere Chat (Command-A)
* **Framework:** LangChain Agent (con Buffer Memory)
* **Base de Datos:** MySQL
* **Base de Conocimiento:** In-Memory Vector Store + Cohere Embeddings

---

##  Instalación Rápida

1. **Clonar e Importar:** Descarga el archivo `workflow.json` de este repositorio e impórtalo en tu panel de n8n.
2. **Credenciales:** Configura los accesos en n8n para tu Bot de Telegram, base de datos MySQL y API Key de Cohere.
3. **Base de Datos:** Estructura tu tabla local usando el esquema básico:
   ```sql
   CREATE TABLE empleados (
     telegram_id BIGINT UNIQUE NOT NULL,
     nombre VARCHAR(100) NOT NULL,
     saldo_vacaciones INT,
     banco_horas DECIMAL(5,2)
   );
