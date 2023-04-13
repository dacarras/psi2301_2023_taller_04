Taller 04
================
dacarras
Thu Apr 13, 2023

# Introducción

- En este taller vamos a cargar datos de una población finita existente.

- Con esto datos vamos a crear muestras aleatorias.

- También vamos a crear una distribución muestral de medias.

- Con esta distribución, vamos a ilustrar que son aplicables las
  formulas de que se desprenden del teorema del limite central.

- Vamos a crear intervalos de confianza.

- Y con estos intervalos, vamos a realizar inferencias intervalares.

- Este taller, replica lo presentado en clases, lo presente en el anexo
  `c06_M02_anexo_distribucion_muestral.pdf`, pero con otros datos de
  ejemplo.

- Finalmente, este taller esta construido para asistir al desarollo de
  la tarea 04.

# Información sobre los datos

- Los datos con los que vamos a trabajar consiste en registros de notas
  de todos los estudiantes de 4to medio, de 2022, de las escuelas en
  Chile. En particular, es el registros de estudiantes en escuelas
  “tradicionales” o tambien llamadas Humanistico-Cientificas (H-C). Este
  tipo de escuelas forma a los estudiantes para acceder a la educación
  superior.

- La fuente original de estos se puede acceder desde
  <https://datosabiertos.mineduc.cl/rendimiento-por-estudiante-2/>

- Los datos originales contienen los regristos de notas de promedio
  general de todos los cursos. En este taller, solo trabajaremos con los
  datos de 4to medio, de es estudiantes en establecimientos con
  Enseñanza Media H-C. Este registro contiene 144760 observaciones.

- El contenido de estos datos, es el siguiente:

<!-- -->


    AGNO       = Año del registro

    RBD        = Código RBD de la escuela

    LET_CUR    = Letra Curso

    COD_DEPE2  = Código de dependencia escolar
                 1 = Municipal
                 2 = Particular Subvecnionado
                 3 = Particular Pagado
                 4 = Coporación de Administración delegada
                 5 = Servicio Local de Educación

    COD_ENSE2  = 5 = Enseñanza Media Humanístico Científica Jóvenes.
    RURAL_RBD  = Indice de ruralidad del establecimiento
                 0 = Urbano
                 1 = Rural
    COD_JOR    = Jornada a la que asiste a clases
                 1 = Mañana
                 2 = Tarde
                 3 = Mañana y Tarde
                 4 = Vespertina/Noctura

    LET_CUR    = Letra del curso

    MRUN       = Máscara del RUN del alumno
                 Código único que identifica al estudiante en los registros nacionales.

    GEN_ALU    = Sexo del estudiante
                 0 = Sin información
                 1 = Hombre
                 2 = Mujer

    EDAD_ALU   = Edad al 30 de Junio del respectivo año escolar.
                 Edad en Años

    PROM_GRAL  = Promedio general anual.
                 Nota: No necesariamente corresponde al promedio aritmético de los promedios por subsector.
                 Para estudiantes retirados se informa promedio “0,0”.

    ASISTENCIA = Porcentaje anual de asistencia

    SIT_FIN_R  = Situación de promoción al cierre del año escolar, con indicador de traslado
                 P = Promovido 
                 R = Reprobado 
                 Y = Retirado 
                 T = Trasladado  
                 En blanco = Sin información
                 Nota: Trasladado: estudiante que se cambió de establecimiento (o a otro curso, en el mismo establecimiento) durante el año escolar

- El archivo que contiene los datos se llama:

<!-- -->


    data_notas_em4_2022.rds

- Estos datos pueden ser cargados en sesión con la condición de tener
  acceso a internet, utilizando el siguiente codigo:

``` r
file_url <- url('https://github.com/dacarras/psi2301_2023_taller_04/raw/main/data_notas_em4_2022.rds')
data_notas <- readRDS(file_url)
```

------------------------------------------------------------------------

# Taller 04: Distribución muestral de medias.

## Ejercicio 1. Abrir los datos.

- Para poder crear una distribución muestral de medias, necesitamos una
  población de valores.

  - Esta población podría ser una población infinita de valores, para
    simular escenarios de inferencia basada en modelos.

  - Esta población, podria ser una población finita. Es decir, un
    conjunto de valores de al menos un atributo, de un conjunto de
    observaciones seleccionables.

  - En este taller, vamos a emplear la población finita de todos los
    estudiantes “jóvenes” de cuarto de enseñanza media de Chile, en
    escuelas cientifico humanistas.

- En el siguiente paso, vamos a abrir los datos, desde el link del
  repositorio que aloja los archivos de este taller.

  - Este repositorio se encuentra en:
    <https://github.com/dacarras/psi2301_2023_taller_04>

  - El link del archivo es:
    ‘<https://github.com/dacarras/psi2301_2023_taller_04/raw/main/data_notas_em4_2022.rds>’

``` r
file_url <- url('https://github.com/dacarras/psi2301_2023_taller_04/raw/main/data_notas_em4_2022.rds')
data_notas <- readRDS(file_url)

# Notas: este código posee dos pasos. Primero creamos un objeto, que guarda la ubicación del archivo que queremos abrir.
#        Este objeto es "file_url". El segundo objeto que vamos a crear, es "data_notas".
#        Este último objeto es una base de datos, que se produce al abrir el archivo "data_notas_em4_2022.rds".
#        El archivo que aloja los datos, es un archivo tipo "rds", el cual es un formato para guardar objetos únicos en R.
#        Para abrir este archivo empleanos la función "readRDS()". Esta función sirve para abrir este tipo de archivos.
```

## Ejercicio 1.1. Vista previa de los datos.

``` r
dplyr::glimpse(data_notas)
```

    ## Rows: 144,760
    ## Columns: 13
    ## $ AGNO       [3m[38;5;246m<int>[39m[23m 2022, 2022, 2022, 2022, 2022, 2022, 2022, 2022, 2022, 2022,…
    ## $ RBD        [3m[38;5;246m<int>[39m[23m 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4,…
    ## $ LET_CUR    [3m[38;5;246m<chr>[39m[23m "A", "A", "A", "A", "A", "A", "A", "A", "A", "A", "A", "A",…
    ## $ COD_DEPE2  [3m[38;5;246m<int>[39m[23m 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5,…
    ## $ COD_ENSE2  [3m[38;5;246m<int>[39m[23m 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5,…
    ## $ RURAL_RBD  [3m[38;5;246m<int>[39m[23m 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ COD_JOR    [3m[38;5;246m<int>[39m[23m 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3,…
    ## $ MRUN       [3m[38;5;246m<int>[39m[23m 34574, 262460, 1237591, 2370451, 2636357, 2673946, 2780597,…
    ## $ GEN_ALU    [3m[38;5;246m<int>[39m[23m 2, 1, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 1,…
    ## $ EDAD_ALU   [3m[38;5;246m<int>[39m[23m 17, 19, 17, 17, 17, 17, 17, 17, 17, 17, 17, 17, 18, 18, 17,…
    ## $ PROM_GRAL  [3m[38;5;246m<dbl>[39m[23m 6.9, 5.4, 6.7, 7.0, 5.7, 5.7, 7.0, 6.8, 0.0, 7.0, 6.6, 6.8,…
    ## $ ASISTENCIA [3m[38;5;246m<int>[39m[23m 98, 97, 98, 100, 95, 85, 100, 100, 0, 100, 98, 98, 100, 100…
    ## $ SIT_FIN_R  [3m[38;5;246m<chr>[39m[23m "P", "P", "P", "P", "P", "P", "P", "P", "T", "P", "P", "P",…

``` r
# Notas: dplyr::glimpse() es  una función que nos sirve para producir una vista previa de datos.
#        En la primera fila nos indica la catidad de filas de la tabla, y por tanto de la cantidad de observaciones que contiene la tabla.
#        En la segunda fila, la salida de esta función nos indica la cantidad de columnas de la tabla de datos.
#        Luego, la salida de esta función sigue con la lista de variables contenidas en la base de datos `data_notas_em4_2022.rds`.
#        Adicionalmente, nos entrega el tipo de variables contenidas en la tabla, con los simbolos
#        <int>, <chr> y <dbl>. Estos son tres tipos de variables diferentes, el primero <int> refiere a valores "integer", que
#        los cuales son valores numericos discretos (i.e., sin decimales). Por su parte, los valores <dbl> son "double", y estos
#        son valores numéricos continuos. Finalmente <chr>, son variables tipo "character". Estos contienen variables en registro de texto.
#        Tradicionalmente, este tipo de registro se emplea para alojar variables de tipo nominal.
```

## Ejercicio 2. Parámetros poblacionales.

- Para crear una distribuciób muestral, y mostrar algunas de sus
  propiedades, necesitamos calcular los parámetros poblacionales.

- Necesitamos crear la media, y la desviación estándar, y la varianza de
  la población.

  - La media poblacional se simboliza con la letra $\mu$

  - La desviación estándar de la población la se representa con la letra
    griega $\sigma$

  - La varianza de la población se la representa como el cuadrado de la
    desviación estandar, $\sigma^2$

- Vamos a calcular estas cifras, respecto a la variable `PROM_GRAL`.

- Esta variable contiene las notas de promedio general de los
  estudiantes.

- Los parámetros poblacionales, los utilizaremos en ejercicios
  posteriores.

``` r
# media poblacional
media_poblacional <- mean(data_notas$PROM_GRAL, na.rm = TRUE)
media_poblacional
```

    ## [1] 5.908

``` r
# desviación estandar de la población
desviacion_poblacional <- sd(data_notas$PROM_GRAL, na.rm = TRUE)
desviacion_poblacional
```

    ## [1] 1.33379

``` r
# varianza de la población
varianza_poblacional <- var(data_notas$PROM_GRAL, na.rm = TRUE)
varianza_poblacional
```

    ## [1] 1.778996

``` r
# Nota: las funciones de R, sd() y var() calculan la desviación estandar y varianza de la muestra.
#       Para este ejemplo, ocuparemos estas cifras porque las diferencias son poco sustantivas.
#       Las difernecias entre ambos estimados se observan al quinto y sexto decimal.
#       En Anexo 2.1 deje unas notas con una explicación más larga de como obtener
#       los resultados de la población.
```

## Ejercicio 3. Obtener una muestra aleatoria simple.

- Vamos a generar una muestra de 400 casos. 07j

``` r
# crear muestra de un tamaño determinado
set.seed(20230413)
muestra_n400 <- dplyr::slice_sample(
                data_notas,
                n = 400, 
                replace = TRUE
                )

# Nota: la función dplyr::slice_sample(), permite crear muestras aleatorias simples.
#       Esta función toma como argumentos a "n = ", con el cual podemos especificar el tamaño de la muestra que queremos.
#       Además, con el argumetno "replace = TRUE", le indicamos a R que pueda generar muestras aleatorias,
#       que tengan la misma probabilidad de ocurrir. Es decir que, las muestras generadas, podrían repetir
#       observaciones entre cada muestreo, replica del ejercicio.
#       Previo a la generacion de la muestra, fijamos el seed, para poder replicar los resultados generados.
```

## Ejercicio 4. Media e intervalo de confianza

- Con la muestra anterior, vamos a calcular la media, y un intervalo de
  confianza sobre la media de esta muestra.

- Como atajo, vamos a emplear un modelo de regresión nulo.

``` r
# extraer el intervalo de confianza sobre la media
lm(PROM_GRAL ~ 1, data = muestra_n400) %>%
confint()
```

    ##               2.5 %  97.5 %
    ## (Intercept) 5.77003 6.03597

``` r
# Nota: el intercepto del modelo nulo, sobre una muestra genera la media de la variable de respuesta.
#       empleando confint(), podemos obtener el intervalo de confianza alreder del intecepto.
#       Este intervalo de confianza, esta generado con una distribucion t. Dado el tamaño de
#       la muestra empleada (n = 400), este intervalo es muy similar a haberlo generado con una distribución z.
#       En el en Anexo 3.1 dejo otra forma de obtener el intervalo de confianza para una media de una muestra.
```

## Ejercicio 5. Crear una colección de muestras.

- Vamos a generar una distribución muestral de 500 muestras, de 400
  observaciones cada una.

  - Antes de generar una distribución muestral, primero necesitamos
    producir un conjunto de muestras aleatorias simples, con reemplazo.

- El siguiente código, con pocas lineas no permite repetir la accion de
  generar una muestra aleatoria, unas “n” veces.

- Y además, que las muestras generadas fueran, unidas como unas sola
  tabla.

  - Si lo hicieramos manualmente, tendriamos que generar primero 100
    muestreas aleatorias.

  - Y luego, unir estas cien muestras aleatorias.

- `purrr::map_df()` es una función que sirver para que una función
  determinada, sea aplicada sobre una lista.

  - Y el resultado de esta aplicación, sea entregado como una sola tabla
    de resultados.

  - Esta función es especialmente útil para escenarios en que se
    requiere “iterarar” n veces, una misma operación.

``` r
set.seed(20230413)
numero_de_muestras <- 500

lista_de_muestras <- 1:numero_de_muestras

coleccion_de_muestras <- purrr::map_df(lista_de_muestras, 
  ~ dplyr::slice_sample(data_notas, n = 400, replace = TRUE),
   .id = 'sample') %>%
dplyr::glimpse()
```

    ## Rows: 200,000
    ## Columns: 14
    ## $ sample     [3m[38;5;246m<chr>[39m[23m "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1",…
    ## $ AGNO       [3m[38;5;246m<int>[39m[23m 2022, 2022, 2022, 2022, 2022, 2022, 2022, 2022, 2022, 2022,…
    ## $ RBD        [3m[38;5;246m<int>[39m[23m 769, 9078, 20181, 14783, 26269, 12603, 16728, 24976, 25855,…
    ## $ LET_CUR    [3m[38;5;246m<chr>[39m[23m "B", "A", "C", "B", "A", "C", "A", "A", "B", "B", "A", "B",…
    ## $ COD_DEPE2  [3m[38;5;246m<int>[39m[23m 2, 1, 2, 2, 2, 2, 2, 3, 1, 2, 2, 1, 3, 1, 2, 2, 2, 1, 2, 3,…
    ## $ COD_ENSE2  [3m[38;5;246m<int>[39m[23m 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5, 5,…
    ## $ RURAL_RBD  [3m[38;5;246m<int>[39m[23m 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ COD_JOR    [3m[38;5;246m<int>[39m[23m 1, 1, 3, 3, 3, 3, 3, 3, 3, 1, 3, 3, 1, 3, 3, 1, 3, 3, 3, 3,…
    ## $ MRUN       [3m[38;5;246m<int>[39m[23m 19356346, 6516136, 11669840, 15538980, 22189478, 2582970, 2…
    ## $ GEN_ALU    [3m[38;5;246m<int>[39m[23m 1, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 1, 1, 2, 2, 2, 2, 2, 2,…
    ## $ EDAD_ALU   [3m[38;5;246m<int>[39m[23m 18, 17, 18, 18, 17, 17, 18, 17, 17, 17, 17, 18, 17, 17, 18,…
    ## $ PROM_GRAL  [3m[38;5;246m<dbl>[39m[23m 6.4, 5.8, 6.4, 5.8, 5.6, 6.6, 6.8, 6.0, 6.3, 6.0, 6.4, 5.9,…
    ## $ ASISTENCIA [3m[38;5;246m<int>[39m[23m 97, 82, 71, 92, 97, 95, 86, 91, 96, 97, 95, 85, 98, 81, 93,…
    ## $ SIT_FIN_R  [3m[38;5;246m<chr>[39m[23m "P", "P", "P", "P", "P", "P", "P", "P", "P", "P", "P", "P",…

## Ejercicio 6. Crear una distribución muestral de medias

- Para crear una distribución muestral de medias, se requiere estimar
  una media por cada muestra generada.

- Empleando a `coleccion_de_muestras`, vamos a calcular las medias de
  cada muestra.

- Vamos a guardar esta tabla en un objeto llamado
  `distribucion_de_medias`.

- La media de cada muestra, las alojaremos en una columna llamada
  `stat_mean`

- Si generamos 100 muestras, entonces en este paso, debieramos obtener
  100 medias.

``` r
# crear distribución de medias
distribucion_de_medias <- coleccion_de_muestras %>%
group_by(sample) %>%
summarize(
  stat_mean = mean(PROM_GRAL)
)


# muestra de 10 muuestras.
distribucion_de_medias %>%
dplyr::slice_sample(n = 10) %>%
knitr::kable(., digits = 2)
```

| sample | stat_mean |
|:-------|----------:|
| 167    |      5.88 |
| 64     |      5.94 |
| 90     |      5.94 |
| 491    |      5.84 |
| 227    |      5.84 |
| 350    |      5.94 |
| 462    |      5.88 |
| 91     |      5.98 |
| 380    |      5.82 |
| 207    |      5.85 |

- **Respuesta**
  - Cantidad de medias:
    `#....indicar su respuesta en esta línea, y borrar este comentario.`

## Ejercicio 7. Media y desviación estándar de la distribución muestral

- Por teorema del limite central y ley de los grandes numeros, la media
  de la dustribución muestral debiera estar muy cercana al parámetro
  poblacional.

- Por su parte, la desviación estandar de las medias en la distribución
  muestrals nos entrega una medida de la dispersión de todas las medias
  posibles.

``` r
# media de la distribución muestral
distribucion_de_medias %>%
summarize(
mean = mean(stat_mean),
sd = sd(stat_mean),
) %>%
knitr::kable(., digits = 2)
```

| mean |   sd |
|-----:|-----:|
| 5.91 | 0.07 |

``` r
# media de la población
media_poblacional
```

    ## [1] 5.908

## Ejercicio 8. Error estandar de media.

- Recordemos que el error estandar de la media, es desviación estandar
  de las medias en la distribución muestral.

- Este término puede ser calculado con la siguiente operación

$$\frac{\sigma}{sqrt{n}}$$

- Vamos a generar este numero, empleando a la distribución muestral, y
  por medio de la formula.

``` r
# desviación estandar de las medias de la distribución muestral.
distribucion_de_medias %>%
summarize(
sd = sd(stat_mean),
) %>%
knitr::kable(., digits = 2)
```

|   sd |
|-----:|
| 0.07 |

``` r
# error estandar, calculado por medio de formula
desviacion_poblacional/sqrt(400)
```

    ## [1] 0.06668951

## Ejercicio 9. Calcule el margen de error

- El margen de error, se puede calcular multiplicando a la desviación
  estandar de las medias de la distribución muestral, por la cantidad de
  veces que, en una distribución probabilística se concentraran la
  cantidad de casos que se quieran.

- Tradicionalmente, este punto critico es 1.96 en el caso de la
  distribución Z.

- En el caso de la distribución t, este valor varía segun los grados de
  libertad en juego en cada caso.

- Podemos obtener estos puntos criticos con diferentes funciones en R.

  - En el caso de la distribución Z, podemos obtener este punto con la
    función `qnorm()`

    - `z_critico <- qnorm(p = 0.975)` nos entrega a 1.96

    - Este es el punto de 1.96, el cual separa el 2.5% de todos los
      casos posibles, en el lado superior de la distribución normal
      estandarizada.

    - Y -1.96, es el punto que separa al 2.5% de todos los valores
      posibles, en el lado inferior de la distribución normal
      estandarizada.

  - En el caso de la distribución t, podemos obtener este punto con la
    función `qt()`

    - `t_critico <- qt(p = 0.975, df = tamaño_muestral - 1)` nos entrega
      al valor -1.97, si el tamaño muestral fuera 400 observaciones.

    - -1.97 es el valor en la distribución t (de 399 grados de libertad,
      o de 400 observaciones para una media), donde bajo este punto
      habrían 2.5% de los valores posibles.

- A continuación calculamos el margen de error, empleando a los valores
  de z, y en el Anexo 9.1 dejamos un ejemplo con la distribución t.

``` r
# especificamos el tamaño muestral
tamaño_muestral <- 400

# calculamos el error estandar
error_estandar <- desviacion_poblacional / sqrt(tamaño_muestral)
error_estandar
```

    ## [1] 0.06668951

``` r
# calculamos el valor crítico de la distribución probabilística
z_critico <- qnorm(p = 0.975)

# Nota: la formula del error estandar de la media, en la distribución
#       muestral, es la desviación estandar de la población,
#       dividido por la raiz cuadrada del tamaño de la muestra,
#       con la cual fuera generada la distribución muestral.

# calculamos al margen de error
margen_de_error <- z_critico*error_estandar
margen_de_error
```

    ## [1] 0.130709

``` r
# Nota: Una forma de pensar al margen de error, es la cantidad de
#       veces que tiene que moverse la desviación estandar de la media,
#       en la distribución muestral, para que se asemeje a la distribución
#       normal estandarizada. Dicho de otro modo, empleamos el valor
#       critico de la distribución probabilístico, de z en este caso,
#       para expandir a la dispersión de la media en la distribución
#       muestral, y con esto capturar un % de las veces al parámetro
#       de la población.
```

## Ejercicio 10. Intervalos de confianza al 95%

- El margen del error, es la distancia con la cual tenemos que
  extendernos desde la media de una muestra, para generar un rango.

- Este rango, que consiste en tener dos valores, uno de limite superior,
  y uno de limite inferior, nos permiten capturar al parámetro
  poblacional del interes un % de las veces, en la distribución
  muestral.

- Por convención, estamos construyendo intervalos de confianza de 95%.

- Empleando el margen de error, calculado en el ejercicio anterior,
  podemos sumar y restar ese valor a la media de la muestra, y obtener
  los intervalos de confianza que queremos.

- Vamos a crear dos variables, sobre la distribución de medias.

  - `ll` es el limite inferior del intervalo

  - `ul` es el limite superior del intervalo

- Vamos a crear una nueva tabla de datos, que llamaremos
  `medias_con_ci`, la cual va a tener como contenidos, a

  - la media de cada muestra generada

  - los limites inferior, y superior de cada intervalo de confianza de
    95% para cada media.

``` r
# calculo de limites de los intervalos
medias_con_ci <- distribucion_de_medias %>%
mutate(ll = stat_mean - margen_de_error) %>%
mutate(ul = stat_mean + margen_de_error) %>%
dplyr::glimpse()
```

    ## Rows: 500
    ## Columns: 4
    ## $ sample    [3m[38;5;246m<chr>[39m[23m "1", "10", "100", "101", "102", "103", "104", "105", "106", …
    ## $ stat_mean [3m[38;5;246m<dbl>[39m[23m 5.90300, 5.92025, 5.97425, 5.87825, 5.90775, 6.05125, 5.9127…
    ## $ ll        [3m[38;5;246m<dbl>[39m[23m 5.772291, 5.789541, 5.843541, 5.747541, 5.777041, 5.920541, …
    ## $ ul        [3m[38;5;246m<dbl>[39m[23m 6.033709, 6.050959, 6.104959, 6.008959, 6.038459, 6.181959, …

``` r
# muestra de 20 valores de la tabla generada
medias_con_ci %>%
dplyr::slice_sample(n = 20) %>%
arrange(stat_mean) %>%
knitr::kable(., digits = 2)
```

| sample | stat_mean |   ll |   ul |
|:-------|----------:|-----:|-----:|
| 197    |      5.75 | 5.62 | 5.88 |
| 304    |      5.79 | 5.66 | 5.92 |
| 227    |      5.84 | 5.71 | 5.97 |
| 185    |      5.85 | 5.72 | 5.98 |
| 485    |      5.86 | 5.73 | 5.99 |
| 106    |      5.87 | 5.74 | 6.00 |
| 261    |      5.88 | 5.75 | 6.01 |
| 372    |      5.88 | 5.75 | 6.01 |
| 26     |      5.88 | 5.75 | 6.01 |
| 422    |      5.90 | 5.77 | 6.03 |
| 169    |      5.91 | 5.78 | 6.04 |
| 331    |      5.92 | 5.79 | 6.05 |
| 225    |      5.93 | 5.80 | 6.07 |
| 64     |      5.94 | 5.81 | 6.07 |
| 213    |      5.96 | 5.83 | 6.10 |
| 36     |      5.98 | 5.85 | 6.11 |
| 369    |      5.99 | 5.86 | 6.12 |
| 368    |      6.01 | 5.88 | 6.14 |
| 387    |      6.02 | 5.89 | 6.15 |
| 481    |      6.03 | 5.90 | 6.16 |

## Ejercicio 11. Porcentaje de CI de medias que contiene a la media poblacional

- Ahora, para mostrar que los intervalos de confianza creados, capturan
  al parámetro poblacional la cantidad de veces esperada, vamos a
  clasificar a los intervalos de confianza, según si contienen a no a la
  media poblacional.

  - Vamos a decir que un intervalo contiene a la media poblacional, si
    el limite superior de cada intervalo es mayor o igual a la media
    poblacional.

  - Complementariamente, vamos a decir que el intervalo de confianza
    contiene a la media poblacional, si el limite inferior es menor o
    igual a la media poblacional.

  - En términos logicos, podemos tambien decir lo inverso:

    - Un intervalo de confianza, no contiene a la media poblacional si
      es el caso que el limite superior del intervalo, es menor a la
      media.

    - O, que el limite inferior del intervalo no puede con tener a la
      media si el límite inferior es mayor a la media poblacional.

  - Vamos a emplear esta segunda versión de la afirmación para
    clasificar a todos los intervalos de confianza, segun contengan a no
    a la meida poblacional.

  - Vamos a crear una variable *dummy*, de valores “sí” y “no”, para
    indicar que un intervalo contiene a la media poblacional. Vamos a
    llamar a esta variable “status”.

  - Vamos a crear esta variable, sobre la tabla de datos anterior, sobre
    `medias_con_ci`

- Una vez clasificados todos los intervalos de confianza, vamos a
  calcular el porcentaje de los intervalos que contiene a la media
  poblacional.

``` r
# recordemos cual es la media poblacional
media_poblacional
```

    ## [1] 5.908

``` r
# generemos la variable status
medias_con_ci <- medias_con_ci %>%
mutate(status = case_when(
  media_poblacional > ul | media_poblacional < ll ~ 'no',
  TRUE ~ 'yes'
  )) %>%
dplyr::glimpse()
```

    ## Rows: 500
    ## Columns: 5
    ## $ sample    [3m[38;5;246m<chr>[39m[23m "1", "10", "100", "101", "102", "103", "104", "105", "106", …
    ## $ stat_mean [3m[38;5;246m<dbl>[39m[23m 5.90300, 5.92025, 5.97425, 5.87825, 5.90775, 6.05125, 5.9127…
    ## $ ll        [3m[38;5;246m<dbl>[39m[23m 5.772291, 5.789541, 5.843541, 5.747541, 5.777041, 5.920541, …
    ## $ ul        [3m[38;5;246m<dbl>[39m[23m 6.033709, 6.050959, 6.104959, 6.008959, 6.038459, 6.181959, …
    ## $ status    [3m[38;5;246m<chr>[39m[23m "yes", "yes", "yes", "yes", "yes", "no", "yes", "yes", "yes"…

``` r
# Nota: hay varias manerar de obtener a la variable status, ver Anexo 11.1


# calculemos la proporcion de intervalos que captura la media poblacional.
medias_con_ci %>%
summarize(
proportion = mean(status == 'yes')
) %>%
knitr::kable(., digits = 2)
```

| proportion |
|-----------:|
|       0.94 |

## Ejercicio 12. Verificar que una muestra contiene a la media de la población.

- Volvamos a nuestra muestra inicial de 400 casos.

- **¿Contiene esta muestra al parámetro poblacional?**

``` r
# media poblacional
media_poblacional
```

    ## [1] 5.908

``` r
# crear muestra de un tamaño determinado
set.seed(20230413)
muestra_n400 <- dplyr::slice_sample(
                data_notas,
                n = 400, 
                replace = TRUE
                )

# extraer el intervalo de confianza sobre la media
lm(PROM_GRAL ~ 1, data = muestra_n400) %>%
confint()
```

    ##               2.5 %  97.5 %
    ## (Intercept) 5.77003 6.03597

- **¿Qué implica que una muestra aleatoria contenga al parámetro
  poblacional?**

- **¿Qué implica sólo 95% de las muestras pueden contener al parámetro
  poblacional?**

# Anexos

## Anexo 2.1: Cálculo de la desviación estandar y varianza de la población.

Las funciones `sd()` y `var()` calculan la desviación estándar de la
muestra, y la varianza de la muestra respectivamente. Empleando
información acerca del tamaño de los datos, i.e., la cantidad de las
observaciones, es posible convertir a los estimados de la muestra, en
los estimados de la población.

``` r
# tamañod de la población
n_pop <- nrow(data_notas)


# estimados de la muestra
sd_sam <- sd(data_notas$PROM_GRAL, na.rm = TRUE)
var_sam <- var(data_notas$PROM_GRAL, na.rm = TRUE)

# estimados de la poblacion
var_pop <- var(data_notas$PROM_GRAL, na.rm = TRUE) * (n_pop-1)/n_pop
sd_pop  <- sd(data_notas$PROM_GRAL, na.rm = TRUE) * sqrt((n_pop-1)/n_pop)


# mostrar ambos estimados
data.frame(
estimados = c('sd', 'sd', 'var', 'var'),
tipo = c('muestral', 'poblacional', 'muestral', 'poblacional'),
valores = c(sd_sam, sd_pop, var_sam, var_pop)
) %>%
knitr::kable(., digits = 6)
```

| estimados | tipo        |  valores |
|:----------|:------------|---------:|
| sd        | muestral    | 1.333790 |
| sd        | poblacional | 1.333786 |
| var       | muestral    | 1.778996 |
| var       | poblacional | 1.778984 |

## Anexo 3.1: Cálculo de intervalos de confianza para descriptivos

- Existen diferentes formas en R de obtener el intervalo de confianza
  alrededor de una cifra.

- Una de ellos, es emplear el modelo de regresion, y sobre este
  solicitar los intervalos de confianza.

  - Como el intercepto del modelo nulo, es la media de la variable de
    respuesta, los intervalos de confianza alrededor del intercepto del
    mdoelo nulo, permiten obtener el intervalo de confianza alrededor de
    una media.

- Otra forma de producir intervalos de confianza, es emplear una una
  libreria de de datos que genera descriptivos para diferentes tipos de
  muestras (e.g., aleatorias simples, muestras estratificas, muestras
  con datos anidas, muestras con seleccion no proporcional al tamaño, y
  otras muestras complejas).

  - Este tipo de librerias, siempre incluyen opciones para obtener
    medidas de incertidumbre alrededor de los estadigrafos, como el
    error estándar, la varianza, y los intervalos de confianza.

  - Una de estas librerias es `srvyr`.

  - En esta libreria se puede especificar que los datos corresponden a
    una muestra aleatoria simple, y ajusta los procedimientos de calculo
    de los descriptivos, de modo tal que produce errores estandar, e
    intervalos de confianza, para estadigrafos generados con este tipo
    de muestras.

- Una tercera manera de producir intervalos de confianza alrededor de un
  estadigrafo, seria calcular los intervalos de forma manual.

- La gracia de conocer prccedimientos como los anteriores, es contar con
  alternativas para evitar producir resultados de forma manual, cuando
  no fuera necesario.

- En el siguiente código se incluyen formas de obtener CI95%, empleandoa
  a `lm()` y a `library(srvyr)`

``` r
#-------------------------------------------------------------------
# generacion de muestra aleatoria simple
#-------------------------------------------------------------------

set.seed(20230413)
muestra_n400 <- dplyr::slice_sample(
                data_notas,
                n = 400, 
                replace = TRUE
                )

#-------------------------------------------------------------------
# intervalo de confianza, por medio de regresion nula
#-------------------------------------------------------------------

lm(PROM_GRAL ~ 1, data = muestra_n400) %>%
confint() %>% 
knitr::kable(., digits = 2)
```

|             | 2.5 % | 97.5 % |
|:------------|------:|-------:|
| (Intercept) |  5.77 |   6.04 |

``` r
#-------------------------------------------------------------------
# intervalo de confianza, por medio de libreria
#-------------------------------------------------------------------

# instalar la libreria srvyr
# install.packages('srvyr')



# especificar que una tabla de datos, es una muestra aleatoria
library(srvyr)
n400_srs <- muestra_n400 %>% 
            as_survey_design(ids = 1)

# intervalo de confianza

n400_srs %>%
summarize(
  mean = survey_mean(PROM_GRAL, vartype = "ci")
  ) %>% 
knitr::kable(., digits = 2)
```

| mean | mean_low | mean_upp |
|-----:|---------:|---------:|
|  5.9 |     5.77 |     6.04 |

## Anexo 9.1: Margen de error

- Podemos calcular margenes de error empleando diferentes distribuciones
  probabilísticas.

  - En este ejemplo, vamos a cubrir como crear margenes de error, para
    la distribución z, y para las distribuciones t.

- En el caso de la distribución z, el valor crítico es solo uno, para la
  construcción de intervalos de confianza de 95%.

  - El margen de error, es lo que sumamos y restamos a la media de una
    muestra, para construir un intervalo de confianza

  - Este valor es 1.96

  - Este valor lo podemos obtener como con el siguiente código.

``` r
z_critico <- qnorm(p = 0.975)
z_critico
```

    ## [1] 1.959964

- Nos basta con indicar el percentil de interés, en la distribución
  probabilística de z.

- Un truco adicional, es pensar que, queremos tener 5% fuera del rango
  del intervalo.

- Podemos llamar a este 5% alpha

- Y luego, podemos dividir a este 5% por 2, y obtener los puntos de la
  distribución probabilística, de los cuales estamos interesados.

- Estos puntos, serian los valores criticos necesarios para dejar fuera
  de un rango a un 5% de todos los valores posibles.

``` r
alpha <- .05

z_limite_inferior <- qnorm(p = alpha/2, lower.tail = TRUE)
z_limite_inferior
```

    ## [1] -1.959964

``` r
z_limite_superior <- qnorm(p = alpha/2, lower.tail = FALSE)
z_limite_superior
```

    ## [1] 1.959964

- En el caso de la distribución t, podemos proceder de la misma forma.

  - Sin embargo, necesitamos un argumento adicional: los grados de
    libertad.

  - Los grados de libertad, para el caso de las medias en la
    distribución t, se asumen como `tamaño_muestral - 1`

  - Formalmente los grados de libertad, son el demominador en la
    obtención de diferentes cifras de la muestra.

  - Se indica que sería equivalente a la cantidad de piezas de
    informacion restantes, en caso de que uno quisiera obtener una media
    a partir de una muestra.

  - Los grados de libertad de una distribución t, para una media, son
    `n - 1`.

- A continuación dejamos codigo para obtener el margen de error,
  calculado con ambas distribuciones.

  - debido al tamaño muestral, los valores criticos obtenidos son muy
    similares.

``` r
#---------------------------------------------------------------
# margen de error empleando a la distribución z
#---------------------------------------------------------------

#--------------------------------------
# especifiquemos tamaño muestral
#--------------------------------------

tamaño_muestral <- 400

#--------------------------------------
# calculamos el error estandar
#--------------------------------------

error_estandar <- desviacion_poblacional / sqrt(tamaño_muestral)
error_estandar
```

    ## [1] 0.06668951

``` r
#--------------------------------------
# valor critico
#--------------------------------------

z_critico <- qnorm(p = 0.975)
z_critico
```

    ## [1] 1.959964

``` r
#--------------------------------------
# margen de error
#--------------------------------------

margen_de_error <- z_critico*error_estandar
margen_de_error
```

    ## [1] 0.130709

``` r
#---------------------------------------------------------------
# margen de error empleando a la distribución t
#---------------------------------------------------------------

#--------------------------------------
# especifiquemos tamaño muestral
#--------------------------------------

tamaño_muestral <- 400

#--------------------------------------
# grados de libertad de una media
#--------------------------------------

grados_de_libertad <- tamaño_muestral-1

#--------------------------------------
# calculamos el error estandar
#--------------------------------------

error_estandar <- desviacion_poblacional / sqrt(tamaño_muestral)
error_estandar
```

    ## [1] 0.06668951

``` r
#--------------------------------------
# valor critico
#--------------------------------------

t_critico = qt(p=0.975, df=grados_de_libertad,lower.tail=TRUE)

#--------------------------------------
# margen de error
#--------------------------------------

margen_de_error <- t_critico*error_estandar
margen_de_error
```

    ## [1] 0.1311067

``` r
#---------------------------------------------------------------
# valores críticos de t a diferentes tamaños muestrales
#---------------------------------------------------------------

data.frame(
n = c(10, 20, 30, 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 1000)
) %>%
mutate(t_lower_value = qt(p=0.975, df=n-1,lower.tail=TRUE)) %>%
mutate(t_upper_value = qt(p=0.975, df=n-1,lower.tail=FALSE)) %>%
knitr::kable(., digits = 2)
```

|    n | t_lower_value | t_upper_value |
|-----:|--------------:|--------------:|
|   10 |          2.26 |         -2.26 |
|   20 |          2.09 |         -2.09 |
|   30 |          2.05 |         -2.05 |
|   50 |          2.01 |         -2.01 |
|  100 |          1.98 |         -1.98 |
|  200 |          1.97 |         -1.97 |
|  300 |          1.97 |         -1.97 |
|  400 |          1.97 |         -1.97 |
|  500 |          1.96 |         -1.96 |
|  600 |          1.96 |         -1.96 |
|  700 |          1.96 |         -1.96 |
|  800 |          1.96 |         -1.96 |
|  900 |          1.96 |         -1.96 |
| 1000 |          1.96 |         -1.96 |

``` r
# Nota: a partir de 500 observaciones, los valores de t, se asemejan a los 
#       valores de z.
```

## Anexo 11.1: Crear variables dummy

- Para crear la variable `status`, estamos empleando las condiciones
  lógicas para saber si un intervalo contiene a la media poblacional.

- En la primera versión del codigo, estamos siguiendo las codiciones,
  tal como fueran enunciadas

  - Esto toma la forma “Si, condición 1 ó condición 2, entonces tal o
    cual cosa”

  - Esto lo escribimos dentro de la función `case_when()` de la
    siguiente manera:

    - `media_poblacional > ul | media_poblacional < ll ~ 'no',`

  - el símbolo `|` sirve para expresar que una u otro condicón deben ser
    cumplidas, y basta con que una se cumpla, para que sea el caso
    esperado.

- En este anexo, incluimos una forma alternativa de escribir este mismo
  código.

- Este segundo codigo tambien funciona, porque cuando separamos las
  condiciones en lineas diferentes, `case_when()` las puede interpretar
  como condiciones que pueden ser todas verdad de forma secuencial.

``` r
#---------------------------------------------------------------
# clasificación de intervalos de confianza
#---------------------------------------------------------------

#--------------------------------------
# clasificación, con operador "|"
#--------------------------------------

medias_con_ci <- medias_con_ci %>%
mutate(status = case_when(
  media_poblacional > ul | media_poblacional < ll ~ 'no',
  TRUE ~ 'yes'
  ))

#--------------------------------------
# descriptivo
#--------------------------------------

medias_con_ci %>%
summarize(
proportion = mean(status == 'yes')
) %>%
knitr::kable(., digits = 2)
```

| proportion |
|-----------:|
|       0.94 |

``` r
#---------------------------------------------------------------
# margen de error empleando a la distribución z
#---------------------------------------------------------------

#--------------------------------------
# clasificación, sin operador "|"
#--------------------------------------

# generemos la variable status
medias_con_ci <- medias_con_ci %>%
mutate(status = case_when(
  media_poblacional > ul ~ 'no',
  media_poblacional < ll ~ 'no',
  TRUE ~ 'yes'
  ))

#--------------------------------------
# descriptivo
#--------------------------------------

medias_con_ci %>%
summarize(
proportion = mean(status == 'yes')
) %>%
knitr::kable(., digits = 2)
```

| proportion |
|-----------:|
|       0.94 |
