Este proyecto de Python tiene como objetivo principal traducir preguntas en lenguaje natural a consultas SQL, ejecutarlas en una base de datos SQL Server y mostrar los resultados. Está diseñado para interactuar con un esquema de base de datos específico relacionado con la gestión de órdenes de compra y productos.

Aquí hay un desglose de las secciones clave del proyecto:

    Importaciones y Configuración Inicial: Importa las librerías necesarias como pandas para manejo de datos, pyodbc para la conexión a SQL Server, y langchain_google_genai para interactuar con modelos de lenguaje grandes (LLM) de Google. También define los detalles de conexión a la base de datos SQL Server.

    Metadatos de la Base de Datos: Contiene un diccionario METADATOS_DB que describe las tablas relevantes (PART_MASTER, ORDER_MASTER, Part_Master_Ext, Vendor_Master, Celsa_Categorias, Transaction_History) y sus columnas. Cada tabla y columna tiene un friendly_name y una description para facilitar la comprensión por parte del LLM.

    Plantilla de Prompt (Template): Define una plantilla template que instruye al LLM sobre cómo generar consultas SQL. Esta plantilla especifica que el LLM debe actuar como un experto en SQL Server, utilizar los nombres técnicos exactos de tablas y columnas, y seguir reglas específicas para el manejo de fechas y el uso de INNER JOIN.

    Extracción de Esquema de la Base de Datos (extract_schema_pyodbc): Esta función se conecta a la base de datos SQL Server, recupera los nombres de las columnas y sus tipos de datos para las tablas especificadas en TABLES_TO_INCLUDE. Combina esta información real del esquema con los metadatos enriquecidos definidos en METADATOS_DB para crear un esquema detallado que se envía al LLM.

    Generación de Consultas SQL (to_sql_query): Utiliza la biblioteca langchain para tomar una pregunta en lenguaje natural del usuario y el esquema de la base de datos (generado por extract_schema_pyodbc) y, a través del modelo Gemini 1.5 Flash, genera una consulta SQL. La función clean_text se encarga de limpiar la salida del LLM, eliminando bloques de pensamiento y formateo de Markdown.

    Ejecución de Consultas SQL (execute_sql_query): Esta función se conecta a la base de datos y ejecuta la consulta SQL generada. Si la consulta es un SELECT, devuelve los resultados en un DataFrame de Pandas; de lo contrario, confirma la ejecución del comando.
