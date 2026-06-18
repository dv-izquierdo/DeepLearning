# Análisis de Reputación Online — Dyson vs. Competencia

Análisis de sentimiento y modelado de topics sobre reseñas de Trustpilot para comparar la percepción online de **Dyson** frente al resto de empresas del sector *Electronics & Technology*, con el objetivo de identificar brechas frente a la competencia y proponer acciones concretas al equipo de negocio.

## Descripción del proyecto

El proyecto parte de un dataset de reseñas extraidas de Trustpilot y construye un pipeline completo de NLP para responder a tres preguntas: ¿qué opinan los usuarios de Dyson en comparación con el resto de marcas del sector?, ¿de qué hablan esas reseñas (servicio o producto)? y ¿en qué combinación de ambas dimensiones se concentra la brecha de percepción? El resultado es un notebook reproducible y una presentación ejecutiva con los hallazgos y las recomendaciones derivadas.

## Objetivos

- Extraer y limpiar las reseñas para dejarlas listas para el análisis (texto, emojis, duplicados, stopwords).
- Clasificar el sentimiento global de cada reseña (positivo / negativo) con un modelo de NLP.
- Descubrir los principales topics de conversación mediante modelado no supervisado.
- Medir cómo varía el sentimiento según el topic tratado.
- Comparar los resultados de Dyson frente al resto de empresas del sector.
- Traducir los hallazgos en conclusiones y próximos pasos para el equipo de negocio.

## Datos

- **Fuente:** [Trustpilot](https://www.trustpilot.com/) (CSV con 123.181 reseñas de múltiples sectores y empresas).
- **Filtro aplicado:** sector `Electronics & Technology` → 5.596 reseñas, de las cuales ~100 son de Dyson y ~5.496 del resto de la competencia.
- El fichero `trustpilot-reviews-123k.csv` no se incluye en este repositorio por su tamaño; debe colocarse en la raíz del proyecto (o ajustar la ruta en el notebook) antes de ejecutar el análisis.

## Metodología

1. **Limpieza de texto** — eliminación de URLs, emojis, caracteres especiales, números y stopwords (`re`, `nltk`).
2. **Análisis de sentimiento** — clasificación binaria (Positivo / Negativo) con el modelo multilingüe [`nlptown/bert-base-multilingual-uncased-sentiment`](https://huggingface.co/nlptown/bert-base-multilingual-uncased-sentiment) vía `transformers`.
3. **Modelado de topics** — `BERTopic` con clustering `HDBSCAN` sobre el texto limpio, ejecutado por separado para Dyson y para la competencia.
4. **Consolidación en categorías de negocio** — los topics descubiertos se agrupan en dos categorías interpretables: `Servicio` (atención al cliente, entregas, postventa) y `Producto` (características, baterías, piezas, fiabilidad).
5. **Cruce sentimiento × categoría** — combinación de las dos dimensiones anteriores para localizar dónde se concentra exactamente la negatividad.

## Resultados principales

| Métrica | Dyson | Competencia |
|---|---|---|
| Reseñas analizadas | ~100 | ~5.496 |
| % Positivo (sentimiento global) | 28,0% | 45,5% |
| % Negativo (sentimiento global) | 72,0% | 54,5% |
| % Servicio (de las reseñas) | 75,0% | 63,8% |
| % Producto (de las reseñas) | 25,0% | 36,2% |
| % Negativo dentro de Servicio | 73,3% | 56,4% |
| % Negativo dentro de Producto | 68,0% | 51,3% |

Dyson registra una proporción de reseñas negativas muy superior a la del resto del sector, y esa brecha no se concentra en un único frente: es prácticamente igual de amplia en Servicio (+17 puntos frente a la competencia) que en Producto (+17 puntos), lo que apunta a un problema de percepción generalizado y no a un foco aislado. Además, el propio modelo de topics identifica entre la competencia a **Shark** (aspiradoras, cepillos y secadores de pelo) como una marca directamente comparable, útil como referencia de benchmark.

## Estructura del repositorio

```
.
├── analisis_empresa.ipynb           # Notebook con el pipeline completo (limpieza, sentimiento, topics, visualizaciones)
├── Dyson_Analisis_Reputacion.pptx   # Presentación ejecutiva con los resultados y próximos pasos
├── data/
│   └── trustpilot-reviews-123k.csv  # Dataset de entrada (no incluido, ver sección "Datos")
├── requirements.txt
└── README.md
```

## Instalación

```bash
git clone <url-del-repositorio>
cd <nombre-del-repositorio>
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Dependencias principales (`requirements.txt`):

```
pandas
numpy
torch
transformers
bertopic
hdbscan
nltk
wordcloud
matplotlib
plotly
kaleido        # necesario para exportar los gráficos de Plotly como imagen
scikit-learn
datasets
```

> El modelo de sentimiento y el de topics se descargan automáticamente desde Hugging Face la primera vez que se ejecuta el notebook; se recomienda tener conexión a internet en esa primera ejecución.

## Uso

1. Coloca el fichero `trustpilot-reviews-123k.csv` en la raíz del proyecto (o ajusta la ruta en la celda de carga de datos).
2. Abre `analisis_empresa.ipynb` y ejecuta las celdas en orden.
3. Para reutilizar el notebook con otra empresa o sector, modifica únicamente `target_company` y `target_category` en la sección *"5) Filtrado del sector de la compañía objetivo"*; el resto del pipeline es independiente de la empresa elegida.
4. Las visualizaciones finales (gráficos de sentimiento, de topics y del cruce entre ambos) se generan con Plotly en la sección *"9) Visualizaciones"*.

## Presentación

`Dyson_Analisis_Reputacion.pptx` resume el análisis en 8 diapositivas (portada, objetivos, metodología, análisis de sentimiento, análisis de topics, cruce sentimiento/topic, conclusiones y próximos pasos), con gráficos nativos editables y un apartado final de propuestas y objetivos cuantificados para el equipo de negocio.

## Próximos pasos

Las recomendaciones completas, con sus objetivos cuantificados, están en la última diapositiva de la presentación. A alto nivel: auditar el proceso de atención al cliente y postventa, revisar la calidad y fiabilidad del producto, realizar un benchmark directo frente a Shark, e implementar un dashboard recurrente de sentimiento y topics para medir el impacto de las acciones.

## Autor

**David** — [LinkedIn](https://linkedin.com/in/dvizquierdo) · [GitHub](https://github.com/dv-izquierdo)

## Licencia

Proyecto con fines educativos y de portfolio.
