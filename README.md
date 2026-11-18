# Atraso en la titulación universitaria en Chile

Análisis estadístico y modelamiento predictivo de la duración real de las carreras de pregrado universitario en Chile, a partir del registro oficial de Titulados en Educación Superior 2025 del Ministerio de Educación (N = 105.063). El proyecto recorre el ciclo completo de un análisis de datos: preparación y calidad de datos, inferencia clásica, validación computacional por remuestreo y simulación, tratamiento de datos faltantes y regresión logística, con énfasis en la reproducibilidad y en la comunicación de resultados para la toma de decisiones.

Desarrollado en el curso MCDI501 *Estadística Computacional para la Toma de Decisiones* del Magíster en Ciencia de Datos e Inteligencia Artificial de la Universidad Andrés Bello (2026).

---

## Problema

¿Cuánto se aparta la duración real de las carreras universitarias de su duración formal, qué factores explican ese atraso y con qué precisión es posible anticipar un atraso alto (más de 4 semestres sobre la duración del plan)?

## Hallazgos principales

| Tema | Resultado |
| :--- | :--- |
| Magnitud del atraso | El titulado típico egresa con 2 semestres de atraso (mediana); la media es 3,40 semestres, IC 95 % [3,37; 3,43]. |
| Brecha por sexo | Los hombres se atrasan 1,07 semestres más que las mujeres (Welch, p < 0,001; d = 0,23). La brecha es robusta en el signo, pero cerca de la mitad se explica por el área de conocimiento: dentro de áreas se reduce a 0,55 semestres. |
| Heterogeneidad institucional | El error estándar jackknife por institución (0,111) es 3,5 veces el de Welch; en el modelo logístico, el bootstrap por universidad ensancha los intervalos entre 1,8 y 9,2 veces respecto de Wald. La incertidumbre entre instituciones domina sobre la del muestreo. |
| Potencia | Según simulación Monte Carlo (binomial negativa), la brecha por sexo solo es detectable con potencia de 80 % en cohortes de unos 700 titulados o más. |
| Datos faltantes | El 12,75 % de los registros no informa el año de ingreso original. El mecanismo es MAR, ligado a la jornada: 75,9 % de faltantes en A Distancia frente a 1,5 % en Diurna (razón 50:1). |
| Imputación simple | Imputar por la mediana invierte el signo de tres jornadas y de un área, e infla la tasa de atraso alto de 24,5 % a 32,7 %. Se descarta. |
| Modelo predictivo | El modelo logístico seleccionado (sexo, área, jornada y edad de ingreso; mínimo AIC y BIC) alcanza AUC = 0,662 y exactitud de 65,3 % en un conjunto de prueba común de casos observados (umbral de Youden 0,2246). Derecho (OR = 4,42), Agropecuaria (2,42) e Ingeniería (2,14) elevan el riesgo; ser mujer lo reduce (OR = 0,75). |
| Impacto de la imputación | La imputación por regresión distorsiona los odds ratios de jornada al omitir esa covariable. La imputación múltiple congeniada (m = 20, reglas de Rubin) reduce la discrepancia media con casos completos de 18,5 % a 7,3 % y calibra las probabilidades predichas. |

## Competencias y métodos

| Área | Métodos y herramientas |
| :--- | :--- |
| Preparación de datos | Filtrado del universo de análisis, códigos centinela a nulos, variables derivadas, detección de atípicos (Tukey) sin eliminación apresurada, modularización en `src/`. |
| Estadística inferencial | Intervalos *t*, prueba *t* de Welch, chi-cuadrado, tamaños de efecto (*d* de Cohen, V de Cramér), correlación de Spearman. |
| Estadística computacional | Bootstrap percentil y BCa (B = 10.000), pruebas de permutación, jackknife por clúster institucional, simulación Monte Carlo de potencia y convergencia. |
| Datos faltantes | Diagnóstico MCAR/MAR, eliminación por lista, imputación simple, imputación por regresión, imputación múltiple con reglas de Rubin, experimento controlado contra *ground truth*. |
| Modelamiento predictivo | Regresión logística, selección por AIC/BIC y stepwise, odds ratios con bootstrap por clúster, curva ROC, umbral de Youden, Brier score, prevención de fuga de datos. |
| Reproducibilidad y comunicación | Python (pandas, NumPy, SciPy, statsmodels, scikit-learn, matplotlib, seaborn), semilla fija 42, entorno virtual con versiones fijadas, informes en LaTeX, Git y GitHub con trabajo colaborativo por ramas y *pull requests*. |

## Recorrido del proyecto

| Fase | Foco | Notebook | Informe |
| :--- | :--- | :--- | :--- |
| Fase 1 | Ficha descriptiva: problema, variables y relaciones preliminares | — | [Ficha](docs/Ficha_Descriptiva.md) · [PDF](S1/informe/Entrega_Formativa1_Fase1_grupo_6.pdf) |
| Fase 2 | Calidad de datos, análisis exploratorio, estimación y pruebas de hipótesis | [Sumativa1_F2_Titulados](S1/notebooks/Sumativa1_F2_Titulados.ipynb) | [PDF](S1/informe/Informe_Sumativa1_F2.pdf) |
| Fase 3 | Validación computacional: bootstrap, permutación, Monte Carlo y robustez | [Sumativa2_F3_Titulados](S2/notebooks/Sumativa2_F3_Titulados.ipynb) | [PDF](S2/informe/Informe_Sumativa2_F3.pdf) |
| Práctica guiada | Flujo de modelamiento supervisado y experimento controlado de imputación | [Formativa2_Informe4](S3/notebooks/Formativa2_Informe4_Titulados_Grupo6.ipynb) | [PDF](S3/notebooks/Formativa2_Informe4_Titulados_Grupo6.pdf) |
| Fase 4 | Imputación, regresión logística e impacto de la imputación en el modelo | [S3_01](S3/notebooks/S3_01_Preparacion_Datos.ipynb) · [S3_02](S3/notebooks/S3_02_Regresion_Logistica.ipynb) · [S3_03](S3/notebooks/S3_03_Impacto_Imputacion.ipynb) | [PDF](S3/informe/Informe_Sumativa3_F4.pdf) |

Los resultados intermedios que conectan cada fase con la siguiente están versionados en `S2/resultados/` y `S3/resultados/`.

## Datos

Base oficial de **Titulados en Educación Superior 2025**, Servicio de Información de Educación Superior (SIES), Mineduc.

1. Descargar el conjunto *Educación Superior - Titulados en Educación Superior 2025* desde [datosabiertos.mineduc.cl](https://datosabiertos.mineduc.cl).
2. Guardar el archivo `20260817_Titulados_Ed_Superior_2025_WEB.csv` (separador punto y coma) en `data/raw/`.
3. Los notebooks aplican el filtro a pregrado universitario (`nivel_global == 'Pregrado'` y `tipo_inst_1 == 'Universidades'`), con lo que se obtienen los 105.063 registros analizados.

El archivo crudo no se versiona por su tamaño.

## Estructura del repositorio

```
data/raw/              archivo crudo del Mineduc (no versionado)
docs/                  ficha descriptiva del problema
S1/notebooks/          análisis exploratorio e inferencia
S1/informe/            informes de las fases 1 y 2, y figuras
S2/notebooks/          validación computacional
S2/informe/            informe de la fase 3 y figuras
S2/resultados/         resultados validados (insumo de la fase 4)
S3/src/                funciones de preparación reutilizables
S3/notebooks/          preparación, regresión logística e impacto de la imputación
S3/informe/            informe final integrado, secciones y presentación
S3/resultados/         métricas, odds ratios y comparaciones en CSV
requirements.txt       dependencias con versiones fijadas
```

## Cómo reproducir

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Con el archivo crudo en `data/raw/`, ejecutar los notebooks en el orden de la tabla anterior. En la fase 4, `S3_01_Preparacion_Datos.ipynb` genera los conjuntos imputados (formato parquet, no versionados) que usan los notebooks siguientes. Todos los procedimientos aleatorios usan semilla 42.

## Autoría y créditos

Repositorio creado y mantenido por **Jorge Álvarez Ossandón**, responsable del diseño metodológico, la implementación de los notebooks y la integración de resultados.

El proyecto se desarrolló como trabajo del Grupo 6 del curso MCDI501, junto a Fernanda Ovalle Román, Sebastián Cajales Cid y César Lorca Bacián, bajo la docencia de Jean Paul Maidana González.
