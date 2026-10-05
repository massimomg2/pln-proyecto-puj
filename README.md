# ¿Qué tan estable es la agenda temática de la prensa colombiana? (1980–2011)

Proyecto de PLN — Pontificia Universidad Javeriana · Entrega 2: EDA avanzado, representación y propuesta metodológica.

**Pregunta:** ¿qué tan estable es la agenda temática de la prensa colombiana entre 1980 y 2011 y cuándo se producen cambios significativos en ella? *Agenda* = prevalencia de tópicos (proporción del corpus dedicada a cada tema en un año).

**Orden de trabajo:** primero entender el corpus; luego detectar cambios con métodos progresivamente más costosos, calibrados con un modelo nulo; y solo al final contrastar con coyunturas históricas.

## Respuesta en breve

| Hallazgo | Evidencia |
| --- | --- |
| La agenda de El Tiempo (1990–2011) es muy estable de un año al siguiente | JS entre años consecutivos = 0,0029, el 6 % de la distancia entre El Tiempo y Semana en el mismo año (0,0497) |
| La deriva es acotada | La divergencia sube de 0,0029 (1 año) a 0,0158 (10 años) y se estabiliza desde ~8 años |
| Hay tres cambios de régimen: **1995, 1999–2000 y 2006** | Aparecen en 6 de 6 variantes (K, semilla, sin nombres propios) y se reproducen en Semana (1994, 1998, 2006) y, el de 2006, en Dinero (2007) |
| El cambio más fuerte es 2005→2006 (JS 0,0078) | Baja economía/negocios; suben justicia, policía, región, familia y deporte. Coincide con el crecimiento de Dinero: posible efecto editorial o de archivo |
| El contraste histórico no es concluyente | Los cambios caen a ±1 año de eventos congelados, pero el azar da 2,5 de 3 coincidencias (P = 0,58) |
| 1980–1989 no se puede evaluar | Solo hay Semana (31–166 documentos por año; falta 1981). El salto 1989→1990 es la llegada de El Tiempo |
| La fuente pesa más que el tiempo | JS entre fuentes ≫ cambio anual dentro de una fuente |

Detalle completo, tablas y figuras en [`docs/informe_entrega2.pdf`](docs/informe_entrega2.pdf); presentación en [`docs/presentacion_entrega2.pdf`](docs/presentacion_entrega2.pdf).

## Etapas del proyecto

| Etapa | Cuadernos | Qué se hizo |
| --- | --- | --- |
| Entrega 1 | `01-corpus`, `02-textometria` | Carga, limpieza y deduplicación del corpus; textometría básica |
| Exploración posterior | `03-eda-agenda`, `04-divergencia-lexica` | EDA de agenda por fuente y resolución temporal; **experimento TF-IDF / JS / ráfagas de Kleinberg** (regla de "2 de 3 señales"), punto de partida de la Entrega 2 |
| Entrega 2 | `05-eda-avanzado-dataset-medido`, `06-agenda-topicos-cambio` | Correcciones, EDA avanzado, dataset medido, representaciones, NMF, estabilidad con nulo, cambios, contraste histórico |

## Cómo reproducirlo (Colab gratis)

Ejecuta los notebooks en orden desde la carpeta `notebooks/` (cada uno necesita `utils.py` al lado; los notebooks 05 y 06 lo crean si falta). Los notebooks 05 y 06 tardan ~5 min cada uno con GPU T4 (la GPU solo acelera los embeddings).

| Orden | Notebook | Qué hace | Colab |
| --- | --- | --- | --- |
| 1 | `notebooks/01-corpus.ipynb` | Tipos, nulos, duplicados y estadísticas del corpus | [Abrir](https://colab.research.google.com/github/CASDAV/proyecto-pln/blob/main/notebooks/01-corpus.ipynb) |
| 2 | `notebooks/02-textometria.ipynb` | Textometría: Zipf, longitudes, vocabulario | [Abrir](https://colab.research.google.com/github/CASDAV/proyecto-pln/blob/main/notebooks/02-textometria.ipynb) |
| 3 | `notebooks/03-eda-agenda.ipynb` | Cobertura por fuente, resolución temporal, anomalías de fecha | [Abrir](https://colab.research.google.com/github/CASDAV/proyecto-pln/blob/main/notebooks/03-eda-agenda.ipynb) |
| 4 | `notebooks/04-divergencia-lexica.ipynb` | Experimento inicial: JS, coseno sobre TF-IDF y ráfagas de Kleinberg | [Abrir](https://colab.research.google.com/github/CASDAV/proyecto-pln/blob/main/notebooks/04-divergencia-lexica.ipynb) |
| 5 | `notebooks/05-eda-avanzado-dataset-medido.ipynb` | Diversidad léxica, colocaciones NPMI, términos distintivos, PPMI, entidades, léxico temático; exporta el dataset medido | [Abrir](https://colab.research.google.com/github/CASDAV/proyecto-pln/blob/main/notebooks/05-eda-avanzado-dataset-medido.ipynb) |
| 6 | `notebooks/06-agenda-topicos-cambio.ipynb` | TF-IDF vs embeddings, NMF, prevalencia, estabilidad con nulo, cambios, robustez, contraste histórico | [Abrir](https://colab.research.google.com/github/CASDAV/proyecto-pln/blob/main/notebooks/06-agenda-topicos-cambio.ipynb) |

- Cada notebook carga el corpus con `utils.load_corpus()` (descarga desde la release `corpus-v1`, verifica SHA-256, deduplica y exige 91.585 documentos).
- Parámetros al inicio de los notebooks 05 y 06. `FAST = True` hace una corrida reducida (6.000 documentos) para probar.
- Resultados: `/content/results` en Colab (el 06 deja además `resultados_entrega2.zip`) y `results/` si se corre en local. Los CSV del 05 deben copiarse desde `/content/results` a `results/entrega2/05_eda/`.
- Semilla fija (`SEED = 42`); los embeddings en GPU pueden variar mínimamente.

En local: `pip install -r requirements.txt` y abrir los notebooks desde `notebooks/`.

## Estructura del repositorio

```text
proyecto-pln/
├── README.md
├── pyproject.toml            # marca la raíz del repo (lo usa utils.repo_root)
├── requirements.txt
├── .gitignore
├── data/                     # ignorada por git; el corpus se descarga solo
│   └── README.md
├── notebooks/
│   ├── utils.py              # carga del corpus, normalización, results_dir
│   ├── 01-corpus.ipynb                          # Entrega 1
│   ├── 02-textometria.ipynb                     # Entrega 1
│   ├── 03-eda-agenda.ipynb                      # exploración posterior
│   ├── 04-divergencia-lexica.ipynb              # experimento inicial (TF-IDF / JS / Kleinberg)
│   ├── 05-eda-avanzado-dataset-medido.ipynb     # Entrega 2
│   └── 06-agenda-topicos-cambio.ipynb           # Entrega 2
├── results/                  # salidas de los cuadernos 01-04 (results_dir() del utils)
│   └── entrega2/
│       ├── 05_eda/           # diversidad_lexica.csv, bigramas_npmi.csv, prevalencia_lexico_por_anio.csv, figuras
│       └── 06_agenda/        # topicos.csv, prevalencia_topicos_por_anio.csv, transiciones_anuales.csv,
│                             # robustez_cambios.csv, soporte_cambios.csv, cambios_otras_series.csv,
│                             # seleccion_K.csv, comparacion_representaciones.csv, decaimiento_rezago.csv,
│                             # theta_documentos.csv.gz, resumen_06.json, figuras
└── docs/
    ├── informe_entrega2.pdf          # informe (secciones 1-14)
    ├── presentacion_entrega2.pdf     # presentación (16 diapositivas, con anexo de referencias)
    └── referencias/                  # enunciado y artículo de Caicedo, Gaviria y Moreno (2012)
```

## Datos

- **Corpus:** copia fiel de [`yabramuvdi/NoticiasColombia`](https://huggingface.co/datasets/yabramuvdi/NoticiasColombia) publicada como asset de la release `corpus-v1`. 91.585 artículos únicos de El Tiempo (89 %), Semana (7,2 %) y Dinero (3,7 %); columnas `id`, `texto`, `fuente`, `fecha`.
- **Dataset medido** (`corpus_medido.parquet`, salida del notebook 05; no se versiona, se publica como asset de release): `id`, `fuente`, `fecha`, `año`, `texto_limpio`, `tokens`, `n_chars`, `n_palabras`, `n_tokens`, `n_types`, `ttr`, `n_entidades`, `frac_entidades`, `categoria_inicial`, `n_hits_lexico`.
- **Composición por tópicos** (`results/entrega2/06_agenda/theta_documentos.csv.gz`): distribución de cada documento sobre los 40 tópicos.

## Método en una vista

1. **Representación:** TF-IDF (22.196 términos) frente a embeddings multilingües (MiniLM-L12). TF-IDF gana en separar periodo y fuente (macro-F1 0,558 y 0,527 frente a 0,390 y 0,439) y es interpretable.
2. **Tópicos:** NMF sobre TF-IDF con K = 40, elegido por coherencia NPMI (0,301) y estabilidad entre semillas (0,825).
3. **Estabilidad:** JS entre años contra un nulo de permutaciones estratificado por fuente (1.000 permutaciones, Benjamini-Hochberg), curva de decaimiento por rezago y escala de referencia (distancia entre fuentes).
4. **Cambios:** segmentación binaria con umbral calibrado por permutación; consenso entre variantes (K = 15/20/30, otra semilla, sin entidades) y contraste con Semana, Dinero y una serie estandarizada.
5. **Contraste histórico:** al final, con lista congelada y línea base aleatoria.

## Limitaciones principales

- Una sola fuente continua (El Tiempo) y solo desde 1990: no se puede separar agenda de cobertura. Los años 80 son solo Semana con muy pocos documentos.
- Volumen y composición del corpus variables (El Tiempo cae a 2.231 documentos en 1999; Dinero crece entre 2005 y 2007): los cambios de 1999 y 2006 pueden ser editoriales o de archivo.
- NMF sobre unigramas sin lematizar; algunos tópicos son de estilo o de sección (opinión, avisos, cotizaciones).
- El nulo por permutación supone documentos intercambiables; con N alto casi todo es significativo y hay que leer la magnitud.
- La segmentación binaria solo ve saltos a resolución anual.
- Con ±1 año y 9 eventos, el azar ya produce 2,5 de 3 coincidencias.

La lista completa y las mejoras propuestas están en las secciones 12 y 13 del informe.

## Referencias

- Caicedo, Gaviria y Moreno (2012). *Hechos y palabras: la realidad colombiana vista a través de la prensa escrita.* Revista de Economía Institucional 14(26). Artículo de referencia que ya usa este tipo de corpus (más de 2 M de artículos); ver `docs/referencias/`.
- Michel et al. (2011). *Quantitative analysis of culture using millions of digitized books.* Science 331.
- Monroe, Colaresi y Quinn (2008). *Fightin' Words: Lexical Feature Selection and Evaluation for Identifying the Content of Political Conflict.* Log-odds con prior informativo de Dirichlet.
- Bouma (2009). *Normalized (Pointwise) Mutual Information in Collocation Extraction.* NPMI.
- Lee y Seung (1999). *Learning the parts of objects by non-negative matrix factorization.* Nature.
- Blei, Ng y Jordan (2003). *Latent Dirichlet Allocation.* JMLR 3. Blei y Lafferty (2006), *Dynamic topic models*; Roberts et al. (2014), *Structural topic models*; Grootendorst (2022), *BERTopic*.
- Kleinberg (2002). *Bursty and hierarchical structure in streams.* KDD. Killick, Fearnhead y Eckley (2012), PELT. Lin (1991), divergencia de Jensen-Shannon.
- Reimers y Gurevych (2019). *Sentence-BERT.* Modelo usado: `paraphrase-multilingual-MiniLM-L12-v2`.
- Baumgartner y Jones (1993). *Agendas and Instability in American Politics.* Comparative Agendas Project y EuroVoc (inspiración del léxico temático).

## Autores

Pontificia Universidad Javeriana · Procesamiento de Lenguaje Natural · profesor Luis Gabriel Moreno Sandoval.

- José Miguel Bejarano
- Massimo Maimone
- Mauricio Morales
- Juan Felipe Guzmán
- David Castillo
