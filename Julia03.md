# Matrices

## Creación de matrices

Para introducir un vector fila escribimos sus componentes **separados por espacios en blanco**. Con la función `transpose(v)` podemos transformar el vector fila en un vector columna (vector transpuesto). La transposición también se puede lograr **colocando un apóstrofe al vector**, por analogía con la notación matemática.

```julia
julia> v = [1 3 5]  # Un vector fila
1x3 Array{Int64,2}:
 1  3  5

julia> transpose(v)
3x1 Array{Int64,2}:
 1
 3
 5

julia> v'
3x1 Array{Int64,2}:
 1
 3
 5

julia> v # El vector v no ha cambiado
1x3 Array{Int64,2}:
 1  3  5
```

> A nivel informático una matriz es un array bidimensional. También existen arrays de mayores dimensiones que son el análago de los tensores matemáticos.
 
**Podemos introducir las matrices de dos formas distintas**:

- **Por filas**. Introducimos las filas, escribiendo los elementos separados por espacios en blanco y  para **crear una nueva fila escribimos un punto y coma**. 

- **Por columnas**. Creamos un array, donde el primer elemento del array es la primera columna, **separado por un espacio en blanco** escribimos la segunda columna, ...

```julia
julia> A = [1 2 3; 4 5 6]
2x3 Array{Int64,2}:
 1  2  3
 4  5  6

julia> B = [[1, 2, 3] [4, 5, 6]]
3x2 Array{Int64,2}:
 1  4
 2  5
 3  6
```

## Indexación y submatrices

Para extraer el elemento $a_{ij}$ de la matriz escribimos `[i,j]` a continuación de la matriz. Los rangos que hemos empleado en los vectores para extraer partes de un vector se emplean también para extraer submatrices.


```julia
julia> A = [1 2 3; 4 5 6; 7 8 9]
3x3 Array{Int64,2}:
 1  2  3
 4  5  6
 7  8  9

julia> A[2,1] # Segunda fila, primera columna
4

julia> A[:,:] # Todas las filas y todas las columnas
3x3 Array{Int64,2}:
 1  2  3
 4  5  6
 7  8  9

julia> A[:,1] # Todas las filas, primera columna
3-element Array{Int64,1}:
 1
 4
 7

julia> A[:, [2,3]]  # Todas las filas, columnas 2 y 3
3x2 Array{Int64,2}:
 2  3
 5  6
 8  9

julia> A[[1,2],[2,3]]  # Filas 1 y 2 y columnas 2 y 3
2x2 Array{Int64,2}:
 2  3
 5  6
```


## Crear matrices especiales

Aunque todas las matrices se pueden crear introduciendo todos sus elementos, existen matrices especiales que se pueden crear de modo más rápido con ciertas funciones: la matriz cero, la matriz identidad, ...

`zero(n)` crea un vector de ceros y tamaño `n`. `zero(n, m)` crea una matriz de orden n por m. El tipo es float.

```julia
julia> zeros(3,4)  # Tipo float
3x4 Array{Float64,2}:
 0.0  0.0  0.0  0.0
 0.0  0.0  0.0  0.0
 0.0  0.0  0.0  0.0
```

`ones(n, m)` crea una matriz llena de unos. 

```julia
julia> ones(3, 4)
3x4 Array{Float64,2}:
 1.0  1.0  1.0  1.0
 1.0  1.0  1.0  1.0
 1.0  1.0  1.0  1.0
```

Para obtener una matriz con todos los elementos iguales a 7 podemos crear una matriz con unos y multiplicar por 7. Sin embargo Julia tiene una función que hace ese trabajo en un solo paso: `fill(x, n, m)`.

```julia
julia> fill(7, 2, 3)
2x3 Array{Int64,2}:
 7  7  7
 7  7  7
```

`eye(n)` crea la matriz identidad de orden n. `eye(n, m)` crea una matriz del orden indicado con la diagonal principal llena de unos. 

```julia
julia> eye(4)
4x4 Array{Float64,2}:
 1.0  0.0  0.0  0.0
 0.0  1.0  0.0  0.0
 0.0  0.0  1.0  0.0
 0.0  0.0  0.0  1.0
 
julia> eye(3,4)
3x4 Array{Float64,2}:
 1.0  0.0  0.0  0.0
 0.0  1.0  0.0  0.0
 0.0  0.0  1.0  0.0 
```

`diagm(v)` crea una matriz diagonal con el vector dado en la diagonal principal. Si le añadimos un segundo argumento podemos "subir" o "bajar" dicha diagonal.

```julia
julia> diagm([1, 2, 3, 4])
4x4 Array{Int64,2}:
 1  0  0  0
 0  2  0  0
 0  0  3  0
 0  0  0  4
 
julia> diagm([1, 2, 3, 4], -1) # Una diagonal "abajo"
5x5 Array{Int64,2}:
 0  0  0  0  0
 1  0  0  0  0
 0  2  0  0  0
 0  0  3  0  0
 0  0  0  4  0

julia> diagm([1, 2, 3, 4], 2) # Dos diagonales "arriba"
6x6 Array{Int64,2}:
 0  0  1  0  0  0
 0  0  0  2  0  0
 0  0  0  0  3  0
 0  0  0  0  0  4
 0  0  0  0  0  0
 0  0  0  0  0  0
```


## Operaciones con matrices

Las operaciones componente a componente se realizan con los operadores precedidos por un punto, puesto que esa es la forma de operar con arrays de cualquier dimensión. Sin embargo ya vimos en el tema de vectores que en el caso particular de la suma, la resta y la multiplicación por escalares se pueden utilizar los operadores sin el punto.

```julia
julia> A = [1 2 3; 4 5 6; 7 8 9]; B =[ 3 8 4; 6 9 7; 3 9 8];

julia> 3.6 * A .+ B .- 5 * A 
3x3 Array{Float64,2}:
  1.6   5.2  -0.2
  0.4   2.0  -1.4
 -6.8  -2.2  -4.6

julia> 3.6 * A + B - 5 * A
3x3 Array{Float64,2}:
  1.6   5.2  -0.2
  0.4   2.0  -1.4
 -6.8  -2.2  -4.6
```

Si sumamos o restamos una constante a una matriz, le sumamos o restamos la constante a cada componente.

```julia
julia> A= [1 2 3; 4 5 6; 7 8 9]
3x3 Array{Int64,2}:
 1  2  3
 4  5  6
 7  8  9

julia> A + 10
3x3 Array{Int64,2}:
 11  12  13
 14  15  16
 17  18  19

julia> -8 - A
3x3 Array{Int64,2}:
  -9  -10  -11
 -12  -13  -14
 -15  -16  -17
```
 
La multiplicación matricial se obtiene con el asterisco y se realiza siempre y cuando esté permitido por las dimensiones de las matrices.

```julia
julia> A= [1 2 3; 4 5 6; 7 8 9]; B =[3 8 4; 6 9 7; 3 9 8];

julia> A * B
3x3 Array{Int64,2}:
 24   53   42
 60  131   99
 96  209  156

julia> B * A
3x3 Array{Int64,2}:
 63   78   93
 91  113  135
 95  115  135
```

La potencia matricial se calcula con el circunflejo. En particular si elevamos una matriz a -1 estamos calculando la inversa de la matriz.

```julia
julia> A = [1 2 3; 4 5 6; 7 8 9]; B = [3 8 4; 6 9 7; 3 9 8];

julia> A^2
3x3 Array{Int64,2}:
  30   36   42
  66   81   96
 102  126  150

julia> A * A
3x3 Array{Int64,2}:
  30   36   42
  66   81   96
 102  126  150
 
julia> B^(-1)
3x3 Array{Float64,2}:
 -0.111111   0.345679  -0.246914
  0.333333  -0.148148  -0.037037
 -0.333333   0.037037   0.259259

julia> B^(-1) * B # "Casi" la matriz identidad
3x3 Array{Float64,2}:
  1.0          -4.44089e-16  -4.44089e-16
  8.32667e-17   1.0           4.44089e-16
 -1.11022e-16  -4.44089e-16   1.0        

julia> A^(-1) # No tiene inversa
ERROR: Base.LinAlg.SingularException(3)
 in inv at linalg/factorization.jl:22
 in inv at linalg/dense.jl:362
 in ^ at linalg/dense.jl:172 
``` 


## Funciones matriciales básicas

Tras las operaciones con matrices, estas son probablemente las funciones matriciales más sencillas.

| | |
| --- | --- |
|`det(A)` | Determinante de A|
|`inv(A)` | Inversa de A, si existe|
|`trace(A)` | Traza de A|
|`rank(A)` | Rango de A|

```julia
julia> A = [4 6 8; 7 3 1; 4 8 9]
3x3 Array{Int64,2}:
 4  6  8
 7  3  1
 4  8  9

julia> det(A)
74.0

julia> rank(A)
3

julia> trace(A)
16

julia> inv(A)
3x3 Array{Float64,2}:
  0.256757   0.135135   -0.243243
 -0.797297   0.0540541   0.702703
  0.594595  -0.108108   -0.405405

julia> A^(-1)
3x3 Array{Float64,2}:
  0.256757   0.135135   -0.243243
 -0.797297   0.0540541   0.702703
  0.594595  -0.108108   -0.405405
```

## Concatenación de matrices

Sobre todo en informática, es común formar matrices a partir de submatrices. Para ello vamos concatenando, de manera horizontal o vertical submatrices. Se puede hacer utilizando  el espacio en blanco y el punto y coma o también utilizar las funciones `hcat(A, B)` y `vcat(A, B)`.

```julia
julia> A= [1 2 3; 4 5 6; 7 8 9]
3x3 Array{Int64,2}:
 1  2  3
 4  5  6
 7  8  9

julia> B =[3 8 4; 6 9 7; 3 9 8]
3x3 Array{Int64,2}:
 3  8  4
 6  9  7
 3  9  8

julia> hcat(A, B)
3x6 Array{Int64,2}:
 1  2  3  3  8  4
 4  5  6  6  9  7
 7  8  9  3  9  8

julia> [A B] # Con el espacio en blanco
3x6 Array{Int64,2}:
 1  2  3  3  8  4
 4  5  6  6  9  7
 7  8  9  3  9  8

julia> vcat(A, B)
6x3 Array{Int64,2}:
 1  2  3
 4  5  6
 7  8  9
 3  8  4
 6  9  7
 3  9  8

julia> [A; B] # Con el punto y coma
6x3 Array{Int64,2}:
 1  2  3
 4  5  6
 7  8  9
 3  8  4
 6  9  7
 3  9  8
```


## Sistemas lineales

Cualquier sistema lineal se puede escribir en la forma `A . X = B` donde `A` es la matriz del sistema y `B` el vector de términos independientes. Resolver el sistema es encontrar el vector (o vectores) `X` que cumplen dicha ecuación. Esto lo resuelve Julia rápidamente con el operador `\`. En realidad Julia puede encontrar `X` aunque sea también una matriz y no un simple vector columna.

```julia
julia> A = [2 6; 7 9]; B = [3,7]; # Sistema compatible

julia> A \ B
2-element Array{Float64,1}:
 0.625   
 0.291667

julia> A * ans # Comprobacion de la solucion
2-element Array{Float64,1}:
 3.0
 7.0
```

Ahora intentamos resolver un sistema incompatible y otro indeterminado.

```julia
julia> A = [2 6; 1 3]; B = [3,7]; A \ B # Incompatible
ERROR: Base.LinAlg.SingularException(2)
 in \ at linalg/factorization.jl:25
 in \ at linalg/dense.jl:450

julia> A = [2 6; 2 6]; B = [3, 3]; A \ B # Indeterminado
ERROR: Base.LinAlg.SingularException(2)
 in \ at linalg/factorization.jl:25
 in \ at linalg/dense.jl:450
``` 


Con los sistemas más simples que no son compatibles y determinados tiene problemas.

> Para comprobar la "velocidad" de Julia podemos usar la **macro** `@time`. Si escribimos esta macro delante de una operación, nos calcula cuanto tiempo tarda en el cálculo.
 
```julia
julia> A = rand(1000, 1000); # Matriz "grande"

julia> B = rand(1000);

julia> @time A \ B;
  0.074712 seconds (15 allocations: 7.645 MB)
```
   