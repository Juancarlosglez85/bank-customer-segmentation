# Segmentación Socioeconómica de Clientes Bancarios (Clustering)

Proyecto de portafolio de Data Science: segmentación de clientes de banca
mediante clustering no supervisado (K-Means), a partir de datos
transaccionales reales (anonimizados).

## Objetivo

Identificar grupos de clientes con perfiles socioeconómicos
diferenciados (edad, saldo de cuenta, importe de transacción) que
permitan diseñar estrategias comerciales específicas por segmento.

## Dataset

[Bank Customer Segmentation](https://www.kaggle.com/datasets/shivamb/bank-customer-segmentation)
(Kaggle, Shivam Bansal) — más de 1M de transacciones bancarias de un
banco indio, agosto-octubre 2016. El dataset no se incluye en este
repositorio por su tamaño; descárgalo desde el enlace y colócalo en
`data/` (no versionada) para reproducir el análisis.

## Metodología (CRISP-DM)

1. **Business Understanding**: exploración de qué segmentación tiene
   sentido de negocio dado el tipo de datos disponibles (transaccional
   puro, sin variables de vinculación bancaria).
2. **EDA**: análisis de calidad de datos — valores en cero, nulos,
   detección de un valor centinela (`1/1/1800`) en `CustomerDOB`
   afectando al 5,5% de las filas, y del sesgo extremo en saldo/importe.
3. **Data Cleaning**: eliminación de identidad no capturada, corrección
   del pivote de año en fechas de nacimiento, conversión de tipos.
4. **Feature Engineering**: cálculo de edad, tramos de saldo/gasto,
   agrupación de ciudades (top 20 + "OTRAS"), winsorización al P99 de
   saldo e importe para uso exclusivo del modelo.
5. **Modelado**: K-Means (k=4, elegido por elbow curve) sobre edad,
   saldo y gasto escalados con `RobustScaler`.
6. **Perfilado y visualización**: ficha de cliente por cluster,
   validación visual con boxplots.

## Resultado — 4 segmentos identificados

| Cluster | % clientes | Perfil | Estrategia propuesta |
|---|---|---|---|
| 0 | 88,9% | Retail de masa (edad 28, saldo bajo) | Fidelización + ahorro |
| 1 | 2,0% | Alto patrimonio (saldo ~1,4M) | Banca privada |
| 2 | 2,8% | Gasto puntual alto (~12,5K) | Medios de pago digitales |
| 3 | 6,2% | Medio-alto | Inversión + tarjeta de crédito |

## Limitaciones

- El dataset es puramente transaccional (sin nº de productos,
  antigüedad, préstamos ni uso de canales) → segmentación de **perfil
  socioeconómico aproximado**, no de vinculación bancaria real.
- El 83,7% de los clientes tiene una única transacción en la ventana de
  2,5 meses del dataset → las variables de saldo/gasto son una
  fotografía puntual, no un patrón de comportamiento confirmado.
- No hay datos de riesgo/impago; cualquier valoración de riesgo por
  perfil es una hipótesis razonada, no un dato verificado.

## Sobre el modelo entrenado

Este proyecto es un análisis exploratorio de una base histórica fija, no
un sistema en producción. No se ha construido un pipeline de inferencia
para clientes nuevos, ya que el dataset no representa un flujo continuo
de datos. Se guardan igualmente el `scaler` (RobustScaler) y el modelo
`KMeans` entrenados en `outputs/` como demostración de buena práctica de
guardado de artefactos.

## Estructura del repositorio

```
bank-customer-segmentation/
├── notebooks/
│   └── clustering_socioeconomico.ipynb
├── outputs/
│   ├── scaler_robusto.pkl
│   └── kmeans_k4.pkl
├── README.md
└── requirements.txt
```
