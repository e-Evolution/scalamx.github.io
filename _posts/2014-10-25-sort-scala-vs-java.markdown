---
layout    : post
author    : "pazthor"
title     : "Un análisis básico sobre la ventaja de implementar algoritmos en el lenguaje Scala"
date      : 2014-08-7 01:16:55
categories: scalamx redaccion sort algoritmos implementacion 
---


###¿Por qué Scala?###

Scala es un lenguaje híbrido y con un gran poder para el desarrollo de software robusto, basta con ver los ejemplos de  [Coursera](https://tech.coursera.org/blog/2014/02/18/why-we-love-scala-at-coursera/) y [Twitter](http://www.artima.com/scalazine/articles/twitter_on_scala.html) para saber porque. 
Es por ello que en este post, pretendo mostrar algunas características a la hora de implementar un algoritmo de ordenamiento en Scala.


Empezaré con una características propia de los lenguajes funcionales (como *Haskell*,*OCaml*,*F#* entre otros) y es la **inferencia de tipos**.

> La inferencia de tipos asigna automáticamente un tipo de datos a una función sin necesidad de que el programador lo escriba.

> El tipo de las funciones es reconstruido a partir de un análisis estático del programa realizado por el compilador o intérprete del lenguaje,

> A partir de las definiciones previas y del uso de las variables en el cuerpo de las funciones.


A continuación se muestra 3 ejemplos del algoritmo *QuickSort* en Java y Scala, notar la diferencia entre las primeras dos implementaciones y compararla con la última implementación y ver la diferencias.


**QuickSort** implementado en Java.


{% highlight scala linenos %}
package de.vogella.algorithms.sort.quicksort;

public class Quicksort  {
  private int[] numbers;
  private int number;

  public void sort(int[] values) {
    if (values ==null || values.length==0){
      return;
    }
    this.numbers = values;
    number = values.length;
    quicksort(0, number - 1);
  }

  private void quicksort(int low, int high) {
    int i = low, j = high;
    int pivot = numbers[low + (high-low)/2];
    while (i <= j) {
      while (numbers[i] < pivot) {
        i++;
      }
      while (numbers[j] > pivot) {
        j--;
      }
      if (i <= j) {
        exchange(i, j);
        i++;
        j--;
      }
    }
    if (low < j)
      quicksort(low, j);
    if (i < high)
      quicksort(i, high);
  }

  private void exchange(int i, int j) {
    int temp = numbers[i];
    numbers[i] = numbers[j];
    numbers[j] = temp;
  }
}
{% endhighlight %}

Este es la implementación en Scala.


{% highlight scala linenos %}
def sort(xs: Array[Int]) {
  def swap(i: Int, j: Int) {
    val t = xs(i); xs(i) = xs(j); xs(j) = t
  }
  def sort1(l: Int, r: Int) {
    val pivot = xs((l + r) / 2)
      var i = l; var j = r
      while (i <= j) {
        while (xs(i) < pivot) i += 1
          while (xs(j) > pivot) j -= 1
            if (i <= j) {
              swap(i, j)
                i += 1
                j -= 1
            }
      }
    if (l < j) sort1(l, j)
      if (j < r) sort1(i, r)
  }
  sort1(0, xs.length - 1)
}
{% endhighlight %}


La siguiente implementación fue escrita en Scala, pero con los principios de la programación funcional y aprovechando la inferencia de tipos:


{% highlight scala linenos %}
def sort(xs: Array[Int]): Array[Int] = {
  if (xs.length <= 1) xs
  else {
    val pivot = xs(xs.length / 2)
      Array.concat(
          sort(xs filter (pivot >)),
          xs filter (pivot ==),
          sort(xs filter (pivot <)))
  }
}
{% endhighlight %}


El algoritmo implementado de manera funcional captura la esencia del algoritmo QuickSort de manera concisa:

* Si el arreglo está vacío o consta de un solo elemento,entonces  ya está ordenado, de modo que se devuelve inmediatamente.

* Si el arreglo no esta vacío, elige un elemento y usalo como pivote.

* Se reparte el arreglo en dos sub-arreglos que contienen elementos que son más pequeños que,
  respectivamente mayor que el elemento de pivote, y una tercera matriz que contiene elementos iguales al elemento pivote.

* Clasificar los primeros dos sub-arreglos por una invocación recursiva de la función  **sort**

* El resultado se obtiene añadiendo los tres sub-arreglos.




###Nota###
  Si no tienes idea de como funciona el algoritmo Quicksort, recomiendo que veas esta [Animación de QuickSort](http://en.wikipedia.org/wiki/Quicksort#mediaviewer/File:Sorting_quicksort_anim.gif).

###Referencia###

* [Scala By Example, Martin Odersky](http://www.scala-lang.org/docu/files/ScalaByExample.pdf)

* [Inferencia de tipos, Wikipedia](http://es.wikipedia.org/wiki/Inferencia_de_tipos)

* [Implementación del algoritmo QuickSort en Java](http://www.vogella.com/tutorials/JavaAlgorithmsQuicksort/article.html#quicksort)
