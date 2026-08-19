Si buscas herramientas de código abierto (_open source_) o libres para la comprobación, escaneo y gestión de candidatos mediante un ATS (o para optimizar CVs frente a estos filtros), existen excelentes alternativas que puedes autoalojar, auditar o utilizar de forma gratuita.

Las principales opciones se dividen en herramientas orientadas a **reclutadores (sistemas ATS completos)** y herramientas orientadas a **candidatos o desarrolladores (comprobadores y parseadores de CVs)**:

## 1. Top Sistemas ATS Open Source (Para gestionar procesos completos)

- **OpenCATS:** Es la opción más veterana y utilizada en el ecosistema open source. Funciona como un ATS tradicional y CRM de contratación.
    
    - _Qué permite comprobar/gestionar:_ Importación de CVs, creación de ofertas, seguimiento de candidatos por etapas del pipeline y registro de notas.
        
    - _Stack:_ Apache, MySQL, PHP (fácil de desplegar vía Docker).
        
- **Reqcore:** Una alternativa moderna pensada para quienes consideran que OpenCATS tiene una interfaz muy anticuada.
    
    - _Qué permite comprobar/gestionar:_ Gestión limpia de candidatos con una experiencia de usuario actual, control total de los datos en servidores propios y sin limitaciones de costes por usuario.
        
    - _Stack:_ Nuxt y PostgreSQL (despliegue rápido con Docker Compose).
        
- **Horilla:** Es un sistema HRMS (Gestión de Recursos Humanos) integral de código abierto que incluye un módulo de reclutamiento muy capaz.
    
    - _Qué permite comprobar/gestionar:_ Publicación de empleos, etapas de contratación personalizables mediante arrastrar y soltar (_drag-and-drop_), programación de entrevistas y flujos automatizados básicos.
        
    - _Stack:_ Django y PostgreSQL.
        
- **Odoo Recruitment (Edición Community):** Aunque Odoo es un ERP empresarial masivo, su módulo de selección de personal es open source en su versión comunitaria.
    
    - _Qué permite comprobar/gestionar:_ Creación de páginas de empleo corporativas, preguntas eliminatorias automáticas (_screening questions_), gestión de candidatos y transición a la contratación.
        

## 2. Herramientas Open Source para Comprobar y Parsear CVs (ATS Checkers)

Si tu objetivo es **analizar cómo lee un ATS un currículum**, extraer palabras clave o comprobar la compatibilidad (_matching_), estas herramientas libres son referentes en GitHub:

- **Resume Matcher:** Una potente herramienta de código abierto basada en IA diseñada específicamente para analizar descripciones de puestos de trabajo y compararlas con currículums.
    
    - _Qué comprueba:_ Extrae las palabras clave exactas que el algoritmo del ATS buscará, genera una puntuación de compatibilidad real (_matching score_) y ayuda a evitar el "agujero negro" de los filtros automáticos.
        
- **OpenResume:** Un creador y analizador de currículums de código abierto.
    
    - _Qué comprueba:_ Cuenta con un **parseador de CVs integrado** que te permite subir un currículum ya existente para verificar de manera visual si la estructura es legible por un ATS o si el sistema pierde información clave durante la lectura.
        
- **Bibliotecas de Procesamiento de Lenguaje Natural (NLP) para desarrolladores:** Si estás construyendo tu propio comprobador o motor de filtrado interno, los desarrolladores suelen utilizar librerías libres como **spaCy** o **Apache OpenNLP** combinadas con scripts de Python para extraer entidades nombradas (habilidades, experiencias, instituciones) directamente de archivos PDF o Word.