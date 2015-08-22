
## Operaciones con números enteros (1ª parte)

Julia es un lenguaje de programación que nos permite realizar operaciones con números enteros. Sin embargo dichos cálculos solamente **son correctos si no sobrepasamos, ni por arriba ni por abajo, unos valores que Julia tiene predefinidos**. Para  conocer dichos límites, en nuestro ordenador, debemos ejecutar los comandos `typemax(Int)` y `typemin(Int)`.

>Para ejecutar un comando en la versión web de Julia, debemos escribir el comando y a continuación pulsar la combinación de teclas **Mayúsculas +  Enter**. También podemos pulsar el triángulo de la barra de herramientas (botón *play*). En la consola basta con teclear **Enter**.


```
julia> typemax(Int)
9223372036854775807

julia> typemin(Int)
-9223372036854775808
```

**Para realizar las operaciones las escribimos del mismo modo que en matemáticas**, utilizando los siguientes operadores: un + para la suma, un - para la resta (o para escribir números negativos), un \* para la multiplicación y un circunflejo para la potencia. Dejamos la división para más tarde.

> En Julia podemos escribir **comentarios**. Para ello escribimos un sostenido (`#`) y todo lo que viene a continuación en dicha línea se considera un comentario. Julia no hace ningún caso de comentarios. Solamente son útiles para los humanos que lean código escrito en Julia. Aunque se pueden escribir caracteres acentuados o especiales no es recomendable hacerlo.

> También podemos hacer comentarios de varias líneas. Para ello el comentario debe comenzar por `#=` y terminar por `=#`. Esto puede ser útil para "anular" alguna parte del código.

```
julia> 34 + 90 # Esta suma la realiza correctamente
124

julia> 1237 - 7899 # Esta resta tambien
-6662

julia> 45 * 789
35505

julia> (-45) * 789 # El menos sirve para negativos
-35505

julia> 2^5 # Una potencia calculada correctamente
32

julia> 2^600 # Obviamente mal. Hemos sobrepasado el limite
0

julia> 9223372036854775807 + 1  
-9223372036854775808
```


**La jerarquía en las operaciones es la misma que en matemáticas**: si hay multiplicaciones y sumas realiza antes las multiplicaciones,... El orden de las operaciones se puede variar utilizando paréntesis. Los paréntesis se pueden anidar (escribir paréntesis dentro de paréntesis). **No podemos utilizar corchetes** aunque éstos se utilicen en la matemática elemental.

```
julia> 4 * 3^2  # Las potencias antes que las multiplicaciones
36

julia> 4 + 6 * 2  # Las multiplicaciones antes que las sumas ...
16

julia> (3 + 6) * (34 - 67) 
-297

julia> ((2 + 6) * 8) - (34 - 23)  # Parentesis anidados
53
```

> Julia tiene "ciertos toques" de los lenguajes funcionales. En un lenguaje funcional, hasta los operadores son funciones. Aunque desde el punto de vista matemático puede parecer extraño, podemos poner el operador en **forma prefija** y entre paréntesis y separados por comas cada uno de los factores o sumandos. Aunque parezca mentira esta forma de escribir los operadores es utilizada en la programación con lenguajes funcionales.

```
julia> +(3, 6) # Equivale a 3 + 6
9

julia> *(3, 6, 2) # Equivale a 3 * 6 * 2
36

julia> ^(2, 8) # Equivale 2^8
256
```

   
## Operaciones con números enteros (2ª parte)



Hemos visto que Julia comete errores evidentes en las operaciones con números enteros. Ello es debido a que si tecleamos un número entero Julia entiende que es un dato del tipo `Int64` (en ordenadores de 64 bits). Para conocer el tipo de dato de un cierto valor utilizamos la función `typeof(x)`.

```
julia> typeof(34)  # El tipo de dato del 34
Int64
```

Si escribimos un número entero que sobrepase el límite, pero sin pasarnos mucho, vemos que Julia considera que es un dato de tipo `Int128`. Naturalmente los datos de tipo Int128 tienen un máximo y un mínimo, que podemos conocer con las funciones `typemax(tipo)` y `typemin(tipo)`.

```
julia> typeof(34341351345134513451451)
Int128

julia> typemax(Int128)  # El valor maximo de este tipo de dato
170141183460469231731687303715884105727
```

Si sobrepasamos los límites del tipo de dato Int128, aparece un nuevo tipo de dato: `Base.GMP.BigInt` (*Big Integer* significa *entero grande*  en inglés).

```
julia> typeof(234123412341234123412341234123412341234123431)
Base.GMP.BigInt
```

> Si estamos escribiendo en Julia, y escribimos `ans` (del inglés **answer**), nos estamos refiriendo al último resultado que Julia nos ha devuelto.

```
julia> 10 + 78
88

julia> ans # Nos devuelve el ultimo resultado
88

julia> ans + 10 # Podemos operar con ans
98
```


Cuando Julia opera con enteros de tipo `BigInt`  **siempre realiza los cálculos de modo correcto**. Si los enteros fuesen siempre del tipo **BigInt** no tendríamos problemas. Sin embargo para hacer esto se lo debemos decir explícitamente a Julia. Para ello podemos utilizar la función `big(x)` o el "constructor" `BigInt(x)`. 

```
julia> big(10)
10

julia> typeof(ans)
Base.GMP.BigInt

julia> BigInt(10) # Otra forma de hacer lo mismo
10

julia> 2^500 # Error pues 2 es de tipo Int64
0

julia> big(2)^500 # Sin errores
327339060789614187001318969682759915221664204604306
478948329136809613379640467455488327009232590415715
0886684127560071009217256545885393053328527589376

julia> big(3)^100 - 3^101 # Nuevo error
515377520732011331036461129774575141238720157790

julia> big(3)^100 - big(3)^101 # Solucionado
-1030755041464022662072922259531242545404215044002
```

> Desde un punto de vista matemático no se entiende la diferencia entre los distintos  tipos de enteros. **Lo más natural es que todos los enteros fuesen de tipo BigInt y entonces todas las operaciones serían correctas**. La existencia de tipos de datos se debe a otra razón, que intentaremos explicar.

> A nivel de ordenador, realizar operaciones con datos de tipo Int64 o Int128 es mucho menos "costoso" que realizar la misma operación pero con datos de tipo BigInt. De esta forma cuando hay muchos cálculos, Julia puede realizar los cálculos de manera más rápida.

> En realidad existen otros tipos de enteros para optimizar todavía más las operaciones: `Int8`, `Int16`, `UInt16`,...  Todos estos tipos tiene problemas de desbordamiento (*overflow* en inglés) pero permiten optimizar los cálculos. Para encontrar los valores que producen desbordamiento utilizamos las funciones *typemax()* y *typemin()*.  Si por algún motivo queremos crear un entero, por ejemplo  de tipo Int8, utilizamos el constructor `Int8(x)`.

```
julia> typemax(Int8)
127

julia> Int8(23) # Utilizamos el constructor
23

julia> typeof(ans)
Int8

julia> Int8(23) * Int8(20) # Desbordamiento
-52

julia> Int8(23) * 20 # Correcto. Se "transforma" en Int64
460

julia> typeof(ans)
Int64
```


# División y números en coma flotante

Para realizar la división se utiliza la barra inclinada. Sin embargo el resultado de una división de enteros **nunca** es un número entero. Es un nuevo tipo de número, que Julia denota `Float64` (en general hablaremos de tipo *float* obviando el 64). Los números de tipo float son el análogo en los ordenadores de los números reales en matemáticas.

```
julia> 5 / 9
0.5555555555555556

julia> typeof(ans)
Float64
```

Con este tipo de números los ordenadores tienen muchos problemas pues es muy habitual que los cálculos sean erroneos. Existen dos
problemas fundamentales por los que Julia comete errores al realizar cálculos con números de tipo *float*.

- Muchos números reales tienen en realidad infinitas cifras y los ordenadores y las calculadoras solamente trabajan con un número finito de cifras decimales.

- La forma que tienen los ordenadores, y en particular Julia, de guardar en memoria estos números. Los números se guardan empleando la base 2 y esto hace que algunos cálculos en base 10 no sea posible realizarlos sin errores de "redondeo".
 

Los números decimales se escriben con un punto. También los podemos escribir en notación científica. Para ello escribimos las parte decimal, después la letra **e** y finalmente el exponente de la potencia de diez.

```
julia> 5 * 4.20 + 1  # Cinco por cuatro veinte mas uno ¡¡¡22!!! :-)
22.0

julia> 1.234e5  # El numero es 1.234 * 10^5
123400.0

julia> 3.2 * 5.7  # Un error "incomprensible". Si lo hace un niño
18.240000000000002

julia> 2.5 ^ 3  # Las potencias pueden tener exponente entero
15.625

julia> 2.5 ^ 6.4  # O cualquier otro tipo de exponente
352.22165671562846
```
Para calcular raíces cuadradas se emplea la función `sqrt(x)`(del inglés *square root*). Para realizar otro tipo de raíces debemos tener en cuenta la relación matemática

$$
\sqrt[n]{x} = x^{1/n}
$$

que nos permite calcular cualquier raíz utilizando potencias.

```
julia> sqrt(2)
1.4142135623730951

julia> 2^0.5
1.4142135623730951

julia> 2^(1/2)
1.4142135623730951

julia> 3456.75^(1/7)
3.2027570506275747
```
Si en una operación combinada se mezclan enteros y flotantes, Julia convierte los enteros  a tipo flotante y realiza las operaciones, dando siempre como resultado un número de tipo flotante, pudiendo cometer errores de cálculo.

```
julia> 3 + 6.2
9.2

julia> 3 + 6.2 - 6.2 # Parece que Julia no sabe "na"
2.999999999999999
```


## Números racionales

Julia puede trabajar también con **números racionales o fracciones**, que forman un nuevo tipo, llamado **Rational**. Para construir un número racional empleamos la doble barra o el constructor `Rational(x,y)`. La operaciones que realizamos con racionales nos devuelven un número racional (simplificado). Pueden existir problemas de desbordamiento por utilizar enteros "finitos", que se solventan utilizando enteros `BigInt`.

```
julia> 5 // 11 # Construimos la fraccion 5/11
5//11

julia> typeof(ans)
Rational{Int64}

julia> Rational(5,11) # Otra forma de hacer lo mismo
5//11

julia> 300 // 400 # El resultado siempre simplificado
3//4
```

Las operaciones con fracciones dan como resultado una fracción. Si aparecen enteros y fracciones el resultado también es fraccionario, pero si en la operación aparece un *float*, entonces el resultado ya nos los devuelve con decimales.

```
julia> (3 // 4) * 7 + 45 * 90
16221//4

julia> 3 // 4 + 0.5 + 6  # Fracciones y flotantes
7.25
```
Se pueden producir errores de desbordamiento, que se solucionan utilizando enteros grandes. No es necesario entender todo lo que nos dice Julia sobre el error. Basta con "saber" que hemos cometido un error.

```
julia> (5 // 11)^50
ERROR: OverflowError()
 in * at rational.jl:179
 in power_by_squaring at intfuncs.jl:94
 in ^ at rational.jl:305

julia> (big(5) // 11)^50
88817841970012523233890533447265625//1173908528
7969531650666649599035831993898213898723001
```
 
Existen varias funciones que trabajan con números racionales. Algunas de ellas son:

| | |
| --- | --- |
|`float(x)` | Devuelve la aproximación decimal|
|`num(x)`   | Devuelve el numerador (simplificado)|
|`den(x)`   | Devuelve el denominador (simplificado)|

```
julia> float(5 // 11)
0.45454545454545453

julia> typeof(ans)
Float64

julia> num(5 // 11)
5

julia> typeof(ans)
Int64

julia> den(2340 // 2362) # Fraccion reducible
1181
```


## Funciones elementales de variable real

Julia tiene predefinidas algunas constantes matemáticas. Estos números son de un nuevo tipo, pero a efectos prácticos podemos considerarlos de tipo *float*.

```
julia> pi
π = 3.1415926535897...

julia> typeof(ans)
Irrational{:π}

julia> e
e = 2.7182818284590...

julia> typeof(ans)
Irrational{:e}

julia> golden # El numero de oro
φ = 1.6180339887498...

julia> typeof(ans)
Irrational{:φ}
```

Las funciones que aparecen en una calculadora científica suelen ser funciones que toman un argumento real y devuelven otro número real. Todas esas funciones (y muchas más) están implementadas en Julia. Calcular valores y operar con ellos es sencillo e intuitivo en Julia. En particular Julia tiene dos versiones de las funciones trigonométricas: las "normales" trabajan en radianes. Las que tienen el sufijo "d" trabajan en grados (grado en inglés es *degree*).

```
julia> sin(pi/2) # En radianes
1.0

julia> sind(90) # En grados
1.0

julia> sinh(3) # El seno hiperbolico
10.017874927409903

julia> asind(0.5) # Funcion inversa en grados
30.000000000000004

julia> asin(0.5) # Lo mismo pero en radianes
0.5235987755982989

julia> exp(5) # La funcion exponencial
148.4131591025766

julia> e^5
148.4131591025766

julia> log(e) # log es el logaritmo neperiano
1

julia> log(10)
2.302585092994046
```

> Para obtener ayuda sobre una función de la que conocemos su nombre, escribimos una `?` delante del nombre. En la consola lo que ocurre es que nos cambian el *prompt*, transformandose en `help>`.



```
help?> log
search: log logm log2 log1p log10 logdet logspace logabsdet Clong

  log(x)

  Compute the natural logarithm of x. Throws DomainError for
  negative Real arguments. Use complex negative arguments to obtain
  complex results.

  There is an experimental variant in the Base.Math.JuliaLibm
  module, which is typically faster and more accurate.

  log(b,x)

  Compute the base b logarithm of x. Throws DomainError for negative
  Real arguments.
```
  
> También podemos escribir el comienzo del nombre de una función y pulsar el tabulador. Julia nos propone los distintos nombres de funciones que comienzan por dichas letras. Por ejemplo, si escribimos `log` y pulsamos el tabulador nos aparecen varias funciones.

```
> julia> log # Pulsar el tabulador
log       log1p      logabsdet  logm
log10     log2       logdet     logspace
```

Leyendo la ayuda hemos visto que la función para calcular el logaritmo en base 10, es la misma función `log(x)` pero escribiendo como primer parámetro (los parámetros se separan con comas), el valor de la base. La función `log10(x)` también sirve para lo mismo.

```
julia> log(10,1000)
2.9999999999999996

julia> log10(1000)
3.0
```
En la siguiente tabla podemos ver algunas de las funciones que tiene predefinidas Julia.

| | |
| --- | --- |
|`sin(x)` | Seno en radianes|
|`cos(x)`   | Coseno en radianes|
|`tan(x)`   | Tangente en radianes|
|`asin(x)`   | Arcoseno en radianes|
|`sinh(x)`   | Seno hiperbólico|
|`log(x)`   | Logaritmo neperiano|
|`log10(x)`   | Logaritmo en base 10|
|`log(b,x)`   | Logaritmo en base b|
|`exp(x)`   | Exponencial|


## Divisibilidad de números enteros

En la teoría elemental de números es importante tratar cuestiones referentes a la divisibilidad (hacer divisiones, saber cuando una división exacta, cálculo del MCD, ...). Julia tiene algunas funciones predefinidas y que tienen como argumento necesariamente números enteros. He aquí algunas.

| | |
| --- | --- |
|`div(n,m)` | Cociente de n entre m|
|`rem(n,m)` | Resto de n entre |
|`divrem(n,m)` | Cociente y resto de n entre m|
|`gcd(n,m)` | Máximo común divisor de n e m|
|`lcm(n,m)` | Mínimo común múltiplo de n e m|

```
julia>  div(89, 5)
17

julia> rem(89, 5)
4

julia> 89 % 5 # Equivale al calculo del resto
4

julia> divrem(89, 5) Devuelve una tupla (cociente, resto)
(17,4)

julia> gcd(24, 78) #"Greatest common divisor" en ingles
6

julia> lcm(24, 78) #$Least common multiple" en ingles
312
```

## Primos y compuestos

Una vez estudiada la divisibilidad nos interesan todo lo referente al estudio de los números primos: saber si un número es primo o compuesto, la descomposición en factores primos, ...

| | |
| --- | --- |
|`isprime(n)` | Devuelve `true` si n es primo|
|`factor(n)` | Descomposición en factores primos|

```
julia> isprime(78)
false

julia> isprime(83)
true

julia> factor(78) # 78 = 13^1 * 2^1 * 3^1
Dict{Int64,Int64} with 3 entries:
  13 => 1
  2  => 1
  3  => 1

julia> factor(360) # 360 = 2^3 * 3^2 * 5^1
Dict{Int64,Int64} with 3 entries:
  2 => 3
  3 => 2
  5 => 1
  
julia> typeof(ans)
Dict{Int64,Int64}
```

El resultado de la función factor es ciertamente extraño. Es un nuevo tipo de dato, llamado `Dict`, que podemos traducir al castellano como *diccionario*. De momento no es necesario conocer como se maneja este tipo de dato en Julia.

## Números en otras bases

Siempre hemos oido que los números se guardan en el ordenador utilizando ceros y unos. Con la función `bits(x)` podemos comprobar como esto es así. Además veremos que el mismo dato se almacena de modo distinto, dependiendo del tipo que tenga asociado.

```
julia> bits(23)  # Un numero tipo Int64 (tiene 64 bits)
"0000000000000000000000000000000000000000000000000000000000010111"

julia> bits(Int8(23))  # Ahora solamente se utilizan 8 bits
"00010111"

julia> bits(23.0) # Parece lo mismo, pero ...
"0100000000110111000000000000000000000000000000000000000000000000"
```
 

En general escribimos los números en base 10. Podemos escribirlos en otras bases. Las dos más utilizadas son la octal y la hexadecimal (además de la binaria). Para las bases hexadecimal, octal y binaria existen funciones que transforman números en notación decimal a dichas bases. Para el resto empleamos la función `base(x)`.

| | |
| --- | --- |
|`hex(n)` | Transforma n en hexadecimal|
|`oct(n)` | Transforma n en octal|
|`bin(n)` | Transforma n en binario|
|`base(b, n)` | Transforma n en base b|


```
julia> hex(347) 
"15b"

julia> oct(347)
"533"

julia> bin(347)
"101011011"

julia> base(5, 347)
"2342"
```

# Números complejos

Los números complejos son un cuerpo numérico distinto y en Julia son un nuevo tipo de dato: `Complex`. Para construir el número complejo podemos emplear el constructor `Complex(x,y)` o también escribirlo en forma binómica, teniendo en cuenta que en Julia la unidad imaginaria se denota `im`, en lugar de la **i** empleada corrientemente en matemáticas o la **j** empleada en algunos estudios técnicos y lenguajes de programación.

```
julia> 4 + 7im
4 + 7im

julia> typeof(ans)
Complex{Int64}

julia> 4.7 + 90.67im
4.7 + 90.67im

julia> typeof(ans)
Complex{Float64}

julia> Complex(4,7) # Utilizando el constructor
4 + 7im
```

Las operaciones con complejos se realizan de la manera habitual. Si en una operación combinada aparece algún número complejo, entonces el resultado es también un número complejo.

```
julia> (3+6im)*(3-6im)^3
-1215 - 1620im

julia> (2.8 + 5.2im)^(3 - 9.1im) # Exponente complejo
3.4690185975468243e6 - 1.322692694462933e6im

julia> (2 + 6im)^2 + 7//3 - 6.3 * 4.1 # Resultado complejo
-55.49666666666667 + 24.0im

julia> e^(pi*im) # La formula de Euler ¡¡Que horror!!
-1.0 + 1.2246467991473532e-16im
```

Existen muchas funciones de variable compleja, pero las más elementales son las que nos permiten calcular el módulo, el argumento, el conjugado, ...

| | |
| --- | --- |
|`real(z)` | Parte real|
|`imag(z)` | Parte imaginaria|
|`abs(z)` | Módulo o valor absoluto|
|`conj(z)` | Conjugado|
|`angle(z)` | Argumento (en radianes)|

> En Julia podemos almacenar los números en variables. Para ello debemos escribir un nombre (más correctamente se llama un **identificador**), un signo igual y después el valor que deseamos almacenar en la variable. Si empezamos el identificador por una letra, prácticamente cualquier identificador es válido (existen algunas excepciones). Con dichas variables se puede operar del mismo modo que si fuesen números. También se pueden "inicializar" dos o más variables en la misma expresión.

```
julia> x= 5
5

julia> x^4 - 5x^2 + 3
503

julia> sin(x)
-0.9589242746631385

julia> a, b = 7, 9
(7,9)

julia> a
7

julia> b
9
```

> Si en Julia separamos por comas distintas operaciones, el resultado aparece ordenado en lo que Julia se llama una **tupla**. De momento no es necesario conocer el manejo de las tuplas en Julia.

```
julia> z = 4 + 7im
4 + 7im

julia> real(z), imag(z)
(4,7)

julia> typeof(ans)
Tuple{Int64,Int64}

julia> abs(z)
8.06225774829855

julia> conj(z)
4 - 7im

julia> angle(z)
1.0516502125483738
```

Muchas de las funciones elementales se pueden aplicar también a argumentos complejos. En algunos casos será necesario informar a Julia de que el número que le pasamos como argumento es un número complejo, pues en caso contrario se producen los llamados errores de dominio (la función sobre ese tipo de número no está definida).

```
julia> sqrt(-1)  # Error pues -1 no es de tipo Complex
ERROR: DomainError:
sqrt will only return a complex result if called with a complex
argument. try sqrt (complex(x))
 in sqrt at math.jl:146

julia> sqrt(-1+0im) # Ahora ya es de tipo Complex
0.0 + 1.0im

julia> 3 * exp(2im) # Modulo 3 y argumento 2. Forma polar
-1.2484405096414273 + 2.727892280477045im

julia> sin(3+89.6im)
5.772237281053336e37 - 4.049370231339495e38im

julia> log(-6 + 0im) # Existen los logaritmos de numeros negativos
1.791759469228055 + 3.141592653589793im
```

 


    


    
