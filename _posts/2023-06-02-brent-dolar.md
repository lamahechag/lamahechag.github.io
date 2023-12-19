---
layout: post
title: Dependencia COP/USD y el barril Brent
date: 2023-06-02
datatable: true
---
El petroleo referencia Brent es el producto que mas aporta a los ingresos de Colombia, entre 30% y 50%.
Debido a ese peso en las exportaciones, el barril Brent y COP/USD usualmente tienen una asociación negativa, es decir, cuando uno aumenta el otro cae. El orden de causalidad, según los economistas del país, es: precio Brent afecta el dolar en Colombia. Por ejemplo: la caida del precio del barril en el año 2014, y posterios subida del dolar, ó el efecto sobre el peso colombiano por la caida en la demanda de combustible durante la pandemia del COVID. 

Para estudiar esta relación, se hara un análisis exploratorio de la tasa de cambio 
y el precio Brent desde el año 2010 hasta el 2023. Periodo con eventos importantes. También se calcularan las métricas de correlación, Pearson y Spearman, de forma directa, y con rezagos&mdash;el efecto del Brent sobre el dolar no es imediato, puede tener un rezago de horas ó días.

Con el análisis exploratorio tratare de responder las siguientes preguntas: ¿Hay siempre una dependencia negativa entre estas dos variables?
¿Cuales son los rezagos(lags) de su dependencia? ¿si tienen una correlación negativa, es ésta simetrica? 

# Análisis exploratorio
Empesare por hacer una exploración gráfica en donde se representa la serie de tiempo de COP/USD simultaneamente 
con el precio Brent y el índice de fortaleza del dolar(dxy). 

<div align="center">
    <img src="{{ site.baseurl }}/images/copusd_dxy.png">
</div>
<div align="center">
    <img src="{{ site.baseurl }}/images/copusd_brent.png">
</div>

En general, COP/USD se mueve en la misma dirección que dxy, y opuesta a el precio Brent. Hay periodos en los que se desconecta de la dependencia típica de estas dos variables. En el caso del Brent, en el 2021 parece tomar un correlación positiva. Del 2010 al 2012, parece no moverse en la dirección del indice DXY.

El análisis mas sencillo de asociación es calcular las correlación de Pearson, la cual resulta ser 0.38. Es un valor positivo, lo cual nos dice que si el precio Brent aumenta, entonces el dolar tambien. Esto es contradictorio. Para encontrar la causa, podemos hacer un scatter-plot.

<div align="center">
    <img src="{{ site.baseurl }}/images/scatter_brent_copusd.png">
</div>

En la gráfica se observa que no hay una relación simple entre las dos variables; y que hay grupos de datos con correlación positiva, mientras en otros es negativa. Por ejemplo, en el año 2021(puntos verdes) se ve una relación creciente. Esto nos lleva a verificar la gráfica con colores para todos los años:

<div align="center">
    <img src="{{ site.baseurl }}/images/copusd_brent_colors.png">
</div>

Donde se observa que el año 2021 se comporta diferente al resto. Lo cual se puede verificar, en un futuro blog, para otros paises que también dependan del petroleo.

# Análisis de correlación

El análisis de correlación de primera mano es la de Pearson, que es lineal. Sin embargo, generalmente se encuentran muchas relaciones no-lineales. Para esto caso de no-linealidad, podemos complementar calculando la correlación de Spearman.

La correlación de los datos de varios años, como un solo grupo, puede llevar a la paradoja de Simpson; observando correlaciones positivas entre el Brent y el dolar en Colombia. En la siguiente tabla se muestran las correlaciones para dos rangos de años diferentes:


<table style="margin-left: auto; margin-right: auto;">
  <tr><th>  </th>           <th>2010-2023</th>      <th>2016-2023</th></tr>
  <tr><td>Pearson</td>   <td>-0.48</td>       <td>0.45</td></tr>
  <tr><td>Spearman</td>        <td>-0.50</td>       <td>0.38</td></tr>
</table>


También hay que tener en cuenta el retardo en el cambio del precio del dolar debido al cambio en el Brent. Como los datos que tenemos son diarios, calcularemos las correlaciones con retardos de días. Por ejemplo, para un retardo de 
10 días buscamos para cada fecha del precio del dolar el valor del Brent 10 días atras.

<p>&nbsp;</p>

<div align="center">
    <figcaption>2010-2023.</figcaption>
    <img src="{{ site.baseurl }}/images/correlation_lags.png">
    
</div>

<p>&nbsp;</p>

<div align="center">
    <figcaption>2016-2023.</figcaption>
    <img src="{{ site.baseurl }}/images/correlation_lags_2016.png">    
</div>

Anteriormente vimos, en un scatterplot, que el grupo de datos(brent vs dolar) para cada año tienen patrones de asociasión diferentes, y que la correlación global tiene signo diferente dependiendo del periodo a estudiar. En la siguiente tabla se muestra la correlación de Pearson para cada año.

<p>&nbsp;</p>
<div align="center">
    <figcaption>Pearson correlation.</figcaption>
    <img src="{{ site.baseurl }}/images/correlation_table.png">    
</div>
<p>&nbsp;</p>

A estos resultados contrarios al analizar el total del conjunto de datos y sus subgrupos, se le conoce como paradoja de Simpson.

Por ultimo, hago unas observaciones sobre la revesibilidad de la dependencia de estas dos varibles. Algunos analistas economicos se refieren a la reversibilidad, como el proceso en el cual el retroceso de una variable reversa a su estado inicial la variable dependiente. Por ejemplo, cuando el barril Brent cae de 120 a 80, el dolar en Colombia sube de 3800 a 4500; pero si reversamos el brent de 80 a los 120 iniciales, el dolar no retorna a los 3800 iniciales. Estas irreversibilidades ó asimetrias es algo que se encuentra frecuentemente en los sistemas complejos. Esto se puede observar con un gráfico de la marcha aleatoria, que muestra la dirección de cada paso.

<p>&nbsp;</p>
<div align="center">
    <figcaption>Marcha aleatoria brent vs COP/USD 2010-2023.</figcaption>
    <img src="{{ site.baseurl }}/images/random_walk.png">    
</div>
<p>&nbsp;</p>

<p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://lamahechag.github.io/brent-dolar/">Dependencia COP/USD y el barril Brent</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://github.com/lamahechag">luis alejandro mahecha</a> is licensed under <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">Attribution 4.0 International<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"></a></p>

