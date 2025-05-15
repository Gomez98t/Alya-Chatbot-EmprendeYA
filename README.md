# Proyecto Chatbot Alya para EmprendeYA (Rasa + RAG con Ollama)

Este es un proyecto de chatbot conversacional, Alya, construido con el framework Rasa. El objetivo principal es servir como un asistente informativo para la plataforma EmprendeYA.

Una característica destacada de este proyecto es la implementación de un pipeline **RAG (Retrieval-Augmented Generation)** para permitir al bot responder preguntas de forma más flexible y detallada, utilizando conocimiento extraído de la documentación de EmprendeYA.

**Stack Tecnológico Principal:**
*   **Rasa Open Source:** Para NLU, gestión de diálogo y acciones personalizadas.
*   **Python:** Lenguaje principal de desarrollo.
*   **Sentence Transformers:** Para la generación de embeddings semánticos.
*   **ChromaDB:** Base de datos vectorial local para la fase de recuperación del RAG.
*   **Ollama:** Para la ejecución local de Modelos de Lenguaje Grandes (ej. Mistral) para la fase de generación del RAG.
*   **HTML, CSS, JavaScript:** Para una interfaz de chat web simple.

Este repositorio sirve como un ejemplo práctico de cómo construir un chatbot avanzado que combina el diálogo estructurado de Rasa con las capacidades de respuesta basadas en conocimiento de los sistemas RAG y LLMs locales.
