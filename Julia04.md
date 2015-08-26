# Cadenas

## Caracteres

Un caracter es una letra o cualquier otro símbolo. En Julia, y en otros muchos lenguajes, para escribir caracteres lo tenemos que hacer entre comillas simples. Los caracteres forman un nuevo tipo, denominado `Char`.

```julia
julia> 'a'
'a'

julia> typeof(ans)
Char

julia> '%' # Son caracteres todos los otros simbolos del teclado
'%'

julia> typeof(ans)
Char
```
Desde los albores de los ordenadores, a cada caracter se le asoció un número entero, de manera más o menos arbitraria. Este el conocido como código ASCII. La tabla ASCII tiene 256 caracteres e incluiye los símbolos más habituales. Para conocer el número asociado a un caracter utilizamos al función `Int(c)`. Para conocer el caracter asociado a un número utilizamos la función `Char(n)`.

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

Pasado el tiempo, se vio que la tabla ASCII no era suficiente para incluir la gran cantidad de símbolos presentes en varios idiomas. Por ello se amplió esta tabla y se pueden asociar más caracteres (los llamamos caracteres UNICODE) a otros números.

```julia
julia> Char(6789)
'᪅'

julia> Char(9321)
'⑩'
```

Para saber como se guarda en el ordenador en notación binaria, utilizamos la funcion `bits(x)` y para saber cuantos bytes de memoria ocupa cada caracter utilizamos la función `sizeof(x)`.

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

Una cadena (*string' en inglés) es un conjunto ordenado de caracteres. En Julia deben estar limitados por dobles comillas. En principio en las cadenas se puede incluir cualquier caracter, con algunas excepciones. Existen diversos caracteres que tienen una función especial dentro de una cadena y que si los imprimimos no aparece como caracteres. Los veremos posteriormente. Las cadenas forman un nuevo tipo denominado `String` (en realidad el tipo no es directamente `String`, pero por ahora no nos interesa la diferencia).

```julia
julia> "Hola Mundo"
"Hola Mundo"

julia> typeof(ans)
ASCIIString
```

Al mostrar una cadena por pantalla vemos que la imprime con sus comillas. Si queremos "imprimir" una cadena, cosa que útil sobre todo al desarrollar programas, la función `println(S)`, que imprime la cadena y produce un salto de línea.

```julia
julia> println("hola mundo")
hola mundo
```



## Concatenación y "multiplicación"


Concatenar dos cadenas de texto es unir la primera con la segunda. Esto se realiza en Julia con el operador asterisco y por ello a veces hablamos de multiplicación de cadenas, aunque realmente no tenga sentido.

```julia
julia> "Hola" * "Mundo"
"HolaMundo"

julia> "Hola" * "Hola" * "Hola"
"HolaHolaHola"

julia> "Hola"^3 # Multiplicar 3 veces la cadena
"HolaHolaHola"
```





## Interpolación de variables

## Indexación y subcadenas

## Funciones sobre cadenas

inmutables, caracteres de escape