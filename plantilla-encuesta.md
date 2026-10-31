# Plantilla de diseño — Encuesta de satisfacción (hito 1)

**Equipo:** 4 · **Integrantes:** Encalada Emanuel, Fogacho Eduardo, Tituaña Emily, Peñaloza Tabatha, Romo Paula  · **Fecha:** 26/10/2026

**Repositorio (fork):** github.com/paularomo2006/IDSD422-lab2-diseno-encuesta

**Enfoque del equipo (aprobado en clase):**  Precios - percepción frente a la competencia


> **Formato del entregable:** exporta esta plantilla completada a PDF, con el enlace de tu fork en la carátula, y guarda el PDF en `informes/` del fork. Entrega el PDF y el enlace del fork en el aula virtual.

## 1. Objetivo de la encuesta
Conocer que opinan los clientes sobre los precios de Retail Andina en comparación con los diferentes supermercados de Quito. Con las respuestas se busca identificar qué porcentaje de personas considera que sus precios son bajos, altos o similares con respecto a los diferentes proveedores.  

## 2. Población objetivo

- Población: personas mayores de 25 años, residentes de la ciudad de Quito, con responsabilidad total o compartida en la compra de alimentos para el hogar, que adquieren alimentos en supermercados Retail Andina al menos una vez al mes. 

- Límites y justificación: la distinción en la edad se hace debido a las responsabilidades que maneja un individuo

## 3. Método de muestreo

- Método: Estratificado por grupos de edad 
- Tamaño y justificación:
Se buscará obtener entre 15 a 25 respuestas donde participe la población que fue definida. La muestra será dividida en dos grupos: personas que rondaran entre los 25 a 35 años y personas mayores de 35 años. 
Con este método se podrán conocer las opiniones de las personas de diferentes edades, viendo si sus opiniones frente a los precios de Retail Andina con los otros supermercados.

- Cómo se seleccionarán los participantes:
Buscar a las personas que cumplan con los requisitos de la población objetivo, pero primero se debe tomar en cuenta que esta será dividida en los dos grupos de edad. Después se puede incluir personas al azar de los dos grupos para comparar percepciones. 
En el caso de haber personas que respondan a la encuesta y no cumplan los requisitos, sus respuestas no serán consideradas para el análisis final, con esto nos aseguraremos de trabajar con las personas que cumplen con las características definidas en la encuesta.  

- **Equivalente en código (Python o R):** la línea que implementa tu método sobre un CSV de clientes (aleatorio simple, estratificado o conveniencia) con el resultado de una corrida con semilla, y una frase sobre por qué tu método (y no la conveniencia) justifica el hito.

```python
import pandas as pd 
df = pd.read_csv(url_400)

# Filtrar clientes que viven en Quito 
df = df[df["ciudad"] == "Quito"].copy()

# Clasificar los clientes por grupos de edad 
df["grupo_edad"] = df["edad"].apply( 
    lambda edad: "25 a 35 años" if 25 <= edad <= 35 
    else "Mayores de 36 años" if edad > 36 
    else None 
) 

# Excluir registros que no pertenecen a los grupos 
df = df.dropna(subset=["grupo_edad"]) 

# Seleccionar hasta 10 clientes por cada grupo 
muestra = ( 
    df.groupby("grupo_edad", group_keys=False) 
    .apply( 
        lambda grupo: grupo.sample( 
            n=min(10, len(grupo)), 
            random_state=42 
         ) 
     ) 
) 
 
print("Clientes seleccionados:") 
print(muestra) 

print("\nCantidad de clientes por grupo:") 
print(muestra["grupo_edad"].value_counts()) 
```

El método de muestreo estratificado fue escogido a diferencia de la conveniencia para evitar el sesgo de la recolección de datos de un grupo de fácil acceso. Dicho de esta manera, decidimos dividirla en personas entre 25 a 35 años como adultos jóvenes y la gente superior a 35 años como adultos. Todo esto debido a que buscamos llegar a las personas que ya presentan una solvencia económica siendo adultos jóvenes y a su vez, personas mayores de 35 años que comúnmente ya tienen sus hogares y buscan optimizar su presupuesto familiar maximizando la relación calidad-precio.  

## 4. Cuestionario (5–10 preguntas)

| # | Pregunta | Tipo de variable | Dtype al cargar | Opciones / escala |
|---|---|---|---|---|
| 1 |¿Cuál es el principal motivo por el que elige comprar en RetailAndina?  |Categórica nominal |object |Precio, calidad, cercanía, variedad, promociones, otro  |
| 2 |¿Cómo considera los precios de RetailAndina en comparación con otros supermercados?  |Categórica ordinal |object |Mucho más económicos, Algo más económicos, Similares, Algo más caros, Mucho más caros, Aparte: No sabe o no puede comparar  |
| 3 |¿En qué rango de edad se encuentra?   |Categórica ordinal |object |Menos de 25 años, 25 a 35 años, 36 años o más |
| 4 |¿Qué tan atractivas considera las promociones y descuentos que ofrece RetailAndina?  |Categórica ordinal |object |1)Nada atractivas  2) Poco Atractivas 3)Neutral 4)Atractivas 5)Muy atractivas, Aparte: No conozco sus promociones  |
| 5 |¿En qué rango se ubica su gasto mensual aproximado en supermercados?   |Categórica ordinal  |object  |Menos de $100, Entre $100 - $250, Superior a $250   |
| 6 |¿Estaría dispuesto a inscribirse en un programa de fidelización gratuito para obtener precios especiales?   |Categórica ordinal  |object  |1)Nada dispuesto, 2)Poco dispuesto, 3)Neutral, 4)Dispuesto, 5)Muy dispuesto, Aparte: No sabe.   |
| 7 |¿Qué categoría de productos compra con más frecuencia?   |Categórica nominal  |object  |Abarrotes(arroz, lentejas,etc) Cárnicos, Lácteos, Limpieza del hogar, Cuidado personal, Bebidas, Otro(especifique)  |
| 8 |¿Con qué supermercado compara principalmente los precios de RetailAndina? | Categórica nominal | object | Supermaxi, Mi Comisariato, Coral, Tía, Otro (especifique), No compara precios |


*Nota: la columna "tipo de variable" usa la clasificación del cap. 3 (numérica continua/discreta, categórica binaria/ordinal). "Dtype al cargar" es cómo crees que llegará la columna en `df.dtypes`/`str()` (`int64`, `float64`, `str`/`object` según la versión de pandas). Pista: el dtype es una pista, no la clasificación — una ordinal puede llegar como `int`.*

## 5. Análisis de sesgos

| Sesgo potencial | Mitigación |
|---|---|
| 1. Sesgo de respuesta: Algunos participantes podrían no recordar correctamente los precios o promociones de RetailAndina y otros supermercados, lo que podría afectar sus comparaciones.  |Incluir opciones como "No sabe o no puede comparar" y "No conozco sus promociones". Además, realizar una prueba piloto del cuestionario y, cuando sea posible, utilizar precios de referencia para facilitar las comparaciones.  |
| 2. Sesgo de selección: Al encuestar principalmente a familiares, amigos y conocidos, es posible que sus opiniones no representen a todos los clientes de RetailAndina. |Establecer cuotas por grupos de edad para evitar concentrar las respuestas en un solo grupo. También se declarará que el muestreo es no probabilístico por cuotas y que los resultados no pueden generalizarse a toda la población.  |

## 6. Confidencialidad (RA3)

- Datos personales que se piden y por qué:
En este caso, no será necesario pedir nombres, números de cédula, teléfonos ni correos electrónicos. Solo necesitaremos la edad, ya que, con esa información, a cada persona se la puede segmentar de acuerdo al grupo que pertenece, si está entre 25 a 35 años o es superior a los 35 años.

- Plan de anonimización (qué se quita o codifica antes de compartir):
Como la encuesta es anónima desde el inicio, no tendremos datos privados que ocultar. Sin embargo, antes de entregar el archivo final, procederemos con la revisión de las respuestas de texto para asegurarnos de borrar cualquier nombre propio o dato personal que el cliente haya escrito por accidente.  

## 7. Verificación con herramientas (Python/R)

*Escribe `scripts/verificacion_encuesta.py` (o `.R`): debe cargar `plantillas/encuesta_respuestas.csv` y reportar los tipos de cada columna, los datos faltantes por columna y la distribución de `satisfaccion_general` y `recomendaria`. Incluye la salida y responde:*

- ¿Coinciden los tipos con tu predicción de la sección 4?
- ¿Cuántos datos faltantes hay por columna?

*Ejecuta el script sobre el CSV final (separa con un filtro de la columna `origen` las filas reales de las simuladas): ¿qué resultado delataría un sesgo de **no respuesta** y cuál un sesgo de **respuesta**? Escribe tu interpretación usando solo las respuestas reales.*
