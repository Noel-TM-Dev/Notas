

# Expresiones regulares con JS 💻
  
-  `/{caracteristicas}/` todas las expresiones que se requieran van dentro de los  slash 
-  `g` encontrar todas las coincidencias de la cadena de texto, va al final de cualquier expresion
-  `i` encontrar coincidencias sin importar si son mayusculas o minusculas, va al final de cualquier expression
-  `[]` buscar todo lo que se encuentra dentro en cada palabra o contenido de la expresion y retorna un array de coincidencias , si no encuentra nada retorna  `null` en el caso de `.match()` y en `test()` retornara un valor `true` o `false`
- `[^]` buscar todo lo que no se encuentra en la expresion con `^` dentro de los corchetes
- `[a-z]` solo letras minusculas
- `[A-Z]` solo letras mayusculas
- `\d` solo digitos `[0-9]`
- `\D` todo lo que no sea digito `[^0-9]`
- `\w` todos los caracteres alfanumericos `[A-Za-z0-9_]`
- `\W` todos los caracteres que no sean alfanumericos `[^A-Za-z0-9_]`
- `\s` espacios y `[ \r\t\f\n\v]`
- `\S` coincide con un caracter no espacial
- `\r` retorno de carro
- `\t` tabulacion
- `\f` form feed 
- `\n` nueva linea 
- `?` coincide 0 o 1 vez, `a?b` coincide ambos `ab` o `b` 
- `+` coincide al menos una vez, `a+` coincide ambas `aaaa` or `a`
-  `*` coincide 0 o mas veces, `a*b` coincide  `aaab`, `ab` o `b` 
- `^` validar el inicio de la cadena
- `$` validar el final de la cadena




> ## Funciones utilizadas en Javascript
- #### `.test()`      
    ``` js
    `${expression}`.test(`${string}`);

     return bool; //false or true
    ```
- #### `.match()`
    ``` js
    `${string}`.match(`${expression}`);
    return []; //retorna array o null si no encuentra coincidencias
    ```


## EJEMPLOS BASICOS CON `.test()` y `.match()`
 
1. **Encontrar una palabra o letra literal**
``` js
  const expression = /Word/
```  
2. **Encontrar mas de una coincidencia**
``` js
 const expression = /Word0|Word1|Word3/
```  
3. **Ignorar mayusculas y minusculas**
``` js
  const expression = /Word/i
```  
4. **Encontrar mas de una coincidencia**
``` js
  const expression = /Word/ig
```  
5. **comodin para buscar coincidencias en cualquier palabra**
``` js
  const expression = /Wor./
  //habria coincidencia en world|word|worm
```  
6. **Coincidencias con multiples posibilidades**
``` js
  const string='world ward wurm';
  const expression = /W[aou]r/
  //habria coincidencia en wor|war|wur
```  
7. **Coincidencias con todas las letras del alfabeto**
``` js
  const string='world ward wurm';
  const expression = /[a-z]/g
  //habria coincidencia con todas las letras y regresaria un array de toda la cadena
```  
8. **Coincidencias con todas las letras del alfabeto y cualquier numero**
``` js
  const string='world ward wurm 15698';
  const expression = /[a-z0-9]/ig
  //habria coincidencia con todas las letras y numeros 
```  
9. **Mostrar todo excepto las vocales**
``` js
  const string='world ward warm';
  const expression = /[^aeiuo]/ig
  //regresaria todas las consonantes existentes includos los espacios
```  
 - Nota: los caracteres como `., !, [, @, /` tambien son tomados en cuenta

10. **Coincidencia con letras repetidas**
``` js
  const string='woorld';
  const expression = /o+/g
  //habria coincidencia y regresaria ['oo']  
```    
11. **Coincidencias en caracteres que aparecen cero o mas veces**
``` js
  const string='wooooooooorld';
  const expression = /wo*/g
  //habria coincidencia y regresaria ['wooooooooo']  
```    
12. **Encontrar caracteres con Coincidencia Perezosa o Lazy Matching**
``` js
  const string='titanic';
  const expression = /t[a-b]*i/
  //habria coincidencia y regresaria ['titani']  
```    
 - Nota: solo coincidira con las palabras que comiencen con la primera y ultima letra en este caso `t` y  `i`

13. **Encontrar todas las coincidencias de una letra en una cadena**
``` js
  const string='P1P5P4CCCcP2P6P3';
  const expression = /C+/g
  //habria coincidencia y regresaria ['CCC']  
```
14. **Encontrar la primera coincidencia en un string**
``` js
  const string='Noel is first and can be found.';
  const expression = /^Noel/
  //habria coincidencia valor `true` y regresaria ['Noel']  
```    
15. **Encontrar la ultima coincidencia en un string**
``` js
  const string='is last and can be found Noel';
  const expression = /Noel$/
  //habria coincidencia valor `true` y regresaria ['Noel']  
```
16. **Encontrar todas las palabras, letras y numeros**

 Se puede encontrar con las siguiente expresion 
 `[A-Za-z0-9_]` o bien utilizar el accesso directo `\w`
  1. *Encontrar todas las palabras o conjunto de numeros*
``` js
  const string='is last and can be found Noel 521';
  const expression = /\w+/g
  //habria coincidencia valor `true` y regresaria
   [ 'is', 'last', 'and', 'can', 'be', 'found', 'Noel', '521'] 
```
  2. *Encontrar todas las letras o numeros*
``` js
  const string='is last and can be found Noel';
  const expression = /\w/g
  //habria coincidencia valor `true` y regresaria todas las letras en un array
```
17. **Encontrar todo lo contrario a palabras, letras y numeros**

 Se puede encontrar con las siguiente expresion 
 `[^A-Za-z0-9_]` o bien utilizar el accesso directo `\W` mayuscula
  1. *Encontrar todo lo que no sean palabras o conjunto de numeros*
``` js
  const string='is 512% last and can ..** be found Noel sds525';
  const expression = /\W+/g
  //habria coincidencia valor `true` y regresaria
   [ ' ', '% ', ' ', ' ', ' ..** ', ' ', ' ', ' ' ]
```
  2. *Encontrar todas los caracteres que no sean letras o numeros*
``` js
  const string='is last and can be found Noel';
  const expression = /\W/g
  //habria coincidencia valor `true` y regresaria todos los caracteres no alfanumericos
```
18. **Encontrar todos los numeros en una cadena**

 Se puede encontrar con las siguiente expresion 
 `[0-9]` o bien utilizar el accesso directo `\d` mayuscula
``` js
  const string='2001: A Space Odyssey';
  const expression = /\d/g
  //habria coincidencia valor `true` y regresaria [ '2', '0', '0', '1' ]
```
19. **Encontrar todo lo que no sea numero en una cadena**

 Se puede encontrar con las siguiente expresion 
 `[^0-9]` o bien utilizar el accesso directo `\d` mayuscula
``` js
  const string='2001: A Space Odyssey';
  const expression = /\D/g
  //habria coincidencia valor `true` y regresaria 
  [ ':', ' ', 'A', ' ', 'S', 'p', 'a', 'c', 'e', ' ', 'O', 'd', 'y', 's', 's', 'e', 'y' ]
```
20. **Encontrar todos los espacios**

 Se puede encontrar con las siguiente expresion 
 `[^0-9]` o bien utilizar el accesso directo `\d` mayuscula
``` js
  const string='2001: A Space Odyssey';
  const expression = /\D/g
  //habria coincidencia valor `true` y regresaria 
  [ ':', ' ', 'A', ' ', 'S', 'p', 'a', 'c', 'e', ' ', 'O', 'd', 'y', 's', 's', 'e', 'y' ]
```
---
   
 
## EJEMPLOS PRACTICOS
 
1. **Validar Usernames**
   - los usuarios solo pueden ingresar datos alfanumericos
   - los numeros solo pueden ir al final de la cadena. Puede ir sin numeros. 
   - El usuario no puede iniciar con un numero
   - las letras pueden ser mayusculas o minusculas
   - el usuario debe contener minimo dos caracteres alfanumerico.

``` js
  const expression = /^[a-z][a-z]+\d*$|^[a-z][a-z]+\d+\d+$/i
```  
2. **Encontrar mas de una coincidencia**
``` js
 const expression = /Word0|Word1|Word3/
```  