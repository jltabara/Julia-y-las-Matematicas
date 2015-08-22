## Matrices

Para introducir un vector fila escribimos sus componentes **separados por espacios en blanco**. Con la función `transpose(v)` podemos transformar el vector fila en un vector columna (vector transpuesto). La transposición también se puede lograr colocando un apóstrofe al vector, por analogía con la notación matemática.

```
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

> A nivel matemático una matriz es una disposición rectangular de un conjunto de números. A nivel informatico una matriz es un array bidimensional.

Si queremos escribir una matriz, la introducimos por filas, escribiendo los elementos separados por espacios en blanco y  para **crear una nueva fila escribimos un punto y coma**. Para acceder a cada elemento de una matriz se emplea una notación similar a la matemáticas, que denota $a_{ij}$ a los elementos de una matriz. Los rangos que hemos empleado en los vectores para extraer partes de un vector se emplean también para extraer submatrices.

```
julia> A = [1 2 3; 4 5 6]
2x3 Array{Int64,2}:
 1  2  3
 4  5  6

julia> size(A) # Calcula el orden (tamaño) de la matriz
(2,3)

julia> A[2,1] # Segunda fila, primera columna
4

julia> A[:,:] # Todas las filas y todas las columnas
2x3 Array{Int64,2}:
 1  2  3
 4  5  6

julia> A[:,1] # Todas las filas, primera columna
2-element Array{Int64,1}:
 1
 4

julia> A[:, [2,3]]  # Todas las filas, columnas 2 y 3
2x2 Array{Int64,2}:
 2  3
 5  6

julia> A[[1,2],[2,3]]  # Filas 1 y 2 y columnas 2 y 3
2x2 Array{Int64,2}:
 2  3
 5  6
```


## Crear matrices especiales

Aunque todas las matrices se pueden crear introduciendo todos sus elementos, existen matrices especiales que se pueden crear de modo más rápido con ciertas funciones: la matriz cero, la matriz identidad, ...

| | |
| --- | --- |
|`zeros(n,m)` | Matriz nula de orden n $\times$ m|
|`ones(n,m)` | Matriz con unos de orden n $\times$ m|
|`eye(n)` | Matriz unidad de orden n|
|`diagm(v)` | Matriz con v en la diagonal principal|


```
julia> zeros(3,4)  # Tipo float
3x4 Array{Float64,2}:
 0.0  0.0  0.0  0.0
 0.0  0.0  0.0  0.0
 0.0  0.0  0.0  0.0

julia> ones(3, 4)
3x4 Array{Float64,2}:
 1.0  1.0  1.0  1.0
 1.0  1.0  1.0  1.0
 1.0  1.0  1.0  1.0

julia> eye(4)
4x4 Array{Float64,2}:
 1.0  0.0  0.0  0.0
 0.0  1.0  0.0  0.0
 0.0  0.0  1.0  0.0
 0.0  0.0  0.0  1.0

julia> diagm([1, 2, 3, 4])
4x4 Array{Int64,2}:
 1  0  0  0
 0  2  0  0
 0  0  3  0
 0  0  0  4
```


## Operaciones con matrices

Las operaciones componente a componente se realizan con los operadores precedidos por un punto, puesto que esa es la forma de operar con arrays de cualquier dimensión. Sin embargo ya vimos en el tema de vectores que en el caso particular de la suma, la resta y la multiplicación por escalares se pueden utilizar los operadores sin el punto.

```
julia> A= [1 2 3; 4 5 6; 7 8 9]; B =[3 8 4; 6 9 7; 3 9 8];

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
 
La multiplicación matricial se obtiene con el asterisco y se realiza siempre y cuando esté permitido por las dimensiones de las matrices.

```
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

```
julia> A= [1 2 3; 4 5 6; 7 8 9]; B =[3 8 4; 6 9 7; 3 9 8];

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


## Otros cálculos matriciales

Tras las operaciones con matrices, lo primero que se estudia en álgebra lineal tiene que ver con la resolución de sistemas lineales. Aquí aparecen conceptos como el determinante, el rango, la "división" de matrices, ...

| | |
| --- | --- |
|`det(A)` | Determinante de A|
|`inv(A)` | Inversa de A, si existe|
|`trace(A)` | Traza de A|
|`rank(A)` | Rango de A|

```
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


## Sistemas lineales

Cualquier sistema lineal se puede escribir en la forma `A . X = B` donde `A` es la matriz del sistema y `B` el vector de términos independientes. Resolver el sistema es encontrar el vector (o vectores) `X` que cumplen dicha ecuación. Esto lo resuelve Julia rápidamente con el operador `\`. En realidad Julia puede encontrar `X` aunque sea también una matriz y no un simple vector columna.


    A = [2 6; 7 9]; B = [3,7];


    A \ B  # Resolucion del sistema




    2-element Array{Float64,1}:
     0.625   
     0.291667




    A * ans  # Comprobacion de la solucion




    2-element Array{Float64,1}:
     3.0
     7.0




    A = [2 6; 1 3]; B = [3,7];
    A \ B  # Ojo que este es incompatible


    LoadError: Base.LinAlg.SingularException(2)
    while loading In[168], in expression starting on line 2

    

     in A_ldiv_B! at linalg/lu.jl:138

     in \ at linalg/factorization.jl:25

     in \ at linalg/dense.jl:400



    A = [2 6; 2 6]; B = [3, 3];
    A \ B  # Ojo que este es indeterminado


    LoadError: Base.LinAlg.SingularException(2)
    while loading In[171], in expression starting on line 2

    

     in A_ldiv_B! at linalg/lu.jl:138

     in \ at linalg/factorization.jl:25

     in \ at linalg/dense.jl:400


Como vemos Julia con los sistemas más simples que no son compatibles y determinados tiene problemas.