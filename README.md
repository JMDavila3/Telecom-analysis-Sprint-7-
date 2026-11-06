Análisis Exploratorio de Datos y Segmentación de Clientes - ConnectaTel

Este repositorio contiene el análisis exploratorio de datos (EDA), limpieza, diagnóstico de nulos y atípicos, así como la segmentación de usuarios de ConnectaTel. El objetivo principal es comprender el comportamiento de consumo de la base de clientes, evaluar el rendimiento de los planes actuales (Básico y Premium) y proponer decisiones estratégicas para optimizar la oferta comercial y reducir el churn.

🛠️ Estructura del Dataset y Tratamiento de Datos

1. Dataset users (Información de Clientes)

Registros totales: ~4,000 usuarios.

Valores nulos:

City (11.72% nulos - 469 filas): Se determinó no adivinar ni imputar geográficamente los datos por tratarse de una variable categórica no deducible; se conservan etiquetados o aislados para no perder la información de consumo asociada a estos usuarios.

Churn_date (88.35% nulos - 3,534 filas): Estructura esperada que representa a los clientes actualmente activos. Solo un 11.65% ha cancelado su servicio (churn).

Valores atípicos/sentinels:

age: Se trataron los valores sentinels (-999) y se analizó su distribución, encontrando una tendencia hacia la derecha con una concentración principal de usuarios en el rango cercano a los 50 años.

2. Dataset usage (Logs de Actividad)

Diagnóstico MAR (Missing At Random): Se confirmó que la presencia de nulos en duration y length depende estructuralmente de la columna type (las llamadas no tienen longitud de mensaje y los mensajes no tienen duración de llamadas).

Consolidación (usage_agg): Se derivaron las métricas agregadas por usuario:

cant_mensajes: Conteo total de SMS por usuario.

cant_llamadas: Conteo total de llamadas efectuadas.

cant_minutos_llamada: Sumatoria de minutos consumidos por usuario.

🔍 Hallazgos Principales del Análisis Exploratorio

Predominio del Plan Básico:

La mayoría de la base de usuarios pertenece al Plan Básico, acumulando además el mayor volumen total de mensajes, llamadas y minutos generados.

Distribución de Uso:

Mensajes (cant_mensajes): Consumo significativamente bajo. La mayoría de los usuarios envía entre 2 y 8 mensajes (con picos en 5 y 6), alcanzando un máximo de 17. Esto demuestra que la gran masa de clientes utiliza una fracción mínima del límite del plan.

Llamadas (cant_llamadas): Sesgada a la derecha. Alta concentración en volúmenes de 3 a 5 llamadas por usuario.

Minutos (cant_minutos_llamada): Concentrados mayoritariamente entre 5 y 20 minutos, registrando una cola larga hacia la derecha hasta un máximo de 155 minutos.

Análisis de Outliers (Método IQR):

cant_mensajes y cant_llamadas: Se tomó la decisión de mantener los outliers superiores, ya que sus valores máximos reales se encuentran dentro de los límites y capacidades del negocio, representando consumidores activos válidos (heavy users).

cant_minutos_llamada: Se identificaron 31 usuarios del Plan Básico que superan el límite nominal de 100 minutos de su tarifa, representando un segmento de uso extremo que requiere seguimiento comercial puntual.

👥 Segmentación de Clientes

Se implementó la lógica de segmentación en el perfil consolidado del usuario (user_profile):

1. Por Nivel de Uso (grupo_uso)

Bajo uso: Llamadas < 5 y Mensajes < 5.

Uso medio: Llamadas < 10 y Mensajes < 10.

Alto uso: Todos los casos restantes (usuarios intensivos o heavy users).

2. Por Grupo de Edad (grupo_edad)

Joven: Edad < 30 años.

Adulto: Edad entre 30 y 59 años.

Adulto Mayor: Edad ≥ 60 años.

Segmento Clave para el Negocio: El grupo de Adultos (30 a 60 años) con nivel de Uso Medio constituye el núcleo más valioso y numeroso para ConnectaTel, siendo el motor principal de tráfico e ingresos.

💡 Recomendaciones Estratégicas para el Negocio

Lanzamiento de un "Plan Ligero / A tu medida":
Dado que gran parte de los usuarios utiliza un volumen muy bajo de mensajes y minutos (lejos del tope del Plan Básico), se sugiere una tarifa de entrada de menor costo para retener a clientes sensibles al precio.

Paquetes Adicionales (Add-ons) para Heavy Users:
Monetizar a los 31 usuarios que rebasan el límite de 100 minutos en el Plan Básico ofreciéndoles módulos adicionales de minutos a un precio preferencial, sin obligarlos a mudarse al Plan Premium.

Plan de Fidelización y Retención para el Segmento Adulto:
Implementar campañas específicas para retener a los usuarios de entre 30 y 60 años, mitigando la tasa de cancelación del 11.65%.

Reestructuración del Plan Premium:
Incorporar valor agregado exclusivo (como servicios digitales, roaming o soporte prioritario) para volver atractiva la migración voluntaria desde el Plan Básico.

🚀 Requisitos Técnicos y Librerías

Python 3.8+

Pandas (manipulación de estructuras de datos y agrupaciones)

NumPy (operaciones vectoriales y manejo de nulos)

Matplotlib y Seaborn (visualizaciones de distribuciones, histogramas y boxplots)

Proyecto elaborado para la optimización operativa y comercial de ConnectaTel.
