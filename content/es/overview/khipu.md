---
title: "Khipu"
weight: 10
showLastmod: false
geekdocAlign: left
---

Khipu es un cluster de computación de alto desempeño que forma  [Centro de Investigación para la Computación Sostenible (COMPSUST)](https://compsust.utec.edu.pe) y el [Departamento de Ciencia de la Computación (CS)](https://cs.utec.edu.pe) de la [Universidad de Ingeniería y Tecnología (UTEC)](https://utec.edu.pe).


{{< columns >}}

> **Misión** \
Somos un grupo académico que apostamos por el desarrollo científico de nuestra comunidad, a traves de la computación de alto desempeño. 

<--->

> **Visión** \
Ser un laboratorio referente en computación de alto desempeño a nivel local, y ser reconocidos por nuestra colaboración al desarrollo científico de nuestra comunidad.

{{< /columns >}}

<p align="center">
  <img src="/overview/khipu_nodes.png">
</p>

Visto de manera sencilla, Khipu es una colección de computadoras que que se encuentran conectadas entre sí y que trabajan como si fueran una sola. A cada computador conectado se le conoce como **nodo**. El nodo de acceso, o también llamado **nodo líder**, es aquel al cual nos conectamos de manera remota para acceder al cluster. Los nodos que se encargan de la ejecución de nuestros **jobs** o trabajos, son llamados **nodos de cómputo**. Para enviar un job hacemos uso de un **scheduler**, el cual se encargado de manejar los diferentes jobs que se envían y garantizar equidad al momento que pasan a ejecutarse. Todos los nodos de computo, poseen un [filesystem]() compartido lo cual permite a los jobs acceder y modificar la data de cualquier nodo sin la necesidad de copiarlo al otro.





## ¿Qué es computación de alto desempeño?



La computación de alto desempeño, o HPC por sus siglas en inglés, es la práctica de agregar poder computacional de diferentes ordenadores, de tal manera que se obtiene un desempeño mucho mayor al de un ordenador convencional aprovechando el poder del paralelismo. Todo ello con el objetivo de resolver de manera más rápida los problemas complejos de campos como matemáticas, ciencias e ingenierías. 

HPC nos permite resolver problemas complejos que tardarían años, meses o semanas para resolverse en un computador convencional, pero que con el uso de HPC, se podría reducir a días, horas o minutos. 

Leer más:

- [What is High Performance Computing? ](https://www.usgs.gov/advanced-research-computing/what-high-performance-computing)
- [What is High-Performance Computing (HPC)?](https://www.oracle.com/middleeast/cloud/hpc/what-is-high-performance-computing/)





