# data/

Esta carpeta no se versiona (ver `.gitignore`). `utils.load_corpus()` descarga aquí
`noticias_colombia.parquet` desde la release `corpus-v1` y verifica su SHA-256
(`92b02aa1a74015192d583cede59c3d8bd7794c214028809cc687704255aa2fc7`).

- Corpus completo: 91.585 artículos únicos tras deduplicar (`id`, `texto`, `fuente`, `fecha`).
- `corpus_medido.parquet` (salida del notebook 05) se publica como asset de una release aparte.
