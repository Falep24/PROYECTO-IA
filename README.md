# Predicción de Casos Semanales de Dengue en Piura (2000–2025)

Proyecto de Ciencia de Datos e Inteligencia Artificial orientado a la predicción de casos confirmados semanales de dengue en el departamento de **Piura (Perú)** mediante **Redes Neuronales Convolucionales 1D (CNN-1D)**, integrando registros epidemiológicos, variables meteorológicas locales y anomalías oceanográficas del Fenómeno El Niño (región Niño 1+2).

## Estructura del Repositorio

```text
PROYECTO-IA/
├── Datasets_PNDA/
│   └── datos_abiertos_vigilancia_dengue_2000_2025_0.csv  # Dataset crudo MINSA (ignorado en Git por tamaño >100 MB)
├── Preprocesamiento/
│   ├── casos_dengue_minsa.ipynb                          # ETL de casos confirmados en Piura (MINSA)
│   ├── datos_metereologicos_open_meteo.ipynb             # Extracción climática diaria/semanal (Open-Meteo - Estación Miraflores UNP)
│   ├── temperatura_superficial_mar_peru.ipynb            # Extracción de TSM y anomalía Niño 1+2 (NOAA / ENFEN)
│   └── integracion_datasets.ipynb                        # Ensamblaje e imputación del DataMart maestro semanal
├── Datasets/
│   ├── casos_dengue_piura_2000_2025_semanal.csv          # Serie semanal de casos confirmados (Piura)
│   ├── clima_piura_2000_2025_semanal.csv                 # Precipitación acumulada y temperaturas semanales
│   ├── temperatura_superficial_mar_piura_2000_2025_semanal.csv # TSM y anomalía semanal Niño 1+2
│   └── df_final.csv                                      # Copia de respaldo del dataset integrado
├── data/
│   └── df_final.csv                                      # Dataset maestro integrado listo para modelado
├── notebooks/
│   └── SimulacionDeTensorDengue.ipynb                    # Transformación a tensor 3D (ventana deslizante) y arquitectura CNN-1D
├── .gitignore
├── requirements.txt
└── README.md
```

## Fuentes de Datos

1. **Vigilancia Epidemiológica (MINSA - PNDA)**: [Plataforma Nacional de Datos Abiertos](https://www.datosabiertos.gob.pe/dataset/vigilancia-epidemiol%C3%B3gica-de-dengue). Filtrado para el departamento de `PIURA` y casos confirmados (`tipo_dx == 'C'`).
2. **Meteorología Local (Open-Meteo Archive API)**: Coordenadas de la Estación Miraflores — Universidad Nacional de Piura (`-5.1783, -80.6169`), agregada por **semana epidemiológica (domingo a sábado)**.
3. **Oceanografía (NOAA CPC)**: Índice semanal `wksst9120.for` para el cuadrante oceánico **Niño 1+2** (`0–10°S, 90°W–80°W`), alineado a **semana epidemiológica**.

## Instalación y Reproducibilidad

1. Crear y activar un entorno virtual (opcional pero recomendado):
   ```bash
   python -m venv .venv
   .venv\Scripts\activate
   ```
2. Instalar las dependencias:
   ```bash
   pip install -r requirements.txt
   ```
3. Descargar el archivo crudo de vigilancia de dengue desde la [Plataforma Nacional de Datos Abiertos](https://www.datosabiertos.gob.pe/dataset/vigilancia-epidemiol%C3%B3gica-de-dengue) y ubicarlo en:
   ```text
   Datasets_PNDA/datos_abiertos_vigilancia_dengue_2000_2025_0.csv
   ```
4. Ejecutar en orden los cuadernos de `Preprocesamiento/`:
   1. `casos_dengue_minsa.ipynb`
   2. `datos_metereologicos_open_meteo.ipynb`
   3. `temperatura_superficial_mar_peru.ipynb`
   4. `integracion_datasets.ipynb`
5. Ejecutar el cuaderno de modelado en `notebooks/SimulacionDeTensorDengue.ipynb`.
