# **IBM Datahack-Power BI**
## **Capston Project**
### Octubre 2023
#### *Autor:* Javier Blanco

## Índice

- [1. Descripción del problema](#1-descripción-del-problema)
- [2. Dataset](#2-dataset)
  - [2.1 Obtención de los datos](#21-obtención-de-los-datos)
  - [2.2. Campos y registros](#22-campos-y-registros)
- [3. ETL](#3-etl)
  - [3.1 Extracción (conexión de orígen)](#31-extracción-conexión-de-orígen)
  - [3.2 Transformación (PowerQuery)](#32-transformación-powerquery)
  - [3.3 Carga (PowerBI)](#33-carga-powerbi)
- [4. Modelo de Datos (Esquema en Estrella)](#4-modelo-de-datos-esquema-en-estrella)
- [5. Ratios y métricas de interés](#5-ratios-y-métricas-de-interés)
- [6. Resultado final](#6-resultado-final)
- [7. Referencias](#7-referencias)
- [8. Apéndice](#8-apéndice)
  - [8.1 Fórmulas DAX utilizadas](#81-fórmulas-dax-utilizadas)
  - [8.2 PowerQuery](#82-powerquery)


## 1. Descripción del problema
DH Marketing Consultants te ha contratado en calidad de analista de datos para investigar y analizar un conjunto de datos del departamento de marketing. El director de marketing requiere generar valor a partir de estos datos y te solicita realizar un análisis. Dicho análisis debe basarse en los diversos factores que podemos medir en el conjunto de datos. Se espera llevar a cabo, como mínimo, las siguientes tareas:

* Limpieza de Datos
* Transformación de Datos
* Visualización

## 2. Dataset
Los datos suministrados provienen de una fuente de datos de acceso público que contiene información relacionada con cinco campañas de marketing llevadas a cabo por una empresa. Estos datos incluyen detalles sobre las plataformas utilizadas y el número de ventas generadas a través de estas plataformas, junto con otros datos de gran relevancia que ofrecen un potencial significativo para extraer una gran cantidad de información valiosa. 

### 2.1 Obtención de los datos

Los datos han sido obtenidos de manera pública y gratuita a través del siguiente [enlace](https://www.kaggle.com/datasets/rodsaldanha/arketing-campaign). El contexto se basa en la necesidad de crear un modelo de respuesta que potencie de manera significativa la eficiencia de una campaña de marketing al aumentar las respuestas o reducir los gastos. El objetivo radica en prever quiénes responderán a una oferta de producto o servicio, lo cual puede optimizar la estrategia de marketing y maximizar los recursos invertidos en la campaña.


### 2.2. Campos y registros

Los campos que conforman el dataset podrían ser clasificados en 4 grupos principales, siendo 29 en total:

* **Datos Demográficos y Personales:** 
<br></br>

1. **ID**: Identificación única del cliente. (Tipo de dato: Entero)

2. **Year_Birth**: Año de nacimiento del cliente. (Tipo de dato: Entero)

3. **Education**: Nivel educativo del cliente. (Tipo de dato: Cadena de texto)

4. **Marital_Status**: Estado civil del cliente. (Tipo de dato: Cadena de texto)

5. **Income**: Ingresos anuales del hogar del cliente. (Tipo de dato: Numérico - decimal)

6. **Kidhome**: Número de niños pequeños en el hogar del cliente. (Tipo de dato: Entero)

7. **Teenhome**: Número de adolescentes en el hogar del cliente. (Tipo de dato: Entero)

8. **Dt_Customer**: Fecha de inscripción del cliente con la empresa. (Tipo de dato: Fecha)
<br></br>

* **Comportamiento de Compra:**
<br></br>

1. **Recency**: Número de días desde la última compra del cliente. (Tipo de dato: Entero)

2. **NumDealsPurchases**: Número de compras realizadas con descuento. (Tipo de dato: Entero)

3. **NumWebPurchases**: Número de compras realizadas a través del sitio web de la empresa. (Tipo de dato: Entero)

4. **NumCatalogPurchases**: Número de compras realizadas usando catálogo. (Tipo de dato: Entero)

5. **NumStorePurchases**: Número de compras realizadas directamente en tiendas. (Tipo de dato: Entero)

6. **NumWebVisitsMonth**: Número de visitas al sitio web de la empresa en el último mes. (Tipo de dato: Entero)
<br></br>

* **Gastos en Productos**:
<br></br>

1. **MntWines**: Monto gastado en productos de vino en los últimos 2 años. (Tipo de dato: Numérico - decimal)

2. **MntFruits**: Monto gastado en productos de frutas en los últimos 2 años. (Tipo de dato: Numérico - decimal)

3. **MntMeatProducts**: Monto gastado en productos de carne en los últimos 2 años. (Tipo de dato: Numérico - decimal)

4. **MntFishProducts**: Monto gastado en productos de pescado en los últimos 2 años. (Tipo de dato: Numérico - decimal)

5. **MntSweetProducts**: Monto gastado en productos dulces en los últimos 2 años. (Tipo de dato: Numérico - decimal)

6. **MntGoldProds**: Monto gastado en productos de oro en los últimos 2 años. (Tipo de dato: Numérico - decimal)
<br></br>

* **Respuestas a Campañas de Marketing:**
<br></br>

1. **AcceptedCmp3**: 1 si el cliente aceptó la oferta en la tercera campaña, 0 en caso contrario. (Tipo de dato: Entero - binario)

2. **AcceptedCmp4**: 1 si el cliente aceptó la oferta en la cuarta campaña, 0 en caso contrario. (Tipo de dato: Entero - binario)

3. **AcceptedCmp5**: 1 si el cliente aceptó la oferta en la quinta campaña, 0 en caso contrario. (Tipo de dato: Entero - binario)

4. **AcceptedCmp1**: 1 si el cliente aceptó la oferta en la primera campaña, 0 en caso contrario. (Tipo de dato: Entero - binario)

5. **AcceptedCmp2**: 1 si el cliente aceptó la oferta en la segunda campaña, 0 en caso contrario. (Tipo de dato: Entero - binario)

6. **Complain**: 1 si el cliente presentó una queja en los últimos 2 años. (Tipo de dato: Entero - binario)

7. **Z_CostContact**: Costo fijo asociado al contacto con el cliente. (Tipo de dato: Entero)

8. **Z_Revenue**: Ingresos generados por el contacto con el cliente. (Tipo de dato: Entero)

9. **Response**: 1 si el cliente aceptó la oferta en la última campaña, 0 en caso contrario. (Tipo de dato: Entero - binario)



## 3. ETL

### 3.1 Extracción (conexión de orígen)
Para la extracción de datos se ha hecho una conexión de orígen de datos desde PowerBI hasta el archivo llamado `marketing_campaign.xlsx` desde Menú `Datos > Libro de Excel` obteniendose la siguiente visualización:

<br></br>

![E1](Capstone_project\img\E1.jpg)

<br></br>
Los datos pasaron a ser mostrados en el Editor de PowerQuery para su posterior tratamiento y transformación (ver siguiente figura)
<br></br>

![E2](Capstone_project\img\E2.jpg)
<br></br>

El código en Lenguaje M implementado para la extracción de datos se encuentra en el apartado [8.2 de este documento](#82-powerquery)

### 3.2 Transformación (PowerQuery)

### 3.3 Carga (PowerBI)

## 4. Modelo de Datos (Esquema en Estrella)
<br>

* ### **Tabla de Hechos:**

La tabla de hechos contiene las métricas cuantitativas que vamos analizar. En este caso, las métricas más relevantes para el conjunto de datos de la campaña de marketing son:

<br>

* **CustomerID**: clave foránea vinculada a la dimensión Cliente
* **CampaignI**: clave foránea vinculada a la dimensión Campaña
* **AcceptedCmp1, AcceptedCmp2, AcceptedCmp3, AcceptedCmp4, AcceptedCmp5, Response**: 1 para aceptado, 0 para no aceptado
* **Complain**: 1 para queja, 0 para no queja
* **NumDealsPurchases, NumWebPurchases, NumCatalogPurchases, NumStorePurchases, NumWebVisitsMonth**: métricas cuantitativas.
<br>


* ### **Dimensión Cliente**:
<br>

* **CustomerID**: clave primaria

* **Year_Birth, Education, Marital_Status, Income, Kidhome, Teenhome**: atributos descriptivos

* ### **Dimensión Campaña**:
<br>

* **CampaignID**: clave primaria
* **CampaignName**: "Campaña 1", "Campaña 2", etc
* **PlatformUsed**: Atributos descriptivos sobre la campaña


## 5. Ratios y métricas de interés

Existen varias métricas clave que podríamos extraer de ese Dataset.
<br>

1. **Tasa de Conversión por Cada Campaña**:
<br>

Métrica: Calcula la tasa de conversión para cada campaña (AcceptedCmp1 hasta AcceptedCmp5) dividiendo el número de clientes que aceptaron la oferta en una campaña entre el total de clientes en esa campaña.
Con esto se evalua la efectividad de cada campaña. Una tasa de conversión muy alta indicará una campaña más exitosa.
<br>

2.**Tasa de Respuesta Global**:
<br></br>

Métrica: Calcula la tasa de respuesta global dividiendo el número total de clientes que respondieron positivamente en la última campaña (Response=1) entre el total de clientes.
Esta métrica proporciona una visión general de la efectividad global de la campaña y la participación del cliente.
<br></br>

3. **Tasa de Quejas de Clientes**:
<br></br>

Métrica: Calcula el porcentaje de clientes que presentaron quejas (Complain=1) en los últimos 2 años.
Se indica con esto el grado de insatisfacción del cliente, lo que podría afectar las futuras estrategias de marketing y los esfuerzos de retención del cliente.
<br></br>

4. **Análisis Demográfico de Clientes**:
<br></br>

Métricas: Explora la distribución de clientes basada en Educación, Estado Civil e Ingresos.
Su busca con ello comprender el perfil demográfico de los clientes puede ayudar a adaptar las campañas de marketing a segmentos de clientes específicos.
<br></br>

5. **Patrones de Compra**:
<br></br>

Métricas: Analiza la cantidad promedio invertida en diferentes categorías de productos (MntWines, MntFruits, MntMeatProducts, etc.).
Calculamos los ratios de gasto en diferentes categorías de productos para entender las preferencias del cliente.
De esta forma identificaremos las categorías de productos populares y preferencias del cliente, facilitando esfuerzos de marketing dirigidos.
<br></br>

6. **Composición del Hogar del Cliente**:
<br></br>

Métricas: Explora el número promedio de niños (Kidhome) y adolescentes (Teenhome) en los hogares de los clientes.
Calculamos los ratios de niños y adolescentes respecto a adultos en los hogares.
Se busca proporcionar un *insight* sobre la estructura familiar de los clientes, lo que puede influir en las estrategias de marketing, especialmente para productos orientados a la familia.
<br></br>

7. **Canales de Compra de Clientes**:
<br></br>

Métricas: Analiza el número de compras realizadas a través de diferentes canales (NumWebPurchases, NumCatalogPurchases, NumStorePurchases, etc.).
Calculamos la proporción de compras en línea respecto al total de compras, compras por catálogo respecto al total de compras, etc.
Identificaremos así los canales de compra más populares, guiando los esfuerzos de marketing y la asignación de recursos.
<br></br>

8. **Frecuencia de Compras:**
<br></br>

Métrica: Analiza el número promedio de días desde la última compra (Recency).
Indicamos así la participación y lealtad del cliente. Un valor de recencia más bajo sugiere una participación activa del cliente.
<br></br>

9. **Análisis de Respuestas de Clientes Basado en la Frecuencia de Compras:**
<br></br>

Métrica: Calcula las tasas de respuesta basadas en diferentes periodos de recencia (por ejemplo, tasas de respuesta para clientes que realizaron una compra en los últimos 30 días, 60 días, etc.).
Identificamos si la actividad reciente del cliente se correlaciona con las respuestas a las campañas, permitiendo campañas dirigidas hacia clientes activos.
<br></br>

10. **Métricas de Lealtad del Cliente**:
<br></br>

Métricas: Explora el número de ofertas y descuentos que los clientes han utilizado (NumDealsPurchases) y analizar las tasas de respuesta para estos clientes.
Identificamos el impacto de los programas de lealtad y descuentos en las respuestas de los clientes, ayudando a optimizar las futuras campañas.
<br></br>

11. **Participación de los Clientes en el Sitio Web de la Empresa:**
<br></br>

Métrica: Analiza el número promedio de visitas al sitio web por mes (NumWebVisitsMonth) y las tasas de respuesta para los clientes basadas en su participación en el sitio web.
Nos Ayuda a entender la correlación entre la participación en línea y las respuestas a las campañas, guiando las estrategias de marketing digital.
<br></br>

12. **Análisis de Cohortes:**
<br></br>

Métrica: Agrupa a los clientes según su fecha de inscripción (DtCustomer) y analizar sus respuestas a lo largo del tiempo.
Proporcionamos *insights* sobre la evolución del comportamiento del cliente, ayudando a comprender las tendencias de participación a largo plazo.
<br></br>

13. **Análisis de Ingresos y Costos:**
<br></br>

Métricas: Calcula los ingresos generados por cliente (Income) y analizar los costos de marketing para cada campaña.
Calculareos el ROI (*Return on Investment*) para cada campaña comparando los ingresos generados con los costos de marketing.
Ayuda a evaluar la rentabilidad de las campañas de marketing y a optimizar la asignación presupuestaria.

## 6. Resultado final

## 7. Referencias

* Louis T. Becker (Contributor) & Elyssa M. Gould (Column Editor) (2019) Microsoft Power BI: Extending Excel to Manipulate, Analyze, and Visualize Diverse Data, Serials Review, 45:3, 184-188, DOI: 10.1080/00987913.2019.1644891

* Michalczyk, S., Nadj, M., Azarfar, D., Maedche, A., & Gröger, C. (2020). A state-of-the-art overview and future research avenues of self-service business intelligence and analytics. https://www.researchgate.net/profile/Sven-Michalczyk/publication/341821870_A_State-Of-The-Art_Overview_and_Future_Research_Avenues_of_Self-Service_Business_Intelligence_and_Analytics/links/5eda115e92851c9c5e818a4f/A-State-Of-The-Art-Overview-and-Future-Research-Avenues-of-Self-Service-Business-Intelligence-and-Analytics.pdf

* Doko, F., & Miskovski, I. (2020). Advanced analytics of big data using power BI: credit registry use case. https://repository.ukim.mk/handle/20.500.12188/8266

* Lennerholt, C., Van Laere, J., & Söderström, E. (2018). Implementation challenges of self service business intelligence: A literature review. https://aisel.aisnet.org/hicss-51/os/org_issues_in_business_intelligence/4/


## 8. Apéndice

### 8.1 Fórmulas DAX utilizadas

### 8.2 PowerQuery

* **Extracción:** 

```javascript
let
    Origen = Excel.Workbook(File.Contents("C:\Users\javie\Documents\IBM_Datahack_PowerBI\Capstone_project\Dataset\marketing_campaign.xlsx"), null, true),
    Sheet1_Sheet = Origen{[Item="Sheet1",Kind="Sheet"]}[Data],
    #"Encabezados promovidos" = Table.PromoteHeaders(Sheet1_Sheet, [PromoteAllScalars=true]),
    #"Tipo cambiado" = Table.TransformColumnTypes(#"Encabezados promovidos",{{"ID", Int64.Type}, {"Year_Birth", Int64.Type}, {"Education", type text}, {"Marital_Status", type text}, {"Income", Int64.Type}, {"Kidhome", Int64.Type}, {"Teenhome", Int64.Type}, {"Dt_Customer", type date}, {"Recency", Int64.Type}, {"MntWines", Int64.Type}, {"MntFruits", Int64.Type}, {"MntMeatProducts", Int64.Type}, {"MntFishProducts", Int64.Type}, {"MntSweetProducts", Int64.Type}, {"MntGoldProds", Int64.Type}, {"NumDealsPurchases", Int64.Type}, {"NumWebPurchases", Int64.Type}, {"NumCatalogPurchases", Int64.Type}, {"NumStorePurchases", Int64.Type}, {"NumWebVisitsMonth", Int64.Type}, {"AcceptedCmp3", Int64.Type}, {"AcceptedCmp4", Int64.Type}, {"AcceptedCmp5", Int64.Type}, {"AcceptedCmp1", Int64.Type}, {"AcceptedCmp2", Int64.Type}, {"Complain", Int64.Type}, {"Z_CostContact", Int64.Type}, {"Z_Revenue", Int64.Type}, {"Response", Int64.Type}})
in
    #"Tipo cambiado"
```

