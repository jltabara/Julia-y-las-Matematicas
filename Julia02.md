
## Vectores

Los vectores en Julia se escriben entre **corchetes y separados por comas**. Por defecto son **vectores columna**. El **tipo del vector lo deduce Julia**, siendo el *menor* tipo que contiene a cada una de las componentes.

> La palabra vector en programación tiene muchos sinóminos: *arrays*, arreglos, listas, ...  Julia emplea la palabra *array* para denotar los vectores.

> Aunque también existen arrays cuyas componentes pueden ser letras, palabras, ... nosotros estudiaremos de momento arrays con componentes numéricas.



```
julia> l = [3, 4, 7] # Un vector de "enteros" de tamaño 3
3-element Array{Int64,1}:
 3
 4
 7
 
julia> typeof(ans)
Array{Int64,1} 
```

Al mostrarnos por pantalla un vector tenemos la siguiente información:

- Los elementos de los consta, dispuestos en forma de columna.

- El tipo de los elementos del array.

- El número de elementos de array.

Existen funciones que nos proporcionan esa información sobre el array.

- La función `print(x)` imprime el array y nos muestra únicamente sus componentes **en una fila**.

- ```
julia> print([5,6,8,90])
[5,6,8,90]
```

- La función `eltype(x)` (*element type* en inglés) no dice el tipo de los elementos que forman el array.

- ```
julia> eltype([5,6,8,90])
Int64
```

- `length(x)` (longitud en inglés) nos da el número de elementos.

- ```
julia> length([5,6,8,90])
4
```



> Si después de una instrucción colocamos un punto y coma, Julia efectua el cálculo, pero no muestra el resultado de dicha operación por pantalla.

Veamos como Julia infiere el tipo del array en función de su contenido.


```
julia> l = [3, 7.8, 9.34];

julia> typeof(l)
Array{Float64,1}

julia> l = [4,8im, 9, 6.6];

julia> typeof(l)
Array{Complex{Float64},1}
```


Utilizando constructores podemos crear arrays del tipo que queramos y no dejar que Julia infiera su tipo. Naturalmente los datos que introduzcamos en el array deben ser de dicho tipo, pues en caso contrario tendríamos un error.

```
julia> l =  Int8[4, 6, 8, 9];

julia> typeof(l)
Array{Int8,1}

julia> l = Complex[5, 7, 8];

julia> typeof(l)
Array{Complex{T<:Real},1}

julia> l = Int8[3, 78.9, 12];
ERROR: InexactError()
 in setindex! at array.jl:303
 in getindex at array.jl:167
```
 

## Operaciones con vectores

Para realizar operaciones componente a componente, **por norma general, utilizamos los operadores precedidos por un punto**, aunque es cierto que para algunos operadores esto es innecesario. Los vectores con los que operamos deben de ser del mismo tamaño. 

```
julia> [3, 5, 8] .+ [4, 7, 9] 
3-element Array{Int64,1}:
  7
 12
 17

julia> [3, 5, 8] .- [4, 7, 9]
3-element Array{Int64,1}:
 -1
 -2
 -1

julia> [3, 5, 8] .* [4, 7, 9]
3-element Array{Int64,1}:
 12
 35
 72

julia> [3, 5, 8] ./ [4, 7, 9] # El resultado es float
3-element Array{Float64,1}:
 0.75    
 0.714286
 0.888889
 
julia> [3, 5, 8].^3
3-element Array{Int64,1}:
  27
 125
 512 
``` 

## Funciones sobre vectores

La mayor parte de la funciones en Julia están **vectorizadas**. Ello quiere decir que si aplicamos una función a un vector, en realidad estamos aplicando la función a cada componente del vector.

```
julia> sin([3, 4, 8])
3-element Array{Float64,1}:
  0.14112 
 -0.756802
  0.989358
  
julia> exp([4, 7.8])
2-element Array{Float64,1}:
   54.5982
 2440.6    
```

Los vectores, además de ser una parte muy importante de las matemáticas, son estructuras de datos muy importantes en cualquier lenguaje de programación. Algunas de las funciones sobre vectores "provienen" del mundo de las matemáticas. Sin embargo un gran número de ellas son importantes en el uso de Julia como lenguaje de programación.

Aquí tenemos una lista de algunas funciones, de orientación matemática, que actuan sobre vectores.

| | |
| --- | --- |
|`dot(x,y)` | Producto escalar de x e y|
|`cross(x,y)` | Producto vectorial de x e y|
|`sum(x)` | Suma todos los elementos del vector|
|`prod(x)` | Multiplica todos los elementos|
|`maximum(x)` | Devuelve el mayor valor|
|`minimum(x))` | Devuelve el menor valor|

  
```
julia> dot([4, 6, 8], [5, 9, -4])
42

julia> cross([4, 6, 8], [5, 9, -4])
3-element Array{Int64,1}:
 -96
  56
   6

julia> sum([4, 6, 90])
100

julia> prod([4, 6, 90])
2160

julia> maximum([5, 7, 8, 3])
8

julia> minimum([5, 7, 8, 3])
3
```


## Rangos

Los **rangos** en Julia son equivalentes a las **sucesiones aritméticas** de la matemática. Para escribir un rango escribimos el número inicial y número final. La diferencia en este caso elemental es 1. Para construir un vector a partir de un rango, utilizamos la función `collect(rango)`.

```
julia> 4:10
4:10

julia> typeof(ans)
UnitRange{Int64}

julia> collect(4:10)
7-element Array{Int64,1}:
  4
  5
  6
  7
  8
  9
 10

julia> sum(collect(1:100))  # Julia es mas rapido que Gauss
5050

julia> prod(collect(1:10))  # La factorial de 10
3628800
```

> Si hacemos actuar una función sobre un rango, como norma general, no es necesario utilizar la función `collect(rango)` y transformar en vector el rango.

```
julia> sum(collect(1:100))
5050

julia> sum(1:100) # Sin usar collect
5050

julia> prod(1:10)
3628800
```



La distancia entre dos números consecutivos del array es 1. Podemos variar esto y hacer que los números vayan de 2 en 2, de 3 en 3, etc. El "paso" lo marcamos como segundo argumento del rango. `comienzo`:`paso`:`final`. En inglés queda mejor estéticamente `start`:`step`:`stop`. **No es necesario que el último argumento sea exactamente el final. Simplemente marca un valor que no se puede superar**. 

También se pueden crear rangos con la función `range(x,n)`. El primer argumento `x` es el comienzo del rango y el segundo `n` el número de elementos que compondran dicho rango. Si tiene tres argumentos el segundo es el "paso". El último argumento indica de nuevo cuantos elementos tendrá el rango.

En los rangos cualquiera de los parámetros puede ser un flotante. Al trabajar con aritmética no exacta, pueden producirse errores y no llegar a constituir verdaderas progresiones aritméticas.

```
julia> collect(1:2:10)  # Del 1 al 10 (sin pasarse) de 2 en 2
5-element Array{Int64,1}:
 1
 3
 5
 7
 9

julia> collect(10:-3:0)  # De 3 en 3 decreciendo
4-element Array{Int64,1}:
 10
  7
  4
  1
 
julia> collect((2:0.1:2.5))
6-element Array{Float64,1}:
 2.0
 2.1
 2.2
 2.3
 2.4
 2.5
 
julia> range(3,5) # Comienzo 3, 5 elementos
3:7

julia> range(2,0.1,6) # Principio 2, paso 0.1, 6 elementos
2.0:0.1:2.5
```

## Indexación y *subarrays*

Los arrays en Julia se **indexan desde el número 1**, contrariamente a otros lenguajes que comienzan la numeración en 0. Para acceder al elemento `i` de un array escribimos `[i]` detrás del nombre del array. Para obtener el último elemento escribimos `[end]`. 

Para obtener partes de un array se pueden utilizar dos métodos:

- Escribir dentro de los corchetes un array indicando las posiciones que queremos extraer.

- Utilizar la notación de rangos. Este se denomina *slide* en inglés. Al hacer *slide* podemos no escribir el primer o el último elemento.


```
julia> v = [2, 5, 8, 90, -8];

julia> v[3]  ## Posicion 3
8

julia> v[end] # Ultima componente
-8

julia> v[[1,4]] # Posiciones 1 y 4
2-element Array{Int64,1}:
  2
 90

julia> v[2:4]  # Componentes de la 2 a la 4, ambas incluidas
3-element Array{Int64,1}:
  5
  8
 90

julia> v[3:end]  # Desde el 3 al final
3-element Array{Int64,1}:
  8
 90
 -8

julia> v[1:2:end]  # Las posiciones impares
3-element Array{Int64,1}:
  2
  8
 -8
 
julia> v[:3] # El rango es 1:3
8

julia> v[:] # Todo el array
5-element Array{Int64,1}:
  2
  5
  8
 90
 -8 
```



> Si en una celda aparecen varias expresiones, se ejecutan todas ellas en orden secuencial, pero **únicamente se muestra el resultado de la última de las expresiones**.  Si queremos hacer esto en una sola línea tenemos que emplear el punto y coma para "ocultar" las expresiones que no queramos que aparezcan.


Las componentes de un vector son en realidad variables. Con la instrucción de asignación, podemos variar el contenido de dicha variable y por lo tanto cambiar parte del vector sin tener que volver a escribirlo.

```
julia> v = [2, 5, 8, -4]; v[3] = 123; v
4-element Array{Int64,1}:
   2
   5
 123
  -4

julia> v[1:3] = 100; v 
4-element Array{Int64,1}:
 100
 100
 100
  -4
```

## Añadir y quitar elementos.

En un lenguaje de programación los array son un tipo de  **estructura de datos** muy importante. Al ser información, es importante saber extraerla (lo hemos hecho con los índices) y también modificarla, añadiendo elementos, quitándolos, ordenándolos, ...

> Como norma general, en Julia, los nombres de funciones que terminan con el signo de admiración modifican el argumento que se les pasa. Si la función tiene una versión sin el signo de admiración lo que se hace es crear un nuevo objeto.
 
La función `push!(v, x)` añade uno o varios elementos al final del array. El array sobre el que actuamos se modifica. Los datos que le añadamos deben ser compatibles con el tipo del array

```
julia> v =[2, 4, 7]; push!(v, 90); v
4-element Array{Int64,1}:
  2
  4
  7
 90

julia> v = [2, 3]; push!(v, 30, 60); v # Añade varios
4-element Array{Int64,1}:
  2
  3
 30
 60

julia> v = [2, 3]; push!(v, 2.922); v
ERROR: InexactError()
 in push! at array.jl:419
``` 

La función `pop!(v)` borra el último elemento del array. Además de esto no lo devuelve por si queremos efectuar alguna operación con el elemento desechado.
 
```
julia> v =[2, 4, 7]; pop!(v); v
2-element Array{Int64,1}:
 2
 4
```

La función `append!(v, w)` concatena dos vectores, en el orden en que están escritos.  También podemos concatenar de otro modo, utilizando el punto y coma. Esta nueva forma de concatenar se entenderá mejor cuando estudiemos las matrices. La función `prepend!(v,w)` concatena los arrays en el orden contrario.

```
julia> append!(v, [4, 6, 7])
5-element Array{Int64,1}:
 2
 4
 4
 6
 7

julia> [[3, 5, 6]; [90, 8, 63]]  # Otra concatenacion
6-element Array{Int64,1}:
  3
  5
  6
 90
  8
 63

julia> prepend!([1, 2, 3], [4, 5, 6])
6-element Array{Int64,1}:
 4
 5
 6
 1
 2
 3
```

`shift!(v)` elimina el primer elemento y `unshift!(v, x)` añade uno o varios elementos al comienzo del array. 

```
julia> v = [1, 2, 3, 4]; shift!(v); v
3-element Array{Int64,1}:
 2
 3
 4

julia> v = [1, 2, 3, 4]; unshift!(v, 90); v
5-element Array{Int64,1}:
 90
  1
  2
  3
  4
```

La función `deleteat!(v, n)` borra el elemento `n` del array, dejando un array con menor longitud.

```
julia> v = [4, 5, 8, 10]; deleteat!(v, 3); v
3-element Array{Int64,1}:
  4
  5
 10
```

La función `sort(v)` ordena el vector y no modifica el vector. La función `sort!(v)` hace lo mismo, pero si modifica el vector.

```
julia> v = [4, 8, 2, 8]
4-element Array{Int64,1}:
 4
 8
 2
 8

julia> sort(v)
4-element Array{Int64,1}:
 2
 4
 8
 8

julia> v
4-element Array{Int64,1}:
 4
 8
 2
 8

julia> sort!(v); v
4-element Array{Int64,1}:
 2
 4
 8
 8
```


## Vectores por comprensión

En matemáticas es muy habitual definir conjuntos por comprensión. Por ejemplo

$$
\{x^2 \  |\  x \in \{1,5,8,4\}\}
$$

crea el conjunto formado por los cuadrados de los elementos. En Julia se pueden construir vectores siguiendo un patrón análogo, cambiando la barra vertical por un `for`, el signo de pertenecia por un `in` y utilizando un vector, o un rango, como conjunto del que se toman los datos.

```
julia> [i^2 for i in [1, 5, 8, 4]] # El conjunto anterior
4-element Array{Int64,1}:
  1
 25
 64
 16

julia> [i^2 for i in 1:5]
5-element Array{Int64,1}:
  1
  4
  9
 16
 25
```

## Vectores aleatorios

Existen diversas funciones en Julia que generarn números aleatorios. Con ellos podemos formar vectores. Los números aleatorios pueden ser aleatorios en el intervalo [0,1] o bien seguir una distribución normal standard. La única diferencia es el sufijo "d". También podemos elegir los números aleatorios de un rango.
  
| | |
| --- | --- |
|`rand(n)` | Vector aleatorio de longitud n|
|`randn(n)` | Vector aleatorio "normal" de longitud n|
|`rand(rango, n)` | Vector aleatorio en el rango de longitud n|

```
julia> rand(3) # Aleatorio entre 0 y 1
3-element Array{Float64,1}:
 0.800577
 0.936807
 0.550451

julia> randn(3) # Distribucion normal
3-element Array{Float64,1}:
 -0.0137139
  0.5558   
  0.964115 

julia> rand(1:10, 3) # Aleatorio en el rango 1:10
3-element Array{Int64,1}:
 6
 3
 6
 
julia> v = rand(10000); # Vector con 10000 elementos
```

## Tuplas

Las tuplas son muy similares a los arrays, pero tienen algunas diferencias importantes:

- Se definen entre paréntesis y no corchetes.
- Se muestran en línea y no en columna.
- Son **inmutables**.

Si nosotros definimos una tupla podemos acceder a sus elementos con la notación de índice. Sin embargo si intentamos cambiar el valor de un elemento nos dará un mensaje de error. Lo mismo es válido para todas las funciones que modifican los arrays.

```
julia> t = (4, 7, 8)
(4,7,8)

julia> t[2]
7

julia> t[2] = 90
ERROR: MethodError: `setindex!` has no method matching
setindex!(::Tuple{Int64,Int64,Int64}, ::Int64, ::Int64)

julia> pop!(t)
ERROR: MethodError: `pop!` has no method matching pop!
(::Tuple{Int64,Int64,Int64})
```

## Los arrays y los conjuntos

La notación de Julia para los arrays y la notación matemática para denotar los conjuntos son muy similares. Existen sin embargo algunas diferencias:

- En un conjunto el orden de los elementos no importa. En los arrays si que importa.

- En un conjunto no se escriben elementos repetidos y si por casualidad alguno aparece repetido lo debemos borrar.

Julia tiene un nuevo tipo de dato, llamado conjunto, y que se puede formar a partir de los arrays o de tuplas. En este tipo de dato no existe el concepto de índice y no existe elementos repetidos. Para crear conjuntos utilizamos el constructor `Set(v)`.

```
julia> c = Set([2, 3, 5, 7, 11, 3]) # Se borra el repetido
Set([7,2,3,11,5])

julia> c[4]
ERROR: MethodError: `getindex` has no method matching
getindex(::Set{Int64}, ::Int64)
```

Con los conjuntos podemos efectuar las típicas operaciones conjuntistas de unión, intersección, ... En realidad Julia también puede realizar dichas operaciones directamente con arrays, eliminando los duplicados.

| | |
| --- | --- |
|`union(A, B)` | $A \cup B$|
|`intersect(A, B)` | $A \cap B$|
|`setdiff(A, B)` | $A - B$|
|`in(A, B)` | Comprueba si $x \in A$|



```
julia> A = Set([1, 2, 3]); B = Set([3, 4, 5]);

julia> union(A, B)
Set([4,2,3,5,1])

julia> typeof(ans)
Set{Int64}

julia> intersect(A, B)
Set([3])

julia> setdiff(A, B)
Set([2,1])

julia> in(3, A)
true

julia> in(8, A)
false
```
Lo mismo se puede realizar directamente con arrays.

```
julia> A = [1, 2, 3]; B = [3, 4, 5];

julia> typeof(ans)
Array{Int64,1}

julia> union(A, B)
5-element Array{Int64,1}:
 1
 2
 3
 4
 5

julia> intersect(A, B)
1-element Array{Int64,1}:
 3

julia> setdiff(A, B)
2-element Array{Int64,1}:
 1
 2
 
julia> in(3, A)
true

julia> in(8, A)
false 
```
 



    
