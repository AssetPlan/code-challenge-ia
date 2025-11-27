# Agente Conversacional para Clínica Veterinaria Virtual "VetCare AI"


### **1. Resumen Ejecutivo**

El objetivo de este proyecto es desarrollar un agente conversacional de IA para una clínica veterinaria virtual. Este agente, llamado "VetCare AI", servirá como el primer punto de contacto para los clientes, ayudándoles con dudas generales sobre el cuidado de sus mascotas y agendando citas.

Este desafío está diseñado para evaluar tu habilidad en la construcción de sistemas de IA complejos utilizando el ecosistema de LangChain, tu comprensión de arquitecturas de agentes y tu capacidad para integrar diferentes componentes (RAG, herramientas, APIs) en una solución cohesiva.

### **2. Objetivo del Proyecto**

Crear un prototipo funcional de un agente conversacional multi-agente capaz de:
1.  Responder preguntas sobre el cuidado de mascotas utilizando una base de conocimientos.
2.  Agendar citas, recopilando la información necesaria y verificando la disponibilidad.
3.  Detectar cuándo un usuario necesita atención humana y escalar la conversación.

### **3. Requisitos Funcionales (Core Features)**

El sistema debe estar orquestado como un sistema multi-agente, donde un agente principal (o un router) dirige las solicitudes del usuario al agente especializado correcto.

#### **3.1. Agente de Consultas Generales (Agente RAG)**
Este agente será responsable de responder preguntas generales sobre el cuidado de las mascotas.

*   **Funcionalidad:** Debe utilizar un enfoque de **Retrieval-Augmented Generation (RAG)**.
*   **Base de Conocimientos:** Se te proporcionará una carpeta llamada `info-mascotas` que contiene varios documentos de texto. El agente debe usar estos documentos como su única fuente de verdad para responder a las preguntas.
*   **Comportamiento Esperado:**
    *   El usuario realiza una pregunta (p. ej., "¿Con qué frecuencia debo bañar a mi perro?").
    *   El agente busca la información más relevante en los documentos de la base de conocimientos.
    *   Utilizando la información recuperada, genera una respuesta coherente y útil en lenguaje natural.
    *   Si no encuentra información relevante, debe indicarlo claramente al usuario (p. ej., "Lo siento, no tengo información sobre ese tema específico").

#### **3.2. Agente de Agendamiento de Citas (Agente con Herramientas)**
Este agente se activará cuando el usuario exprese la intención de agendar una visita.

*   **Recopilación de Datos:** El agente debe recopilar la siguiente información del usuario de manera conversacional:
    *   **Datos del Dueño:** Nombre completo, número de teléfono, email.
    *   **Datos de la Mascota:** Nombre, especie (perro, gato, etc.), raza (si aplica), edad.
    *   **Motivo de la Consulta:** Una breve descripción del motivo de la visita.
*   **Coordinación de Horarios:**
    *   El agente debe preguntar al usuario por el día y la hora deseados para la cita.
    *   Debe utilizar una **herramienta (Tool)** para verificar la disponibilidad.
*   **Simulación de API de Disponibilidad:**
    *   No necesitas construir una API real. Debes implementar una función que simule esta API.
    *   **`check_availability(dia: str, hora: str) -> bool`**: Esta función recibirá un día y una hora y deberá devolver `True` (disponible) o `False` (no disponible) de forma **aleatoria**.
    *   Si la hora solicitada no está disponible, el agente debe informar al usuario y sugerirle que pruebe con otra fecha/hora.
*   **Confirmación:** Una vez que se encuentra un horario disponible y se han recopilado todos los datos, el agente debe confirmar la cita con el usuario, resumiendo toda la información.

#### **3.3. Mecanismo de Escalación a Humano ("Escape Hatch")**
El sistema debe ser capaz de reconocer cuándo la conversación debe ser transferida a un agente humano.

*   **Detección de Intención:** El agente principal (o un agente de triage) debe analizar el sentimiento del usuario o buscar frases explícitas como "quiero hablar con una persona", "conectar con un humano", "estoy frustrado con este bot", etc.
*   **Simulación de API de Escalación:**
    *   Al detectar la necesidad de escalación, el sistema debe llamar a una **herramienta (Tool)** que simule una llamada a una API para solicitar atención humana.
    *   **`request_human_agent(user_info: dict)`**: Esta función recibirá los datos del usuario y simulará la creación de un ticket de soporte. Para este desafío, es suficiente con que la función imprima un mensaje en la consola, como: `TICKET CREADO: El usuario [Nombre del usuario] en el [teléfono] ha solicitado atención humana.`

### **4. Requisitos Técnicos y Arquitectónicos**

*   **Lenguaje:** Python.
*   **Framework Principal:** **LangChain**. Se recomienda encarecidamente el uso de **LangGraph** para orquestar el flujo entre los diferentes agentes.
*   **Modelo de Lenguaje (LLM):** Utiliza los modelos de OpenAI. Se te proporcionará una clave de API con crédito suficiente para el desarrollo y las pruebas.
*   **Vector Store (para RAG):** Eres libre de elegir la biblioteca que prefieras para crear los embeddings y el índice vectorial (p. ej., ChromaDB, FAISS, etc.).
*   **Interfaz:** La interfaz de usuario no es el foco principal. Puedes optar por:
    *   Un **CLI (Command-Line Interface)** interactivo.
    *   (Opcional) Una interfaz web simple usando **Streamlit** o **Gradio**, si te sientes cómodo con ello.

### **5. Entregables**

1.  **Código Fuente:** El código completo de tu proyecto.
    *   Debe ser entregado en un repositorio Git (p. ej., GitHub, GitLab), al cual nos darás acceso.
2.  **Documentación (`README.md`):** Un archivo `README.md` claro y completo en la raíz del repositorio que incluya:
    *   Una breve descripción del proyecto.
    *   Instrucciones detalladas sobre cómo configurar el entorno virtual e instalar las dependencias (p. ej., un archivo `requirements.txt`).
    *   Instrucciones claras sobre cómo ejecutar la aplicación.
    *   Una breve explicación de tus **decisiones arquitectónicas**: ¿Cómo estructuraste los agentes? ¿Por qué elegiste esa estructura? ¿Cómo funciona el flujo en LangGraph (si lo usaste)?

### **6. Criterios de Evaluación**

Serás evaluado/a en base a los siguientes criterios:

*   **Funcionalidad:** ¿El agente cumple con todos los requisitos funcionales descritos en este documento?
*   **Calidad de la Arquitectura:** La lógica y la solidez del diseño de tu sistema multi-agente. La claridad en la separación de responsabilidades entre los agentes.
*   **Calidad del Código:** Legibilidad, modularidad, eficiencia y adherencia a las buenas prácticas de Python.
*   **Uso de LangChain/LangGraph:** Tu capacidad para utilizar las herramientas del ecosistema de LangChain de manera efectiva e idiomática.
*   **Documentación:** La claridad y exhaustividad de tu archivo `README.md`. Un buen `README` es fundamental.
*   **(Bonus) Robustez:** ¿Cómo manejas los errores y los casos límite? (p. ej., entradas de usuario ambiguas, fallos en la simulación de API, etc.).
