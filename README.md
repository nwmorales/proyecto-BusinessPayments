# proyecto-BusinessPayments

## Equipo del Proyecto

El equipo detrás de este análisis está compuesto por:
- Ivan López Tomàs
- Nelson Williams Morales

## Introducción al trabajo:
Este proyecto tiene como propósito analizar y obtener conclusiones clave a partir de datos relacionados con tarifas (`fees`) y solicitudes de adelanto de efectivo (`cash requests`) en Business Payments, para una empresa del sector financiero que ha contratado nuestros servicios. A través de este análisis, se pretende entender mejor el comportamiento de los usuarios, evaluar cómo diferentes tipos de tarifas afectan el uso del servicio y descubrir patrones que puedan apoyar la toma de decisiones estratégicas.

## Planificación

(FOTO)


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


# Fusionar los datasets usando cash_request_id from fees and cash_id from cash_request para vincularlos
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
merged_df.info()
````
##  Información del Dataset
![image](https://github.com/user-attachments/assets/d7f62b3a-c055-4356-b45c-8c5d37db82a5)

## Grafico de valores faltantes del Credit_Request y gráfico de valores nulos en el Fees:

![image](https://github.com/user-attachments/assets/057a2020-1fe6-48c6-b599-5c1b2788387f)

![image](https://github.com/user-attachments/assets/f8c37035-6985-4337-826f-cb280e04e14d)


## Grafico de valores faltantes del dataset usando un Left Join:
![newplot](https://github.com/user-attachments/assets/3ac799ec-5524-4a39-8aba-98a02853387f)


