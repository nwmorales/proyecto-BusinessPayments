Capítulo 3: Metodología y Diseño del Sistema

Introducción al Capítulo

Un breve párrafo introductorio que explique el propósito de este capítulo.
Ejemplo: "Este capítulo detalla la metodología seguida para el desarrollo del proyecto de análisis y predicción de ETFs estadounidenses. Se describirán las fases de adquisición y preprocesamiento de datos, el diseño e implementación de la arquitectura Big Data, el desarrollo y entrenamiento de los modelos de predicción, y el uso de herramientas para la visualización y minería de datos. Asimismo, se abordarán los principales desafíos técnicos encontrados y las soluciones adoptadas."
(Paso 21 de la guía general): Adquisición de Datos
* Objetivo: Describir de dónde y cómo obtuviste los datos para tu proyecto.
* 3.1.1. Fuente de Datos: Kaggle
* Nombre del Dataset Específico: (Si es un dataset conocido, menciónalo).
* Descripción del Contenido: ¿Qué tipo de información contenía este CSV (ETFs específicos, rango de fechas, variables como OHLCV, etc.)?
* Método de Obtención: ¿Descarga directa? ¿Uso de la API de Kaggle?
* Justificación (breve): ¿Por qué elegiste este dataset de Kaggle? (Ej: completitud, limpieza inicial, relevancia para el estudio).
* Ejemplo: "Una de las fuentes de datos para este proyecto fue un conjunto de datos obtenido de la plataforma Kaggle, denominado '[Nombre del Dataset en Kaggle, si aplica]'. Este dataset contenía información histórica de precios [especificar variables, ej: OHLCV] para una selección de [N] ETFs estadounidenses, cubriendo el periodo desde [Fecha Inicio] hasta [Fecha Fin]. Los datos se obtuvieron mediante descarga directa del archivo CSV proporcionado en la plataforma..."
* 3.1.2. Fuente de Datos: API de Yahoo Finance
* Librería de Acceso (si usaste Python): Especifica la librería (ej: yfinance, pandas-datareader).
* Proceso de Obtención: ¿Cómo realizaste las llamadas a la API? ¿Programaste un script para ello?
* Selección de ETFs: ¿Qué ETFs específicos o criterios de selección de ETFs utilizaste para las consultas a la API?
* Variables Recopiladas: OHLCV, volumen, dividendos, splits, etc.
* Periodo Temporal Cubierto: Desde qué fecha hasta qué fecha obtuviste los datos.
* Manejo de Limitaciones de la API (si hubo): ¿Tuviste que gestionar "rate limits" o alguna otra restricción? ¿Cómo?
* Ejemplo: "Complementariamente, se accedió a datos históricos y actualizados de ETFs mediante la API de Yahoo Finance. Para ello, se utilizó la librería yfinance de Python para realizar consultas programadas. Se seleccionaron [N o criterios] ETFs, incluyendo [mencionar algunos ejemplos clave como SPY, QQQ, etc.], para los cuales se extrajeron datos de precios OHLCV y volumen para el periodo comprendido entre [Fecha Inicio] y [Fecha Fin]. Se implementaron [mecanismos de espera/manejo de errores] para gestionar las limitaciones de frecuencia de la API..."
* 3.1.3. Formato y Almacenamiento Inicial de los Datos:
* Breve mención de que los datos se obtuvieron en formato CSV y cómo los almacenaste inicialmente antes del preprocesamiento.

(Paso 22): Preprocesamiento y Análisis Exploratorio de Datos (EDA)
* Objetivo: Detallar cómo limpiaste, transformaste y analizaste inicialmente los datos para entenderlos mejor y prepararlos para el modelado.
* 3.2.1. Entorno y Herramientas de Preprocesamiento
* Menciona Google Colab como entorno principal.
* Librerías clave: Pandas (para manipulación), NumPy (para operaciones numéricas), Matplotlib y Seaborn (para visualizaciones del EDA).
* 3.2.2. Proceso de Limpieza de Datos
* Manejo de Valores Nulos (Missing Values): ¿Cómo los identificaste? ¿Qué estrategia seguiste (eliminación, imputación – con qué método: media, mediana, valor anterior, etc.)? Justifica tu elección.
* Manejo de Datos Atípicos (Outliers) (si lo hiciste): ¿Cómo los detectaste? ¿Qué tratamiento les diste (eliminación, transformación, dejarlos si eran válidos)?
* Verificación de Duplicados y Consistencia de Datos.
* Ejemplo: "Una vez adquiridos los datos brutos en formato CSV, se procedió a su preprocesamiento en un entorno de Google Colab utilizando principalmente la librería Pandas. El primer paso consistió en la inspección de valores nulos. Para las columnas [mencionar columnas], se optó por [estrategia de imputación/eliminación] debido a [justificación]. Se verificó la existencia de registros duplicados, los cuales fueron [acción tomada]..."
* 3.2.3. Transformación de Datos
* Cálculo de Nuevas Variables (Feature Engineering, si aplica en esta etapa):
* Cálculo de retornos (logarítmicos, simples).
* Creación de indicadores técnicos (medias móviles, RSI, MACD, Bandas de Bollinger, etc.), si los generaste en esta fase para el EDA o como features para los modelos. Describe brevemente cómo se calcularon los más importantes.
* Normalización o estandarización de datos (si se aplicó de forma general o solo para ciertos análisis/modelos).
* Unión de Datasets (si los CSVs de Kaggle y Yahoo Finance se combinaron): ¿Cómo se realizó la unión? ¿Cuál fue la clave?
* Ejemplo: "Posteriormente, se calcularon los retornos logarítmicos diarios para cada ETF, ya que esta métrica es comúnmente utilizada en el análisis financiero. Adicionalmente, se generaron indicadores técnicos como medias móviles simples (SMA) de 50 y 200 periodos y el Índice de Fuerza Relativa (RSI) con una ventana de 14 periodos, los cuales podrían servir como características predictivas para los modelos..."
* 3.2.4. Análisis Exploratorio de Datos (EDA)
* Estadísticas Descriptivas: Presenta tablas con las principales estadísticas de tus variables clave (media, mediana, desviación estándar, mínimo, máximo).
* Visualizaciones:
* Gráficos de series temporales de precios y retornos para ETFs representativos.
* Histogramas o gráficos de densidad para ver la distribución de los retornos.
* Boxplots para identificar outliers.
* Matrices de correlación entre diferentes ETFs o entre variables.
* Análisis de autocorrelación (ACF) y autocorrelación parcial (PACF) de los retornos.
* Principales Hallazgos del EDA: Resume brevemente qué patrones, tendencias, estacionalidades (si las hubo a simple vista), o relaciones interesantes encontraste. Estos hallazgos pueden justificar algunas de las decisiones que tomaste después en el modelado.
* Ejemplo: "El EDA reveló [mencionar un hallazgo, ej: 'una alta correlación positiva entre los ETFs tecnológicos seleccionados']. Los gráficos de series temporales (ver Figura 3.X) mostraron [ej: 'periodos de alta volatilidad coincidentes con eventos de mercado conocidos']. Las funciones de autocorrelación de los retornos sugirieron [ej: 'una rápida decaída, indicativa de mercados eficientes o la necesidad de modelos más complejos que capturen dependencias no lineales']..."
* Recuerda referenciar tus figuras y tablas (ej. "Como se observa en la Figura 3.1...", "La Tabla 3.2 muestra...").

(Paso 23): Diseño e Implementación de la Arquitectura Big Data
* Objetivo: Explicar cómo y por qué construiste tu sistema con Docker, NiFi, Cassandra y Hadoop.
* 3.3.1. Diagrama de Arquitectura General del Sistema
* ¡Fundamental! Incluye un diagrama claro que muestre todos los componentes (fuentes de datos, Docker, NiFi, Cassandra, Hadoop, entorno de modelado en Colab, Power BI, Orange) y cómo fluyen los datos e interactúan entre ellos. Descríbelo brevemente.
* 3.3.2. Contenerización con Docker
* Propósito: ¿Por qué usaste Docker? (Reproducibilidad, aislamiento de servicios, simplificación del despliegue de NiFi, Cassandra, Hadoop).
* Implementación:
* ¿Qué servicios/componentes se ejecutaron en contenedores Docker?
* ¿Utilizaste Docker Compose para orquestar los contenedores? Si es así, describe brevemente tu archivo docker-compose.yml (puedes incluirlo en anexos).
* ¿Utilizaste imágenes oficiales de Docker Hub o creaste tus propias Dockerfiles? (Menciona cuáles y si los Dockerfiles propios están en anexos).
* Ejemplo: "Para asegurar la portabilidad, reproducibilidad y una gestión eficiente del entorno de los diferentes servicios de la arquitectura Big Data, se utilizó Docker. Se crearon contenedores para Apache NiFi, Apache Cassandra y [especificar nodos de Hadoop si aplica]. La orquestación de estos contenedores se gestionó mediante Docker Compose, cuyo archivo de configuración se detalla en el Anexo X..."
* 3.3.3. Flujo de Datos con Apache NiFi
* Propósito en tu Arquitectura: (Ej: Ingesta automatizada desde la API de Yahoo Finance, transformaciones básicas en ruta, carga de datos en Cassandra y/o HDFS).
* Diseño del Flujo (Dataflow):
* Describe los principales procesadores de NiFi que utilizaste y su función en el flujo (ej: InvokeHTTP, EvaluateJSONPath, SplitJSON, ReplaceText, PutCassandraQL, PutHDFS).
* Explica la lógica del flujo: ¿Cómo se programó? ¿Cómo se manejaron los errores? ¿Cómo se aseguró la idempotencia o la no duplicación de datos si fue necesario?
* Incluye una captura de pantalla (o un diagrama simplificado) de tu flujo principal en NiFi y referénciala.
* Ejemplo: "Apache NiFi se utilizó como la herramienta central para la ingesta y el enrutamiento automatizado de los datos de ETFs. Se diseñó un flujo (ver Figura 3.Y) que comenzaba con un procesador InvokeHTTP para consultar la API de Yahoo Finance a intervalos programados. Los datos JSON recibidos eran luego parseados y transformados utilizando procesadores como EvaluateJSONPath y ReplaceText para adaptarlos al esquema de destino. Finalmente, los datos limpios se cargaban en Apache Cassandra mediante PutCassandraQL y una copia de respaldo se almacenaba en HDFS a través de PutHDFS..."
* 3.3.4. Almacenamiento con Apache Cassandra
* Propósito: ¿Por qué elegiste Cassandra? (Ej: Escalabilidad para grandes volúmenes de series temporales, alta disponibilidad para escritura, consultas eficientes por clave primaria).
* Diseño del Modelo de Datos (Data Model):
* Define tu Keyspace.
* Define la estructura de tus Tablas (Column Families) para los datos de ETFs. Muestra el CREATE TABLE statement.
* Explica y justifica tu Clave Primaria (PRIMARY KEY): Cómo elegiste la PARTITION KEY y las CLUSTERING COLUMNS. ¿Cómo facilita este diseño las consultas que necesitas para tu análisis y modelos?
* Ejemplo: "Para el almacenamiento persistente de los datos de series temporales de los ETFs, se seleccionó Apache Cassandra debido a su arquitectura distribuida y su capacidad para manejar grandes volúmenes de escritura con alta disponibilidad. Se definió un keyspace denominado etf_data y dentro de él, una tabla principal prices_by_etf con la siguiente estructura [mostrar CREATE TABLE]. La clave primaria se diseñó con (ticker, date) donde ticker es la clave de partición para distribuir los datos por ETF y date la columna de clustering para ordenar los datos cronológicamente dentro de cada partición, optimizando así las consultas por rango de fechas para un ETF específico..."
* 3.3.5. Almacenamiento y Procesamiento con Apache Hadoop
* Propósito: ¿Para qué usaste Hadoop? (Ej: Almacenamiento a largo plazo de datos brutos o procesados en HDFS, procesamiento batch con MapReduce o Spark si lo llegaste a implementar para tareas específicas que Cassandra no cubría eficientemente).
* Componentes Utilizados:
* HDFS: Describe la estructura de directorios que usaste en HDFS para organizar tus datos.
* MapReduce/Spark (si aplica): Si realizaste algún procesamiento con estas herramientas (ej. agregaciones complejas, preparación de datos para modelos a gran escala), describe brevemente los jobs o scripts. Si no, simplemente enfócate en HDFS como repositorio.
* Integración: ¿Cómo se transferían los datos a/desde HDFS (ej. vía NiFi)?
* Ejemplo: "El ecosistema Hadoop se utilizó principalmente para el almacenamiento a largo plazo de los datos de ETFs en HDFS, proporcionando una solución robusta y escalable para grandes volúmenes. Los datos eran transferidos a HDFS [mencionar cómo, ej: 'directamente por un flujo de NiFi en formato Parquet para optimizar el almacenamiento y futuras consultas con Spark']. Si bien el procesamiento principal para el modelado se realizó en Google Colab accediendo a datos de Cassandra o CSVs exportados, HDFS se consideró como el repositorio para [ej: 'datos brutos históricos y resultados de procesamientos batch futuros']..."

(Paso 24): Desarrollo y Entrenamiento de Modelos de Predicción
* Objetivo: Detallar cómo construiste, entrenaste y evaluaste cada uno de tus modelos.
* Introducción a la sección de modelado:
* Menciona las herramientas generales usadas para el modelado (Google Colab, Python, librerías como scikit-learn, Prophet, TensorFlow/Keras).
* Cómo dividiste tus datos (conjuntos de entrenamiento, validación y prueba – especifica los periodos o la estrategia de división).
* Para cada modelo o familia de modelos (crea subsecciones 3.4.1, 3.4.2, etc.):

    * **3.4.X. [Nombre del Modelo/Familia, ej: Regresión Lineal]**
        * **Justificación de la Elección:** ¿Por qué este modelo para tu problema?
        * **Preparación Específica de Datos y Selección de Características (Features):**
            * ¿Qué variables de entrada (features) utilizaste para este modelo? (Ej: precios rezagados -lags-, volumen, indicadores técnicos calculados en el EDA).
            * ¿Cómo transformaste los datos (escalado, normalización) específicamente para este modelo?
        * **Implementación y Configuración:**
            * Librería(s) utilizada(s).
            * **Para Regresión Lineal y Prophet:** Parámetros clave de configuración.
            * **Para Redes Neuronales:** Tipo (MLP, LSTM, etc.), arquitectura detallada (número de capas, neuronas por capa, funciones de activación), función de pérdida, optimizador, tasa de aprendizaje, número de épocas, tamaño del batch, estrategias de regularización (dropout, L1/L2).
            * **Para Modelos de Clasificación (ESPECIFICA CUÁLES):** Definición clara de la variable objetivo (tus clases, ej. "sube > 1%", "baja > 1%", "estable"). Parámetros clave del modelo específico (ej. para SVM: kernel, C; para Random Forest: número de árboles, profundidad máxima). ¿Hiciste ajuste de hiperparámetros (GridSearchCV, RandomizedSearchCV)?
        * **Proceso de Entrenamiento:** Breve descripción.
        * **Métricas de Evaluación Utilizadas:** Nómbralas aquí (las detallarás con sus valores en el Capítulo 4). Justifica brevemente por qué son apropiadas para este modelo y tarea (regresión vs. clasificación).
(Paso 25): Uso de Herramientas de Visualización y Minería
* Objetivo: Explicar cómo utilizaste Power BI y Orange para explorar y presentar tus hallazgos.
* 3.5.1. Visualización de Datos y Resultados con Power BI
* Conexión a Fuentes de Datos: ¿Cómo conectaste Power BI a tus datos (CSV, Cassandra directamente, datos exportados de Hadoop)?
* Diseño de Dashboards: Describe los principales dashboards que creaste. ¿Qué información clave buscabas comunicar con cada uno?
* Tipos de Visualizaciones Utilizadas: (Ej: gráficos de líneas para series temporales, KPIs para rendimiento de modelos, gráficos de barras para comparaciones, mapas si aplica).
* Propósito: (Ej: facilitar la comprensión de tendencias, comparar el rendimiento de diferentes ETFs, mostrar los resultados de las predicciones de forma interactiva).
* Ejemplo: "Para la visualización interactiva de los datos de los ETFs y los resultados de los modelos predictivos, se utilizó Power BI. Se establecieron conexiones con [fuente de datos] para importar los datos relevantes. Se diseñaron [N] dashboards principales: uno enfocado en el análisis exploratorio de los precios históricos y retornos (ver Figura 3.Z), y otro para la visualización y comparación del rendimiento de los modelos predictivos implementados (ver Figura 3.W)..."
* Referencia a capturas de pantalla de tus dashboards más importantes.
* 3.5.2. Minería de Datos y Análisis Complementario con Orange Datamining
* Propósito: ¿Para qué tareas específicas utilizaste Orange? (Ej: prototipado rápido de algún modelo, análisis de clustering para agrupar ETFs con comportamientos similares, análisis de componentes principales, validación visual de hipótesis).
* Workflows (Flujos de Trabajo): Describe brevemente los flujos de widgets que construiste en Orange para estas tareas. Si es posible, incluye una captura de pantalla de un workflow representativo.
* Hallazgos o Utilidad: ¿Qué insights adicionales o facilidades te proporcionó Orange?
* Ejemplo: "Orange Datamining se empleó como herramienta complementaria para [ej: 'el prototipado rápido de modelos de clasificación y para realizar un análisis de clustering sobre los ETFs basado en sus perfiles de retorno y volatilidad']. Se construyeron flujos de trabajo visuales (ver Figura 3.V) que permitieron [ej: 'explorar interactivamente diferentes algoritmos de clustering como K-Means e identificar grupos de ETFs con características similares']..."

(Paso 26): Desafíos Técnicos y Soluciones Implementadas
* Objetivo: Ser transparente sobre las dificultades que enfrentaste y cómo las resolviste. Esto demuestra tu capacidad de resolución de problemas.
* Piensa en cada etapa (adquisición, preprocesamiento, arquitectura Big Data, modelado) y anota los principales obstáculos.
* Ejemplos de desafíos:
* Configuración compleja de algún componente de Hadoop o Cassandra.
* Problemas de rendimiento con NiFi.
* Dificultad para que un modelo de Red Neuronal convergiera o evitara el sobreajuste.
* Limitaciones de las herramientas o librerías.
* Problemas de integración entre diferentes tecnologías.
* Para cada desafío, explica brevemente la solución que implementaste o la mitigación que aplicaste.
* Ejemplo: "Durante la implementación de la arquitectura, uno de los principales desafíos fue la configuración de la replicación de datos entre los nodos de Cassandra en el entorno Dockerizado, lo cual se solucionó [explica cómo]. En la fase de modelado, el entrenamiento de las Redes Neuronales LSTM presentó inicialmente problemas de sobreajuste, que se mitigaron mediante la aplicación de técnicas de Dropout y la optimización del número de épocas de entrenamiento a través de 'early stopping' basado en el conjunto de validación..."

Conclusión del Capítulo

Un breve párrafo que resuma que se ha descrito la metodología completa y que el siguiente capítulo presentará los resultados derivados de esta implementación.
Ejemplo: "Este capítulo ha detallado exhaustivamente la metodología seguida en el proyecto, desde la adquisición y preparación de los datos hasta el diseño de la arquitectura Big Data, el desarrollo de los modelos predictivos y el uso de herramientas de visualización. Los pasos y decisiones técnicas aquí descritos sientan las bases para la presentación y análisis de los resultados que se abordará en el capítulo siguiente."
