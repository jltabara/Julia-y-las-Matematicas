# Cadenas

## Caracteres

Un caracter es una letra o cualquier otro símbolo. En Julia, y en otros muchos lenguajes, para escribir caracteres lo tenemos que hacer entre comillas simples. Los caracteres forman un nuevo tipo, denominado `Char`.

```julia
julia> 'a'
'a'

julia> typeof(ans)
Char

julia> '%'
'%'

julia> typeof(ans)
Char
```
Desde los albores de los ordenadores, a cada caracter se le asoció un número entero, de manera más o menos arbitraria. Este es el conocido como código ASCII. La tabla ASCII tiene 128 caracteres e incluye todas las letras, signos de puntuación, ... que son necesarios para escribir en lengua inglesa. Para conocer el número asociado a un caracter utilizamos al función `Int(c)`. Para conocer el caracter asociado a un número utilizamos la función `Char(n)`.

```julia
julia> Int('c')
99

julia> Char(99)
'c'

julia> Int('%')
37

julia> Char(37)
'%'
```

Sin embargo existen muchos más caracteres: las vocales acentuadas, nuestra letra `ñ`, todas las letras chinas, ... Por ello se amplió esta tabla y se creo UNICODE. Julia permite trabajar con caracteres unicode y "asociar" caracteres a números superiores al 127.

```julia
julia> Char(9321)
'⑩'

julia> Char(2731)
'ફ'
```

Para saber como se guarda un caracter en el ordenador, en notación binaria, utilizamos la funcion `bits(x)` y para saber cuantos bytes de memoria ocupa cada caracter utilizamos la función `sizeof(x)`.

```julia
julia> bits('x')
"00000000000000000000000001111000"

julia> sizeof('x')
4

julia> bits('⑩')
"00000000000000000010010001101001"
```

Como vemos, los caracteres en Julia ocupan 4 bytes. Es el precio (en memoria) que  debemos pagar para poder utilizar los caracteres unicode.


## Cadenas y su impresión

Una cadena (*string* en inglés) es un conjunto ordenado de caracteres. En Julia deben estar limitados por dobles comillas. En principio en las cadenas se puede incluir cualquier caracter, con algunas excepciones, que veremos posteriormente. 

Las cadenas forman un nuevo tipo denominado `String` (en realidad el tipo no es directamente `String`, pero por ahora no nos interesa la diferencia).

```julia
julia> "Hola Mundo"
"Hola Mundo"

julia> typeof(ans)
ASCIIString
```

Al mostrar una cadena por pantalla vemos que la imprime con sus comillas. Si queremos "imprimir" una cadena, cosa que es útil sobre todo al desarrollar programas, podemos utilizar la función `println(S)`, que imprime la cadena y produce un salto de línea.

```julia
julia> println("hola mundo")
hola mundo
```

La función `println(x)` además de imprimir cadenas puede imprimir muchos otros tipos de datos.


## Concatenación y "multiplicación"


Concatenar dos cadenas de texto es unir la primera con la segunda. Esto se realiza en Julia con el operador asterisco y por ello a veces hablamos de multiplicación de cadenas. Esto es muy distinto al estandard de los lenguajes de programación, que suelen utilizar el signo de suma para concatenar cadenas. 

Multiplicar varias veces el mismo objeto es calcular la potencia.

```julia
julia> "Hola" * "Mundo"
"HolaMundo"

julia> "Hola" * "Hola" * "Hola"
"HolaHolaHola"

julia> "Hola"^3 # Multiplicar 3 veces la cadena
"HolaHolaHola"
```

## Caracteres de escape

Existen algunos caracteres que al imprimirlos se comportan de manera inesperada. Veamos un ejemplo

```julia
julia> println("Hola\nmundo")
Hola
mundo
```

Resulta que en vez de imprimir `\n` hemos creado una nueva línea. Este es un ejemplo de caracteres de escape. Existen más caracteres de escape y todos comienzan con la barra invertida. A pesar de estar escritos con dos símbolos forman un único caracter.

```julia
julia> Int('\n')
10

julia> Char(10)
'\n'

julia> print('\n') # Al imprimirlo, salta una linea


julia> # Se ha creado una linea nueva
```

Veamos una lista simplifica de caracteres de escape.

| Caracter| Acción al imprimir|
| --- | --- |
|`\n` | Crea una nueva línea|
|`\t`   | Crea una tabulación|
|`\"`   | Imprime comillas dobles|
|`\\`   | Imprime la barra invertida|
|`\$`   | Imprime el caracter del dolar|

```julia
julia> println("Uno\tdos\ttres")
Uno	dos	tres

julia> println("Una comilla \" he impreso")
Una comilla " he impreso

julia> println("Una barra \\ y un dolar \$")
Una barra \ y un dolar $
```

## Interpolación de variables

Si tenemos una variable de nombre `algo` muchas veces nos interesa imprimir el contenido de la variable. Lo más sencillo es utilizar la función `println(S)` sobre la variable y nos imprime su valor. Pero, ¿y si queremos que imprima su valor dentro de una cadena? Ello es posible en Julia y se realiza de modo muy sencillo utilizando la interpolación de variables y la construcción `$()`.  Si dentro del paréntesis existe una variable o una operación con variables, a la hora de imprimir, Julia realiza el cálculo y en lugar de escribir los caracteres correspondientes, imprime el valor del cálculo.

```julia
julia> x = 56
56

julia> println("El valor de x es $(x)")
El valor de x es 56

julia> println("Una operacion con x nos da $(x^2 + 6x-3)")
Una operacion con x nos da 3469

julia> println("El seno de $(x) es $(sin(x))")
El seno de 56 es -0.5215510020869119
```

La técnica de incluir valores de variables dentro de cadenas se conoce como interpolación.

## Indexación y subcadenas


En cierto sentido, una cadena es similar a un vector de caracteres. Muchas de las funciones que actuan sobre vectores se pueden aplicar también sobre cadenas. En particular todo lo referente a la indexación se traslada sin ningún cambio.


```julia
julia> s = "Hola mundo"
"Hola mundo"

julia> s[1]
'H'

julia> s[1:4]
"Hola"

julia> s[4:end]
"a mundo"
```

Sin embargo las cadenas presentan una diferencia fundamental y es que al igual que las tuplas, las cadenas son inmutables. Si intentamos variar el contenido de una cadena obtendremos un mensaje de error.


## Funciones sobre cadenas

La función `length(S)` calcula la longitud de la cadena. Es decir, nos dice el número de caracteres que la forman.

```julia
julia> s = "Hola mundo"
"Hola mundo"

julia> length(s) # El espacio es un caracter
10
```

`sizeof(S)` nos dice el número de bytes que ocupa en memoria dicha cadena.


```julia
julia> sizeof("Hola mundo")
10
```

Como vemos, las cadenas utilizan un byte para cada caracter, lo cual choca con los 4 bytes que utiliza para guardar un caracter.

```
julia> sizeof('a')
4

julia> sizeof("a")
1
```

`string(n)` transforma objetos en cadenas. Por ejemplo, puede convertir un número en una cadena.

```julia
julia> string(235)
"235"

julia> string(34.89)
"34.89"

julia> string(34e5)
"3.4e6"

julia> s = big(2)^2^2^2^2; # Numero muy grande. No se muestra  

julia> length(string(s)) # Convertido en cadena tiene ...
19729
```




