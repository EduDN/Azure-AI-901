# Azure AI-901 — Recursos

> Guía de repaso para la certificación **Microsoft Azure AI Fundamentals (AI-900/AI-901)**, con comparaciones equivalentes en AWS para facilitar la retención.

---

## 1. Cloud Computing — Modelos de Servicio

| Modelo | Descripción | Responsabilidad del cliente |
|---|---|---|
| **IaaS** (Infrastructure as a Service) | Provisión de infraestructura básica en la nube (VMs, redes, almacenamiento). El proveedor gestiona el hardware; el cliente administra el sistema operativo, runtime y aplicaciones. | Alta |
| **PaaS** (Platform as a Service) | Entorno gestionado para el despliegue y ejecución de aplicaciones, sin preocuparse por el sistema operativo o la infraestructura subyacente. | Media |
| **SaaS** (Software as a Service) | Producto de software completo, listo para usar, entregado directamente al usuario final (ej. Microsoft 365). | Baja |

**Regla general:** a medida que se avanza de IaaS → PaaS → SaaS, disminuye el control técnico del cliente pero también su carga operativa.

---

## 2. Costos en Azure

Referencia oficial de precios: [azure.microsoft.com/pricing/details/azure-openai](https://azure.microsoft.com/en-us/pricing/details/azure-openai/)

Los tres factores principales que determinan el costo de un servicio en la nube son:

- **Computación** — se cobra por el tiempo de uso de los recursos de procesamiento (CPU/GPU), ya sea por segundo, minuto u hora, dependiendo del SKU contratado.
- **Almacenamiento** — se cobra en función del volumen de datos guardados (GB/TB) y, en algunos casos, del número de operaciones de lectura/escritura.
- **Transferencia de datos (egress)** — el tráfico de salida de datos *fuera* de la nube de Azure hacia internet u otras redes normalmente tiene costo; el tráfico de entrada (ingress) suele ser gratuito.

---

## 3. Infraestructura Global de Azure

- **Regiones emparejadas (Paired Regions)** — cada región de Azure está vinculada a otra dentro de la misma geografía para replicación de datos y recuperación ante desastres (DR), reduciendo el riesgo de que ambas fallen simultáneamente.
- **Regiones soberanas (Sovereign Regions)** — instancias aisladas de Azure operadas para cumplir con marcos regulatorios específicos de un país o industria (ej. gobierno de EE. UU., China).
- **Zonas de disponibilidad (Availability Zones)** — ubicaciones físicamente separadas dentro de la misma región, cada una con energía, refrigeración y redes independientes, usadas para lograr alta disponibilidad ante fallos de un solo datacenter.
- **Centros de datos (Datacenters)** — instalaciones físicas donde reside el hardware (servidores, almacenamiento, redes) que soporta los servicios de Azure.
- **Puntos de presencia (PoP – Points of Presence)** — ubicaciones de borde (edge) distribuidas globalmente que acercan el contenido y el acceso de red al usuario final, reduciendo la latencia.

---

## 4. Evaluación de Modelos de Machine Learning

- **Matriz de Confusión (Confusion Matrix)** — tabla que compara las predicciones de un modelo de clasificación contra los valores reales, mostrando verdaderos positivos, falsos positivos, verdaderos negativos y falsos negativos. Es la base para calcular métricas como precisión, exhaustividad (recall) y F1-score.
- **AUC-ROC** — el área bajo la curva ROC (*Receiver Operating Characteristic*) mide qué tan bien el modelo distingue entre clases, graficando la tasa de verdaderos positivos frente a la tasa de falsos positivos en distintos umbrales. Un valor cercano a 1 indica un modelo casi perfecto; 0.5 equivale a una clasificación aleatoria.

---

## 5. Implementación de Modelos: Estándar vs. Aprovisionada

| Tipo | Facturación | Caso de uso ideal |
|---|---|---|
| **Estándar (Pay-as-you-go)** | Pago por uso, según los **tokens procesados** (entrada + salida). | Cargas de trabajo variables o impredecibles, prototipos, bajo volumen. |
| **Aprovisionada (Provisioned)** | Capacidad reservada mediante **PTU** (*Provisioned Throughput Units* / Unidades de Rendimiento Aprovisionado). Puede facturarse **por horas** (uso flexible y a corto plazo) o mediante **Reservas de Azure** (compromiso a largo plazo con tarifa preferencial). | Producción, alto volumen, tráfico constante y predecible, donde se requiere latencia y throughput garantizados. |

---

## 6. Inferencia: Lotes vs. Tiempo Real

- **Por lotes (Batch)** — procesamiento masivo de grandes volúmenes de datos donde no se requiere una respuesta inmediata. Suele ser más económico, ya que aprovecha capacidad ociosa.
- **Tiempo real (Real-time)** — prioriza la latencia mínima, entregando una respuesta inmediata; es el modo típico para chatbots, asistentes o aplicaciones interactivas.

---

## 7. Parámetros de Configuración de LLMs

- **Temperature** — controla la aleatoriedad/creatividad de las respuestas. Valores bajos (cercanos a 0) generan respuestas más deterministas y predecibles; valores altos generan mayor variedad y creatividad, a riesgo de menor coherencia.
- **Top K** — limita la selección de la siguiente palabra a las *K* opciones más probables del vocabulario. Reducir Top K ayuda a disminuir alucinaciones, al descartar palabras de muy baja probabilidad.
- **Top P (nucleus sampling)** — selecciona la siguiente palabra a partir del conjunto más pequeño de opciones cuya probabilidad acumulada alcanza el umbral *P*. Favorece la diversidad de vocabulario mientras mantiene coherencia, ya que el tamaño del conjunto se ajusta dinámicamente según el contexto.

---

## 8. Computer Vision

- **Segmentación semántica** — técnica que clasifica **cada píxel** de una imagen, asignándolo a la categoría del objeto al que pertenece (a diferencia de la detección de objetos, que solo dibuja un cuadro delimitador alrededor del objeto).

### OCR vs. Extracción Inteligente

| Técnica | Función |
|---|---|
| **OCR** (Optical Character Recognition) | Convierte y transforma el texto presente en una imagen en texto digital plano, sin comprender su significado. |
| **Extracción inteligente** | Va un paso más allá del OCR: identifica el **significado semántico** de cada dato extraído (por ejemplo, reconoce que un número es un "total de factura" y no solo una cadena de dígitos). |

---

## 9. Azure AI Content Understanding

Servicio **multimodal** capaz de extraer información semántica de **imágenes, archivos de audio, video y documentos complejos** (facturas, PDFs con tablas y diseños fijos), combinando modelos generativos y LLMs integrados. El resultado se entrega en **formato JSON estructurado**, listo para integrarse en flujos de datos o aplicaciones.

Sus tres capacidades principales son:

- **Classify** — clasifica el contenido de entrada según categorías definidas.
- **Extract** — extrae los datos específicos definidos en un **esquema (schema)** personalizado, sin importar el formato de origen.
- **Generate** — genera contenido derivado (resúmenes, descripciones, etc.) a partir del contenido analizado.

> En resumen: permite definir *qué* datos exactos se necesitan (el esquema) y delega en el LLM multimodal la tarea de encontrarlos, sin importar si vienen de una imagen, un audio o un documento escaneado.

### Analyzers
Motores que aplican la lógica de extracción de datos dentro de Content Understanding:
- **Prebuilt** — analizadores predefinidos por Microsoft para escenarios comunes (facturas, recibos, identificaciones, etc.).
- **Custom** — analizadores entrenados o configurados a medida para esquemas y documentos específicos del negocio.

---

## 10. SDKs y Autenticación

- **Azure AI Foundry SDK** — expone una **API REST** que permite conectar aplicaciones propias con los servicios de IA de Azure, orientada a construir **aplicaciones empresariales robustas** y escalables.
- **OpenAI SDK** — SDK de **uso exclusivo para modelos de OpenAI**; resulta útil principalmente para **pruebas rápidas o demos**, al no estar optimizado para el ecosistema completo de Azure.
- **Microsoft Entra ID** (anteriormente Azure Active Directory) — servicio de **identidad y autenticación**; gestiona el acceso seguro de usuarios y aplicaciones a los recursos de Azure mediante tokens y control de identidades.

---

## 11. Diseño de Infraestructura y Monitoreo en Azure AI Foundry

Para **supervisar un modelo** en producción se utilizan **Azure Monitor** y **Application Insights**, que permiten rastrear métricas de rendimiento, errores y trazas de las solicitudes.

Un **Proyecto de Foundry** típicamente se apoya en cuatro componentes clave:

- **Storage** — almacena datos y artefactos del proyecto.
- **AI Search** — provee el índice utilizado para escenarios de **RAG** (*Retrieval-Augmented Generation*).
- **Key Vault** — resguarda de forma segura secretos, claves y credenciales.
- **App Insights** — recopila métricas y trazas para el monitoreo continuo del proyecto.

> Adicionalmente, puede añadirse **cómputo administrado (Managed Compute)** cuando se necesitan GPUs dedicadas para entrenar o alojar modelos propios.

---

## 12. Procesamiento del Lenguaje Natural (NLP / PLN)

- **Lematización (Lemmatization)** — reduce una palabra a su forma o expresión mínima con significado (lema). Ejemplo: *"corriendo" → "correr"*. Esto ayuda a que el modelo trate variaciones de una misma palabra como un único concepto.

### Azure AI Language
*(similar a AWS Comprehend)*

- Funcionalidades de PLN: comprensión y análisis de texto, incluyendo **análisis de sentimiento**.
- **Reconocimiento de entidades (NER)** — identifica nombres, lugares, fechas, organizaciones, etc., dentro del texto.
- **Detección de información sensible (PII)** — identifica y permite **redactar (enmascarar)** datos confidenciales presentes en texto libre (números de tarjeta, identificaciones, correos, etc.).
- **Extracción de frases clave (Key Phrase Extraction)** — identifica los conceptos o temas principales de un texto.
- **Limitación importante:** el servicio trabaja **únicamente con texto plano**, no con audio ni imágenes.

---

## 13. Azure AI Speech
*(similar a AWS Transcribe + AWS Polly)*

- **Text to Speech** — convierte texto escrito en audio hablado (equivalente a AWS Polly).
- **Speech to Text** — transcribe audio hablado a texto (equivalente a AWS Transcribe).

---

## 14. Azure AI Translator
*(similar a AWS Translate)*

Servicio de **traducción automática** de texto entre múltiples idiomas, con soporte para traducción de documentos completos.

---

## 15. Azure AI Vision
*(similar a AWS Rekognition)*

- Reconocimiento y clasificación de imágenes.
- OCR (reconocimiento óptico de caracteres).
- Reconocimiento facial.
- Análisis de video.

### Custom Vision
Permite la **personalización de modelos de reconocimiento de imágenes**, entrenando clasificadores o detectores propios con imágenes etiquetadas específicas del negocio (a diferencia de los modelos preentrenados y genéricos de Azure AI Vision).

---

## 16. Document Intelligence
*(similar a AWS Textract)*

Servicio especializado en el **reconocimiento de texto y datos estructurados dentro de documentos** (facturas, recibos, contratos, formularios, etc.), extrayendo pares clave-valor, tablas y campos específicos con alta precisión.

---

## 17. Azure AI Search
*(similar a AWS Kendra u OpenSearch)*

- Permite el **consumo de bases de conocimiento** y actúa como **generador/motor de búsquedas** sobre datos indexados.
- Incluye un **explorador de búsquedas** para probar e inspeccionar los resultados de las consultas.
- Es el componente central en arquitecturas **RAG**, ya que provee el índice semántico/vectorial que alimenta al LLM con contexto relevante.

---

## 18. Almacenamiento: Blob Container
*(similar a Amazon S3)*

Servicio de almacenamiento de objetos de Azure, utilizado para guardar datos no estructurados como imágenes, videos, documentos y artefactos de modelos.

---

## 19. Herramientas de Machine Learning en Azure

### Azure ML — AutoML
*(similar a AWS SageMaker Canvas)*

Automatiza el proceso de entrenamiento de modelos de Machine Learning: prueba distintos algoritmos y combinaciones de hiperparámetros, y selecciona automáticamente los que mejor desempeño ofrecen para el caso de uso planteado, reduciendo la necesidad de intervención manual experta.

### Machine Learning Designer
*(similar a Amazon SageMaker Pipelines / Pipeline Designer)*

Permite crear modelos de Machine Learning **sin escribir código**, mediante una interfaz visual de **arrastrar y soltar (drag and drop)** que conecta bloques de datos, transformación y entrenamiento.

---

## 20. Ecosistema "IQ" de Microsoft Foundry

| Servicio | Tipo de datos que consume | Enfoque |
|---|---|---|
| **MS Foundry IQ** | Bases de conocimiento generales | Plataforma para **conectar knowledge bases a agentes** de IA. |
| **Work IQ** | Datos no estructurados | Mapea **cómo colaboran las personas**: chats de Teams, correos electrónicos, transcripciones de reuniones. Su foco está en el ecosistema **Microsoft 365**. |
| **Fabric IQ** | Datos estructurados y semi-estructurados | Mapea la **realidad transaccional del negocio**: Data Warehouses, bases de datos relacionales, CRMs. |

---

## 21. PTU — Unidades de Rendimiento Aprovisionado

**PTU** (*Provisioned Throughput Units*) representa la capacidad reservada de cómputo dedicada a un despliegue de modelo, medida en función de los parámetros **tokens por minuto (TPM)** y **solicitudes por minuto (RPM)**.

### Ejemplo de cálculo con la Capacity Calculator

Al planificar un despliegue con varias cargas de trabajo (chat con RAG, chat básico, resumen, clasificación, etc.), la calculadora de capacidad de Azure estima el total de PTUs necesarias sumando el consumo estimado de cada carga:

| Concepto | Significado |
|---|---|
| **Total PTUs: 50 (33.00)** | El primer número (**50**) es la cantidad de PTUs realmente **asignadas** en el despliegue. El número entre paréntesis (**33.00**) es el **valor de rendimiento bruto calculado** (sin redondear) que la carga de trabajo actual realmente necesita. |

### ¿Por qué 50 y no 33?

Azure obliga a aprovisionar e incrementar las PTUs en **bloques específicos** (comúnmente múltiplos de 50 para despliegues regionales de ciertos modelos). Como el cálculo real arrojó 33.00 PTUs, el sistema **redondea automáticamente hacia arriba** al siguiente bloque disponible, en este caso **50 PTUs**.

Esto genera un **margen o "colchón" de capacidad** de aproximadamente **17 PTUs sin utilizar** (50 − 33 = 17), disponibles como buffer antes de que las solicitudes comiencen a recibir errores de límite de tarifa (**HTTP 429 – Too Many Requests**).

> **Punto clave para el examen:** el redondeo hacia bloques fijos de PTU no es un desperdicio "malo" — es un margen de seguridad que absorbe picos de tráfico inesperados sin degradar el servicio.

---

## Resumen de Equivalencias Azure ↔ AWS

| Azure | AWS |
|---|---|
| Azure AI Language | Amazon Comprehend |
| Azure AI Speech | Amazon Transcribe + Amazon Polly |
| Azure AI Translator | Amazon Translate |
| Azure AI Vision | Amazon Rekognition |
| Document Intelligence | Amazon Textract |
| Azure AI Search | Amazon Kendra / OpenSearch |
| Blob Container | Amazon S3 |
| Azure ML AutoML | Amazon SageMaker Canvas |
| Machine Learning Designer | Amazon SageMaker Pipelines (Pipeline Designer) |
