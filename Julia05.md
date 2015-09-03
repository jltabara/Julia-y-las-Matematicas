# Control de flujo

## Introducción

Hasta ahora todo lo que hemos realizado en Julia consistía en evaluar una única expresión. A veces también efectuamos dos o tres instrucciones en cada linea, separando cada instrucción por comas o por puntos y comas. 

Un programa en Julia consta de varias expresiones, escritas en un orden determinado. Dichas intrucciones se pueden escribir en un archivo de texto y ejecutarlas en la consola o bien escribir todas las instrucciones en una única celda de la versión web. En ambos casos las sentencias se van ejecutando en orden. Es lo que se denomina el **flujo** del programa. Podemos estar interesados en variar este flujo y no ejecutar de modo lineal las instrucciones. Para variar el flujo existen, en prácticamente todos los lenguajes de programación, dos tipos de construcciones:

- La sentencia condicional: Una parte de las sentencias se  ejecuta solamente en ciertos casos.

- Bucles: Una parte del código se puede repetir varias veces.

Para controlar las sentencias condicionales y los bucles se utilizan los valores booleanos y por ello comenzaremos haciendo un estudio de dicho tipo de datos.

##Valores booleanos

Los dos valores booleanos los representa Julia por `true` (verdadero) y `false` (falso). Forman un nuevo tipo de dato, `Bool`. Muchas funciones, que se utilizan normalmente para hacer comprobaciones, arrojan como resultado un dato de tipo `Bool`.

```julia
julia> true
true

julia> false
false

julia> typeof(true)
Bool

julia> typeof(false)
Bool

julia> isprime(9) # Comprueba si un numero es primo
false
```

Algunas veces nos interesa tratar a los valores booleanos como números enteros. Julia considera que `true`es `1` y que `false` es `0`. En general no son aconsejables estas prácticas.

```julia
julia> true + false + true
2

julia> (true + true) * false
0
```



## Operaciones con valores booleanos

La operación lógica de conjunción (la *y* lógica) se realiza con el operador `&&` y la *o* lógica con `||`. La negación se consigue colocando `!`delante del valor lógico.

```julia
julia> true && true
true

julia> true && false
false

julia> true || false
true

julia> !true
false

julia> !false
true
```


## Los operadores de orden

La siguiente tabla presenta los operadores de orden. El resultado de una comparación es siempre un valor booleano: `true` si la comparación es cierta y `false` si no es cierta.

| | |
| --- | --- |
|`==` | Igualdad|
|`!=` | Desigualdad|
|`<` | Menor que|
|`>` | Mayor que|
|`<=` | Menor o igual que|
|`>=` | Mayor o igual que|

```
julia> 5 == 5
true

julia> 5 != 5
false

julia> 4 > 6
false

julia> 4 >= 4
true

julia> 5 < 12.895
true

julia> !(4 == 4) # Negamos el resultado de la comparacion
false

julia> >(4, 7) # Tambien en forma prefija
false
```

También se pueden comparar vectores, o en general arrays de cualquier dimensión, componente a componente. En este caso debemos emplear la versión precedida por un punto. 

```julia
julia> a = [3, 5, 8]; b = [5, -7, 9];

julia> a .< b
3-element BitArray{1}:
  true
 false
  true

julia> a .== b
3-element BitArray{1}:
 false
 false
 false
 
julia> A = eye(3); B = ones(3,3); # Tambien con matrices

julia> A .== B
3x3 BitArray{2}:
  true  false  false
 false   true  false
 false  false   true 
```
 
## La sentencia `if ... end`

La sentecia más elemental de control de flujo es `if ... end`. Después del `if` debemos colocar una condición booleana (cualquier "cosa" que al evaluarla nos proporcione un `true` o un `false`). Entre la condición y el `end` final podemos colocar un bloque de sentencias. Dicho bloque se ejecuta si y solamente si la condición es verdadera. Si la condición es falsa, no se ejecuta ninguna instrucción del bloque y el flujo pasa a la siguiente instrucción que hayamos escrito tras el `end` (si no hay nada tras el `end` entonces se detiene la ejecución).

```julia
julia> if true
           println("Se ejecuta la orden de imprimir")
       end
Se ejecuta la orden de imprimir

julia> if false
           println("NO se ejecuta la orden de imprimir")
       end

julia> 
```

Hemos visto que si la condición es verdadera se ejecuta el bloque y en caso contrario no se ejecuta. Lo más habitual es que en la condición hagamos una comprobación de la que no sabremos "a priori" si es cierta o falsa. Si resulta que es cierta deseamos que se ejecute el bloque. En caso contrario no realiza nada.

```julia
julia> if isprime(7)
           println("El numero es primo")
       end
El numero es primo

julia> if isprime(9)
           println("El numero es primo")
       end

julia> 
```

En el bloque pueden ir más de una expresión.

```julia
julia> if true
           println("Imprimo esta linea")
           x = 9
           println(x)
       end
Imprimo esta linea
9
```

Como la condición es verdadera se ejecuta la primera instrucción y se imprime una frase por pantalla. Después se le asigna un valor a una variable `x` y por último se imprime por pantalla el valor de dicha variable.

En los ejemplos anteriores todo lo que hemos escrito entre la condición y el `end` lo hemos sangrado con una tabulación. Esta técnica se conoce en programación como **indentación**. En realidad la indentación nos ayuda a leer el código a los humanos, pero no es necesario indentar en Julia (en el lenguaje Python la indentación es obligatoria).

```julia
julia> if isprime(7) println("El numero es primo") end
El numero es primo
```

La estructura sintáctica de esta contrucción la podemos resumir como

```
if condicion
    bloque de sentencias
end
```


## La sentencia if else end

## La notacion  ? :

## Condicionales anidados
						


## El bucle for

## El bucle while
