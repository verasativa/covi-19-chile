# Análisis de inflexiones de infectados COVID-19 en Chile

## Prólogo
Después de un par de experimentaciones con datos de Kragle sobre el Covid-19, por el 31 de marzo 2020, mi viejo (médico, doctor en salud pública y mateo en general) me presionó para que usara [generative linear models](https://pygam.readthedocs.io/en/latest/notebooks/tour_of_pygam.html) para buscar segmentos de tendencias en la curva de contagio Covid-19 en Chile.

Luego de una semana actualizándolo a diario, esperando que él escribiera un texto de análisis, siento que me corresponde hacerlo a mí, al menos para motivarlo a escribir un contra-análisis. En este punto vale la pena aclararle al lector, que mi padre y yo nos encontramos en posiciones opuestas al aproximarnos al fenómeno de manera subjetiva, yo _alarmista_ y el _no-pasa-nadista_. Sincerando aquello, en este reporte, intentaré restringirme al análisis numérico tanto como me sea posible.

## Intro
### Calidad de los datos: Renuncia Piñera!
Si usted vive en Chile y ya vio [el meme de del "director Skinner" con el "Superintendente Chalmers"](docs/photo_2020-04-04 12.37.01.jpeg), puede saltarse este punto. A los demás, intentaré "hacerla corta".

El coronavirus llegó a Chile en medio de un [estallido social](https://es.wikipedia.org/wiki/Protestas_en_Chile_de_2019-2020). El 18 de octubre pasado, masas de estudiantes evadieron el pago del metro protestando por el alza del pasaje, y varias estaciones fueron incendiadas de manera coordinada. Pronto el gobierno sacó a los milicos de los cuarteles, hubo muertos, heridos y detenidos por miles. Con el pasar de los días, en vez de hacerse más suave, la protesta se endureció. Millones de personas marcharon por las calles durante semanas pidiendo la renuncia del presidente y un cambio radical de sistema político, económico y social. En ese contexto llegó el coronavirus, con la población convulsionada, polarizada y la credibilidad de los burócratas, armados y civiles, por el suelo.

Hasta este momento, Chile tenía una tradición decente de transparencia con los datos de salud pública; de hecho, los tres primeros informes oficiales sobre el Covid-19 entregados por el ministerio de salud traían los datos desagregados por comuna, edad y sexo registral. Pero en pocos días eso cambió. Se adoptó la estrategia de entregar solo el total de defunciones e infectados, y esconder la información desagregada. Después de la presión ejercida por el colegio médico, expertos epidemiólogos y la ciudadanía, en los últimos días se han dignado a entregar nuevos informes en PDF con un poco más de detalle, pero ni soñar con un CSV.


Personalmente estoy convencida de que los datos oficiales están distorsionados principalmente por negligencia, pero también, al menos en parte, por interés político. Dicho esto, son los únicos datos que hay, así que vale la pena analizarlos.


### Generative aditive models: buscando infecciones en la curva
Los [GAMs](https://www.youtube.com/watch?v=XQ1vk7wEI7c) son una familia de modelos compuestos de múltiples términos sumados, una idea simple pero poderosa. Primera vez que me los encuentro y entiendo que en mundo de la ciencia de datos se usan sobre todo para modelar componentes con intepretabilidad y también para abordar problemas de series de tiempo ([prophet](https://facebook.github.io/prophet/) se basa en estos).

Esta "idea simple pero poderosa" crea muchas oportunidades de análisis. En este caso, lo que busco es __observar inflexiones en la curva de infectados__. Entonces, observaremos estas inflexiones desde la línea de predicciones del modelo entrenado, y su rango de confidence al 95%, siempre comparándola con los puntos de _ground true_.

A quienes les interese el procedimiento completo, les invito a revisar [el notebook en github](https://github.com/verasativa/covi-19-chile/blob/master/GAMs.ipynb), sus PR son bienvenidos y las críticas en general.

## LinearGAM de un término sobre _N_ días transcurridos con 20 splines
Después de explorar varias técnicas de entrenamiento y cantidad de splines en [el notebook](https://github.com/verasativa/covi-19-chile/blob/master/GAMs.ipynb), decidí quedarme con un "fit naive" y 20 splines porque parece ser un buen equilibrio entre confidence y overfit para el objetivo de observar inflecciones.

![plot](docs/LinearGAM%28s%280%2C%20n_splines%3D20%29%5D%29%20%28rojo%29%20%2B%2095%25%20prediction%20interval%20%28azul%29.png)

### Inflexiones observadas
#### 2 al 13 de marzo: cuentas muy bajas y alta incertidumbre
La primera inflexión que podemos observar es la de un primer periodo con muy pocas infecciones. Hay una variabilidad tan mínima en esos datos que el modelo tiene una baja certeza. Se podría decir que aun "no sabe para dónde va la micro".
#### 13 al 21 de marzo: primer aceleramiento moderado
En esta segunda inflección se observa un aceleramiento desde la base anterior, pero aun con bastante incertidumbre.
#### 21 al 28 de marzo: segundo aceleramiento con mayor certidumbre
Aquí no solo aumenta el factor de crecimiento, sino que también se estabilizan un poco los números dándole una certidumbre mayor al modelo.
#### 28 de marzo al 3 de abril: desaceleramiento con menor certidumbre
La taza de infecciones empieza a bajar, pero también empieza a bajar la certidumbre.
#### 3 al 5 de abril: parece desacelerar, pero aumenta mucho la incertidumbre
Parecen desacelerarse levemente las infecciones, pero hay mucha diferencia entre los números, lo que crea mayor incertidumbre para el modelo.

## Conclusiones: no hay todavia.
Este modelado es altamente experimental, y son muy poco los datos como para entregar conclusiones aun. Más bien, por el contrario, la invitación es a discutir el análisis e irlo mejorando entre todos.

### Invitación
A quienes quieran hacer otros puntos sobre este análisis, y en la medida que sean rigurosos y vengan de este mismo código o código que aporten al repositorio, los invito a agregar su texto acá por PR o a enviármelo a mí. Feliz lo agregaré en una sección de análisis de terceros a este mismo documento.