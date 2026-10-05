# Entrega 2 · Estabilidad de la agenda temática de la prensa colombiana (1980–2011)

Oct 4, 2026 · @Massimo

**Integrantes:** José Miguel Bejarano, Massimo Maimone, Mauricio Morales, Juan Felipe Guzmán y David Castañeda · Procesamiento de Lenguaje Natural, Pontificia Universidad Javeriana · Profesor: Luis Gabriel Moreno Sandoval

## 1. Resumen

**La agenda temática de El Tiempo entre 1990 y 2011 es muy estable de un año al siguiente, pero no es estática: deriva de forma gradual y cambia de régimen en 1995, 1999 y 2006.** El cambio medio entre años consecutivos (divergencia Jensen‑Shannon, JS = 0,0029) equivale al 6 % de la distancia entre dos fuentes distintas en el mismo año (JS = 0,0497, El Tiempo frente a Semana).

- **Pregunta:** ¿qué tan estable es la agenda temática de la prensa colombiana entre 1980 y 2011 y cuándo cambia de forma significativa? Agenda = prevalencia de tópicos (proporción del corpus dedicada a cada tema en un año).
- **Orden de trabajo:** primero entender el corpus (EDA avanzado), luego medir estabilidad y detectar cambios con métodos progresivamente más costosos, y solo al final contrastar con coyunturas históricas.
- **Método:** TF‑IDF + NMF (K = 40 tópicos), prevalencia anual, divergencia JS contra un modelo nulo de permutaciones estratificado por fuente, segmentación binaria con umbral calibrado por permutación y pruebas de robustez.
- **Hallazgo central:** los tres cambios de El Tiempo (1995, 1999, 2006) sobreviven a 6 de 6 variantes (otro K, otra semilla, sin nombres propios) y reaparecen en Semana (1994, 1998, 2006) y Dinero (2007), que son fuentes independientes.
- **Alcance real:** 1980–1989 solo tiene Semana, con 31–166 documentos por año; no hay datos de 1981. La respuesta sólida cubre 1990–2011; los años 80 se describen, no se contrastan.
- **Contraste histórico:** los tres cambios caen a ±1 año de eventos de la lista congelada, pero con esa lista el azar daría 2,5 aciertos de 3 (P = 0,58): no es evidencia a favor de ninguna interpretación histórica.

## 2. Estado del arte

**La pregunta combina tres tradiciones: la lectura cuantitativa de textos a gran escala (culturomics), la teoría de la agenda como equilibrio puntuado, y los modelos de tópicos con detección de cambios.** El artículo de referencia sobre este mismo corpus pertenece a la primera; la pregunta del proyecto pertenece a la segunda y la metodología, a la tercera.

| Línea | Trabajos | Qué aportan | Uso en este proyecto |
| --- | --- | --- | --- |
| Culturomics y frecuencia de palabras | Michel et al. (2011), *Science*; Caicedo, Gaviria y Moreno (2012), *Hechos y palabras*, Revista de Economía Institucional 14(26) | Frecuencia de palabras normalizada por el total de 1‑gramas como indicador social. Caicedo et al. usan >2 millones de artículos (\~600 M de palabras) de El Tiempo, Semana y Dinero y validan contra indicadores externos (desempleo, recesión, El Niño) | Corpus de referencia (el nuestro es un subconjunto de 91.585 artículos). Adoptamos la normalización por total de tokens, pero medimos prevalencia de tópicos y su estabilidad, no frecuencias de palabras sueltas |
| Agenda y cambio | McCombs y Shaw (1972); Baumgartner y Jones (1993), equilibrio puntuado; Comparative Agendas Project | La agenda pública alterna largos periodos estables con cambios abruptos; códigos temáticos comparables | Marco conceptual: “estabilidad + saltos” es lo que contrastamos. El léxico de 13 categorías se inspira en los *major topics* de CAP |
| Modelos de tópicos para atención política | Lee y Seung (1999), NMF; Blei, Ng y Jordan (2003), LDA; Quinn et al. (2010), *AJPS*; Blei y Lafferty (2006), tópicos dinámicos; Roberts et al. (2014), STM; Grootendorst (2022), BERTopic | Medir la atención como mezcla de tópicos por periodo; incorporar covariables (fuente, tiempo); tópicos sobre embeddings | NMF sobre TF‑IDF como modelo base; STM y BERTopic como mejoras (sección 13) |
| Representación del texto | Salton y Buckley (1988), TF‑IDF; Reimers y Gurevych (2019), Sentence‑BERT | Ponderación dispersa e interpretable frente a embeddings densos de oración | Comparación empírica en la sección 8 |
| Ráfagas y puntos de cambio | Kleinberg (2002); Killick, Fearnhead y Eckley (2012), PELT; Truong, Oudre y Vayatis (2020), revisión | Detectar cuándo cambia un flujo de texto o una serie temporal | Kleinberg se probó en el experimento previo y se sustituyó por segmentación binaria con nulo por permutación; PELT queda como mejora |
| Estadística textual | Monroe, Colaresi y Quinn (2008), log‑odds; Bouma (2009), NPMI; Covington y McFall (2010), MATTR | Términos distintivos, colocaciones y diversidad léxica robustos al tamaño de muestra | EDA avanzado (sección 6) |

**Qué añadimos.** Respecto al artículo de referencia, el cambio es de enfoque: pasamos de series de frecuencia de palabras a la prevalencia de tópicos y cuantificamos cuánto cambia con un modelo nulo que descuenta el ruido de muestreo y la composición por fuente. Respecto a la literatura de agenda, usamos un corpus de tres medios colombianos de 1980 a 2011.

## 3. Correcciones respecto a la Entrega 1 y al experimento exploratorio

Se descartó el detector del experimento exploratorio (cuaderno 04, posterior a la Entrega 1) porque marcaba “cambio” por construcción y no distinguía cambio de ruido; se reemplazó por un modelo nulo.

| Problema detectado | Por qué es un problema | Qué se hace ahora |
| --- | --- | --- |
| Regla “≥2 de 3 señales en el cuartil superior” | Marca \~25 % de los periodos por construcción, haya cambio o no | Se descarta; el umbral sale de permutaciones (modelo nulo) |
| Periodos mixtos (bienio hasta 1989, trimestre desde 1990) | La resolución cambia a mitad de la serie | Resolución anual homogénea |
| JS/coseno sin control del tamaño de muestra | Con pocos documentos la divergencia es alta solo por ruido | Se calcula el piso de ruido con permutaciones al mismo N |
| Vocabulario top‑5000 dominado por nombres propios | Los “cambios” pueden ser de personajes, no de temas | Se mide el peso de entidades y se repite el análisis sin ellas |
| Fuente confundida con tiempo | Un cambio de fuente parece un cambio de agenda | Todo se reporta por fuente; las permutaciones se estratifican por fuente |
| Kleinberg sobre 150 términos con bins heterogéneos | Supone bins homogéneos; muy sensible a la elección de términos | Se sustituye por prevalencia de tópicos + segmentación binaria |
| Deduplicación min‑hash ruidosa | Casi‑duplicados inflan frecuencias | Se reporta (\~1 %) y se deja como limitación |

## 4. Del experimento exploratorio a la propuesta

**Entre la Entrega 1 y esta entrega probamos un primer acercamiento con TF‑IDF, divergencia léxica y ráfagas (cuaderno 04); funcionó como diagnóstico y sus defectos definen el diseño actual.** Fue un ejercicio exploratorio, no un modelo, y se hizo después del EDA de agenda (cuaderno 03), que fijó sus condiciones.

**Lo que dejó el cuaderno 03 (EDA de agenda).** Cobertura desigual por fuente y año; tres resoluciones posibles (31 años; 125 trimestres con 5 vacíos; 373 meses con 17 vacíos); hora de publicación sin información; vocabulario que no se satura (305.904 tipos) y \~1 % de casi‑duplicados (408 grupos, 919 documentos).

**Lo que hizo el cuaderno 04.** Periodos adaptativos (5 bienios en 1980–89 y trimestres desde 1990: 93 periodos) y tres señales:

| Señal | Qué se calculó | Qué aprendimos |
| --- | --- | --- |
| Jensen‑Shannon | Distribución de los 5.000 términos más frecuentes (70,9 % de los tokens) en cada periodo frente al anterior | Depende del tamaño de muestra: periodos con pocos documentos parecen cambiar más solo por ruido |
| TF‑IDF + distancia coseno | TF‑IDF de los documentos (88.542 términos, `min_df` 5, `max_df` 0,7) promediado por periodo; coseno entre periodos consecutivos | Los términos distintivos son sobre todo nombres propios; no hay referencia para saber qué distancia es grande |
| Kleinberg (2002), 2 estados | Ráfagas en los 150 términos más frecuentes; cuenta de términos en ráfaga por periodo | Supone periodos homogéneos; aquí se mezclaban bienios y trimestres |
| Regla combinada | Periodo “candidato” si ≥2 de las 3 señales están en su cuartil superior | Marcó 22 de 93 periodos (23,7 %), casi el 25 % que la regla produce por construcción |

**De ahí al diseño actual.** Cada defecto se tradujo en una decisión:

1. La pregunta va primero: se mide estabilidad de la agenda (prevalencia de tópicos), no distancia entre vocabularios.
2. Un modelo nulo por permutación reemplaza la regla del cuartil, y el umbral de cambio sale de los datos.
3. Resolución anual única y N controlado por año.
4. Todo por fuente, con permutaciones estratificadas.
5. TF‑IDF se mantiene, pero como representación de entrada de un modelo de tópicos (NMF) y no como distancia directa entre periodos.
6. Los eventos históricos se contrastan al final, con una lista congelada.

## 5. Corpus y preprocesamiento

**El corpus tiene 91.585 artículos únicos (31,9 M de tokens) pero su cobertura es muy desigual: antes de 1990 solo hay Semana y El Tiempo aporta el 89 % de los documentos.** Es la copia fiel del conjunto `yabramuvdi/NoticiasColombia`, cargada con `utils.load_corpus()` (deduplicado por texto normalizado, checksum SHA‑256 verificado).

| Fuente | Documentos | Años con datos | Observación |
| --- | --- | --- | --- |
| El Tiempo | 81.539 | 1990–2011 | 807 docs en 1990; caídas a 2.231 (1999) y 2.358 (2006) frente a 4–5 mil en años vecinos |
| Semana | 6.620 | 1980–2011 (sin 1981) | 31–166 docs/año en los 80; 813 en 2011 |
| Dinero | 3.426 | 1993–2011 (sin 1996) | menos de 100 docs/año hasta 2004; 532–668 entre 2007 y 2010 |

- **Tamaño:** 31.896.684 tokens; 15.532.463 tokens de contenido; 266.562 tipos; 42,3 % de hapax. Mediana de 245 palabras por artículo (media 348).
- **Normalización:** `normalizar_texto` reconstruye cifras y horas que la tokenización de origen separó con espacios.
- **Tokenización:** expresión regular sobre letras (con tilde y ñ), minúsculas, sin números. *Tokens de contenido* = sin stopwords (NLTK español + lista corta de periodismo) y longitud ≥ 3.
- **Sin lematización:** spaCy sobre 32 M de tokens es demasiado lento en Colab gratis; se declara como limitación y mejora.
- **Entidades (proxy):** palabras con mayúscula inicial a mitad de oración. Son el 22 % de los tokens de contenido en promedio (12–15 % en los años 80, 17–20 % desde 1990; el salto coincide con el cambio de fuente).
- **Resolución temporal:** anual (31 años con datos). La hora de publicación es siempre 04:00 o 05:00 y no aporta información.

## 6. EDA avanzado (notebook 05)

**El vocabulario cambia con la época y con la fuente, pero buena parte de lo que parece “cambio temático” en el léxico es cambio de nombres propios y de fuente.** Cada técnica se corrió sobre el corpus completo.

### 6.1 Diversidad léxica robusta al tamaño

Se compara con muestras de igual tamaño (30.000 tokens por grupo) para que el N no distorsione.

Por qué no se usa la razón tipos/tokens (TTR) simple: crece o baja con la longitud del texto, así que un año con más artículos parecería más o menos diverso solo por tamaño. Por eso, para cada año y fuente se arma una muestra de artículos al azar hasta reunir 30.000 tokens de contenido; si el grupo no alcanza ese tamaño se omite. Sobre esa muestra se calculan tres medidas:

- MATTR (moving‑average type‑token ratio): se desliza una ventana de 500 palabras, de a una palabra por vez; en cada posición se cuenta cuántas palabras distintas hay y se divide entre 500; se promedian todas las ventanas. 1 significa que ninguna palabra se repite; un valor menor, más repetición.
- Yule's K: mide qué tan concentrado está el texto en pocas palabras repetidas y casi no depende de la longitud. Mayor K = vocabulario más repetitivo.

```latex
K = 10^{4}\,\frac{\sum_{m} m^{2}V_m - N}{N^{2}}
```

con N el número de tokens y V\_m el número de palabras distintas que aparecen exactamente m veces.

- Ley de Heaps: el tamaño del vocabulario crece como V = k·N^β con el número de tokens N (se ajusta en escala logarítmica). Si β fuera cercano a 0 el vocabulario se cerraría; β ≈ 0,5 indica que sigue abriéndose con cada texto nuevo.

&#91;image: MATTR y Yule's K por año y fuente\]

La figura muestra MATTR (izquierda) y Yule's K (derecha) por año: todas las fuentes caen juntas hacia 2001 y 2004–05, y Dinero queda por encima en K.

- **MATTR** (ventana 500) baja de \~0,80 en los años 80–90 a \~0,75 en los 2000. Cae a la vez en las tres fuentes hacia 2001 y 2004–05, lo que sugiere un cambio de formato o de secciones en el archivo y no un cambio de estilo de los periodistas.
- **Yule's K** (concentración): \~3–3,5 en El Tiempo, 3,3–4,6 en Semana y 4,3–5,5 en Dinero; el vocabulario de Dinero es el más repetitivo (léxico financiero).
- **Ley de Heaps** V = K·N^β: β = 0,46 global, 0,46 El Tiempo, 0,50 Semana y 0,51 Dinero. El vocabulario no se satura (nombres, cifras, neologismos): no se puede “cerrar”.

### 6.2 Colocaciones (NPMI)

Cómo se calcula. Se cuentan pares de palabras adyacentes en las que ambas son de contenido (no stopwords) y cada una aparece al menos 30 veces. La fuerza de asociación es la NPMI (información mutua puntual normalizada), la misma fórmula de la sección 9.2 pero con p(a,b) = frecuencia del par adyacente: vale 1 cuando las dos palabras solo aparecen juntas y 0 cuando aparecen juntas lo esperado por azar. Los trigramas se arman uniendo dos bigramas con NPMI de al menos 0,4. Sirve para detectar expresiones fijas y nombres compuestos que un modelo de unigramas parte en piezas.

12.205 bigramas con n ≥ 30. Los de mayor NPMI son nombres extranjeros (*wall street*, *hong kong*, *são paulo*) y expresiones fijas (*derechos humanos*, 3.574; *naciones unidas*, 1.735; *América Latina*, 3.768). Los trigramas más frecuentes son políticos: *Juan Manuel Santos* (1.292), *presidente Álvaro Uribe* (1.152), *producto interno bruto* (534), *Fondo Monetario Internacional* (504), *presidente Ernesto Samper* (498).

| Época | Colocaciones frecuentes |
| --- | --- |
| 1980–89 | opinión pública, fuerzas armadas, Unión Soviética, Vargas Llosa, presidente Betancur |
| 1990–99 | derechos humanos (1.223), medio ambiente (1.182), servicios públicos, Ernesto Samper |
| 2000–11 | señor director (1.889), presidente Uribe (1.737), Corte Suprema (1.639), Juan Manuel |

### 6.3 Términos distintivos (log‑odds con prior de Dirichlet)

Cómo se calcula. Una palabra es distintiva de un grupo (una época o una fuente) si es mucho más frecuente en él que en el resto. Para no premiar palabras rarísimas se usa el estadístico de log‑odds con prior informativo de Dirichlet (Monroe, Colaresi y Quinn, 2008):

```latex
\hat\delta_w=\ln\frac{y^{A}_w+\alpha_w}{n^{A}+\alpha_0-y^{A}_w-\alpha_w}-\ln\frac{y^{B}_w+\alpha_w}{n^{B}+\alpha_0-y^{B}_w-\alpha_w},\qquad z_w=\frac{\hat\delta_w}{\sqrt{\sigma^{2}_w}}
```

donde y es la cuenta de la palabra w en cada grupo, n el total de palabras del grupo y α un prior proporcional a la frecuencia en todo el corpus (“una palabra se supone tan frecuente como en el corpus hasta que los datos digan lo contrario”). El z‑score divide por la incertidumbre: una palabra con z alto es distintiva y está respaldada por suficientes datos. Cada época y cada fuente se compara contra el resto.

- **Por época:** 1980–89: *Reagan, norteamericano, Betancur, Barco, soviéticos*; 1990–99: *Samper, Clinton, Gaviria, constituyente, Bosnia*; 2000–11: *Uribe, FARC, Chávez, Santos, paramilitares, víctimas, referendo, TLC, internet*.
- **Por fuente:** El Tiempo: *calle, alcalde, municipio, barrio, Cali* (agenda local); Semana: *guerra, FARC, Uribe, paramilitares, política* (agenda nacional y conflicto); Dinero: *crecimiento, empresas, mercado, exportaciones, inversión* (economía).

### 6.4 Co‑ocurrencias (PPMI por documento)

Cómo se calcula. Se eligen términos semilla (violencia, paz, economía, guerrilla, corrupción, salud, empleo, fútbol). Para cada época se mide con qué palabras aparece cada semilla en el mismo artículo más de lo esperado por azar, mediante la información mutua puntual positiva:

```latex
\mathrm{PPMI}(a,b)=\max\!\Big(0,\ \ln\frac{p(a,b)}{p(a)\,p(b)}\Big)
```

con p(a,b) la fracción de artículos de la época que contienen ambas. Las diez palabras con mayor PPMI son los vecinos. Si los vecinos de un mismo término cambian entre épocas, el concepto cambió de contexto, aunque su frecuencia no cambie.

El contexto de un mismo concepto cambia entre épocas. “Salud” pasa de términos genéricos en los 80 a *contributivo, Sisbén, EPS, IPS* en los 90 y *Fosyga, dengue* en los 2000. “Economía” pasa de *crecimiento, producción* a *PIB, recesión* (90) y *Greenspan, Bernanke, Fed* (2000). “Guerrilla” pasa de *ejército, militares* a *Tirofijo, subversión* (90) y *Caguán, secretariado, Jojoy* (2000). Las vecindades de 1980–89 para *corrupción, empleo* y *fútbol* son ruido (pocas co‑ocurrencias en una sola fuente) y no deben interpretarse.

### 6.5 Entidades

Los nombres propios son el 22 % de los tokens de contenido. Si los cambios se debieran a personajes, el análisis tendría que separarlos; por eso la robustez incluye una variante sin entidades.

Cómo se mide: un token cuenta como entidad si empieza con mayúscula a mitad de oración y no está todo en mayúsculas; la fracción por año es entidades / tokens de contenido. En los años 80 es 12–15 % y desde 1990 sube a 17–20 %: el salto de 1989 a 1990 coincide exactamente con el cambio de fuente (de Semana a El Tiempo), así que es un efecto de fuente y no de época. Es otra razón para no leer el salto 1989–1990 como cambio de agenda.

&#91;image: Fracción de tokens que son entidades por año\]

### 6.6 Léxico temático externo (inspirado en CAP y EuroVoc)

Se construyó un léxico de 13 categorías (conflicto armado, narcotráfico, política, justicia, economía, empleo, salud, educación, deporte, cultura, ambiente, internacional, infraestructura) expandido por prefijos contra el vocabulario: cubre 2.319 palabras y el 6,8 % de los tokens. Es una medida *model‑free* de agenda (tokens de la categoría por mil tokens) con la misma normalización que Caicedo et al. (2012). En su serie anual, el narcotráfico alcanza su máximo en 1987–89, el conflicto armado en 2001–02, la economía en 1999–2002 y justicia/crimen crece desde 2005; los años 80 son ruidosos por el N pequeño (31–166 documentos por año).

Cómo se mide la prevalencia léxica: para cada año, tokens del léxico de cada categoría por mil tokens, dividido por el promedio de esa categoría en todos los años (1 = promedio; 2 = el doble). Así categorías muy distintas en tamaño se pueden comparar en un mismo mapa de calor. No usa ningún modelo ni etiquetas humanas.

&#91;image: Prevalencia léxica por año y categoría\]

Qué se ve: narcotráfico en 1987–89 (rojo oscuro), conflicto armado con máximos en 2001–02, economía y empleo entre 1999 y 2002, justicia y crimen en alza desde 2005, deporte hacia 2009–11 y cultura concentrada en 1985–86. Las filas de los años 80 son ruidosas por el N pequeño (31–166 artículos por año); se muestran, pero no se interpretan.

### 6.7 Resumen de hallazgos del EDA y decisiones que implican

| Hallazgo | Evidencia | Decisión en el modelo |
| --- | --- | --- |
| La fuente está confundida con el tiempo | 1980–89 solo Semana; El Tiempo desde 1990; la fracción de entidades salta de 12–15 % a 17–20 % en 1990 | Serie principal: El Tiempo 1990–2011; permutaciones estratificadas por fuente |
| El vocabulario no se cierra | β de Heaps = 0,46; 42,3 % de los tipos aparecen una sola vez | Filtrar por frecuencia (min\_df 15, max\_df 0,4, 25.000 términos) |
| Los nombres propios pesan | 22 % de los tokens de contenido; los términos distintivos por época son gobernantes y coyunturas | Variante sin entidades en la robustez |
| La diversidad léxica cae a la vez en las tres fuentes | MATTR baja de \~0,80 a \~0,75; mínimos en 2001 y 2004–05 | Posible cambio de formato del archivo: desconfiar de cambios cercanos (1999–2001, 2006) |
| Dinero es más repetitivo | Yule's K de 4,3 a 5,5 frente a \~3,3 en El Tiempo | Esperar tópicos de mercados (bolsa, café, petróleo, oro) que fragmentan lo económico |
| Las expresiones se parten al usar unigramas | derechos humanos, naciones unidas, Juan Manuel Santos | Mejora propuesta: bigramas NPMI como rasgos |
| Un mismo concepto cambia de contexto | guerrilla pasa de subversión a Cagudán y secretariado; salud, de genérico a Sisbén y Fosyga | La agenda cambia también por vocabulario dentro de un tema; NMF de unigramas puede dispersarlo |
| El léxico temático coincide con la historia conocida | narcotráfico 1987–89, conflicto armado 2001–02, economía 1999–2002 | Referencia externa para validar los tópicos |
| Los años 80 son ruido | 31–166 artículos por año, una sola fuente | Se describen, no se contrastan |

## 7. Dataset medido

**El notebook 05 exporta `corpus_medido.parquet` con 91.585 filas y 15 columnas, más una muestra de 2.000 filas en CSV para inspección rápida.** El notebook 06 añade `theta_documentos.csv.gz`, con la distribución de cada documento sobre los 40 tópicos.

| Columna | Contenido |
| --- | --- |
| `id`, `fuente`, `fecha`, `año` | identificador original, fuente (sin `www.`/`.com` en el 06), fecha y año |
| `texto_limpio` | texto tras `normalizar_texto` |
| `tokens` | tokens de contenido separados por espacio (sin stopwords, longitud ≥ 3) |
| `n_chars`, `n_palabras` | longitud en caracteres (mediana 1.565) y en palabras (mediana 245) |
| `n_tokens`, `n_types`, `ttr` | tokens de contenido (media 170), tipos (media 129) y TTR (media 0,82) |
| `n_entidades`, `frac_entidades` | tokens que son entidades proxy y su fracción (media 0,22) |
| `categoria_inicial`, `n_hits_lexico` | categoría del léxico temático con más aciertos (≥ 2) o `sin_categoria`, y número de aciertos |

La categoría inicial es una etiqueta débil (léxico por prefijos, sin supervisión humana) pensada como punto de partida y como referencia para validar los tópicos, no como verdad.

## 8. Representación del texto

**TF‑IDF disperso fue la mejor representación para separar periodo y fuente (macro‑F1 0,558 y 0,527) y es además la que permite leer los tópicos; los embeddings multilingües quedaron últimos (0,390 y 0,439).**

- **BoW:** cada documento es un vector de conteos sobre el vocabulario; sirve de base para ver qué términos aparecen (muy disperso: un artículo usa una fracción mínima de los 22.196 términos).
- **TF‑IDF:** pondera cada término por su frecuencia en el documento (escala sublineal) y su rareza en el corpus. Parámetros: `min_df` 15, `max_df` 0,4, 25.000 términos como máximo (22.196 efectivos), ajustado sobre una muestra balanceada de 1.000 documentos por año.
- **Embeddings:** `paraphrase-multilingual-MiniLM-L12-v2` (384 dimensiones) sobre los primeros 700 caracteres, ventana de 128 tokens.

Comparación con validación cruzada de 5 particiones sobre una muestra estratificada por año (hasta 150 documentos por año). El objetivo “periodo” tiene 4 clases (80–89, 90–99, 00–05, 06–11); “fuente” tiene 3.

### 8.1 Cómo se calcula cada vector

Un modelo no lee palabras, lee números. Representar un artículo es convertirlo en un vector (una lista de números). Las cuatro opciones comparadas son:

| Representación | Idea | Cada artículo es… | Parámetros usados |
| --- | --- | --- | --- |
| BoW (bolsa de palabras) | contar cuántas veces aparece cada palabra | un vector de 22.196 conteos, casi todos 0 | vocabulario del TF‑IDF |
| TF‑IDF | conteo ponderado por lo rara que es la palabra | el mismo vector con pesos reales, normalizado a longitud 1 | min\_df 15, max\_df 0,4, máximo 25.000 términos, escala sublineal |
| TF‑IDF + SVD | comprimir TF‑IDF a pocas dimensiones | un vector denso de 128 números | SVD truncada, 128 componentes |
| Embeddings | una red neuronal preentrenada resume el texto | un vector denso de 384 números | MiniLM‑L12 multilingüe, primeros 700 caracteres, ventana de 128 tokens |

Fórmula de TF‑IDF (scikit‑learn con escala sublineal y suavizado):

```latex
\mathrm{tfidf}(t,d) = \big(1+\ln \mathrm{tf}(t,d)\big)\cdot\Big(\ln\frac{1+N}{1+\mathrm{df}(t)}+1\Big)
```

donde tf es cuántas veces aparece la palabra t en el artículo d, df es en cuántos artículos aparece y N es el total de artículos. Después cada vector se divide por su longitud euclidiana. La primera parte premia la repetición, pero con rendimientos decrecientes (el logaritmo); la segunda castiga las palabras que aparecen en casi todos los artículos.

Ejemplo ilustrativo (números inventados para mostrar el cálculo): con N = 10.000 artículos, la palabra “farc” aparece 3 veces en un artículo y está en 800 artículos: (1 + ln 3) × (ln(10.001/801) + 1) = 2,10 × 3,52 ≈ 7,4. La palabra “gobierno”, también con 3 apariciones pero presente en 3.000 artículos, pesa 2,10 × 2,20 ≈ 4,6. A igual frecuencia, la palabra más específica pesa más.

SVD (descomposición en valores singulares truncada) proyecta los 22.196 pesos a 128 combinaciones lineales que conservan la mayor varianza; se pierde interpretabilidad (una dimensión ya no es una palabra) a cambio de un vector pequeño y denso. Los embeddings los produce un modelo de lenguaje entrenado con frases de muchos idiomas para que textos de significado parecido queden cerca; aquí se usan los primeros 700 caracteres de cada artículo y los vectores se normalizan.

### 8.2 La tarea de comparación: qué es el macro‑F1

Necesitamos una vara común para decidir qué representación conserva más información útil. Como nuestro problema es ver cómo cambia el texto con el tiempo y entre medios, la prueba es: darle a un clasificador solo el vector de un artículo y pedirle que adivine cuándo o dónde se publicó. Si la representación conserva lo que distingue épocas y medios, el clasificador acierta más.

- Objetivo “periodo” (4 clases): 1980–89, 1990–99, 2000–05 y 2006–11.
- Objetivo “fuente” (3 clases): El Tiempo, Semana y Dinero.
- Muestra: hasta 150 artículos por año (unos 4.000 en total), para que los años grandes no dominen y los embeddings sean viables en Colab gratis.
- Clasificador: un modelo lineal. LinearSVC para el TF‑IDF disperso y regresión logística para SVD y embeddings.
- Validación cruzada estratificada de 5 particiones: los artículos se reparten en 5 grupos que conservan la proporción de cada clase; se entrena con 4 grupos, se evalúa con el quinto y se repite 5 veces. Ningún artículo se evalúa con un modelo que lo vio al entrenar. Las clases con menos de 5 artículos se excluyen.

Cuatro conceptos, de menor a mayor:

- Precisión de una clase: de los artículos que el modelo etiquetó como esa clase, qué fracción lo era.
- Recall (cobertura) de una clase: de los artículos que realmente eran de esa clase, qué fracción encontró.
- F1 de una clase: la media armónica de ambos, F1 = 2·P·R / (P + R). Es alta solo si precisión y recall son altos a la vez.
- Macro‑F1: el promedio simple de los F1 de todas las clases, sin ponderar por tamaño. Cada clase pesa igual.

Por qué macro y no exactitud (accuracy): El Tiempo es el 89 % del corpus. Un clasificador que respondiera siempre “El Tiempo” tendría 89 % de exactitud, pero su F1 sería 0,94 para El Tiempo y 0 para las otras dos fuentes: macro‑F1 = 0,31. El macro‑F1 obliga a acertar también las clases pequeñas.

Ejemplo ilustrativo con 100 artículos (números inventados para mostrar el cálculo):

| Real \\ Predicho | El Tiempo | Semana | Dinero | Total real |
| --- | --- | --- | --- | --- |
| El Tiempo | 54 | 4 | 2 | 60 |
| Semana | 8 | 15 | 2 | 25 |
| Dinero | 3 | 2 | 10 | 15 |

| Clase | Precisión | Recall | F1 |
| --- | --- | --- | --- |
| El Tiempo | 54/65 = 0,83 | 54/60 = 0,90 | 0,86 |
| Semana | 15/21 = 0,71 | 15/25 = 0,60 | 0,65 |
| Dinero | 10/14 = 0,71 | 10/15 = 0,67 | 0,69 |

Macro‑F1 = (0,86 + 0,65 + 0,69) / 3 = 0,73, mientras que la exactitud sería 79/100 = 0,79: la exactitud sube porque El Tiempo, la clase grande, se acierta mucho.

Referencia de azar: con clases balanceadas, adivinar al azar da un macro‑F1 cercano a 0,25 en periodo (4 clases) y 0,33 en fuente (3 clases). Las muestras no están perfectamente balanceadas, así que son referencias aproximadas. Un macro‑F1 de 0,56 está claramente por encima del azar, pero lejos de acertar siempre: periodo y fuente se confunden entre sí (los años 80 son solo Semana).

### 8.3 Resultados de la comparación

| Representación | Dimensión | Macro‑F1 periodo | Macro‑F1 fuente | Cómputo en la muestra | Interpretable |
| --- | --- | --- | --- | --- | --- |
| TF‑IDF disperso (LinearSVC) | 22.196 | 0,558 | 0,527 | \~0 s | Sí |
| TF‑IDF + SVD (regresión logística) | 128 | 0,448 | 0,495 | 1,7 s | Parcial |
| Embeddings MiniLM‑L12 (regresión logística) | 384 | 0,390 | 0,439 | 10,1 s (GPU T4) | No |

**Cómo leerlo.** Periodo y fuente se distinguen sobre todo por nombres propios, formato y estilo, justo lo que los embeddings de oración están entrenados para ignorar; que pierdan aquí no significa que lo harían en una tarea semántica. El periodo está además correlacionado con la fuente (los años 80 son solo Semana), así que ambos objetivos se solapan. Con esas reservas, TF‑IDF + NMF es el modelo base y los embeddings quedan como mejora (BERTopic o clustering sobre embeddings).

### 8.4 Reservas y control pendiente

- Clasificadores distintos. El TF‑IDF disperso se evaluó con LinearSVC y los otros dos con regresión logística, así que parte de la diferencia podría deberse al clasificador y no a la representación. El cuaderno 06 incluye ahora una celda de control (misma regresión logística para las tres, línea base aleatoria y F1 por clase). No hace parte de la corrida de referencia, por lo que sus cifras no están en este informe.
- Muestra pequeña. Con unos 4.000 artículos y 5 particiones no hay intervalos de confianza; diferencias de unas pocas centésimas no son concluyentes. La ventaja del TF‑IDF disperso (0,558 frente a 0,448 y 0,390 en periodo) es amplia frente a ese ruido, pero no está cuantificada.
- Qué no prueba. Que una representación distinga época y medio no demuestra que capture mejor el tema de los artículos. Es una prueba de separabilidad léxica, que es la propiedad que necesita un análisis de cambio de vocabulario.
- Decisión. Se elige TF‑IDF por tres razones: separa mejor, cuesta menos y es la única entrada que NMF factoriza en temáticas legibles (cada eje es una palabra).

## 9. Propuesta metodológica (notebook 06)

**La propuesta mide la agenda con TF‑IDF + NMF y distingue cambio de ruido con un modelo nulo, en cuatro pasos de costo creciente.** Cada paso responde una parte de la pregunta y alimenta el siguiente.

1. **Prevalencia de tópicos.** NMF sobre TF‑IDF, ajustado con una muestra balanceada por año; luego cada documento se proyecta a los tópicos y se normaliza a una composición (suma 1). Prevalencia del tópico *k* en el año *t* = media de esa composición en los documentos del año.
2. **Estabilidad.** Divergencia Jensen‑Shannon entre años consecutivos, comparada con la distribución nula obtenida al permutar las etiquetas de año entre los documentos de cada par, dentro de cada fuente (1.000 permutaciones, corrección Benjamini‑Hochberg). Además, curva de decaimiento de la similitud por rezago y una escala de referencia: la distancia entre fuentes en el mismo año.
3. **Cambios de régimen.** Segmentación binaria sobre la serie anual de prevalencias (raíz cuadrada, aproximación a la distancia de Hellinger), con coste de mínimos cuadrados ponderado por el N de cada año. Cada división solo se acepta si su ganancia supera el percentil 95 de la ganancia máxima al barajar el orden de los años: el porcentaje de años marcados no está fijado de antemano.
4. **Robustez.** Se repite sobre variantes y solo se aceptan cambios que aparecen en al menos la mitad (±1 año): otra semilla, K = 15, 20 y 30, y sin nombres propios. Semana, Dinero y una serie estandarizada (El Tiempo + Semana con pesos fijos) sirven como contraste independiente.

### 9.1 Los pasos, uno por uno

Cada paso se explica con la misma lógica: qué entra, qué se hace, qué sale y cómo se lee.

Paso 1. Muestra de ajuste y TF‑IDF. Entra: los 91.585 artículos tokenizados. Se toman hasta 1.000 artículos por año (unos 22.500 en total) para aprender el vocabulario y los pesos de rareza (IDF), y después se transforman todos los artículos con ese mismo vocabulario. Sin esa muestra balanceada, los más de 40.000 artículos de El Tiempo de 2000–2011 dominarían la estructura de los tópicos. Sale: una matriz artículos × 22.196 términos.

Paso 2. NMF (factorización de matrices no negativas). La matriz TF‑IDF V se aproxima como el producto de dos matrices con valores no negativos:

```latex
V \approx W\,H,\qquad V\in\mathbb{R}_{\ge 0}^{D\times 22196},\; W\in\mathbb{R}_{\ge 0}^{D\times K},\; H\in\mathbb{R}_{\ge 0}^{K\times 22196}
```

- Cada fila de H es un tópico: un peso por palabra. Un tópico se lee mirando sus 10 palabras de mayor peso (por ejemplo T05: farc, ejército, militares, guerrilleros, militar).
- Cada fila de W dice cuánto de cada tópico hay en un artículo. Como todo es no negativo, un artículo es una suma de tópicos y nunca una resta, lo que hace los tópicos interpretables como partes (Lee y Seung, 1999).
- El algoritmo minimiza el error de reconstrucción ‖V − WH‖ (norma de Frobenius, el valor por defecto de scikit‑learn) con hasta 150 iteraciones y tolerancia 10⁻³. La semilla 0 usa inicialización nndsvda y la semilla 1 una aleatoria; la comparación entre ambas mide la estabilidad.
- Se ajusta solo con la muestra balanceada y luego cada artículo del corpus se proyecta a los tópicos. Cada fila se divide por su suma y queda una composición θ: θ\_dk ≥ 0 y la suma de θ\_dk sobre k es 1. Ningún artículo quedó con vector vacío.
- Por qué NMF y no LDA: es rápido en CPU, determinista dada la semilla y trabaja directamente sobre TF‑IDF. LDA y BERTopic quedan como comparación futura.

Paso 3. Agenda = prevalencia. La prevalencia del tópico k en el año t es el promedio de θ sobre los artículos de ese año:

```latex
P_{k,t} = \frac{1}{|D_t|}\sum_{d\in D_t}\theta_{dk}, \qquad \sum_{k=1}^{K}P_{k,t}=1
```

Se lee como la fracción del corpus del año dedicada al tópico (por ejemplo 0,03 = 3 %). La agenda de un año es el vector de 40 prevalencias. Se calcula para el total y por fuente, y solo para años con al menos 40 artículos.

Paso 4. Distancia entre agendas: divergencia Jensen‑Shannon (JS). Compara dos distribuciones P y Q:

```latex
\mathrm{JS}(P\Vert Q)=\tfrac12\,\mathrm{KL}(P\Vert M)+\tfrac12\,\mathrm{KL}(Q\Vert M),\qquad M=\tfrac12(P+Q)
```

con logaritmo base 2, por lo que va de 0 (agendas idénticas) a 1 (sin ningún tópico en común). Para tener una idea de la escala: si en una agenda de dos temas el segundo pasa de 10 % a 11 % de la atención, JS ≈ 0,0002; si pasa de 10 % a 15 % (y otro baja de 20 % a 15 % en una agenda de tres), JS ≈ 0,006. La escala de referencia del proyecto es la distancia entre fuentes en el mismo año: JS = 0,0497 entre El Tiempo y Semana.

Paso 5. Cuánto es más que ruido: el modelo nulo. Aunque la agenda no cambiara, dos muestras finitas de artículos darían un JS mayor que cero. Para saber cuánto es ruido se procede así:

1. Se juntan los artículos de los años t y t+1 con sus θ.
2. Se barajan las etiquetas “año t” y “año t+1” entre esos artículos, pero solo dentro de cada fuente, para que la mezcla de fuentes de cada año quede intacta y se pruebe cambio dentro de la fuente.
3. Se recalcula el JS con las etiquetas barajadas. Se repite 1.000 veces y resulta una distribución nula: el JS que aparece solo por azar.
4. El p‑valor es (1 + número de JS nulos ≥ JS observado) / 1.001. Como se prueban 21 pares de años a la vez, se aplica la corrección de Benjamini‑Hochberg, que controla la proporción esperada de falsos descubrimientos en 5 %.
5. Se reporta también el exceso (JS observado menos la media del nulo) y el z.

Cómo se lee: un punto rojo en el gráfico es un año cuyo cambio supera el ruido. Con unos 4.000 artículos por año el nulo es muy estrecho (≈ 0,0008) y casi toda transición resulta significativa, así que lo que informa es la magnitud frente a la escala de referencia.

Paso 6. Deriva por rezago. Para cada rezago L = 1 a 15 se promedia el JS entre todos los pares de años separados por L años. Si la curva sube sin parar hay deriva acumulativa; si sube y se aplana, la variación está acotada; si queda pegada al piso de ruido, la agenda es estable. En El Tiempo pasa de 0,0029 (L = 1) a 0,0158 (L = 10) y queda plana desde L ≈ 8.

Paso 7. Puntos de cambio: segmentación binaria. Se parte de la serie anual de El Tiempo (22 años × 40 prevalencias) y se repite:

1. Se aplica raíz cuadrada a las prevalencias (aproxima la distancia de Hellinger y es coherente con JS).
2. Para cada posible año de corte se calcula cuánto baja el error cuadrático al reemplazar cada tramo por su promedio. Cada año pesa según su número de artículos (tope 3.000). Esa reducción es la ganancia del corte.
3. Se toma el mejor corte. Para decidir si es real, se baraja 200 veces el orden de los años, se calcula la mejor ganancia de cada barajada y se toma el percentil 95 como umbral. El corte se acepta solo si su ganancia lo supera.
4. Se repite dentro de cada tramo, con un mínimo de 2 años por tramo.

Sale: la lista de años en que empieza un nuevo régimen y su fuerza (ganancia dividida por el umbral). No hay un porcentaje fijo de años marcados, que era el defecto de la regla del cuaderno 04.

Paso 8. Robustez y consenso. Todo el paso 7 se repite en seis variantes de El Tiempo: la corrida principal (todos los artículos, K = 40), K = 15, K = 20, K = 30, K = 40 con otra semilla (inicialización aleatoria) y K = 40 sin entidades (se quitan las palabras con al menos 5 apariciones que en 50 % o más de ellas van en mayúscula a mitad de oración). Un año es cambio de consenso si en al menos la mitad de las variantes hay un cambio a ±1 año; los candidatos contiguos se colapsan en el de mayor soporte. Las variantes comparten el TF‑IDF y la muestra, así que no son independientes entre sí; por eso se agregan como contraste Semana, Dinero y una serie estandarizada (El Tiempo + Semana con pesos fijos), que sí son datos distintos.

Paso 9. Cuánta varianza explican el año y la fuente (η²). Para cada tópico se mide qué fracción de la variación de θ entre artículos se explica al agrupar por año (o por fuente): η² = suma de cuadrados entre grupos / suma de cuadrados total. Se promedia ponderando por el peso del tópico. Resultado: año 0,41 %, fuente 0,58 %. Son valores bajos porque cada artículo es muy ruidoso; sirven para comparar entre sí, no como medida absoluta.

Paso 10. Contraste histórico (el último). Se congeló una lista de nueve eventos antes de ver resultados. Se cuenta cuántos cambios de consenso caen a ±1 año de algún evento (3 de 3). Para saber si eso es mucho, se sortean 5.000 veces tres años al azar entre los años válidos y se cuenta lo mismo: el promedio es 2,5 de 3 y la probabilidad de obtener 3 de 3 por azar es 0,58.

### 9.2 Elección de K

**Elección de K.** Para cada K y dos semillas se mide la coherencia NPMI (top‑10 palabras) y la estabilidad entre semillas (similitud coseno media con asignación húngara).

| K | Coherencia NPMI | Coherencia mínima por tópico | Estabilidad entre semillas |
| --- | --- | --- | --- |
| 15 | 0,284 | 0,212 | 0,787 |
| 20 | 0,291 | 0,168 | 0,812 |
| 30 | 0,297 | 0,152 | 0,828 |
| 40 | 0,301 | 0,146 | 0,825 |

Se eligió K = 40 (máxima coherencia entre los K con estabilidad por encima de la mediana). La coherencia sigue subiendo con K sin alcanzar un máximo, y K = 40 es el borde de la grilla: es una elección práctica, no un óptimo.

Cómo se calculan las dos medidas. La coherencia NPMI de un tópico es el promedio de la NPMI de todos los pares de sus 10 palabras principales (45 pares):

```latex
\mathrm{NPMI}(a,b)=\frac{\ln\dfrac{p(a,b)}{p(a)\,p(b)}}{-\ln p(a,b)}\in[-1,1]
```

donde p(a,b) es la fracción de artículos de la muestra de ajuste que contienen ambas palabras y p(a), p(b) la de cada una. Un valor positivo indica que las palabras aparecen juntas más de lo esperado por azar (0,30 es un nivel claramente temático); la “coherencia mínima” es la del peor tópico. La estabilidad entre semillas ajusta dos modelos con semillas distintas, empareja sus tópicos uno a uno de la mejor manera posible (algoritmo húngaro) usando la similitud coseno entre sus vectores de palabras y promedia: 1 significa que ambos modelos encontraron los mismos tópicos y 0 que no comparten ninguno. Con K = 40 el 82 % de similitud promedio indica que la mayoría de los tópicos reaparecen, pero no todos.

### 9.3 Criterios para comparar modelos

**Criterios para comparar modelos.** Todo modelo candidato se juzga con los mismos criterios, que son los que se pueden medir con este corpus:

| Modelo | Estado | Coherencia | Estabilidad | Interpretable | Costo en Colab gratis | Sensible a fuente/entidades |
| --- | --- | --- | --- | --- | --- | --- |
| Léxico temático (13 categorías) | Implementado (05) | n/a | Determinista | Sí | Bajo | Sí, cubre 6,8 % de tokens |
| TF‑IDF + NMF | Implementado (06), base | 0,30 (K = 40) | 0,82 | Sí | \~5 min | Se mide con variante sin entidades |
| Embeddings + clustering / BERTopic | Propuesto | por medir | por medir | Parcial | GPU; solo sobre muestra | Menos sensible a nombres propios |
| Modelo de tópicos con covariables (STM) | Propuesto | por medir | por medir | Sí | Alto | Separa efecto de fuente y de tiempo |

Qué significa cada columna y cómo se mide:

- Coherencia: NPMI promedio de las 10 palabras principales de cada tópico (sección 9.2). Mide si los tópicos son legibles. Se compara entre modelos solo con el mismo corpus de ajuste.
- Estabilidad: similitud promedio entre los tópicos de dos corridas con semillas distintas (0 a 1). Mide si el modelo encuentra siempre lo mismo.
- Interpretable: si un humano puede leer el resultado sin otra herramienta. TF‑IDF + NMF sí (cada tópico es una lista de palabras); los embeddings no (cada dimensión es abstracta).
- Costo en Colab gratis: tiempo y memoria disponibles en el plan gratuito. Es un criterio práctico, no de calidad.
- Sensible a fuente y entidades: si el modelo confunde estilo editorial o nombres propios con temas; se estima repitiendo el análisis sin entidades y por fuente.
- Representación: macro‑F1 de periodo y fuente (sección 8.2), usado solo para elegir la entrada del modelo.
- Acuerdo de cambios: para cualquier modelo que se agregue, qué fracción de los cambios de consenso (1995, 1999–2000, 2006) reproduce a ±1 año. Un modelo nuevo solo suma evidencia si reproduce los cambios o explica por qué no.

Regla de decisión: se prefiere el modelo que mantiene coherencia y estabilidad al menos iguales a las del modelo base, reproduce o explica los cambios de consenso y sigue siendo interpretable. Los modelos marcados “por medir” no se corrieron en esta entrega.

## 10. Resultados (versión final del notebook 06)

**La serie que se puede interpretar es la de El Tiempo entre 1990 y 2011 (22 años, entre 807 y 6.351 documentos por año); Semana y Dinero sirven de contraste.** El pooled mezcla un cambio de fuente en 1989–1990 y por eso no se usa como serie principal.

### 10.1 Los tópicos

NMF con K = 40 produce tópicos temáticos legibles: conflicto (*farc ejército militares*), paz y derechos humanos, justicia (*corte justicia fiscalía*), Congreso, elecciones, fútbol, torneos, cine, arte, salud, educación, internet, banca, bolsa, petróleo, café, Venezuela y Estados Unidos. También aparecen tópicos que no son temáticos: T00 (*tan cómo vez*, estilo de opinión), T18 (nombres de pila), T31 (*realizará próximo mañana*, avisos de eventos), T37 (*semana fin pasada*) y varios de mercados (T10 bolsa, T17 café, T19 petróleo, T29 oro, T27 Nueva York) que fragmentan la sección económica. Son tópicos de sección o de género y entran en la agenda medida; es una limitación declarada.

&#91;image: Prevalencia de tópicos por año: área apilada y mapa de calor relativo a la media (pooled)\]

### 10.2 Estabilidad año a año

- En El Tiempo, **20 de 21 transiciones superan el ruido** (Benjamini‑Hochberg 5 %). El JS medio es 0,0029 y el ruido de las permutaciones ronda 0,0008. Con unos 4.000 documentos por año casi cualquier cambio es “significativo”, así que importa la magnitud, no el p‑valor.
- Esa magnitud es pequeña: **el cambio anual en El Tiempo es el 6 % de la distancia entre El Tiempo y Semana en el mismo año** (0,0029 frente a 0,0497). Entre Dinero y El Tiempo la distancia es 0,129 y entre Dinero y Semana 0,163.
- Los saltos mayores de El Tiempo son 2005→2006 (JS 0,0078, 2,7 veces la media), 1990→1991 (0,0057, afectado porque 1990 tiene solo 807 documentos), 1994→1995 (0,0043), 2008→2009 (0,0042) y 2001→2002 (0,0040).
- **En los años 80 ninguna transición es significativa** (Semana, 31–166 documentos por año): no hay potencia para decir que la agenda fue estable ni que cambió.

&#91;image: JS entre años consecutivos frente al ruido: pooled, El Tiempo y Semana (rojo = significativo, BH 5 %)\]

### 10.3 Deriva por rezago

En El Tiempo la divergencia entre años separados por *L* años crece de 0,0029 (L = 1) a 0,0105 (L = 5) y 0,0158 (L = 10), y **se queda en \~0,016 desde L = 8**. Es decir, la agenda se aleja gradualmente hasta cierto punto y luego no sigue alejándose: la variación está acotada. En Semana la curva sigue subiendo (0,038 a 15 años) y en Dinero llega a 0,08, aunque con pocos años y con crecimiento fuerte del volumen. La línea de “piso de ruido” del gráfico (0,0093) es el promedio de la serie pooled, inflado por los años 80 de N pequeño; para El Tiempo el piso real es \~0,0008.

&#91;image: Decaimiento de la similitud de agenda por rezago\]

### 10.4 Cuándo cambia: tres cambios de régimen en El Tiempo

**Los cambios de 1995, 1999–2000 y 2006 aparecen en las 6 variantes de El Tiempo; 2002 y 1992 aparecen en una sola, así que no se consideran.** Soporte = variantes con un cambio a ±1 año.

| Variante (El Tiempo) | Años en que empieza un nuevo régimen |
| --- | --- |
| Principal (todos los documentos, K = 40) | 1995, 2000, 2006 |
| K = 15 | 1992, 1995, 2000, 2006 |
| K = 20 | 1995, 2000, 2006 |
| K = 30 | 1995, 1999, 2006 |
| Otra semilla (K = 40) | 1995, 2000, 2006 |
| Sin nombres propios | 1995, 1999, 2002, 2006 |

Contraste con series independientes (±1 año respecto a los cambios de El Tiempo):

| Serie | Cambios detectados | Reproduce 1995 | Reproduce 1999–2000 | Reproduce 2006 |
| --- | --- | --- | --- | --- |
| Semana (1982–2011) | 1986, 1994, 1998, 2006, 2008 | Sí (1994) | Sí (1998) | Sí (2006) |
| Dinero (1998–2011) | 2007 | No | No | Sí (2007) |
| Estandarizada (El Tiempo + Semana, pesos fijos) | 1995, 1999, 2002, 2006 | Sí | Sí | Sí |

&#91;image: Prevalencia relativa de cada tópico en El Tiempo; líneas negras = cambios de consenso (1995, 1999, 2006)\]

Qué cambia en cada uno (diferencia de prevalencia media entre regímenes de El Tiempo, en puntos porcentuales):

| Cambio | Baja | Sube | Alerta |
| --- | --- | --- | --- |
| 1995 | EE. UU. (−1,1), policía (−1,0), torneos (−0,9), presidente‑gobierno (−0,8), elecciones (−0,8) | Internet (+1,2), empresas (+1,1), avisos de eventos (+0,8) | Cambio moderado; ningún tópico se mueve más de 1,2 puntos |
| 1999 | Regional Cali‑Valle‑Medellín (−1,4), avisos de eventos (−1,2), río‑municipio (−0,5) | Indicadores económicos (+0,7), FARC‑ejército (+0,6), comercio (+0,5), internet (+0,5) | El Tiempo cae a 2.231 documentos en 1999 (la mitad que en 1998): posible cambio de cobertura del archivo |
| 2006 | Empresas (−1,5), pago e impuestos (−1,2), bancos (−1,1), comercio (−1,1) | Casa y familia (+1,5), regional Cali‑Valle (+1,3), justicia (+1,1), policía (+1,0), fútbol (+0,9) | Es el salto más fuerte (JS 0,0078); coincide con el crecimiento de Dinero (de 167 a 560 documentos entre 2005 y 2007) |

**Lectura.** En 2006 la cobertura de El Tiempo pasa de economía y negocios a justicia, policía, región, familia y deporte. Una hipótesis plausible, no comprobada con estos datos, es que parte del contenido económico migró a la fuente Dinero (que despega justo entonces) o a otra sección no incluida en el corpus; si fuera así, el cambio sería editorial y de archivo, no un cambio de interés público. Por la caída de documentos en 1999, el cambio de 1999–2000 debe tomarse con la misma cautela.

### 10.5 La fuente pesa más que el tiempo

Entre El Tiempo y Semana en el mismo año el JS es 0,0497, 17 veces el cambio anual de El Tiempo y 3 veces la deriva máxima de 10 años (0,0158). Incluso a nivel de documento, la fuente explica más varianza de la composición de tópicos (η² = 0,58 %) que el año (0,41 %); los valores son bajos porque cada artículo es individualmente ruidoso. Esto confirma que comparar años sin controlar la fuente confunde cambio de agenda con cambio de fuente (el salto 1989→1990 del pooled, con JS 0,062, es simplemente la llegada de El Tiempo).

### 10.6 Contraste histórico (al final, con lista congelada)

Lista congelada antes de ver resultados: 1985 (Palacio de Justicia, Armero), 1989 (asesinato de Galán), 1991 (Constitución), 1993 (muerte de Escobar), 1996 (Proceso 8000), 1998 (Pastrana y Caguán), 2002 (Uribe), 2006 (reelección y parapolítica), 2010 (Santos, ola invernal).

- Los tres cambios caen a ±1 año de un evento: 1995 con 1996 (Proceso 8000), 1999 con 1998 (Pastrana y Caguán) y 2006 con la reelección de Uribe.
- Con esta lista, tres años elegidos al azar entre los años válidos tendrían en promedio 2,5 aciertos de 3, y la probabilidad de 3 de 3 por azar es 0,58. **La coincidencia no es evidencia a favor de los eventos**: la ventana de ±1 año cubre buena parte de la serie.
- Los eventos más politizados (1991, 1993, 2002) no producen un cambio de consenso, y lo que cambia en 1995 y 2006 es económico, regional y de orden público, no político. A resolución anual y con 40 tópicos, la agenda temática parece responder más a la estructura editorial que a las coyunturas.

## 11. Conclusiones respecto a la pregunta inicial

**Entre 1990 y 2011 la agenda temática de El Tiempo es muy estable a corto plazo, deriva de forma acotada y cambia de régimen en 1995, 1999–2000 y 2006; para 1980–1989 el corpus no permite responder.**

1. **Estabilidad.** El cambio entre años consecutivos (JS 0,0029) es el 6 % de la distancia entre fuentes (0,0497). Casi todas las transiciones son estadísticamente distintas de cero, pero con magnitud pequeña: ningún tópico cambia más de \~1,5 puntos porcentuales entre regímenes.
2. **Deriva.** La divergencia crece con el rezago (0,0029 → 0,0158 a 10 años) y se estabiliza desde \~8 años: la agenda se aleja gradualmente y luego queda acotada. No hay una tendencia sin límite en El Tiempo.
3. **Cuándo cambia.** Tres cambios sobreviven a K, semilla y exclusión de nombres propios (6/6 variantes) y reaparecen en Semana (1994, 1998, 2006) y, el de 2006, en Dinero (2007). El de 2006 es el más fuerte (JS 0,0078): se reduce la economía y los negocios y sube justicia, policía, región, familia y deporte.
4. **Qué significan.** Los cambios son de composición temática del corpus, y al menos dos (1999 y 2006) coinciden con cambios en el número de documentos o en la fuente que crece (Dinero). No se puede afirmar que reflejen un cambio del interés público y no de la cobertura o del archivo.
5. **Historia.** Los cambios caen a ±1 año de eventos de la lista congelada (1996, 1998, 2006), pero la probabilidad de esa coincidencia por azar es 0,58. Los eventos más políticos (1991, 1993, 2002) no dejan huella robusta a resolución anual.
6. **Fuente y tiempo.** La fuente pesa más que el tiempo. Por eso el pooled 1980–2011 no responde la pregunta: su mayor salto (1989→1990, JS 0,062) es la llegada de El Tiempo.
7. **Años 80.** Con solo Semana y 31–166 documentos por año, ninguna transición es significativa y los resultados descriptivos son ruidosos. Responder para 1980–1989 exige más fuentes o más documentos.

## 12. Limitaciones inherentes

**La mayor limitación es del corpus, no del método: una sola fuente continua (El Tiempo) y solo desde 1990 impiden separar agenda de cobertura.** Las demás limitaciones acotan lo que se puede afirmar.

### 12.1 Del corpus

- **Cobertura temporal desigual.** Los años 80 son solo Semana, con 31–166 documentos por año; falta 1981; El Tiempo empieza en 1990 con 807 documentos. El “cambio” de 1989 a 1990 es un cambio de fuente.
- **Fuente y tiempo confundidos.** Con una fuente por época no se puede aislar el efecto temporal; el contraste entre fuentes solo es posible en 1990–2011 y con N muy distinto (El Tiempo 81.539, Semana 6.620, Dinero 3.426).
- **Volumen y composición variables.** El Tiempo cae a 2.231 documentos en 1999 y 2.358 en 2006; Dinero crece de 167 a 560 en dos años. Una muestra del archivo digital no es una muestra del periódico: no sabemos qué secciones, ediciones o formatos faltan en cada año. La caída simultánea de MATTR hacia 2001 y 2004–05 en las tres fuentes sugiere cambios de formato.
- **Prevalencia, no importancia.** Un artículo cuenta igual sin importar si fue portada o nota breve, ni su audiencia. Mide qué se publicó, no qué le importó al público.
- **Sesgos de selección y de medio.** Tres medios (dos semanarios y un diario nacional con sede en Bogotá) no representan a la prensa colombiana; la prensa regional y la audiovisual no están.
- **Texto ya tokenizado y con ruido.** Cifras y horas partidas, caracteres de control, firmas, avisos y cartas al director mezclados con noticias. Un \~1 % de casi‑duplicados no se eliminó.
- **Resolución temporal.** Las fechas llevan siempre hora 04:00 o 05:00 (sin información); la unidad real es el día, y el análisis se hace por año por falta de documentos en periodos más cortos.
- **Sin etiquetas.** No hay temas anotados a mano: la validación de tópicos es interna (coherencia, estabilidad) y contra un léxico propio.

### 12.2 De las técnicas

- **NMF sobre unigramas, sin lematización.** Las formas de una misma palabra se dispersan y las expresiones (*derechos humanos*, *Corte Suprema*) se parten. Los bigramas calculados en el EDA no entran al modelo.
- **Tópicos de sección y de estilo.** Algunos tópicos son género (opinión, avisos, cotizaciones) y fragmentan una misma sección; su peso se confunde con “agenda”.
- **K y semilla.** NMF no es determinista y no hay K “correcto”: la coherencia sigue creciendo hasta K = 40 (borde de la grilla) y la estabilidad entre semillas es 0,82, no 1.
- **Entidades.** El proxy por mayúscula es imperfecto (pierde nombres al inicio de oración, incluye siglas y títulos). La variante sin entidades solo mide parte del efecto.
- **Prevalencia por promedio de composición.** Un artículo con varios temas se reparte entre tópicos; un artículo corto pesa igual que uno largo.
- **Nulo por permutación.** Supone documentos intercambiables dentro de fuente y año: ignora que los artículos de una misma edición o de un mismo evento están correlacionados, lo que subestima el ruido. Con N alto casi todo es “significativo” y hay que mirar la magnitud.
- **Segmentación binaria.** Es voraz, supone saltos en la media y descompone una deriva gradual en escalones; solo ve cambios a resolución anual y exige ≥ 2 años por tramo. El umbral 95 % se aplica a cada división sin corrección por múltiples pruebas.
- **Robustez limitada.** Las variantes comparten vectorizador, tokenización y familia de modelo; usan la muestra balanceada (menos N) y el contraste con otras fuentes depende de pocos años (Dinero 13).
- **Embeddings.** La comparación usa un modelo pequeño, solo los primeros 700 caracteres y \~4.000 documentos; no es concluyente sobre su valor semántico.
- **Contraste histórico.** Con ventana de ±1 año y 9 eventos, el azar ya produce 2,5 de 3 coincidencias: sirve para contar una historia plausible, no para probarla.

## 13. Mejoras propuestas

**Las mejoras que más cambiarían las conclusiones son las que separan agenda de cobertura: controlar por sección y por fuente, y quitar los tópicos de estilo.** Ordenadas por impacto esperado y esfuerzo:

| Prioridad | Mejora | Qué resuelve | Esfuerzo |
| --- | --- | --- | --- |
| 1 | Excluir o reagrupar tópicos de estilo (T00, T18, T31, T37) y fusionar los de mercados antes de medir JS | Evita que cambios de formato se lean como cambios de agenda | Bajo |
| 2 | Verificar 1999 y 2006 contra el archivo: número de documentos por sección y año, longitud media, secciones ausentes | Decide si son cambios editoriales, de archivo o de agenda | Medio |
| 3 | Modelo de tópicos con covariables (STM) con fuente y año | Separa el efecto de fuente del de tiempo en un solo modelo | Alto |
| 4 | Lematización con spaCy (`es_core_news_sm`, sin parser ni NER, `nlp.pipe` en lotes) y bigramas NPMI como features | Reduce dispersión léxica; recupera expresiones | Medio |
| 5 | BERTopic o clustering sobre embeddings (GPU) y comparar con NMF en coherencia, estabilidad y cambios | Valida si los cambios dependen del modelo de tópicos | Medio |
| 6 | Resolución trimestral (solo 1990–2011 en El Tiempo) con el mismo nulo; PELT en vez de segmentación binaria | Detecta cambios más rápidos; evita descomponer deriva en escalones | Medio |
| 7 | Intervalos de confianza *bootstrap* por prevalencia y corrección por múltiples pruebas en el umbral | Cuantifica incertidumbre de cada año y de cada cambio | Bajo |
| 8 | Validación externa: codificar a mano \~300 documentos (códigos CAP) y calcular acuerdo con tópicos y léxico | Da una medida de validez de los tópicos | Alto |
| 9 | Deduplicación más robusta (MinHash con varios umbrales) | Quita el \~1 % de casi‑duplicados | Bajo |
| 10 | Incorporar más fuentes para los años 80 | Permite responder la pregunta para 1980–1989 | Alto (datos) |

## 14. Reproducibilidad y repositorio

**Todo se reproduce en Colab gratis ejecutando `05` y luego `06`; cada notebook tarda unos 5 minutos con GPU T4 (la GPU solo acelera los embeddings).** El corpus se descarga con verificación SHA‑256 desde la release `corpus-v1` del repositorio y los notebooks escriben sus resultados en `results/` (en Colab, `/content/results`; el 06 genera además un zip).

Estructura propuesta del repositorio:

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
├── results/                  # salidas de 01-04
│   └── entrega2/
│       ├── 05_eda/           # diversidad, bigramas, léxico, figuras
│       └── 06_agenda/        # tópicos, prevalencia, transiciones, robustez, figuras, theta
└── docs/
    ├── informe_entrega2.pdf
    ├── presentacion_entrega2.pdf
    └── referencias/          # enunciado y artículo de Caicedo, Gaviria y Moreno (2012)
```

- **Se versiona:** notebooks, `utils.py`, CSV y figuras de `results/` (el `theta_documentos.csv.gz` pesa \~4 MB) e informe.
- **No se versiona:** el parquet del corpus y `corpus_medido.parquet` (superan los límites cómodos de GitHub); este último se publica como asset de una release (por ejemplo `dataset-medido-v1`), igual que el corpus.
- **Orden de ejecución:** `05` (EDA y dataset medido) → `06` (representación, tópicos, estabilidad, cambios, contraste histórico). Los dos son independientes en datos (cada uno carga el corpus) y deterministas con `SEED = 42`, salvo el entrenamiento de embeddings en GPU.

## Anexo A. Glosario de métodos y métricas

Una línea por término. Las fórmulas y ejemplos están en las secciones que se indican.

| Término | Qué responde | Cómo se calcula, en corto | Rango y lectura | Sección |
| --- | --- | --- | --- | --- |
| Token / tipo / hapax | Cuántas palabras hay, cuántas distintas y cuántas aparecen una sola vez | Token = cada palabra; tipo = palabra distinta; hapax = tipo con una aparición | 31,9 M tokens, 266.562 tipos, 42,3 % hapax | 5 |
| MATTR | Qué tan variado es el vocabulario sin depender de la longitud | Tipos distintos / 500 en una ventana deslizante, promediado | 0 a 1; más bajo = más repetición | 6.1 |
| Yule's K | Qué tan concentrado está el texto en pocas palabras | 10⁴ (Σ m² V\_m − N) / N² | Mayor K = más repetitivo (3 a 5,5 en este corpus) | 6.1 |
| Ley de Heaps (β) | Si el vocabulario se cierra o sigue creciendo | Ajuste log‑log de V = k·N^β | β ≈ 0,5: no se cierra | 6.1 |
| NPMI | Qué tan fuerte es la asociación entre dos palabras | ln(p(a,b)/(p(a)p(b))) / (−ln p(a,b)) | −1 a 1; >0 aparecen juntas más que el azar | 6.2, 9.2 |
| Log‑odds con prior de Dirichlet | Qué palabras distinguen a un grupo | Diferencia de log‑odds regularizada por el corpus, dividida por su error | z alto = distintiva y bien respaldada | 6.3 |
| PPMI | Con qué palabras aparece un concepto en el mismo artículo | máx(0, ln(p(a,b)/(p(a)p(b)))) a nivel de documento | Mayor = vecino más asociado | 6.4 |
| TF‑IDF | Qué palabras caracterizan un artículo | (1+ln tf) · (ln((1+N)/(1+df))+1), normalizado | Mayor = más específica del artículo | 8.1 |
| Precisión, recall, F1 | Qué tan bien se identifica una clase | P = VP/(VP+FP); R = VP/(VP+FN); F1 = 2PR/(P+R) | 0 a 1; F1 alto exige P y R altos | 8.2 |
| Macro‑F1 | Qué tan bien se identifican todas las clases por igual | Promedio simple de los F1 por clase | 0 a 1; el azar es \~0,25 (4 clases) o \~0,33 (3 clases) | 8.2 |
| Validación cruzada (5 particiones) | Cómo evaluar sin usar los datos de entrenamiento | Entrenar con 4/5 de los datos, evaluar con 1/5, repetir 5 veces | Promedio de los 5 resultados | 8.2 |
| NMF | Qué temas contiene el corpus | V ≈ W·H con todo ≥ 0 | H = tópicos (palabras), W = mezcla por artículo | 9.1 |
| θ (composición) | Cuánto de cada tema tiene un artículo | Fila de W dividida por su suma | Suma 1 | 9.1 |
| Prevalencia | Qué fracción del año ocupa cada tema (la agenda) | Promedio de θ de los artículos del año | 0 a 1; suma 1 por año | 9.1 |
| Coherencia NPMI | Si un tópico es legible | NPMI promedio de los 45 pares de sus 10 palabras principales | −1 a 1; \~0,30 es temático | 9.2 |
| Estabilidad entre semillas | Si el modelo encuentra siempre los mismos tópicos | Similitud coseno promedio entre tópicos emparejados (algoritmo húngaro) | 0 a 1; 0,82 con K = 40 | 9.2 |
| Divergencia JS | Qué tan distintas son dos agendas | ½KL(P‖M)+½KL(Q‖M), log base 2 | 0 a 1; 0,0029 entre años; 0,0497 entre fuentes | 9.1 |
| Modelo nulo por permutación | Cuánto JS aparece solo por azar | Barajar etiquetas de año dentro de cada fuente, 1.000 veces | Su percentil 95 es el umbral de ruido | 9.1 |
| Benjamini‑Hochberg | Cómo no declarar demasiados cambios por probar muchos años | Ajuste de p‑valores que controla la tasa de falsos descubrimientos al 5 % | p ajustado < 0,05 = significativo | 9.1 |
| Rezago | Cuánto se aleja la agenda con el tiempo | JS medio entre años separados por L años | Curva que sube y se aplana = variación acotada | 9.1 |
| Segmentación binaria | En qué años empieza un nuevo régimen | Mejor corte por reducción de error cuadrático, aceptado si supera el percentil 95 de cortes al azar | Años de cambio y su fuerza | 9.1 |
| Soporte / consenso | Qué tan robusto es un cambio | Fracción de variantes con un cambio a ±1 año | ≥ 50 % = consenso; aquí 6/6 | 9.1 |
| η² | Cuánta variación entre artículos explica el año o la fuente | Suma de cuadrados entre grupos / total | 0 a 1; 0,0041 (año), 0,0058 (fuente) | 9.1 |

## Anexo B. Cómo leer los gráficos

- Mapas de calor de prevalencia de tópicos: cada fila es un tópico y cada columna un año; el color es la prevalencia dividida por el promedio del propio tópico. Blanco = promedio, rojo = por encima (2 = el doble), azul = por debajo. Las columnas en blanco son años con menos de 40 artículos. Las líneas negras verticales son los cambios de consenso.
- JS contra el nulo: la línea azul es el JS observado entre cada año y el anterior; la línea discontinua negra es el JS que se espera por puro ruido; la banda gris llega al percentil 95 del ruido; los puntos rojos son transiciones significativas (BH < 5 %). Lo importante es la distancia entre la línea azul y la banda, y la escala del eje: en El Tiempo el eje llega a 0,008, mientras que en el panel de todas las fuentes llega a 0,08 por el salto de 1990.
- Curva de decaimiento: eje x = años de separación entre dos agendas; eje y = JS medio de todos los pares con esa separación. Una curva que sube y se aplana indica variación acotada.
- Mapa de calor léxico: igual que el de tópicos, pero cada fila es una categoría del léxico temático y el valor es tokens por mil, relativo al promedio de la categoría.
- MATTR y Yule's K: una línea por fuente; cada punto usa una muestra de 30.000 tokens. Los grupos sin muestra suficiente no aparecen.
