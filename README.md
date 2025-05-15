# Alya: Chatbot Inteligente para la Plataforma EmprendeYA

Este repositorio contiene el código fuente de Alya, un asistente virtual conversacional diseñado para la plataforma **EmprendeYA**, una iniciativa de apoyo a emprendedores en Nariño, Colombia.

Alya utiliza el framework **Rasa Open Source** para su núcleo de Comprensión del Lenguaje Natural (NLU) y gestión de diálogo. Para responder preguntas específicas y detalladas sobre la información de EmprendeYA, se ha implementado un sistema **RAG (Retrieval-Augmented Generation)**. Este sistema utiliza:

*   **Sentence Transformers** para la creación de embeddings de texto.
*   **ChromaDB** como base de datos vectorial para almacenar y recuperar fragmentos de conocimiento relevantes.
*   **Ollama** para ejecutar Modelos de Lenguaje Grandes (LLMs) como **Mistral** de forma local, generando respuestas naturales basadas en el contexto recuperado.

El proyecto incluye una interfaz web simple (HTML, CSS, JavaScript) para demostración y pruebas.

## 🚀 Características Principales

Alya puede:
*   **Responder preguntas frecuentes sobre EmprendeYA:**
    *   Qué es la plataforma y su propósito.
    *   Quiénes son sus autores.
    *   Detalles sobre sus secciones: Gestión de Proyectos, Academia EmprendeYA, Servicios Especializados, Mentoría, Comunidad, Guía SECOP II, Blog, Herramientas Exclusivas.
    *   La visión futura de la plataforma.
*   **Manejar Chitchat Básico:** Saludos, agradecimientos, afirmaciones, negaciones y despedidas.
*   **Asistir en la Solicitud de Asesorías:** Capturando información relevante como el tema de interés (ej. marketing, finanzas), ubicación y tipo de emprendimiento.
*   **Proporcionar Información General:** Sobre oportunidades disponibles, beneficios de la plataforma y cómo registrarse.
*   **Utilizar RAG para Preguntas Abiertas:** Cuando una pregunta no coincide con un intent predefinido con alta confianza, Alya busca en su base de conocimiento (construida a partir del texto descriptivo de EmprendeYA) e intenta generar una respuesta utilizando un LLM local.

## 🛠️ Tecnologías Utilizadas

*   **Rasa Open Source:** Framework principal para el chatbot.
*   **Python 3.10+:** Lenguaje de desarrollo.
*   **Ollama:** Para ejecutar LLMs locales (ej. Mistral 7B).
*   **ChromaDB:** Base de datos vectorial local.
*   **Sentence Transformers (`all-MiniLM-L6-v2`):** Para generar embeddings de texto.
*   **Librería `openai` de Python:** Como cliente para interactuar con la API compatible de Ollama.
*   **HTML, CSS, JavaScript:** Para la interfaz de chat web de demostración.

## 🏁 Cómo Empezar

Sigue estos pasos para configurar y ejecutar Alya en tu máquina local.

### Requisitos Previos

1.  **Python** (versión 3.10 o superior recomendada). Asegúrate de que esté añadido al PATH.
2.  **Ollama** instalado y funcionando. Puedes descargarlo desde [ollama.ai](https://ollama.ai/).
3.  Un **Modelo LLM descargado** a través de Ollama. Este proyecto está configurado para usar `mistral` por defecto.
    ```bash
    ollama pull mistral
    ```
4.  (Opcional, pero recomendado si encuentras errores de compilación) **Microsoft C++ Build Tools** (para Windows).

### Instalación

1.  **Clona el Repositorio:**
    ```bash
    git clone https://URL_DE_TU_REPOSITORIO_AQUI.git
    cd nombre_de_la_carpeta_del_repositorio
    ```

2.  **Crea y Activa un Entorno Virtual:**
    ```bash
    python -m venv venv
    # Windows (PowerShell)
    .\venv\Scripts\Activate.ps1
    # Windows (CMD)
    # venv\Scripts\activate.bat
    # Linux/macOS
    # source venv/bin/activate
    ```

3.  **Instala las Dependencias:**
    (Asegúrate de tener un archivo `requirements.txt` en la raíz del proyecto. Si no, créalo desde tu entorno de desarrollo con `pip freeze > requirements.txt` o instala manualmente las librerías listadas en la sección de tecnologías).
    ```bash
    pip install -r requirements.txt
    ```
    Si no tienes `requirements.txt`, instala al menos:
    ```bash
    pip install rasa sentence-transformers chromadb openai
    ```

4.  **Indexa la Base de Conocimiento:**
    Este script procesará el texto de EmprendeYA (definido dentro del script) y creará la base de datos vectorial en una carpeta `chroma_db/`.
    ```bash
    python index_knowledge.py
    ```
    *Nota: Ejecuta este script solo una vez, o cada vez que actualices la información base en `index_knowledge.py`.*

5.  **Entrena el Modelo Rasa:**
    ```bash
    rasa train
    ```

## 🚀 Cómo Ejecutar el Bot

Necesitarás tener **tres terminales abiertas** (con el entorno virtual activado en las que ejecutan comandos de Rasa/Python).

1.  **Terminal X: Iniciar Servidor Ollama**
    ```bash
    ollama serve
    ```
    *Deja esta terminal corriendo. Debería indicar "Listening on 127.0.0.1:11434".*

2.  **Terminal 1: Iniciar Servidor de Acciones Rasa**
    (Navega a la carpeta del proyecto y activa venv si no lo has hecho)
    ```bash
    rasa run actions
    ```
    *Deja esta terminal corriendo. Verifica que cargue los embeddings, ChromaDB y el cliente Ollama sin errores.*

3.  **Terminal 2: Iniciar el Bot Rasa**
    (Navega a la carpeta del proyecto y activa venv si no lo has hecho)
    *   **Para interactuar por consola:**
        ```bash
        rasa shell
        ```
    *   **Para usar la interfaz web de demostración (si la incluiste en el repo):**
        ```bash
        rasa run --enable-api --cors "*"
        ```
        Luego, abre el archivo `index.html` de la interfaz web en tu navegador. La interfaz se conectará a `http://localhost:5005`.

## 📁 Estructura del Proyecto (Principales)

*   `actions/`: Contiene el código Python para las acciones personalizadas (incluyendo la lógica RAG).
*   `data/`: Archivos de entrenamiento NLU (`nlu.yml`), reglas (`rules.yml`) e historias (`stories.yml`).
*   `models/`: Donde Rasa guarda los modelos entrenados.
*   `chroma_db/`: Base de datos vectorial generada por `index_knowledge.py`. **Importante: Incluir en el `.gitignore` si no quieres subirla directamente al repo, pero es necesaria para ejecutar el RAG.**
*   `index_knowledge.py`: Script para procesar el texto de conocimiento y crear la base de datos vectorial.
*   `config.yml`: Configuración del pipeline NLU y políticas de diálogo.
*   `domain.yml`: Definición de intents, entidades, slots, respuestas y acciones.
*   `endpoints.yml`: Configuración de endpoints, como el servidor de acciones.
*   `requirements.txt`: (Recomendado) Lista de dependencias Python.
*   `(Opcional) web_interface/`: Carpeta con los archivos `index.html`, `style.css`, `script.js` para la demo web.

## 🔮 Próximos Pasos y Mejoras Futuras

*   Refinar continuamente los datos de NLU para mayor precisión.
*   Implementar Rasa Forms para flujos de recopilación de datos más complejos (ej. agendar asesoría con selección de fecha/hora).
*   Mejorar el prompt enviado al LLM para respuestas aún más controladas.
*   Explorar diferentes modelos LLM a través de Ollama.
*   Integrar con canales de mensajería reales.
*   Añadir más capacidades y conocimientos a la base de EmprendeYA.

    Nota sobre `chroma_db/`:** Esta carpeta es generada por `index_knowledge.py` y contiene la base de datos vectorial. No está incluida en el repositorio para mantenerlo ligero. Deberás ejecutar `python index_knowledge.py` para crearla después de instalar las dependencias.

---

*Desarrollado con ❤️ para el proyecto EmprendeYA.*
