# proyecto-BusinessPayments

## Equipo del Proyecto

El equipo detrás de este análisis está compuesto por:
- Ivan López Tomàs
- Nelson Williams Morales

## Introducción al trabajo:
Este proyecto tiene como propósito analizar y obtener conclusiones clave a partir de datos relacionados con tarifas (`fees`) y solicitudes de adelanto de efectivo (`cash requests`) en Business Payments, para una empresa del sector financiero que ha contratado nuestros servicios. A través de este análisis, se pretende entender mejor el comportamiento de los usuarios, evaluar cómo diferentes tipos de tarifas afectan el uso del servicio y descubrir patrones que puedan apoyar la toma de decisiones estratégicas.

## Planificación

![image](https://github.com/user-attachments/assets/247f999d-5a31-4c9f-ac16-eecb98a30af1)


## Objetivos del Análisis
Este estudio tiene como objetivo abordar preguntas clave sobre el uso y rendimiento de los servicios financieros de Business Payments.
Los objetivos principales son:
- Exploración y depuración de los datos para asegurar su calidad y consistencia.
- Identificación de patrones en el uso y la frecuencia de pagos para comprender el comportamiento de los usuarios.
- Análisis de cohortes para analizar la retención y el comportamiento de los usuarios a lo largo del tiempo.
- Desarrollo de un modelo de clasificación para identificar usuarios con alto riesgo de abandonar la plataforma.

## Estructura del conjunto de Datos
### 1. Solicitudes de Efectivo (*cash requests*)
Registra todas las transacciones en las que los usuarios solicitan adelantos de efectivo, permitiendo analizar tendencias de uso y posibles factores de riesgo.
- **Número de registros:** 23,970
- **Número de columnas:** 16
- **Enumeración de Campos y Posibles Valores:**  
  1. status:
     - Descripción: Estado del Cash Request (CR).
     - Posibles valores:
       
       ·  approved: Aprobado automáticamente o manualmente; los fondos se enviarán.
       
       ·  money_sent: Fondos transferidos al cliente; cambiará a active cuando el cliente los reciba.
       
       ·  rejected: CR revisado manualmente y rechazado.
       
       ·  pending: Pendiente de revisión manual.
       
       ·  transaction_declined: Error en la transferencia de fondos.
       
       ·  waiting_user_confirmation: El usuario debe confirmar la solicitud.
       
       ·  direct_debit_rejected: Débito directo SEPA rechazado.
       

       ·  canceled: Solicitud cancelada por falta de confirmación del usuario.

       ·  direct_debit_sent: Débito directo programado pero no confirmado.

       ·  waiting_reimbursement: Reembolso pendiente de selección de fecha.

       ·  active: Fondos recibidos en la cuenta del cliente.

       ·  money_back: CR reembolsado exitosamente.


  2. created_at:
     - Descripción: Fecha y hora de creación del CR.
     - Posibles valores: Marca de tiempo (ISO 8601).

  3. updated_at:
     - Descripción: Fecha y hora de la última actualización del CR.
     - Posibles valores: Marca de tiempo (ISO 8601).
  4. user_id:
     - Descripción: ID único del usuario que solicitó el CR.
     - Posibles valores: Identificador único.
  5. moderated_at
     - Descripción: Fecha y hora de la revisión manual (si aplica).
     - Posibles valores: Marca de tiempo (ISO 8601) o null.
  6. deleted_account_id
      - Descripción: ID de usuario anonimizado si la cuenta fue eliminada.
      - Posibles valores: Identificador único o null.
  7. reimbursement_date
      - Descripción: Fecha planificada para el reembolso.
      - Posibles valores: Fecha (ISO 8601).
  8. cash_request_debited_date
      - Descripción: Fecha del último débito directo observado (si aplica).
      - Posibles valores: Fecha (ISO 8601) o null.
  9. cash_request_received_date
      - Descripción: Fecha de recepción del CR por parte del cliente.
      - Posibles valores: Fecha (ISO 8601) o null.
  10. money_back_date
      - Descripción: Fecha en que el CR fue reembolsado completamente.
      - Posibles valores: Fecha (ISO 8601) o null.
  11. transfer_type
      - Descripción: Tipo de transferencia elegida por el usuario.
      - Posibles valores:
          · Instant
          · Regular
          · send_at
  12. send_at
      - Descripción: Fecha y hora de la transferencia de fondos.
      - Posibles valores: Marca de tiempo (ISO 8601) o null.
  13. recovery_status
      - Descripción: Estado del incidente de pago (si aplica).
      - Posibles valores:

        · null: Sin incidentes de pago.

        · completed: Incidente resuelto.

        · pending: Incidente abierto.

        · pending_direct_debit: Incidente abierto con débito directo lanzado.
  14. reco_creation
      - Descripción: Fecha y hora de creación del caso de recuperación.
      - Posibles valores: Marca de tiempo (ISO 8601) o null.
  15. reco_last_update
      - Descripción: Última fecha de actualización del caso de recuperación.
      - Posibles valores: Marca de tiempo (ISO 8601) o null.

### 2. Tarifas (*fees*)
Contiene información detallada sobre las tarifas aplicadas a los usuarios por diferentes conceptos, como pagos instantáneos, incidentes de reembolso fallidos y aplazamientos de pago.
- **Número de registros:** 21,061
- **Total de columnas:** 13
- **Principales columnas:**  
  - *id*: Identificador único de la tarifa.
  - *cash_request_id*: Identificador de la solicitud de efectivo relacionada.
  - *type*: Tipo de tarifa (`instant_payment`, `split_payment`, `incident`, `postpone`).
  - *status*: Estado de la tarifa (`confirmed`, `rejected`, `cancelled`, `accepted`).
  - *total_amount*: Monto total de la tarifa.
  - *created_at*: Fecha de creación de la tarifa.


### 3. Lexique (*Lexique - Data Analyst*)
Proporciona definiciones y referencias clave para comprender los términos utilizados en los datos.
- **Número de registros:** 16,375
- **Columnas:**  
  - *Column name:* Nombre del término en la base de datos.
  - *Description:* Descripción del término.


## Métricas para Analizar Rentabilidad Financiera de BP

1. **% Revenue**:
```math
\text{\% Revenue} = \frac{\text{ingresos por fees de adelanto + ingresos por fees de prorrogas}}{\text{total adelantos}} \times 100
```

2. **Porcentaje de Adelantos con Fee**:
```math
\text{Porcentaje de adelantos con fee} = \frac{\text{adelantos con fee}}{\text{total adelantos}} \times 100
```

## Métricas para Analizar Comportamiento de Clientes de BP

1. **Porcentaje de Clientes Repetitivos**:
```math
\text{Porcentaje de clientes repetitivos} = \frac{\text{Clientes repetitivos}}{\text{Total de Clientes}} \times 100
```

2. **Porcentaje de Nuevos Clientes que Pagan Fees**:
```math
\text{Porcentaje de nuevos clientes que pagan fees} = \frac{\text{Clientes nuevos que pagan fees}}{\text{Total de clientes nuevos}} \times 100
```

3. **Porcentaje de Clientes Repetitivos que Pagan Fees**:
```math
\text{Porcentaje de clientes repetitivos que pagan fees} = \frac{\text{Clientes repetitivos que pagan fees}}{\text{Total de clientes repetitivos}} \times 100
```

4. **Tasa de Incumplimiento de Nuevos Clientes**:
```math
\text{Tasa de incumplimiento de nuevos clientes} = \frac{\text{Clientes nuevos que no cumplen el plazo}}{\text{Total de clientes nuevos}} \times 100
```

5. **Tasa de Incumplimiento de Clientes Repetitivos**:
```math
\text{Tasa de incumplimiento de clientes repetitivos} = \frac{\text{Clientes repetitivos que no cumplen el plazo}}{\text{Total de clientes repetitivos}} \times 100
```


## EDA (análisis exploratorio de datos)

Cargar Archivos:

```python
import pandas as pd
import numpy as np
import seaborn as sns
import plotly.express as px
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import plotly.graph_objects as go
import itertools
from statsmodels.tsa.seasonal import seasonal_decompose
import warnings
import matplotlib.patches as mpatches
warnings.filterwarnings('ignore')

# Cargar los datasets
# Montar Google Drive: "drive.mount('/content/drive')"
# Cambiar al directorio donde se encuentra el dataset
dataset_path = '/content/drive/My Drive/ESPECIALITZACIÓ/IA/PROYECTOS/Proyecto2/Elementos'

# Cargar el dataset principal de solicitudes de adelanto en efectivo
credit_requests = pd.read_csv('/content/drive/MyDrive/ESPECIALITZACIÓ/IA/PROYECTOS/Proyecto2/Elementos/extract - cash request - data analyst.csv')
# Crear una copia del DataFrame para preservar el original
cr = credit_requests.copy()

# Cargar el dataset de tarifas o fees para análisis de cohortes de tarifas
fees = pd.read_csv('/content/drive/MyDrive/ESPECIALITZACIÓ/IA/PROYECTOS/Proyecto2/Elementos/extract - fees - data analyst - .csv')
# Crear una copia del DataFrame para preservar el original
fs = fees.copy()

Join (Left):

# Renombrar 'id' a 'cash_id' para mayor claridad (y para el merge posterior con fees)
cr.rename(columns={'id': 'cash_id'}, inplace=True)

# Ver NA en columna 'user_id'
cr[cr['user_id'].notna()].nunique() # 21867 válidos
cr[cr['user_id'].isna()].nunique() # 2103 Nan
# Comprobación: 21867 válidos + 2103 NaN = 23970 filas

# Comprobar que 'user_id' y 'deteled_account_id' son complementarios
cr[cr['user_id'].isna() & cr['deleted_account_id'].isna()] # Empty dataframe -> Siempre tenemos un valor válido en una de las dos columnas

# Comprobar solapamientos
cr[cr['user_id'].notna() & cr['deleted_account_id'].notna()] # La fila crid=280 tiene user_id=3161 y a la vez deleted_account_id=262

# Crear una nueva columna 'id_completo' a partir de user_id + deleted_account_id
cr.loc[:, 'id_completo'] = cr['user_id'].combine_first(cr['deleted_account_id'])
# Conversión de float a int
cr['id_completo'] = cr['id_completo'].astype(int)

# Definir los bins y las etiquetas para los segmentos de gasto
bins = [cr['amount'].min(), 50, 150, cr['amount'].max()]
labels = ['Low', 'Medium', 'High']

# Asignar cada usuario a un segmento de gasto
cr['spend_segment'] = pd.cut(cr['amount'], bins=bins, labels=labels, include_lowest=True)

# Crear diccionario para almacenar los id_completo por segmento
grouped_users = {
    'Low': cr.loc[cr['spend_segment'] == 'Low', 'id_completo'].tolist(),
    'Medium': cr.loc[cr['spend_segment'] == 'Medium', 'id_completo'].tolist(),
    'High': cr.loc[cr['spend_segment'] == 'High', 'id_completo'].tolist()
}

# Mostrar los resultados
print(grouped_users)


# Merge datasets using cash_request_id from fees and cash_id from cash_request
merged_df = cr.merge(fs, left_on='cash_id', right_on='cash_request_id', how='left')

# Drop redundant columns (ensure 'id_y' exists before dropping)
if 'id_y' in merged_df.columns:
    merged_df = merged_df.drop(columns=["id_y"])

# Rename columns for clarity
merged_df.rename(columns={"id_x": "fee_id"}, inplace=True)

#  Correct Column Naming
if 'created_at_y' in merged_df.columns:  # Prefer cash request date
    merged_df['created_at_cash_request'] = merged_df['created_at_y']
    merged_df = merged_df.drop(columns=["created_at_y"])
    if 'created_at_x' in merged_df.columns:  # Use fee date if cash request date is missing
        merged_df['created_at_fees'] = merged_df['created_at_x']
        merged_df = merged_df.drop(columns=["created_at_x"])
else:
    raise KeyError("Column 'created_at' not found in merged dataset")

#  Convert 'created_at_cash_request' to datetime
merged_df['created_at_cash_request'] = pd.to_datetime(merged_df['created_at_cash_request'], errors='coerce')

#  Create Month Column for Cash Request
merged_df['Month_cash_request'] = merged_df['created_at_cash_request'].dt.to_period('M')
merged_df['Month_cash_request'] = merged_df['Month_cash_request'].astype(str)  # Convert to string
merged_df['Month_cash_request'] = pd.to_datetime(merged_df['Month_cash_request'], format='%Y-%m')

merged_df['Month_Num_cash_request'] = ((merged_df['Month_cash_request'] - merged_df['Month_cash_request'].min()).dt.days // 30)

#  Convert 'created_at_fees' to datetime
merged_df['created_at_fees'] = pd.to_datetime(merged_df['created_at_fees'], errors='coerce')

#  Create Month Column for Fees
merged_df['Month_fees'] = merged_df['created_at_fees'].dt.to_period('M')
merged_df['Month_fees'] = merged_df['Month_fees'].astype(str)  # Convert to string
merged_df['Month_fees'] = pd.to_datetime(merged_df['Month_fees'], format='%Y-%m')

merged_df['Month_Num_fees'] = ((merged_df['Month_fees'] - merged_df['Month_fees'].min()).dt.days // 30)

## Ingenieria

# Si 'deleted_account_id' tiene un valor, el usuario eliminó su cuenta (1), si es NaN, sigue activo (0)
merged_df['is_deleted_user'] = merged_df['deleted_account_id'].notnull().astype(int)

# 1️⃣ Calcular 'total_purchases' (Cantidad total de solicitudes realizadas por usuario)
total_purchases_df = merged_df.groupby('id_completo').size().reset_index(name='total_purchases')
merged_df = merged_df.merge(total_purchases_df, on='id_completo', how='left')

# 3️⃣ Calcular 'rejection_rate' (Tasa de rechazos por usuario)
# Contar solicitudes rechazadas
rejected_requests_df = merged_df.groupby('id_completo')['status_x'].apply(lambda x: (x == 'rejected').sum()).reset_index()
rejected_requests_df.rename(columns={'status_x': 'rejected_requests'}, inplace=True)

# Calcular la tasa de rechazo
rejection_rate_df = total_purchases_df.merge(rejected_requests_df, on='id_completo', how='left')
rejection_rate_df['rejection_rate'] = rejection_rate_df['rejected_requests'] / rejection_rate_df['total_purchases']
rejection_rate_df['rejection_rate'] = rejection_rate_df['rejection_rate'].fillna(0)  # Evitar valores NaN


# Contar solicitudes canceladas por usuario
canceled_requests_df = merged_df.groupby('id_completo')['status_x'].apply(lambda x: (x == 'canceled').sum()).reset_index()
canceled_requests_df.rename(columns={'status_x': 'canceled_requests'}, inplace=True)

# Calcular la tasa de cancelación
canceled_rate_df = total_purchases_df.merge(canceled_requests_df, on='id_completo', how='left')

canceled_rate_df['canceled_rate'] = canceled_rate_df['canceled_requests'] / canceled_rate_df['total_purchases']
canceled_rate_df['cancelled_rate'] = canceled_rate_df['canceled_rate'].fillna(0)  # Evitar valores NaN

# Agregar la nueva columna al dataset
merged_df = merged_df.merge(canceled_rate_df[['id_completo', 'canceled_rate']], on='id_completo', how='left')

# Agregar las nuevas métricas al dataset
merged_df = merged_df.merge(rejection_rate_df[['id_completo', 'rejection_rate']], on='id_completo', how='left')


# Contar solicitudes aprobadas (Definiendo aprobadas como 'money_back' y 'direct_debit_sent')
approved_requests_df = merged_df.groupby('id_completo')['status_x'].apply(lambda x: ((x == 'money_back') | (x == 'direct_debit_sent')).sum()).reset_index()
approved_requests_df.rename(columns={'status_x': 'approved_requests'}, inplace=True)

# Calcular la tasa de aprobación
approval_rate_df = total_purchases_df.merge(approved_requests_df, on='id_completo', how='left')
approval_rate_df['approval_rate'] = approval_rate_df['approved_requests'] / approval_rate_df['total_purchases']
approval_rate_df['approval_rate'] = approval_rate_df['approval_rate'].fillna(0)  # Evitar valores NaN

# Agregar la nueva métrica al dataset
merged_df = merged_df.merge(approval_rate_df[['id_completo', 'approval_rate']], on='id_completo', how='left')

# Calcular user_lifetime solo con los datos disponibles
merged_df["user_lifetime"] = (merged_df["updated_at_x"] - merged_df["created_at_cash_request"]).dt.days

# Contar cuántos valores NaN quedaron en user_lifetime
num_nan_user_lifetime = merged_df["user_lifetime"].isna().sum()
num_nan_user_lifetime

spend_mapping = {'Low': 0, 'Medium': 1, 'High': 2}
merged_df['spend_segment_encoded'] = merged_df['spend_segment'].map(spend_mapping)

# Definir umbrales basados en datos reales
lifetime_threshold = merged_df["user_lifetime"].median()
purchase_threshold = merged_df["total_purchases"].median()
rejection_threshold = merged_df["rejection_rate"].quantile(0.75)

# Aplicar la nueva lógica de worth_retaining con umbrales dinámicos
merged_df["worth_retaining"] = (
    (merged_df["spend_segment_encoded"] > 0) &  # Usuario no está en la categoría más baja de gasto
    (merged_df["user_lifetime"] > lifetime_threshold) &  # Usuario ha estado al menos lo que marca la mediana
    (merged_df["total_purchases"] > purchase_threshold) &  # Usuario ha hecho más que la mediana de solicitudes
    (merged_df["rejection_rate"] < rejection_threshold)   # Tasa de rechazo menor al percentil 75
).astype(int)

#  Display the merged dataset info
merged_df.info()

````
##  Información del Dataset

![image](https://github.com/user-attachments/assets/e46951d3-fba7-422c-b387-9ae2fe32541d)

## Grafico de valores faltantes segun nuestra classificacion:

![image](https://github.com/user-attachments/assets/fdc6a922-7ab6-4519-a972-24474db8815e)


## Grafico de valores faltantes del dataset completo usando Left Join:

![newplot](https://github.com/user-attachments/assets/3ac799ec-5524-4a39-8aba-98a02853387f)


## Histogramas variables mas relevantes, separando por nuestro cohorte
### LOW
![image](https://github.com/user-attachments/assets/bf1b2dbf-1ac5-443a-806e-00d094186ab9)

### MEDIUM
![image](https://github.com/user-attachments/assets/c8dc09af-fabd-443a-b376-05afd778edd5)

### HIGH
![image](https://github.com/user-attachments/assets/7a9b73b1-39bd-4a60-a95a-412189436c77)

## Boxplot por segmentos
![image](https://github.com/user-attachments/assets/38d13944-003f-4cdd-8547-29dc4f4f6b03)
![image](https://github.com/user-attachments/assets/4f09c023-1280-4cfa-b5d1-f6d7d66fc8b6)
![image](https://github.com/user-attachments/assets/9c259056-4eeb-4e21-9e7b-fa34edf8e67a)

## Graficos de violin
![image](https://github.com/user-attachments/assets/f0f6936a-7be8-4cae-a93d-b0a206419a7e)
![image](https://github.com/user-attachments/assets/b251841f-931c-4c48-a122-7803349442d0)
![image](https://github.com/user-attachments/assets/6b6db2a4-ebe5-4dc3-bbf9-553add9ca110)

## Diferenciación por dispersion
![image](https://github.com/user-attachments/assets/f8dcb4ac-c256-4e9e-9842-2050cb93d5d8)


## Evolución por segmentos
![image](https://github.com/user-attachments/assets/6421f9ba-7d4c-4778-8a56-71d52d84d4cc)


## Matiz de correlacion de las variables
![image](https://github.com/user-attachments/assets/16939249-bce3-4653-811a-04d65858c315)

## Número de Solicitudes por Mes separado por segmentos
### LOW
![image](https://github.com/user-attachments/assets/ee56a638-6286-462d-a0bf-83f4cecfb37b)

### MEDIUM
![image](https://github.com/user-attachments/assets/a0d94fdc-ef2a-4579-b4ad-400cf8053c29)

### HIGH
![image](https://github.com/user-attachments/assets/6da4f96c-7f61-4847-af35-98e58d24c783)


## Graficas de violin para comparacion segmentadas
![image](https://github.com/user-attachments/assets/d37b89ce-1242-4577-976d-989624c7bab0)

![image](https://github.com/user-attachments/assets/ddbf0e33-97b6-456d-aa0d-2e11f4b50d69)

![image](https://github.com/user-attachments/assets/f10ee544-8c74-48e4-ab28-7b9768eaf794)


## Grafico de evolucion de status
![image](https://github.com/user-attachments/assets/db3b449f-b57e-485b-8843-29157a8669b6)

## Grafico de Distribución de Usuarios por Cohorte
![image](https://github.com/user-attachments/assets/d00ba26d-2cbb-44ca-a5a5-44630bca76fe)
![image](https://github.com/user-attachments/assets/4698b644-82af-4d1a-b61f-79c0cc873352)
![image](https://github.com/user-attachments/assets/6caff8cc-0a13-404d-bfb0-c6c53c85893a)

## Evolucion de frecuencia de solicitudes por cohorte
![image](https://github.com/user-attachments/assets/3dd08495-6da8-4b2e-bab3-6fb88d7828b0)
![image](https://github.com/user-attachments/assets/e8e50eb5-57a4-455f-b334-9b142e8c152c)
![image](https://github.com/user-attachments/assets/8693bcbf-c865-4951-b6c4-4fa801559093)


## Cantidad completa de adelanto de efectivo por cohorte y mes
![image](https://github.com/user-attachments/assets/86eee44e-72cb-463d-9b68-78a6c92db552)

## HEAT MAPS POR COHORTE (Low, Medium, High) Combinados
![image](https://github.com/user-attachments/assets/a236f1ba-72e7-40de-876a-3c60db3c7202)

![image](https://github.com/user-attachments/assets/7b9997e7-7885-4c4a-8845-6ffc18b23194)

![image](https://github.com/user-attachments/assets/07a51e34-c3a5-434c-ab6b-ca2a4ac5ab97)

## HEAT MAPS POR COHORTE (Low, Medium, High) Activos
![image](https://github.com/user-attachments/assets/9e39c3b9-902c-4ac9-abe0-64f262042323)

![image](https://github.com/user-attachments/assets/9619eb34-7e98-4cc3-95a9-2b8998f98881)

![image](https://github.com/user-attachments/assets/02a5015a-590b-4839-b773-413350f68bfe)

## HEAT MAPS POR COHORTE (Low, Medium, High) Eliminados

![image](https://github.com/user-attachments/assets/a8cf598a-ebb7-45eb-b86e-417d84716fac)

![image](https://github.com/user-attachments/assets/b3b5311f-070d-479b-b24b-187d53bfdb73)

![image](https://github.com/user-attachments/assets/05918907-76d5-4664-8e87-49f87bf7adb7)


## Matriz de confusion de datos acumulados Datos Combinados

![image](https://github.com/user-attachments/assets/30e39ec6-343c-47b1-a7e0-ac2903abf251)

![image](https://github.com/user-attachments/assets/d97ec4bb-11db-46a1-a989-115b62a24cd5)

![image](https://github.com/user-attachments/assets/63b61ea4-8f97-499c-a8c4-cef5d5b3e017)

## Matriz de confusion de datos acumulados Usuarios activos
![image](https://github.com/user-attachments/assets/433669c4-bebd-4f9f-95b2-734edcb88671)

![image](https://github.com/user-attachments/assets/727b3527-6a07-473c-831d-18828f0004d3)

![image](https://github.com/user-attachments/assets/9f374aa7-62fe-4f89-8016-ccf8cf52353a)

## Matriz de confusion de datos acumulados Usuarios eliminados
![image](https://github.com/user-attachments/assets/85204f13-e837-40b8-8d9c-f2e28042a438)

![image](https://github.com/user-attachments/assets/23311ebc-0223-47cb-a3f9-345598a8c73e)

![image](https://github.com/user-attachments/assets/0f6dbf8a-6321-4058-8eb9-60f5f756fb71)

  
# Nuestros Modelos de regressión

### Primeros modelos
Usamos primeramente datos acumulados segun el cohorte, para comporvar si funcionaba el codigo
![image](https://github.com/user-attachments/assets/12b6c697-611d-4f7e-a879-acde8bac2ecc)
![image](https://github.com/user-attachments/assets/6ffb65a7-b350-4a49-9968-23c8b71e6d78)
![image](https://github.com/user-attachments/assets/730547b4-6daf-43ca-81d1-98b0aba7e4c8)

### Una vez vimos que funcionaba, tratamos de realizar el modelo completo con los datos normales
![image](https://github.com/user-attachments/assets/b5f5f812-3c19-450b-8fd9-e7abd6a80793)
![image](https://github.com/user-attachments/assets/8fb66c43-8206-4917-b5ca-04b149fb5093)
![image](https://github.com/user-attachments/assets/bd450bbe-221e-465d-b1ff-d1dba479f979)
![image](https://github.com/user-attachments/assets/b27dbf6b-a9ad-4ebe-b992-91b1221674d4)
![image](https://github.com/user-attachments/assets/0718fa04-e342-4afa-809b-48f2d06c9a23)
![image](https://github.com/user-attachments/assets/4c91f3b3-b711-426b-a1c3-32b13a9c4d6a)
![image](https://github.com/user-attachments/assets/90796515-e315-40af-8e85-905d6eed0e04)
![image](https://github.com/user-attachments/assets/f928a636-723d-4366-a393-2ac0ab1f84c9)


### Pero al observar que no obteniamos ninguna prediccion valida, optamos por realizar una estandarizacion de los datos y aplicar modelos de Lasso y Ridge:
![image](https://github.com/user-attachments/assets/50a5a85b-706e-4fa4-a74b-b7921d198582)
![image](https://github.com/user-attachments/assets/5137e27a-15c2-4c4d-9ba5-fd45f76eef68)
![image](https://github.com/user-attachments/assets/a0bd7ef0-0905-418d-bd3b-aa71877c6393)      ![image](https://github.com/user-attachments/assets/8f927938-54ee-426f-9422-29d191a927f9)
![image](https://github.com/user-attachments/assets/df39e474-1cf3-4894-a693-77b9d66136cf)

### Posterior a esto, decidimos realizar únicamente el apartado de Interpolacion

![image](https://github.com/user-attachments/assets/29971059-c13f-4e03-8358-40264e49c792)
![image](https://github.com/user-attachments/assets/a604ee01-8cd1-4173-9000-405c80112266)
![image](https://github.com/user-attachments/assets/0212c167-2825-4544-a851-7cee5fabf73f)
![image](https://github.com/user-attachments/assets/7a325a79-0b08-4f4c-9c45-5264719d1dab)

### Pero finalmente decidimos volver a intentar sacar modelos predictivos realizando la extrapolacion:
![image](https://github.com/user-attachments/assets/f4a2e596-06b6-4b7b-b223-26f91a708220)
![image](https://github.com/user-attachments/assets/35d43da6-3580-41f1-b27f-f5636441e6c8)
![image](https://github.com/user-attachments/assets/f9b33142-79cd-4bb7-b657-8a7f189f8e41)
![image](https://github.com/user-attachments/assets/71e0d7c9-3633-4720-aaba-ff512cf3cb93)
![image](https://github.com/user-attachments/assets/38a80d16-fa2f-49d8-843e-6c92894ec132)

### Finalmente, y teniendo en cuenta modelos que pudimos observar de otros compañeros, este fue el mejor modelo que obtuvimos
![image](https://github.com/user-attachments/assets/c2a6a073-0134-47e6-83c8-282924ce72ca)


# Modelos de clasificación
📌 1. Definición de Variables Objetivo
* is_deleted_user: Identificaba si un usuario eliminaba su cuenta.
* worth_retaining: Indicaba si un usuario era valioso para la retención.
  
📌 2. Selección de Variables Predictoras
+ approval_rate: Tasa de aprobación de transacciones.
* user_lifetime: Tiempo de vida del usuario en la plataforma.
* rejection_rate: Tasa de rechazo de transacciones.
* total_purchases: Cantidad total de solicitudes realizadas.
* spend_segment_encoded: Segmento de gasto del usuario (codificado numéricamente)

![image](https://github.com/user-attachments/assets/78b6becf-9258-4032-8582-e49c1cae2b24)

📌 1. Variables Más Relevantes para is_deleted_user (Usuarios que eliminan su cuenta) Variable Correlación con is_deleted_user Interpretación approval_rate -0.21 A mayor tasa de aprobación, menor probabilidad de eliminar la cuenta. rejection_rate 0.27 A mayor tasa de rechazo, mayor probabilidad de eliminar la cuenta. user_lifetime -0.09 Usuarios con más tiempo en la plataforma tienen menor probabilidad de irse.

🔹 Conclusión: approval_rate y rejection_rate son los mejores predictores de abandono. user_lifetime influye pero no tan fuertemente.

📌 2. Variables Más Relevantes para worth_retaining (Usuarios valiosos) Variable Correlación con worth_retaining Interpretación user_lifetime 0.52 Usuarios con más tiempo en la plataforma son más valiosos. total_purchases 0.38 Usuarios con más compras tienen mayor probabilidad de ser retenidos. approval_rate 0.24 A mayor tasa de aprobación, más probable que el usuario sea valioso. spend_segment_encoded 0.34 Usuarios con mayor nivel de gasto tienden a ser más valiosos.

🔹 Conclusión: user_lifetime, total_purchases, y spend_segment_encoded son esenciales para predecir worth_retaining. approval_rate refuerza el perfil de usuario valioso.


Preparamos los datos para los modelos de clasificacion

```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

# 📌 Para is_deleted_user (Usuarios que eliminan su cuenta)
X_deleted = merged_df[['approval_rate', 'user_lifetime', 'rejection_rate']]
y_deleted = merged_df['is_deleted_user']

# 📌 Para worth_retaining (Usuarios valiosos para retención)
X_retaining = merged_df[['approval_rate', 'user_lifetime', 'total_purchases', 'spend_segment_encoded']]
y_retaining = merged_df['worth_retaining']

# Asegurar que spend_segment_encoded sea numérico
X_retaining['spend_segment_encoded'] = pd.to_numeric(X_retaining['spend_segment_encoded'], errors='coerce')

## 📌 Ejecución del PASO 2 (Train/Test Split)

# Nueva división para `is_deleted_user`
X_train_deleted, X_test_deleted, y_train_deleted, y_test_deleted = train_test_split(
    X_deleted, y_deleted, test_size=0.2, random_state=42
)

# Nueva división para `worth_retaining`
X_train_retaining, X_test_retaining, y_train_retaining, y_test_retaining = train_test_split(
    X_retaining, y_retaining, test_size=0.2, random_state=42
)

# Verificar formas
print("Nuevos tamaños de los conjuntos:")
print("X_train_deleted:", X_train_deleted.shape, "X_test_deleted:", X_test_deleted.shape)
print("X_train_retaining:", X_train_retaining.shape, "X_test_retaining:", X_test_retaining.shape)
````
Nuevos tamaños de los conjuntos:

X_train_deleted: (25675, 3) X_test_deleted: (6419, 3)

X_train_retaining: (25675, 4) X_test_retaining: (6419, 4)

## Modelos usados: SVM / KNN / Árbol de Decisión
### SVM (sin ajustar)
```python
from sklearn.preprocessing import StandardScaler

# Inicializar el escalador
scaler = StandardScaler()

# Escalar los datos para is_deleted_user
X_train_deleted_scaled = scaler.fit_transform(X_train_deleted)
X_test_deleted_scaled = scaler.transform(X_test_deleted)

# Escalar los datos para worth_retaining
X_train_retaining_scaled = scaler.fit_transform(X_train_retaining)
X_test_retaining_scaled = scaler.transform(X_test_retaining)

from sklearn.svm import SVC
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# 1️⃣ Entrenar SVM para `is_deleted_user`
svm_deleted = SVC(kernel='linear', C=1, random_state=42)
svm_deleted.fit(X_train_deleted_scaled, y_train_deleted)

# Predicción en test
y_pred_deleted_svm = svm_deleted.predict(X_test_deleted_scaled)

# Evaluación del modelo
print("🔹 SVM - is_deleted_user")
print("Accuracy:", accuracy_score(y_test_deleted, y_pred_deleted_svm))
print("Matriz de Confusión:\n", confusion_matrix(y_test_deleted, y_pred_deleted_svm))
print("Reporte de Clasificación:\n", classification_report(y_test_deleted, y_pred_deleted_svm))

# 2️⃣ Entrenar SVM para `worth_retaining`
svm_retaining = SVC(kernel='linear', C=1, random_state=42)
svm_retaining.fit(X_train_retaining_scaled, y_train_retaining)

# Predicción en test
y_pred_retaining_svm = svm_retaining.predict(X_test_retaining_scaled)

# Evaluación del modelo
print("\n🔹 SVM - worth_retaining")
print("Accuracy:", accuracy_score(y_test_retaining, y_pred_retaining_svm))
print("Matriz de Confusión:\n", confusion_matrix(y_test_retaining, y_pred_retaining_svm))
print("Reporte de Clasificación:\n", classification_report(y_test_retaining, y_pred_retaining_svm))
````
![image](https://github.com/user-attachments/assets/d2e5eeb9-e0d2-4b9c-8170-f3647150da5f)

### SVM (ajustado)
📌 Se ha aplicado el cambio del kernel a RBF en ambos objetivos, se ha cambiado el class_weight a 'balanced' pero solo en el objetivo 'is_deleted_user', se ha aplicado el "GridSearchSV' para el 'is_deleted_user' y por ultimo se probo distintos umbrales.

![image](https://github.com/user-attachments/assets/29b1e3ef-420f-4195-886a-80a7b96ac0b1)  
![image](https://github.com/user-attachments/assets/dacd701c-e797-41ba-92a0-250774cb97e4)

### KNN (sin ajustar)
```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# 1️⃣ Entrenar KNN para `is_deleted_user`
knn_deleted = KNeighborsClassifier(n_neighbors=5)
knn_deleted.fit(X_train_deleted_scaled, y_train_deleted)

# Predicción en test
y_pred_deleted_knn = knn_deleted.predict(X_test_deleted_scaled)

# Evaluación del modelo
print("🔹 KNN - is_deleted_user")
print("Accuracy:", accuracy_score(y_test_deleted, y_pred_deleted_knn))
print("Matriz de Confusión:\n", confusion_matrix(y_test_deleted, y_pred_deleted_knn))
print("Reporte de Clasificación:\n", classification_report(y_test_deleted, y_pred_deleted_knn))

# 2️⃣ Entrenar KNN para `worth_retaining`
knn_retaining = KNeighborsClassifier(n_neighbors=5)
knn_retaining.fit(X_train_retaining_scaled, y_train_retaining)

# Predicción en test
y_pred_retaining_knn = knn_retaining.predict(X_test_retaining_scaled)

# Evaluación del modelo
print("\n🔹 KNN - worth_retaining")
print("Accuracy:", accuracy_score(y_test_retaining, y_pred_retaining_knn))
print("Matriz de Confusión:\n", confusion_matrix(y_test_retaining, y_pred_retaining_knn))
print("Reporte de Clasificación:\n", classification_report(y_test_retaining, y_pred_retaining_knn))
````
![image](https://github.com/user-attachments/assets/60ae815a-e3fe-4df9-813d-8175ceb232ef)

### KNN (ajustado)
📌 Se ha probado con distintos valores para 'k' y aplicado el 'GridSearchCV' para ambos objetivos y por ultimo se ha aplicado 'weights='distance' para 'is_deleted_user'

![image](https://github.com/user-attachments/assets/431ca767-958f-4ac6-abf7-d306378d5643)

### Árbol de Decisión (sin ajustar)
```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# 1️⃣ Reentrenar Árbol de Decisión para `is_deleted_user`
optimized_tree_deleted = DecisionTreeClassifier(max_depth=6, min_samples_split=10, min_samples_leaf=5, random_state=42)
optimized_tree_deleted.fit(X_train_deleted, y_train_deleted)

# Predicción
y_pred_deleted_opt = optimized_tree_deleted.predict(X_test_deleted)

# Evaluación del modelo optimizado
print("🔹 Árbol de Decisión Optimizado (Nuevas Features) - is_deleted_user")
print("Accuracy:", accuracy_score(y_test_deleted, y_pred_deleted_opt))
print("Matriz de Confusión:\n", confusion_matrix(y_test_deleted, y_pred_deleted_opt))
print("Reporte de Clasificación:\n", classification_report(y_test_deleted, y_pred_deleted_opt))

# 2️⃣ Reentrenar Árbol de Decisión para `worth_retaining`
optimized_tree_retaining = DecisionTreeClassifier(max_depth=6, min_samples_split=10, min_samples_leaf=5, random_state=42)
optimized_tree_retaining.fit(X_train_retaining, y_train_retaining)

# Predicción
y_pred_retaining_opt = optimized_tree_retaining.predict(X_test_retaining)

# Evaluación del modelo optimizado
print("\n🔹 Árbol de Decisión Optimizado (Nuevas Features) - worth_retaining")
print("Accuracy:", accuracy_score(y_test_retaining, y_pred_retaining_opt))
print("Matriz de Confusión:\n", confusion_matrix(y_test_retaining, y_pred_retaining_opt))
print("Reporte de Clasificación:\n", classification_report(y_test_retaining, y_pred_retaining_opt))
````
![image](https://github.com/user-attachments/assets/099b2d01-00a8-4a89-a2cf-ba094400427e)

### Árbol de Decisión (ajustado)
📌 Se ha aplicado 'GridSearchCV' para 'is_deleted_user'

![image](https://github.com/user-attachments/assets/412e41a6-5e64-4eb2-a6ca-3c96c182ea2c)

### Comparación
![image](https://github.com/user-attachments/assets/49ce8177-247f-4b5d-bfb0-dd2833d9c78c)
📌 Comparación de Modelos

Tabla de Métricas
* SVM tiene el mejor Recall (1) = 0.72, lo que significa que detecta mejor a los usuarios eliminados.
* KNN tiene la mejor Precisión (1) = 0.39, pero su Recall es muy bajo (0.21), lo que indica que no detecta bien a los usuarios eliminados.
* Árbol de Decisión tiene un buen Recall (0.79), pero su AUC es el más bajo.

Curva ROC
* SVM (AUC = 0.80) tiene la mejor área bajo la curva, lo que indica que es el modelo con mejor capacidad predictiva.
* Árbol de Decisión (AUC = 0.60) es bajo, lo que sugiere que no separa bien las clases.
* KNN (AUC = 0.59) está casi en la línea de azar, lo que indica un rendimiento pobre en este problema.

📢 El modelo SVM es el mejor para is_deleted_user porque tiene: ✔️ Mayor AUC (0.80) → Mejor capacidad de clasificación.

✔️ Buen Recall (0.72) → Detecta bien a los usuarios que eliminan su cuenta.

📢 KNN y Árbol de Decisión no son recomendables porque tienen un AUC muy bajo y detectan mal la clase positiva.

![image](https://github.com/user-attachments/assets/765ea32b-c184-4874-91ae-9955d6dcd786)

📌 Observaciones:
* Todos los modelos tienen una precisión y un recall muy altos → lo que indica que todos son buenos para predecir si un usuario es valioso para retención.
* El Árbol de Decisión tiene la mejor combinación de Precision y Recall (0.98 - 0.99 - 0.99).
* El SVM y KNN también tienen muy buen rendimiento, con valores casi idénticos.

🔹 Curva ROC
*  El SVM tiene el mejor AUC (1.00), lo que indica una separación perfecta entre clases.
*  KNN también tiene un AUC muy alto (0.99), lo que significa que también es un excelente modelo.
*  Árbol de Decisión tiene un AUC de 0.50, lo que indica que su desempeño es aleatorio para este problema (lo que es extraño dado su accuracy alto 🤔).

📢 🔹 La mejor opción es SVM porque: ✔️ AUC = 1.00 → Máxima capacidad de clasificación.
*  Recall (1) = 0.99 → Detecta casi todos los usuarios valiosos.
*  Accuracy alto (0.99+) → Clasificación muy precisa.

* 🔹 KNN también es una opción válida, pero tiene un AUC ligeramente menor.
* 🔹 El Árbol de Decisión no es confiable en este caso debido a su AUC de 0.50.

![image](https://github.com/user-attachments/assets/93c9bb75-f711-41b5-a7d6-7d26b837e2a2)
![image](https://github.com/user-attachments/assets/66dd0005-3a56-4ff2-b5b4-d39cfa225ef8)
![image](https://github.com/user-attachments/assets/9e3d3e9e-5de7-4f8e-8683-54e20d134dcc)
📌 is_deleted_user:
* approval_rate y rejection_rate son los factores más influyentes.
* A menor tasa de aprobación (approval_rate), mayor probabilidad de eliminar la cuenta.
* A mayor tasa de rechazo (rejection_rate), más propenso a eliminar la cuenta.
* user_lifetime tiene menor impacto, pero sigue influyendo.

📌 worth_retaining:
* total_purchases es el factor más influyente. Más compras → Más probable que sea valioso.
* user_lifetime también es clave: Usuarios con más tiempo tienen más valor.
* spend_segment_encoded muestra que los clientes con mayor nivel de gasto tienen más probabilidades de ser valiosos.
* approval_rate tiene menor impacto, pero sigue siendo relevante.

### Predicciones
```python
# 🔹 1️⃣ Generar Predicciones para `is_deleted_user`
y_pred_deleted_final = best_svm_deleted.predict(X_test_deleted_scaled)

# 🔹 2️⃣ Generar Predicciones para `worth_retaining`
y_pred_retaining_final = svm_retaining_rbf.predict(X_test_retaining_scaled)

# Crear un DataFrame con los resultados
df_predictions = pd.DataFrame({
    "id_usuario": y_test_deleted.index,  # Ajusta según tu dataset
    "is_deleted_user (Predicho)": y_pred_deleted_final,
    "is_deleted_user (Real)": y_test_deleted.values,
    "worth_retaining (Predicho)": y_pred_retaining_final,
    "worth_retaining (Real)": y_test_retaining.values
})

# Mostrar primeras filas de las predicciones
print("\n🔹 Predicciones Finales:")
df_predictions.head(20)
````

![image](https://github.com/user-attachments/assets/74eea0ad-3e4e-4ef8-9b16-091fb2e03ff0)
