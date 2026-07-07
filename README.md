# Análisis de Tendencias de YouTube (EE. UU.) — Trabajo Final

**Curso:** Fundamentos de Data Science (UPC) · **Grupo 4**  
**País asignado:** Estados Unidos (US)  
**Metodología:** CRISP-DM

Una consultora internacional con sede en Lima desarrolla un proyecto de Ciencia de
Datos para conocer las tendencias de los videos de YouTube. El cliente final es una
empresa de marketing digital que necesita respuestas concretas sobre categorías,
canales, evolución temporal y geografía de las métricas de engagement.

---

## Equipo y roles

| Integrante | Rol | Fases CRISP-DM | Responsabilidad principal |
|---|---|---|---|
| Vivianne Ríos | Business Project Sponsor | Comprensión del negocio, Conclusiones | Define objetivos de negocio y de Data Science; supervisa el cumplimiento del proyecto y elabora conclusiones orientadas al cliente. |
| Vanessa Barrientos | Data Scientist | Modelado, Evaluación | Selecciona la técnica de modelado, entrena y evalúa el modelo predictivo e interpreta las métricas de desempeño. |
| Sergio Iglesias | Data Engineer | Comprensión de datos, Preparación de datos | Recolecta, integra y prepara los datos; realiza limpieza, tratamiento de nulos/outliers, creación de variables derivadas y genera el dataset final para modelar. |
| Matías Del Castillo | Data Analyst | Comprensión de datos (EDA) | Explora el conjunto de datos con estadísticas descriptivas y visualizaciones; identifica patrones e interpreta resultados para responder los requerimientos. |

---

## Requerimientos del cliente

Cada respuesta debe ir acompañada de una visualización con título, leyenda y, de ser
necesario, una tabla de datos complementaria.

### Por categoría de videos
1. ¿Qué categorías de videos son las de mayor tendencia?
2. ¿Qué categorías son las que más gustan? ¿Y las que menos gustan?
3. ¿Qué categorías tienen la mejor proporción (ratio) de "Me gusta" / "No me gusta"?
4. ¿Qué categorías tienen la mejor proporción (ratio) de "Vistas" / "Comentarios"?

### Por el tiempo transcurrido
5. ¿Cómo ha cambiado el volumen de los videos en tendencia a lo largo del tiempo?

### Por canales de YouTube
6. ¿Qué canales son tendencia más frecuentemente? ¿Y cuáles con menos frecuencia?

### Por la geografía del país
7. ¿En qué estados se presenta el mayor número de "Vistas", "Me gusta" y "No me gusta"?

### Adicionales
8. ¿Los videos en tendencia son los que mayor cantidad de comentarios positivos reciben?
9. ¿Es factible predecir el número de "Vistas" o "Me gusta" o "No me gusta"?

---

## Objetivo de Data Science

El enunciado permite elegir entre `views`, `likes`, `dislikes` o `comment_count` como
variable dependiente. **Este grupo modela `likes`** (en escala logarítmica: `log_likes`).

**Variables independientes principales** (ver detalle en
[`.cursor/context/dataset_modelo.md`](.cursor/context/dataset_modelo.md)):

- Engagement: `log_views`, `log_dislikes`, `log_comments`, `dislike_view_ratio`, `comment_view_ratio`
- Temporal: `log_days_to_trend`, `first_trend`
- Texto: `title_length`, `n_tags`, `desc_faltante`
- Categoría: one-hot de 4 grupos por impacto (`catgrupo_MedioBajo`, `catgrupo_MedioAlto`, `catgrupo_Alto`; referencia = Bajo)
- Banderas opcionales: `comments_disabled`, `ratings_disabled`

---

## Dataset

Conjunto *Trending YouTube Video Statistics* (Kaggle), adaptado para el curso con
columnas geográficas añadidas: `state`, `lat`, `lon`, `geometry`.

| Archivo | Descripción |
|---|---|
| `dataset/USvideos_cc50_202101.csv` | Datos crudos de tendencias en EE. UU. |
| `dataset/US_category_id.json` | Catálogo de categorías (`id` → nombre) |
| `dataset/USvideos_clean.csv` | Dataset model-ready (~40 899 filas × 29 columnas) |
| `dataset/USvideos_clean_base.csv` | Checkpoint intermedio post-limpieza |

---

## Estructura del repositorio

```text
├── upc_2026_01_seccion_NRC_grupo4_tf.ipynb   # notebook principal (entrega)
├── notebooks/
│   ├── eda_inicial.ipynb                     # EDA enriquecida
│   └── limpieza.ipynb                        # limpieza + feature engineering
├── dataset/                                  # datos crudos y limpios
├── .cursor/context/                          # documentación de referencia
├── requirements.txt
└── README.md
```

---

## Puesta en marcha

```bash
python -m venv .venv
```

**Windows (PowerShell):**
```powershell
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter notebook
```

**Linux / macOS:**
```bash
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

Ejecutar los notebooks desde la **raíz del repositorio**; las rutas de datos son
relativas (`dataset/USvideos_cc50_202101.csv`).

---

## Avance por fase CRISP-DM

| Fase  | Artefacto |
|---|---|
| Comprensión del negocio  | Objetivos, 9 preguntas, roles definidos |
| Comprensión de datos / EDA | `notebooks/eda_inicial.ipynb`, notebook principal |
| Preparación de datos | `notebooks/limpieza.ipynb`, `USvideos_clean.csv` |
| Modelado | `RLM_Completo.ipynb` |
| Evaluación y conclusiones | `RLM_Completo.ipynb` |

```mermaid
flowchart LR
  negocio[ComprensionNegocio] --> eda[EDA_y_Requerimientos]
  eda --> limpieza[PreparacionDatos]
  limpieza --> modelo[Modelado_likes]
  modelo --> eval[Evaluacion_CRISP_DM]
```

---