+++
showonlyimage = true
draft = false
image = "img/portfolio/z-buffer.png"
date = "2017-09-27T18:25:22-05:00"
title = "Entendiendo z-buffering"
weight = 0
+++

Mi motivación detrás de este artículo es dar un breve resumen del algoritmo de procesamiento de imagen conocido como z-buffering en español. Dar a entender cómo funciona y en qué aspectos es posible mejorarlo.
<!--more-->

Z-buffering es una solución al problema de visibilidad, que es el problema de decidir qué elementos de una escena renderizada son visibles y que están ocultos. Además de este tipo de solución, se encuentran otras como: el algoritmo del pintor, uso de C-Buffer y S-Buffer, Lista de bordes activos ordenados, Particionamiento en espacio binario, Ray tracing, entre otros.

Cuando un objeto es dibujado por una tarjeta gráfica 3D, una descripción de como Z-buffering trabaja es la siguiente:

1. Se almacena la profundidad del píxel generado (coordenada z) en un buffer de datos (el z-buffer).

2. Este buffer se distribuye como un array de 2 dimensiones (x, y) con un elemento por cada pixel de la pantalla.

3. Si algún otro objeto de la escena se tiene que renderizar en el mismo píxel:  la tarjeta gráfica o el software que ejecuta z-buffering compara las dos profundidades y elige el más cercano al observador.

4. La profundidad elegida es entonces salvada en el z-buffer.

5. El z-buffer permitirá a la tarjeta gráfica reproducir correctamente la percepción de la profundidad normal.


La Z-Culling es la eliminación temprana de un píxel basada en la profundidad, un método que provee un incremento en el rendimiento cuando la renderización de superficies ocultas es costosa. Es una consecuencia directa del Z-buffering, donde la profundidad de cada píxel candidato es comparada con la profundidad de la geometría existente detrás de la cual puede estar oculto.


Hablamos un poco más del algoritmo de z-buffering como tal, podemos encontrar la siguiente implementación:


<b>Algoritmo de Z-buffer</b>: 

<b>Entrada</b>: Una lista de polígonos {P1,P2,.....Pn}

<b>Salida</b>: Una matriz COLOR, que muestra la intensidad de las superficies de polígonos visibles.

Inicializa:

                z-buffer(x,y)=maxima profundidad; 

                COLOR(x,y) = color de fondo.


Inicio:

     para(cada polígono P en la lista de polígonos) hacer {

         Para (cada pixel(x,y) que intersecta P) hacer {

             Calcular z-depth de P en el punto (x,y)

             Si (z-depth < z-buffer[x,y]) entonces {
                 
                  Z-buffer[x,y] = z-depth;

                  COLOR(x,y) = Intensidad de P en el punto (x,y);

            }

         }

      }

  Retornar la matriz COLOR.

Y se calcula z-depth para un polígono P en el punto (x, y) de la siguiente manera:

![z-depth](/img/portfolio/z-depth.svg)
<img src="/img/portfolio/z-depth.svg">

Donde:

<i>Far</i>: Es la máxima distancia que se alcanza a ver en la cámara.

<i>Near</i>: Es la mínima distancia que se alcanza a ver en la cámara.


<b>Alternativas a z-buffering:</b>

Como se mencionó antes existen bastantes algoritmos que resuelven el mismo problema que Z-buffering, como el algoritmo del pintor, uso de C-Buffer y S-Buffer, Lista de bordes activos ordenados, Particionamiento en espacio binario, Ray tracing, entre otros. Sin embargo, el más utilizado es z-buffering o una variante de este conocida como w-buffering.

En el algoritmo w-buffering, se utiliza el mismo proceso que en el de z-buffering pero los valores de z-depth se almacenan en formato de coma flotante. Por ejemplo: 
![floating-point](/img/portfolio/floating-point.svg)
<img src="/img/portfolio/floating-point.svg">

Por lo que es necesario hacer un proceso de interpolación para guardarlos.
