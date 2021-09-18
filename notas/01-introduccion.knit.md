---
output:
  pdf_document: default
  html_document: default
---
# Introducción {#introduccion}


## ¿Qué es aprendizaje de máquina (machine learning)? 




Métodos **computacionales** para **aprender de datos**  con el fin
de producir reglas para 
mejorar el **desempeño** en alguna tarea o toma de decisión. 

En este curso nos enfocamos en las tareas de *aprendizaje supervisado* (predecir o estimar una variable respuesta a partir de datos de entrada) y *aprendizaje no supervisado* (describir estructuras interesantes en datos,
donde no necesariamente hay una respuesta que predecir). Existe también
*aprendizaje por refuerzo*, en donde buscamos aprender a tomar decisiones
en un entorno en donde la decisión afecta directa e inmediatamente al entorno.

#### Ejemplos de tareas de aprendizaje: {-}

- Predecir si un cliente de tarjeta de crédito va a caer en impago en los próximos
tres meses.
- Reconocer palabras escritas a mano (OCR).
- Detectar llamados de ballenas en grabaciones de boyas. 
- Estimar el ingreso mensual de un hogar a partir de las características
de la vivienda, posesiones y equipamiento y localización geográfica.
- Dividir a los clientes de Netflix según sus gustos.
- Recomendar artículos a clientes de un programa de lealtad o servicio online.

Las razones usuales para intentar resolver estos problemas **computacionalmente**
son diversas:

- Quisiéramos obtener una respuesta barata, rápida, **automatizada**, y 
con suficiente precisión.
Por ejemplo, reconocer caracteres en una placa de coche de una fotografía 
se puede hacer por personas, pero eso es lento y costoso. Igual oír cada segundo de grabación
de las boyas para saber si hay ballenas o no. Hacer mediciones directas
del ingreso de un hogar requiere mucho tiempo y esfuerzo.
- Quisiéramos **superar el desempeño actual** de los expertos o de reglas simples utilizando
datos: por ejemplo, en la decisión de dar o no un préstamo a un solicitante,
puede ser posible tomar mejores decisiones con algoritmos que con evaluaciones personales
o con reglas simples que toman en cuenta el ingreso mensual, por ejemplo.
- Al resolver estos problemas computacionalmente tenemos
oportunidad de aprender más del problema que nos interesa: estas
soluciones forman parte de un ciclo de **análisis de datos** donde podemos 
aprender de una forma más concentrada cuáles son
características y patrones importantes de nuestros datos.


Es posible aproximarse a todos estos problemas usando reglas (por ejemplo,
si los pixeles del centro de la imagen están vacíos, entonces es un cero, 
si el crédito total es mayor al 50\% del ingreso anual, declinar el préstamo, etc). Las razones para no tomar un enfoque de reglas
 construidas "a mano":

- Cuando conjuntos de reglas creadas a mano se desempeñan mal (por ejemplo, para otorgar créditos, reconocer caracteres, etc.)

- Reglas creadas a mano pueden ser difíciles de mantener (por ejemplo, un corrector
ortográfico), pues para problemas interesantes muchas veces se requieren grandes
cantidades de reglas. Por ejemplo: ¿qué búsquedas www se enfocan en
dar direcciones como resultados? ¿cómo filtrar comentarios no aceptables
en foros?


## Ejemplo: reglas y aprendizaje

*Lectura de un medidor mediante imágenes*. Supongamos que en
una infraestructura donde hay medidores análogos (de agua, electricidad, gas, etc.) que no se comunican. ¿Podríamos pensar en utilizar
fotos tomadas automáticamente para medir el consumo?

Por ejemplo, consideramos el siguiente problema (tomado de [aquí](https://community.openhab.org/t/water-meter-digitizer-version-2-all-in-device-including-mqtt/107188), ver [código y datos](https://github.com/jomjol/water-meter-system-complete)):


```r
library(imager)
library(tidyverse)
library(gt)
set.seed(43)
path_img <- "../datos/medidor/"
path_full_imgs <- list.files(path = path_img, full.names = TRUE)
medidor <- load.image(sample(path_full_imgs, 1))
par(mar = c(1, 1, 1, 1))
plot(medidor, axes = FALSE)
```

<img src="01-introduccion_files/figure-html/unnamed-chunk-2-1.png" width="384" />

Nótese que las imágenes y videos son matrices o arreglos de valores de pixeles, por ejemplo
estas son las dimensiones para una imagen:


```r
dim(medidor)
```

```
## [1] 193 193   1   3
```

En este caso, la imagen es de 193 x 193 pixeles y tiene tres canales, o 
tres matrices de 193 x 193 donde la entrada de cada matriz es la intensidad
del canal correspondiente. Buscámos hacer cálculos con estas matrices para extraer la información
que queremos. En este caso, construiremos estos cálculos a mano.

Primero filtramos (extraemos canal rojo y azul, restamos, difuminamos y
aplicamos un umbral):









































